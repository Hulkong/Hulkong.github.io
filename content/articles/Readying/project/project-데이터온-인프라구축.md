---
title: project [데이터온](https://data-on.co.kr) 웹 인프라 구축
description: project [데이터온](https://data-on.co.kr) 웹 인프라 구축
renderimg: /blog/project_512.png
img: /blog/landscape_1920.jpg
alt: project [데이터온](https://data-on.co.kr) 웹 인프라 구축
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - project
  - web development
---

## docker-compose 개발/운영 환경세팅

## 방향성

오픈메이트온에서는 **트렌드온, 세이프온, 라이트온, 스마트온, 데이트온(출시예정)** 등 다양한 서비스를 운영하고 있다. 그러면서 기존 서비스 운영환경은 상황에 따라 다르게 독립적으로 관리하고 있는 중이다. 운영서버 인프라 구조는 Iaas형태를 띄고 있으며 2가지 종류로 크게 나눌 수 있을 것 같다.

1. ubuntu + Docker(nginx + Sprigin framework + postgreSQL)
2. ubuntu + Docker(nginx + nuxtJS + dJango + postgreSQL)

개발서버 인프라 구조는 On-Premise(요즘 라이젠 사양이 어마무시...)형태를 가지며, 모든 애플리케이션을 도커기반으로 구성하고 있다.  
**But,** 이렇게 운영환경이 제각기 달라 현재 CI/CD(지금 만들고 있는 중...)가 없는 상태에서 배포과정이 매우 복잡하기 때문에 통일성을 가지기 위해서 모든 애플리케이션을 docker-compose 기반으로 관리하려고 한다. 추후에 Gitlab CI/CD를 구축할 때 Kubernates까지 생각하고 있기에 docker를 선택하게 되었다.

## 1. host설정

```bash
vi /etc/hosts # 127.0.0.1 openmateon-116698 입력
```

## 2. docker-compose 설치

```bash
apt update && apt install -y docker-compose
```

## 3. MTU 변경(Host OS)

```bash
# iwinv는 기본 MTU 1450 But, docker 컨테이너 기본 MTU 1500
# 다르면 docker 네트워크 통신 안됨

cp /lib/systemd/system/docker.service /etc/systemd/system/docker.service
vi /etc/systemd/system/docker.service # ExecStart=/usr/bin/dockerd -H fd:// --mtu 1450 # 추가
systemctl daemon-reload
service docker restart
```

## 4. docker 컨트롤 유저 생성

```bash
adduser data-on
usermod -aG sudo data-on
```

## 5. 프로젝트 다운로드

```bash
su - data-on
git config --global http.sslVerify false
git clone git@git.openmate-on.co.kr:Data-Service/Data-ON.git
```

## 6. ssl 저장 볼륨 직접 생성

```bash
sudo docker volume create --name=letsencrypt_etc
sudo docker volume create --name=letsencrypt_lib
```

## 7. ssl 생성

```bash
# 도메인 발급기관에서 해당도메인에 대한 txt타입 추가
sudo docker run -it --rm --name certbot \
  -v 'letsencrypt_etc:/etc/letsencrypt' \
  -v 'letsencrypt_lib:/var/lib/letsencrypt' \
  certbot/certbot certonly -d 'data-on.co.kr' --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory
```

## 8. crontab에 ssl 갱신 명령어 추가

```bash
sudo crontab -e # 하단 명령어 기입

# 6, 5,10, * * * docker run -it --rm --name certbot -v 'letsencrypt_etc:/etc/letsencrypt' -v 'letsencrypt_lib:/var/lib/letsencrypt' certbot/certbot renew --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory
```

[참조](https://openmatetest.sharepoint.com/:p:/s/OPENmate_ONIntranet2/EUYgKyHAmvlCvtRRn3OmNkoBMC6Q4EiNZRRffKJQa1esMA?e=a0USGz)

## 9. 서비스 컨테이너 생성

```bash
cd Data-ON && sudo docker-compose up -d
```

<br/><br/>

# 예외처리

## 1. nginx 컨테이너만 적용

```bash
sudo docker stop data_on_nginx
sudo docker rm data_on_nginx
sudo docker start data_on_nginx
```

## 2. FE 컨테이너만 적용

```bash
sudo docker stop data_on_frontend
sudo docker rm data_on_frontend
sudo docker start data_on_frontend
```

## 3. BE 컨테이너만 적용

```bash
sudo docker stop data_on_backend
sudo docker rm data_on_backend
sudo docker start data_on_backend
```

## 4. 모든 서비스 컨테이너 삭제

```bash
# 관련 볼륨, 이미지, 네트워크 제거
cd /home/data-on/Data-ON
sudo docker-compose down
sudo docker system prune -a
sudo docker volume prune
```
