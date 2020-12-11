---
title: CSS 반응형웹에서 문단 줄바꿈이 마음에 들지 않으면?
description: CSS 반응형웹에서 문단 줄바꿈이 마음에 들지 않으면?
renderimg: /blog/css_512.png
img: /blog/landscape_1920.jpg
alt: CSS 반응형웹에서 문단 줄바꿈이 마음에 들지 않으면?
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - CSS
  - web development
---

## 영문은 어절이 깨지지 않음

- keep-all로 되어있음

## 한글은 어절이 깨짐

- break-all 로 되어있음

## work-break는 상속

```css
/* keep-all은 어절 단위로 끊음 */
/* 시각적으로 안이쁨 */
.box1 {
  font-size: 20px;
  background-color: antiquewhite;
  height: 100%;
  padding: 20px;
  box-sizing: border-box;
  word-break: keep-all;
}

/* break-all은 화면 단위로 끊음 */
/* 시각적으로 안정감을 줌 */
.box2 {
  font-size: 20px;
  background-color: antiquewhite;
  height: 100%;
  padding: 20px;
  box-sizing: border-box;
  word-break: break-all;
}
```

## 참고

[[빔캠프](https://www.youtube.com/watch?v=t_J5s3Fs13A)]
[[word-break 속성과 word-wrap 속성 알아보기](https://wit.nts-corp.com/2017/07/25/4675)]
[[word-break 줄바꿈 속성(break-all, keep-all](https://aboooks.tistory.com/189)]
