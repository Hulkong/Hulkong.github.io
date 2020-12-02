---
title: postgreSQL 이중화-pgpool
description: postgreSQL 이중화-pgpool
renderimg: /blog/postgreSQL_512.png
img: /blog/landscape_1920.jpg
alt: postgreSQL 이중화-pgpool
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - postgreSQL
  - web development
---

> pgpool.conf

- pgpool.conf는 메인구성파일
- 시작할 때 -f 옵션을 부여하여 파일 경로를 지정해야 함

> Mode

- 스트리밍 복제모드(Streaming replication mode)
- 논리 복제모드(logical replication mode)
- 마스터 슬레이브 모드(master slave mode)
- 네이티브 복제모드(native replication mode)
- 원시 모드(raw mode)  
  <br/>
  **상위 모드들은 서로 독점적이며 서버를 시작한 후에는 변경할 수 없음**  
  **시스템을 설계하는 초기단계에서 사용 권장**  
  **확실하지 않은 경우 스트리밍 복제모드 사용 권장**

## 스트리밍 복제모드(Streaming replication mode)

- Pgpool-ll를 사용하는 가장 좋은 방법
- Load balancing 가능

## 논리 복제모드(logical replication mode)

- Load balancing 가능
- 모든 테이블을 복제하는 것이 아님
- 위의 상황으로서 유저는 오래된 테이블을 조회할 수도 있음

## 마스터 슬레이브 모드(master slave mode)

- Slony를 사용해야 할 구체적인 이유가 없는 한 해당 모드 권장안함

## 네이티브 복제모드(native replication mode)

- 동기식으로 동기화 진행
- 즉, 모든 postgreSQL 서버가 쓰기 작업을 완료할 때까지 데이터베이스에 쓰기가 반환되지 않는다는 것
- postgreSQL-9.6이상에서는 스트리밍 복제에 synchronous_commit=remote_apply를 설정한 것과 유사
- 데이터 일관성이 손실될 수 있음. 해결하려면 사용자가 데이터에 대한 탐색잠금을 발행해야 함
- 상위 이유로 인해 스트리밍 복제모드를 권장

## 원시 모드(row mode)

- pgpool-ll은 데이터베이스 동기화에 개입안함
- 로드밸런싱 불가능

<br/>
# 출처
- [Seon's IT Story](https://hsunnystory.tistory.com/136)
- [인디노트](https://indienote.tistory.com/422)
