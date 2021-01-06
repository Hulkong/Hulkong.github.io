---
title: Project Data-ON GitLab CI & CD 구축
description: Project Data-ON GitLab CI & CD 구축
renderimg: /blog/project_512.png
img: /blog/landscape_1920.jpg
alt: Project Data-ON GitLab CI & CD 구축
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - project
  - web development
---

## TL;DR

1. GitLab Runner 설치(docker vs non docker)
2. Register JOB(Interact vs Non Interact)
3. Write .gitlab-ci.yml
4. TEST

## 1. GitLab Runner 설치(docker vs non docker)

docker run --detach \
 --name gitlab-runner \
 --restart always \
 --volume gitlab-runner:/etc/gitlab-runner \
 --volume /var/run/docker.sock:/var/run/docker.sock \
 gitlab/gitlab-runner:alpine-v10.7.4

## 2. Register JOB(Interact vs Non Interact)

### 1) ssl 서버인증서 설정

```bash
docker exec -it gitlab-runner bash

echo | openssl s_client -CAfile /etc/gitlab-runner/certs/git.openmate-on.co.kr.crt -connect git.openmate-on.co.kr:443


openssl s_client -connect 'input server domain':443 |tee certlog  # QUIT 입력
openssl x509 -inform PEM -in certlog -text -out /etc/gitlab-runner/certs/'input the filename' # 서버 인증서 생성
openssl x509 -inform PEM -text -in /etc/gitlab-runner/certs/'input the filename' # 확인 과정
```

### 2) Register

```bash
docker exec -it gitlab-runner bash

gitlab-runner register -n \
--url 'input git domain or ip' \
--registration-token 'input the token' \
--description gitlab-runner \
--executor docker \
--docker-image docker:latest \
--docker-volumes /var/run/docker.sock:/var/run/docker.sock
```

### 3) Runner 설치 및 시작

```bash
docker exec -it gitlab-runner gitlab-runner install
docker exec -it gitlab-runner gitlab-runner start
```

## 3. Write .gitlab-ci.yml

```yml
# This file is a template, and might need editing before it works on your project.
# Official docker image.
image: docker:latest

stages:
  - build
  - deploy

variables:
  GIT_STRATEGY: fetch
  GIT_SSL_NO_VERIFY: 'true'

services:
  - docker:dind

before_script:
  - ls -al
  - apk add docker-compose
  #- apk add --no-cache docker-compose

after_script:
  - echo "After script section"

build-release:
  stage: build
  script:
    - echo "Do your build here in release"
  only:
    - release

build-dev-frontend:
  stage: build
  script:
    - echo "build frontend"
    - docker build -t docker.openmate-on.co.kr/hulkong/test-cicd/Frontend:$CI_COMMIT_TAG frontend/
    - docker push docker.openmate-on.co.kr/hulkong/test-cicd/Frontend:$CI_COMMIT_TAG
  only:
    - develop
  cache:
    key: '$CI_JOB_STAGE-$CI_COMMIT_REF_SLUG'
    paths:
      - frontend/node_modules/
      #- backend/venv/

deploy-release:
  stage: deploy
  script:
    - echo "Do your deploy here in release"
    #- docker-compose down
    #- docker-compose -f docker-compose.prod.yml up -d
  only:
    - release
deploy-dev:
  stage: deploy
  script:
    - echo "Do your deploy here in development"
    #- docker-compose down
    #- docker-compose -f docker-compose.dev.yml up -d
  only:
    - develop
```

```yml
# This file is a template, and might need editing before it works on your project.
# Official docker image.
stages:
  - build
  - test
  - deploy

variables:
  GIT_STRATEGY: fetch
  GIT_SSL_NO_VERIFY: 'true'
  DOCKER_IMAGE_NAME: $CI_REGISTRY_IMAGE/Frontend:1.0.0

# 자동으로 실행되는 git clone(또는 fetch) disable
.skip_git_clone: &skip_git_clone
  variables:
    GIT_STRATEGY: none

#********************************************************
#                     [PIPELINE JOBS]
#********************************************************
stages:
  - print_variables


# build 용 docker image 가 없으면 생성 & container registry 에 등록
# docker image 가 있으면 skip
view_variables:
  <<: *skip_git_clone
  stage: print_variables
  script:
    - echo "-------------- [PRINT variables] --------------"
    - echo "CI_SERVER_URL                       :$CI_SERVER_URL"        # https://local.gitlab.co.kr
    - echo "CI_PROJECT_ID                       :$CI_PROJECT_ID"        #: 43          -> 동적 변경

    - echo "CI_JOB_ID                           :$CI_JOB_ID"            #: 519          -> 동적 변경
    - echo "CI_JOB_NAME                         :$CI_JOB_NAME"          #: view_variables
    - echo "CI_JOB_STAGE                        :$CI_JOB_STAGE"         #: print_variables
    - echo "CI_JOB_TOKEN                        :$CI_JOB_TOKEN"         #: [MASKED]
    - echo "CI_JOB_MANUAL                       :$CI_JOB_MANUAL"        #: n/a      -> stage 가 manual 설정이면 true

    - echo "CI_BUILDS_DIR                       :$CI_BUILDS_DIR"        #: /home/gitlab-runner/builds

    - echo "CI_PIPELINE_ID                      :$CI_PIPELINE_ID"       #: 291
    - echo "CI_PIPELINE_IID                     :$CI_PIPELINE_IID"      #:
    - echo "CI_PIPELINE_URL                     :$CI_PIPELINE_URL"      #: https://local.gitlab.co.kr/twoseed-dev/twoseed-demo-api-project/pipelines/282
    # branch 생성, merge request 요청, git push 등으로 pipeline 실행되면 'push' 로 인식
    - echo "CI_PIPELINE_SOURCE                  :$CI_PIPELINE_SOURCE"   #: push         -> (branch 생성 / merge request 요청) 자동 pipeline 실행 : 'push' 로 인식

    # merge request 요청 시에는 destination branch 가 설정
    - echo "CI_COMMIT_BRANCH                    :$CI_COMMIT_BRANCH"     #: master       -> test-env-infos    -> (merge request 시 merge 된 branch) master
    - echo "CI_COMMIT_TAG                       :$CI_COMMIT_TAG"        #: n/a
    - echo "CI_COMMIT_REF_NAME                  :$CI_COMMIT_REF_NAME"   #: master       -> test-env-infos    -> (merge request 시 merge 된 branch) master
    # 전체 commit id : $CI_COMMIT_SHA
    - echo "CI_COMMIT_SHORT_SHA                 :$CI_COMMIT_SHORT_SHA"  #: 78fc1e9c     -> 1908daa4     -> 동적 변경
    # merge request 요청 시에도 출력되지 않음... -> 'only: merge_requests' 또는 rules 설정을 한 경우에만 출력되는 것으로 판단
    - echo "CI_MERGE_REQUEST_TARGET_BRANCH_NAME :$CI_MERGE_REQUEST_TARGET_BRANCH_NAME"  #: n/a
    - echo "CI_COMMIT_MESSAGE                   :$CI_COMMIT_MESSAGE"    #: init source  -> 동적 변경

    - echo "CI_REGISTRY_IMAGE                   :$CI_REGISTRY_IMAGE"    #: local.gitlab.co.kr:5050/twoseed-dev/twoseed-demo-api-project
    - echo "CI_REGISTRY                         :$CI_REGISTRY"          #: local.gitlab.co.kr:5050

    # pipeline 을 실행하는 계정정보
    - echo "GITLAB_USER_ID                      :$GITLAB_USER_ID"       #: 2                -> 1
    - echo "GITLAB_USER_LOGIN                   :$GITLAB_USER_LOGIN"    #: twoseed-dev      -> root (issue&merge request 생성계정)     -> git push 한 계정    -> merge request 요청계정
    - echo "GITLAB_USER_EMAIL                   :$GITLAB_USER_EMAIL"    #: test@twoseed.co.kr  -> admin@example.com

    - echo "-------------- [PRINT local variables] --------------"
    - echo "DOCKER_IMAGE_NAME                   :$DOCKER_IMAGE_NAME"    #: local.gitlab.co.kr:5050/twoseed-dev/twoseed-demo-api-project/build-jdk8-ant
```

```yml
#********************************************************
#                     [DEFINE DEFAULT]
#********************************************************
default:
  tags:
    - tag-dind-runner # docker runner

#********************************************************
#                     [DEFINE VARIABLE]
#********************************************************
variables:
  GIT_SSL_NO_VERIFY: 'true'
  # 사설인증서를 사용한 경우, container-scanning 실행 시 에러 발생. DOCKER_INSECURE: "true" 로 설정하여 skip 처리
  DOCKER_INSECURE: 'true'
  DEPLOY_FILE_NAME: 'test-hello-world-maven-0.1.0.jar'
  DEPLOY_CONTAINER_NAME: 'mx_tmp_container'
  DEPLOY_IMAGE_NAME: $CI_REGISTRY_IMAGE/deploy_image
  RELEASE_VERSION: '1.0'
  WAS_IMAGE_NAME: 'tomcat:8.0-alpine'
  DEV_WAS_CONTAINER_NAME: 'dev-was-container'
  STG_WAS_CONTAINER_NAME: 'stg-was-container'

#********************************************************
#                     [DEFINE ANCHOR]
#********************************************************
# skip auto git clone
.skip_git_clone: &skip_git_clone
  variables:
    GIT_STRATEGY: none

#********************************************************
#                     [PIPELINE STAGES]
#********************************************************

stages:
  - build
  - test
  - package_image
  - view_image
  - deploy
  - dast
  - review
  - cleanup
  - performance
```
