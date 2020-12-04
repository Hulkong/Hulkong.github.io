---
title: docker container Gitlab과 docker container Gitlab-runner 연동
description: docker container Gitlab과 docker container Gitlab-runner 연동
renderimg: /blog/docker_512.png
img: /blog/landscape_1920.jpg
alt: docker container Gitlab과 docker container Gitlab-runner 연동
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - docker
  - web development
---

## 이슈사항
<br/>
1. 현재 docker 명령어로 되어있는 부분을 docker-compose 파일로 변경<br/>
2. .gitlab-ci.yml 프로세스 재정리<br/>
3. 컨테이너 기반 runner는 job실행시 로컬 캐쉬를 사용하지 못한다는 단점으로 인해 사용을 지양<br/>
4. Job 등록시 특정 디렉토리 마운트(최적화 위함)<br/>
5. Job 등록시 필요한 마라피터를 config.toml에 작성<br/>
6. config.toml에 concurrent = 4

<br/><br/>
## Runner 설치 및 Job 등록
<br/>
<h4>Docker 컨테이너 기반 runner 설치방법</h4>
<br/>
1. GitLab이 없을경우 컨테이너 생성

```bash
docker run --detach \
    --publish 80:80 --publish 22:22 \
    --name gitlab \
    --volume gitlab-config:/home/ailab/GitLab \
    --volume gitlab-logs:/home/ailab/GitLab/log \
    --volume gitlab-data:/home/ailab/GitLab/data \
    gitlab/gitlab-ce:10.2.8-ce.0
```
<br/>
2. GitLab Runner 컨테이너 생성

```bash
docker run --detach \
    --name gitlab-runner \
    --restart always \
    --volume gitlab-runner:/etc/gitlab-runner \
    --volume /var/run/docker.sock:/var/run/docker.sock \
    gitlab/gitlab-runner:alpine-v10.7.4
```

<br/>
3. 컨테이너간 통신을 위한 네트워크 설정

```bash
docker network create gitlab-network
docker network connect gitlab-network gitlab
docker network connect gitlab-network gitlab-runner
```

<br/>
4. ssl 서버인증서 설정

```bash
docker exec -it gitlab-runner bash

openssl s_client -connect 'input server domain':443 |tee certlog  # QUIT 입력
openssl x509 -inform PEM -in certlog -text -out /etc/gitlab-runner/certs/'input the filename' # 서버 인증서 생성
openssl x509 -inform PEM -text -in /etc/gitlab-runner/certs/'input the filename' # 확인 과정
```

<br/>
5. Job 등록
<br/><br/>

- Inter active 방법
```bash
gitlab-runner register -n \
--url http://gitlab/ \
--registration-token 'input the token' \
--description gitlab-runner \
--executor shell
```
<br/>

- Non-Inter active 방법
```bash
gitlab-runner register -n \
--url 'input git domain or ip' \
--registration-token 'input the token' \
--description gitlab-runner \
--executor docker \
--docker-image docker:latest \
--docker-volumes /var/run/docker.sock:/var/run/docker.sock
```
<br/>

- Runner 생성과 동시에 Job 등록하는 방법
```bash
docker run -it  \
--detach \
--name gitlab-runner \
--volume /home/hulkong/'input .crt file':/'input .crt file' \
--volume /var/run/docker.sock:/var/run/docker.sock \
gitlab/gitlab-runner:alpine-v10.7.4 \
register -n \
--tls-ca-file /'input .crt file' \
--url 'input git domain or ip' \
--registration-token 'input the token' \
--description gitlab-runner \
--executor docker \
--docker-image docker:latest \
--docker-volumes /var/run/docker.sock:/var/run/docker.sock

# Runner 설치 및 시작
docker exec -it gitlab-runner gitlab-runner install
docker exec -it gitlab-runner gitlab-runner start
```

```bash
# crt 포맷을 PEM 포맷으로 변경하는 방법
openssl x509 -in mycert.crt -out mycert.pem -outform PEM
```
<br/><br/>
## .gitlab-ci.yml 생성

```yml
# This file is a template, and might need editing before it works on your project.
# Official docker image.
image: docker:latest

stages:
  - build
  - test
  - deploy

variables:
  GIT_SSL_NO_VERIFY: "1"

cache:
  paths:
    - frontend/node_modules/
    - backend/venv/

services:
  - docker:dind

before_script:
  - echo "Before script section"
  - echo "For example you might run an update here or install a build dependency"
  - echo "Or perhaps you might print out some debugging details"
  - apk add --no-cache docker-compose

after_script:
  - echo "After script section"
  - echo "For example you might do some cleanup here"

build-release:
  stage: build
  script:
    - echo "Do your build here in release"
  only:
    - release

build-dev:
  stage: build
  script:
    - echo "Do your build here in development"
  only:
    - develop

test-1:
  stage: test
  script: 
    - echo "Do a test here"
    - echo "For example run a test suite"
  except:
    - develop

test-2:
  stage: test
  script: 
    - echo "Do a test here"
    - echo "For example run a test suite"
  except:
    - develop

deploy-release:
  stage: deploy
  script:
    - echo "Do your deploy here in release"
    - docker-compose down
    - docker-compose -f docker-compose.prod.yml up -d
  only:
    - release
deploy-dev:

  stage: deploy
  script:
    - echo "Do your deploy here in development"
    - docker-compose down
    - docker-compose -f docker-compose.dev.yml up -d
  only:
    - develop
```
<br/><br/>

## 오류

```bash
# 오류내용
Couldn't execute POST against https://hostname.tld/api/v4/jobs/request:
Post https://hostname.tld/api/v4/jobs/request: x509: certificate signed by unknown authority

# 해결책
echo | openssl s_client -CAfile /etc/gitlab-runner/certs/gitlab-hostname.tld.crt -connect gitlab-hostname.tld:443
```

<br/><br/>

## 참고

[[stackoverflow](https://stackoverflow.com/questions/44458410/gitlab-ci-runner-ignore-self-signed-certificate)]
[[gitlab1](https://gitlab.com/gitlab-org/gitlab-runner/-/issues/3180)]
[[gitlab2](https://docs.gitlab.com/runner/configuration/tls-self-signed.html)]
[[lesstif](https://www.lesstif.com/gitbook/https-ssl-curl-web-browser-16744456.html)]
