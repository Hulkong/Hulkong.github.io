---
title: docker 컨테이너와 데이터를 공유하는 방법
description: docker 컨테이너와 데이터를 공유하는 방법
renderimg: /blog/docker_512.png
img: /blog/landscape_1920.jpg
alt: docker 컨테이너와 데이터를 공유하는 방법
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - docker
  - web development
---

## Volume 생성

```bash
docker volume create --name volume-name
```

## Volume 확인

```bash
docker volume inspect volume-name
```

<br/>

## 참고

- [banana in a box](https://override1592.tistory.com/26)
