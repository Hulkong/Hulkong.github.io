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
13. sudo docker network create devops
14. cd Data-ON && sudo docker-compose up -d