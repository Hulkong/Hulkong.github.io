# Gitlab CI CD로 자동 배포하기

>> GitLab CI is an open source CI service included with GitLab Since ver8.0

CI(Continuous Integration)는 애플리케이션에 대한 새로운 코드와 같은 변경사항이 일어났을 때, 정기적으로 테스트, 빌드 과정을 거쳐 공유 리퍼지토리에 통합되는 과정을 의미한다. 다수의 개발자가 같은 시기에 작업을 하여 발생할 수 있는 충돌을 해결하는데 도움을 준다.

CD(Continuous Deliver/Deployment)는 CI과정을 거친 애플리케이션을 리퍼지토리에 자동 업로드 도는 서비스 환경까지 바로 배포하는 것을 의미한다.

**Gitlab은 on-premise git을 제공하는 업체이다. Gitlab은 Commit / Merge / Release등의 이벤트가 발생하는 것을 감지하고 이로부터 CI/CD 파이프라인이 동작할 수 있는 환경을 제공한다.**

<br/>

Gitlab의 CI&CD기능을 이용하기 위해서는 배포될 서버에 `gitlab-runner`를 설치해야 한다.

원격서버에 `gitlab-runner`을 설치하고, Gitlab에 runner를 `register`하면 commit과 같은 이벤트 발생시 `gitlab-ci.yml`에 작성된 스크립트가 원격서버에서 실행되는 것이 전체 프로세스다.

<br/>

## Step1. Installing the Runner
### 1. Linux일 경우 Gitlab 공식 Repository 추가
```bash
# Debian/Ubuntu/Mint
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash

# RHEL/CentOS/Fedora
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh | sudo bash
```

### 2. Gitlab Runner 설치
2-1. 최신버전 설치
```bash
# Debian/Ubuntu/Mint
sudo apt install gitlab-runner

# RHEL/CentOS/Fedora
sudo yum install gitlab-runner

# MacOS
brew install gitlab-runner
```
<br/>

2-2 특정버전 설치
```bash
# Debian based systems
apt-cache madison gitlab-runner
sudo apt install gitlab-runner='version'

# RPM based systems
yum list gitlab-runner --showduplicates | sort -r
sudo yum install gitlab-runner-'version'
```

## Step2. Registering Runners
>> 별도 gitlab-runner 계정 생성하여 관리

<br/>

### 1. 계정생성
```bash
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```

### 2. 설치 및 실행
```bash
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start
```

### 3. Gitlab Runner 등록
```bash
sudo gitlab-runner register

# gitlab의 서버 주소를 입력
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com )
'input the domain or ip'

# Gitlab CI에서 발급된 토큰 값 입력
Please enter the gitlab-ci token for this runner:
'input the token'

# Runner 설명 추가
Please enter the gitlab-ci description for this runner:
'input the description'

# Runner 태그 설정(!important)
Please enter the gitlab-ci tags for this runner (comma separated):
'input the tag'

# Runner 동작기 선택
Please enter the executor: docker-ssh, ssh, virtualbox, docker, parallels, shell, docker+machine, docker-ssh+machine, kubernetes:
'select the executor'
```

## Step3. ADD .gitlab-ci.yml in the root folder of your project/repo

```yml
# Master 브랜치에 push가 들어올 때 동작하는 job
deploy-to-server:
  stage: deploy
  only:
    - master
  script:
    - echo 'hello world!'
  tags:
    - deploy
```
- deploy-to-server 은 임의로 지어준 JOB 이름
- Gitlab 은 스테이지(단계) 별로 특정 작업들을 수행할 수 있는 큰 그룹이 정해져 있음
- build, test, deploy 3단계의 스테이지가 있음
- 자세한 내용은 여기를 참고
- only는 master 브런치에 이벤트가 발생했을 경우 파이프라인 기능이 활성화 된다.
- script 항목에선 runner에 의해 수행될 쉘 스크립트를 작성한다.
- tags 는 특정 태그가 달린 runner에 명령을 내릴 수 있다. (여기서는 deploy 태그를 가진 runner에 명령을 내린다.)

## Step4. Commit and push file to gitlab repo

## Step5. Make any change in the project > commit > push

# 출처
- [Automation Step by Step - Raghav Pal
](https://www.youtube.com/watch?v=jUiKi6FWYrg&list=PLhW3qG5bs-L8YSnCiyQ-jD8XfHC2W1NL_&index=7)
- [내 머릿속 데이터베이스](https://namioto.ip.or.kr/2018/07/16/gitlab-ci%EB%A1%9C-%EC%9E%90%EB%8F%99%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0/)
- [이야기박스](https://box0830.tistory.com/297)
- [Kamang's IT Blog](https://kamang-it.tistory.com/entry/gitlabgitlab-cigitlab-ci%EC%9D%98-%EA%B8%B0%EC%B4%88%EC%99%80-%EA%B0%84%EB%9E%B5%ED%95%98%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)