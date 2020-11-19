# postgreSQL11 + postGIS 설치
1. 패키지 업데이트
sudo apt update
sudo apt -y upgrade

2. Server locale 변경
locale
apt install language-pack-ko
vi /etc/default/locale
LANG=ko_KR.UTF-8

[참고](https://www.manualfactory.net/11951)

3. UFW 활성화
dpkg -r iptables-persistent
dpkg -r netfilter-persistent
ufw enable
ufw allow 22
ufw allow 5432
reboot
ufw status

[참고](https://idchowto.com/?p=39516)

4. PostgreSQL11 + postGIS 설치
sudo apt -y install gnupg2
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list
sudo apt update
sudo apt install postgis postgresql-11-postgis-3

[참고1](https://www.vultr.com/docs/install-the-postgis-extension-for-postgresql-on-ubuntu-linux)
[참고2](https://computingforgeeks.com/install-postgresql-11-on-ubuntu-linux/)
[참고3](https://computingforgeeks.com/how-to-install-postgis-on-ubuntu-debian/)
[참고4](https://browndwarf.tistory.com/58)

5. postgres 계정 sudo 권한부여
usermod -aG sudo postgres

[참고](https://www.vultr.com/docs/create-a-sudo-user-on-ubuntu-best-practices)

6. OS postgres 계정 비밀번호 업데이트
passwd postgres

7. postgresql.conf 설정변경(최적화)
cd /etc/postgresql/11/main
cp -R postgresql.conf postgresql.conf.bak
vi postgresql.conf

# DB Version: 11
# OS Type: linux
# DB Type: web
# Total Memory (RAM): 32 GB
# CPUs num: 8
# Connections num: 100
# Data Storage: ssd

listen_addresses = '*'
max_connections = 100
shared_buffers = 8GB
effective_cache_size = 24GB
maintenance_work_mem = 2GB
checkpoint_completion_target = 0.7
wal_buffers = 16MB
default_statistics_target = 100
random_page_cost = 1.1
effective_io_concurrency = 200
work_mem = 20971kB
min_wal_size = 1GB
max_wal_size = 4GB
max_worker_processes = 8
max_parallel_workers_per_gather = 4
max_parallel_workers = 8
max_parallel_maintenance_workers = 4

[참고](https://pgtune.leopard.in.ua/#/)

8. pg_hba.conf 설정변경
cp -R pg_hba.conf pg_hba.conf.bak

9. 데이터베이스 초기화(인코딩 설정)
sudo service postgresql stop
rm -rf /var/lib/postgresql/11/main/*
/usr/lib/postgresql/11/bin/initdb -E UTF-8 --locale=ko_KR.UTF-8 --lc-collate=C -D /var/lib/postgresql/11/main

8. postgres DB계정 비밀번호 업데이트
[방법1]
psql -c "alter user postgres with password '@dnsdud395&%$)'"

[방법2]
psql
ALTER USER postgres PASSWORD '@dnsdud395&%$)';
\q

[방법3]
psql
\password postgres
\q

8. 한글정렬 가능한 데이터베이스 구축
pg_dump [DATABASE_NAME] > db.dump
psql

[방법1]
drop database [DATABASE_NAME]
create database [DATABASE_NAME];

[방법2]
CREATE DATABASE [DATABASE_NAME] TEMPLATE template0 LC_COLLATE 'C';

\q
psql -f db.dump [DATABASE_NAME]

[참고](https://jupiny.com/2016/12/12/sort-korean-in-postgresql/)

# 예외처리
1. postgresql패키지 삭제

sudo apt-get --purge remove postgresql\*
dpkg -l | grep postgres

[참고](https://stackoverflow.com/questions/31645550/postgresql-why-psql-cant-connect-to-server)

2. dpkg/lock오류해결

sudo killall apt apt-get
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock*
sudo dpkg --configure -a 

[참고1](https://enant.tistory.com/18)
[참고2](https://yoonwould.tistory.com/122)

3.[could not connect to server: Connection refused 해결방법](https://jupiny.com/2016/12/13/could-not-connect-to-server-connection-refused-when-remote-access-to-postgresql/)

4.[FATAL: Peer authentication failed for user "postgres" 해결방법](https://m.blog.naver.com/PostView.nhn?blogId=jodi999&logNo=221337438660&proxyReferer=https:%2F%2Fwww.google.com%2F)


# 유용한 TIPs
1. 유저 생성
[방법1]
createuser jira --interactive

[방법2]
psql
CREATE USER confluence WITH ENCRYPTED PASSWORD 'secret123';

[참고](https://www.lesstif.com/dbms/postgresql-61899197.html)

2. 데이터베이스 생성
[방법1]
createdb jiradb -O jira --encoding='utf-8' --locale=en_US.utf8 --template=template0

[방법2]
CREATE DATABASE jiradb OWNER jira ENCODING 'utf-8';

[연결 확인]
psql -U confluence -d confluencedb  -W -h localhost

[참고](https://www.lesstif.com/dbms/postgresql-61899197.html)

3. test postgis extension 
su - postgres
createuser postgis_test
createdb postgis_db -O postgis_test
psql -d postgis_db
CREATE EXTENSION postgis;
SELECT PostGIS_version();

4. [iwinv postgres9.5.3 install manual](https://help.iwinv.kr/manual/read.html?idx=239)