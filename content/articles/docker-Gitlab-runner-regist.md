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

## 설치

<h4>gitlab과 gitlab-runner가 같은 공간에 있을 경우</h4>

```bash

# gitlab 컨테이너 생성
docker run --detach \
    --publish 80:80 --publish 22:22 \
    --name gitlab \
    --volume gitlab-config:/home/ailab/GitLab \
    --volume gitlab-logs:/home/ailab/GitLab/log \
    --volume gitlab-data:/home/ailab/GitLab/data \
    gitlab/gitlab-ce:10.2.8-ce.0

# gitlab-runner 컨테이너 생성
docker run --detach \
    --name gitlab-runner \
    --restart always \
    --volume gitlab-runner:/etc/gitlab-runner \
    --volume /var/run/docker.sock:/var/run/docker.sock \
    gitlab/gitlab-runner:alpine-v10.7.4

docker network create my-network
docker network connect my-network gitlab
docker network connect my-network gitlab-runner
docker exec -it gitlab-runner bash

# ssl 서버인증서 설정
touch /etc/gitlab-runner/certs/'input .crt file'
echo | openssl s_client -CAfile /etc/gitlab-runner/certs/'input .crt file' -connect 'input server domain':443

# runner 등록
gitlab-runner register -n \
--url http://gitlab/ \
--registration-token 'input the token' \
--description gitlab-runner \
--executor shell

```

<h4>gitlab과 gitlab-runner가 다른 공간에 있을 경우</h4>

```bash

# gitlab-runner 컨테이너 생성
docker run --detach \
    --name gitlab-runner \
    --restart always \
    --volume gitlab-runner:/etc/gitlab-runner \
    --volume /var/run/docker.sock:/var/run/docker.sock \
    gitlab/gitlab-runner:alpine-v10.7.4

docker exec -it gitlab-runner bash

# ssl 서버인증서 설정
touch /etc/gitlab-runner/certs/'input .crt file'
echo | openssl s_client -CAfile /etc/gitlab-runner/certs/'input .crt file' -connect 'input server domain':443

# runner 등록
gitlab-runner register -n \
--url 'input git domain or ip' \
--registration-token 'input the token' \
--description gitlab-runner \
--executor docker \
--docker-image docker:latest \
--docker-volumes /var/run/docker.sock:/var/run/docker.sock

```

```bash

# 컨테이너 생성과 동시에 runner 등록하는 방법
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

```

```bash
# 서버인증서 가져오는 프로세스
openssl s_client -connect 'input server domain':443 |tee certlog
openssl x509 -inform PEM -in certlog -text -out certdata
openssl x509 -inform PEM -text -in certdata

# or
openssl x509 -in mycert.crt -out mycert.pem -outform PEM
```

<h4>.gitlab-ci.yml 생성</h4>

```yml
variables:
  GIT_SSL_NO_VERIFY: '1'
```

## 오류

```bash
Couldn't execute POST against https://hostname.tld/api/v4/jobs/request:
Post https://hostname.tld/api/v4/jobs/request: x509: certificate signed by unknown authority
```

<h4>해결책</h4>

```bash
echo | openssl s_client -CAfile /etc/gitlab-runner/certs/gitlab-hostname.tld.crt -connect gitlab-hostname.tld:443
```

<br/><br/>

## 참고

[[stackoverflow](https://stackoverflow.com/questions/44458410/gitlab-ci-runner-ignore-self-signed-certificate)]
[[gitlab1](https://gitlab.com/gitlab-org/gitlab-runner/-/issues/3180)]
[[gitlab2](https://docs.gitlab.com/runner/configuration/tls-self-signed.html)]
[[lesstif](https://www.lesstif.com/gitbook/https-ssl-curl-web-browser-16744456.html)]
