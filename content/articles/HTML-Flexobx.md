---
title: HTML Flexbox
description: HTML Flexbox
renderimg: /blog/html_512.png
img: /blog/landscape_1920.jpg
alt: HTML Flexbox
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - HTML
  - web development
---

## 구성요소

1. 1개의 container
2. 여러개의 item

## container에 적용시킬 수 있는 속성

- direction
- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

## item 적용시킬 수 있는 속성

- order
- flex-grow
- flex-shrink
- flex
- align-self

## main axis와 cross axis

## Test 코드

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>FLEX BOX</title>
</head>
<body>
  <div class="container">
    <div class="item item1">1</div>
    <div class="item item2">2</div>
    <div class="item item3">3</div>
    <div class="item item4">4</div>
    <div class="item item5">5</div>
    <div class="item item6">6</div>
    <div class="item item7">7</div>
    <div class="item item8">8</div>
    <div class="item item9">9</div>
    <div class="item item10">10</div>
  </div>
</body>
</html>
```

```css
.container {
  background: beige;
  height: 100vh;
}

.item1 {
  background: #ef9a9a;
}
.item2 {
  background: #f48fb1;
}
.item3 {
  background: #ce93d8;
}
.item4 {
  background: #b39ddb;
}
.item5 {
  background: #90caf9;
}
.item6 {
  background: #a5d6a7;
}
.item7 {
  background: #e6ee9c;
}
.item8 {
  background: #ef9a9a;
}
.item9 {
  background: #ef9a9a;
}
.item10 {
  background: #ef9a9a;
}
```

## [스터디 게임 사이트](https://flexboxfroggy.com/#ko)


## 출처

- [나만의 기록들](https://mine-it-record.tistory.com/294)
