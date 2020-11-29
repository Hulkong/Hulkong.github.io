# postgreSQL11 + postGIS 설치

## 1. 패키지 업데이트

```bash
sudo apt update
sudo apt -y upgrade
```

## 2. Server locale 변경

```bash
locale
apt install language-pack-ko
vi /etc/default/locale # LANG=ko_KR.UTF-8 입력
```

[참고](https://www.manualfactory.net/11951)

## 3. UFW 활성화

```bash
dpkg -r iptables-persistent
dpkg -r netfilter-persistent
ufw enable
ufw allow 22
ufw allow 5432
reboot
ufw status
```

[참고](https://idchowto.com/?p=39516)

## 4. PostgreSQL11 + postGIS 설치

```bash
sudo apt -y install gnupg2
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudoapt-key add -
echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" sudo tee /etc/apt/sources.list.d/pgdg.list
sudo apt update
sudo apt install postgis postgresql-11-postgis-3
```

[참고1](https://www.vultr.com/docs/install-the-postgis-extension-for-postgresql-on-ubuntu-linux)
<br/>[참고2](https://computingforgeeks.com/install-postgresql-11-on-ubuntu-linux/)
<br/>[참고3](https://computingforgeeks.com/how-to-install-postgis-on-ubuntu-debian/)
<br/>[참고4](https://browndwarf.tistory.com/58)

## 5. postgres 계정 sudo 권한부여

```bash
usermod -aG sudo postgres
```

[참고](https://www.vultr.com/docs/create-a-sudo-user-on-ubuntu-best-practices)

## 6. OS postgres 계정 비밀번호 업데이트

```bash
passwd postgres
```

## 7. postgresql.conf 설정변경(최적화)

```bash
cd /etc/postgresql/11/main
cp -R postgresql.conf postgresql.conf.bak
vi postgresql.conf # 하단 정보 입력

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
```

[참고](https://pgtune.leopard.in.ua/#/)

## 8. pg_hba.conf 설정변경

```bash
cp -R pg_hba.conf pg_hba.conf.bak
vi pg_hba.conf # host    all     all     all     md5  입력
```

## 9. 데이터베이스 초기화(인코딩 설정)

```bash
su - postgres
sudo service postgresql stop
rm -rf /var/lib/postgresql/11/main/*
/usr/lib/postgresql/11/bin/initdb -E UTF-8 --locale=ko_KR.UTF-8 --lc-collate=C -D var/lib/postgresql/11/main
sudu service postgresql start
```

## 10. postgres DB계정 비밀번호 업데이트

```bash
# 방법1
psql -c "alter user postgres with password '@dnsdud395&%\$)'"

# 방법2
psql
ALTER USER postgres PASSWORD '@dnsdud395&%\$)';
\q

# 방법3
psql
\password postgres
\q
```

## 8. 한글정렬 가능한 데이터베이스 구축

```bash
pg_dump [DATABASE_NAME] > db.dump
psql


# 방법1
drop database [DATABASE_NAME]
create database [DATABASE_NAME];

# 방법2
CREATE DATABASE [DATABASE_NAME] TEMPLATE template0 LC_COLLATE 'C';

\q
psql -f db.dump [DATABASE_NAME]
```

[참고](https://jupiny.com/2016/12/12/sort-korean-in-postgresql/)

<br/><br/>

# 예외처리

## 1. postgresql패키지 삭제

```bash
sudo apt-get --purge remove postgresql\*
dpkg -l | grep postgres
```

[참고](https://stackoverflow.com/questions/31645550/postgresql-why-psql-cant-connect-to-server)

## 2. dpkg/lock오류해결

```bash
sudo killall apt apt-get
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock\*
sudo dpkg --configure -a
```

[참고1](https://enant.tistory.com/18)
<br/>[참고2](https://yoonwould.tistory.com/122)

## 3. [could not connect to server: Connection refused 해결방법](https://jupiny.com/2016/12/13/could-not-connect-to-server-connection-refused-when-remote-access-to-postgresql/)

## 4. [FATAL: Peer authentication failed for user "postgres" 해결방법](https://m.blog.naver.com/PostView.nhn?blogId=jodi999&logNo=221337438660&proxyReferer=https:%2F%2Fwww.google.com%2F)

<br/><br/>

# 유용한 TIPs

## 1. 유저 생성

```bash
# 방법1
createuser jira --interactive

# 방법2
psql
CREATE USER confluence WITH ENCRYPTED PASSWORD 'secret123';
```

[참고](https://www.lesstif.com/dbms/postgresql-61899197.html)

## 2. 데이터베이스 생성

```bash
# 방법1
createdb jiradb -O jira --encoding='utf-8' --locale=en_US.utf8 --template=template0

# 방법2
CREATE DATABASE jiradb OWNER jira ENCODING 'utf-8';

# 연결 확인
psql -U confluence -d confluencedb -W -h localhost
```

[참고](https://www.lesstif.com/dbms/postgresql-61899197.html)

## 3. test postgis extension

```bash
su - postgres
createuser postgis_test
createdb postgis_db -O postgis_test
psql -d postgis_db
CREATE EXTENSION postgis;
SELECT PostGIS_version();
```

## 4. Srid 추가

```sql
# katech 좌표 추가(EPSG: 100004)

insert into spatial_ref_sys values(
100004
,'EPSG'
,100004
,'PROJCS["Korean_1985_Korea_Central_Belt",
GEOGCS["GCS_Korean_Datum_1985",
DATUM["D_Korean_Datum_1985",
SPHEROID["Bessel_1841", 6377397.155, 299.1528128],
TOWGS84[-115.8, 474.99, 674.11, 1.16, -2.31, -1.63, 6.43]],
PRIMEM["Greenwich", 0.0],
UNIT["degree", 0.017453292519943295],
AXIS["Longitude", EAST],
AXIS["Latitude", NORTH]],
PROJECTION["Transverse_Mercator"],
PARAMETER["central_meridian", 128.0],
PARAMETER["latitude_of_origin", 38.0],
PARAMETER["scale_factor", 0.9999],
PARAMETER["false_easting", 400000.0],
PARAMETER["false_northing", 600000.0],
UNIT["m", 1.0],
AXIS["x", EAST],
AXIS["y", NORTH],
AUTHORITY["EPSG","100004"]]'
,'+proj=tmerc +lat_0=38 +lon_0=128 +k=0.9999 +x_0=400000 +y_0=600000 +ellps=bessel +towgs84=-145.907,505.034,685.
756,-1.162,2.347,1.592,6.342 +units=m +no_defs'
)

# 다음 좌표 추가(postgresql-11 postgis-3버전에는 기본으로 있음, EPSG:5181)

insert into spatial_ref_sys values(
5181
,'EPSG'
,5181
,'PROJCS["Korea 2000 / Central Belt",
GEOGCS["Korea 2000",
DATUM["Geocentric datum of Korea",
SPHEROID["GRS 1980", 6378137.0, 298.257222101, AUTHORITY["EPSG","7019"]],
TOWGS84[0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0],
AUTHORITY["EPSG","6737"]],
PRIMEM["Greenwich", 0.0, AUTHORITY["EPSG","8901"]],
UNIT["degree", 0.017453292519943295],
AXIS["Geodetic longitude", EAST],
AXIS["Geodetic latitude", NORTH],
AUTHORITY["EPSG","4737"]],
PROJECTION["Transverse_Mercator", AUTHORITY["EPSG","9807"]],
PARAMETER["central_meridian", 127.0],
PARAMETER["latitude_of_origin", 38.0],
PARAMETER["scale_factor", 1.0],
PARAMETER["false_easting", 200000.0],
PARAMETER["false_northing", 500000.0],
UNIT["m", 1.0],
AXIS["Easting", EAST],
AXIS["Northing", NORTH],
AUTHORITY["EPSG","5181"]]'
,'+proj=tmerc +lat_0=38 +lon_0=127 +k=1 +x_0=200000 +y_0=500000 +ellps=GRS80 +units=m +no_defs'
)

# 테이블의 컬럼 별로 srid 적용(db명, 스키마명 없어도 됨)
# ex) SELECT updategeometrysrid('테이블명', '_geometry', 1);
# ex) SELECT UpdateGeometrySRID('roads','geom',4326);

select updategeometrysrid('DB명','스키마명','테이블명', 'geometry컬럼명', SRID코드);
```
