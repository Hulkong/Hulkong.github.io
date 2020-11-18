[Postgres11 설치]

1. not-root user 생성
2. server locale 변경: https://www.manualfactory.net/11951
3. ufw 활성화: https://idchowto.com/?p=39516
4. Postgres11 + postgis 설치
   https://computingforgeeks.com/install-postgresql-11-on-ubuntu-linux/
   https://computingforgeeks.com/how-to-install-postgis-on-ubuntu-debian/
   https://browndwarf.tistory.com/58
5. postgresql.conf 설정변경(최적화): https://pgtune.leopard.in.ua/#/
6. pg_hba.conf 설정변경
7. 데이터베이스 초기화
   sudo service postgresql stop
   rm -rf /var/lib/postgresql/11/main/\*
   /usr/lib/postgresql/11/bin/initdb -E UTF-8 --locale=ko_KR.UTF-8 --lc-collate=C -D /var/lib/postgresql/11/main
8. 한글정렬 가능한 데이터베이스로 생성

- postgresql패키지 삭제: https://stackoverflow.com/questions/31645550/postgresql-why-psql-cant-connect-to-server
- dpkg/lock오류해결: https://enant.tistory.com/18
- iwinv postgres9.5.3 install manual: https://help.iwinv.kr/manual/read.html?idx=239
- 외부서버에서 postgresql 접속
