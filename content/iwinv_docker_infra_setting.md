1. host설정: vi /etc/hosts
2. docker-compose 설치: apt update && apt install -y docker-compose
### mtu 변경
 - iwinv는 기본 MTU 1450 But, docker 컨테이너 기본 MTU 1500
 - 다르면 docker 네트워크 통신 안됨 
 - ExecStart=/usr/bin/dockerd -H fd:// --mtu 1450   # 해당 옵션 추가
4. cp /lib/systemd/system/docker.service /etc/systemd/system/docker.service
5. vi /etc/systemd/system/docker.service
6. systemctl daemon-reload
7. service docker restart 
###
8. adduser data-on
9. usermod -aG sudo example_user
10. su - data-on
11. git config --global http.sslVerify false
12. git clone


2. ssl 저장 볼륨 직접 생성
sudo docker volume create --name=letsencrypt_etc
sudo docker volume create --name=letsencrypt_lib

3. ssl 생성
sudo docker run -it --rm --name certbot \
  -v 'letsencrypt_etc:/etc/letsencrypt' \
  -v 'letsencrypt_lib:/var/lib/letsencrypt' \
  certbot/certbot certonly -d 'data-on.co.kr' --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory

* 도메인 발급기관에서 해당도메인에 대한 txt타입 추가

4. docker-compose up -d

5. crontab에 ssl 갱신 명령어 추가
sudo crontab -e
docker run -it --rm --name certbot -v 'letsencrypt_etc:/etc/letsencrypt' -v 'letsencrypt_lib:/var/lib/letsencrypt' certbot/certbot renew --manual --preferred-challenges dns --server https://acme-v02.api.letsencrypt.org/directory

참조: https://openmatetest.sharepoint.com/:p:/s/OPENmate_ONIntranet2/EUYgKyHAmvlCvtRRn3OmNkoBMC6Q4EiNZRRffKJQa1esMA?e=a0USGz
14. cd Data-ON && sudo docker-compose up -d

nginx 컨테이너만 적용

sudo docker stop data_on_nginx
sudo docker rm data_on_nginx
sudo docker start data_on_nginx

cd /home/data-on/Data-ON
sudo docker-compose down
sudo docker system prune -a
sudo docker volume prune