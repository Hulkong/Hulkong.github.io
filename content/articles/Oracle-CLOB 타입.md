---
title: Oralce CLOB 타입에 대해서
description: Oralce CLOB 타입에 대해서
renderimg: /blog/postgreSQL_512.png
img: /blog/landscape_1920.jpg
alt: Oralce CLOB 타입에 대해서
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - Oracle
  - web development
---

## 특징
<h4>- 사이즈가 큰 데이터를 외부 파일로 저장하기 위한 데이터 타입</h4>
<h4>- CLOB 데이터의 최대 길이는 외부 저장소에서 생성 가능한 파일 크기</h4>
<h4>- CLOB 타입은 SQL문에서 문자열 타입으로 입출력 값을 표현</h4>

    1. 즉, char(n), varchar(n), nchar varying(n) 타입과 호환  
    2. 단 명시적 타입 변환만 허용
    3. 데이터 길이가 서로 다른 경우에는 최대 길이가 작은 타입에 맞추어 절삭(truncate) 됨

<h4>- 파일에 사이즈와 관련없이 DB에서 LOB데이터 조회시 최대 1GB까지 표현이 가능</h4>
<h4>- Maximum size is ((4 GB - 1) * db_block_size)</h4>  

```bash
SQL> show parameter db_block_size

NAME           TYPE        VALUE

———————————— ———– ———————-

db_block_size   integer     8192
```
<h4>- 실제 LOB데이터가 1GB를 넘게 되면 1GB를 초과한 값은 DB에서 LOB 조회 시 절삭 되어 표현</h4>
<h4>- 문자형 대용량 파일 저장하는데 유용(가변길이로 저장)</h4>
<br/><br/>

## 오류
<h4>1. ORA-22835: 버퍼가 너무 작아 CLOB를 CHAR 또는 BLOB에서 RAW로 변환할 수 없습니다 (실제: 8552, 최대: 4000)</h4>

    원인: LOB 컬럼에 4000Byte 이상 저장되어 있는 경우, Varchar2 형태로 출력되는 과정에서 최대 4000Byte 제한조건이 발동!

    해결방법: 다음과 같이 4000Byte만 읽도록 DBMS_LOB.SUBSTR()을 설정해주면 됨
```sql
replace(translate(dbms_lob.substr(content, 4000, 1), 'abc', 'aaa'), 'd', '')
```
<br/>

<h4>2. export to mariaDB</h4>

    원인: 내부 MariaDB에 설정된 longtext의 용량이 65,535Byte로 설정

    해결방법: 오라클에서 해당 Byte만큼 잘라서 Import
```bash
# 모든 단위는 byte 통일
# mariaDB longtext default maximum byte: 4,294,967,295
# 내부 mariaDB longtext default maximum byte: 65,535
# oracle-char - 1byte
# oracle-db_block_size - 8,192
# oracle-CLOB 실제 저장가능 용량(4GB * db_block_size): 35,184,372,088,832(4 * 1024 * 1024 * 1024 * 8192)
# oracle-CLOB 실제 조회가능 용량(1GB * db_block_size): 8,796,093,022,208(1 * 1024 * 1024 * 1024 * 8192)
# oracle-CLOB 표현가능 용량(varchar2로 출력): 4,000
```

<br/><br/>

## 참고
[[CUBRID](https://www.cubrid.com/qna/3805244)]
[[aladdin76](https://blog.naver.com/aladdin76/40089393928)]
[[ra2kstar](https://ra2kstar.tistory.com/82)]
[[orskl](https://www.orskl.com/how-to-find-block-sizes-of-all-oracle-database-files/)]
[[dba-oracle](http://www.dba-oracle.com/t_maximum_oracle_datatypes_lengths.htm)]
[[재유러스](https://blog.naver.com/PostView.nhn?blogId=rlasksdud53&logNo=220595010315&proxyReferer=https:%2F%2Fwww.google.com%2F)]