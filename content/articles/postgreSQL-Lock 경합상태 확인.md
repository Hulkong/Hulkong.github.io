---
title: postgreSQL Lock 경합상태 확인
description: postgreSQL Lock 경합상태 확인
renderimg: /blog/postgreSQL_512.png
img: /blog/landscape_1920.jpg
alt: postgreSQL Lock 경합상태 확인
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - postgreSQL
  - web development
---

## block된 SQL 프로세스 조회

```sql
SELECT blocked_locks.pid         AS blocked_pid,
       blocked_activity.usename  AS blocked_user,
       blocking_locks.pid        AS blocking_pid,
       blocking_activity.usename AS blocking_user,
       blocked_activity.query    AS blocked_statement,
       blocking_activity.query   AS current_statement_in_blocking_process
FROM   pg_catalog.pg_locks blocked_locks
JOIN   pg_catalog.pg_stat_activity blocked_activity
ON     blocked_activity.pid = blocked_locks.pid
JOIN   pg_catalog.pg_locks blocking_locks
ON     blocking_locks.locktype = blocked_locks.locktype
AND    blocking_locks.database IS NOT distinct
FROM   blocked_locks.DATABASE
AND    blocking_locks.relation IS NOT DISTINCT
FROM   blocked_locks.relation
AND    blocking_locks.page IS NOT DISTINCT
FROM   blocked_locks.page
AND    blocking_locks.tuple IS NOT DISTINCT
FROM   blocked_locks.tuple
AND    blocking_locks.virtualxid IS NOT DISTINCT
FROM   blocked_locks.virtualxid
AND    blocking_locks.transactionid IS NOT DISTINCT
FROM   blocked_locks.transactionid
AND    blocking_locks.classid IS NOT DISTINCT
FROM   blocked_locks.classid
AND    blocking_locks.objid IS NOT DISTINCT
FROM   blocked_locks.objid
AND    blocking_locks.objsubid IS NOT DISTINCT
FROM   blocked_locks.objsubid
AND    blocking_locks.pid != blocked_locks.pid
JOIN   pg_catalog.pg_stat_activity blocking_activity
ON     blocking_activity.pid = blocking_locks.pid
WHERE  NOT blocked_locks.granted;
```

## Lock 전체 목록 조회

```sql
SELECT relation :: regclass, mode, granted, pid, * FROM pg_locks;
```

## Lock 테이블 조회

```sql
select t.relname,l.locktype,page,virtualtransaction,pid,mode,granted
from pg_locks l, pg_stat_all_tables t
where l.relation=t.relid order by relation asc;
```

## Lock 삭제

```sql
select pg_cancel_backend(pid);
```

# 출처

- [복세편살](https://americanopeople.tistory.com/293)
- [래채'sTory](https://foco85.tistory.com/288)
- [chrisjune
  ](https://medium.com/29cm/db-postgresql-lock-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0-57d37ebe057)
