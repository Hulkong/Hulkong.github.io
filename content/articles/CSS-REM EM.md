---
title: CSS REM vs EM
description: CSS REM vs EM
renderimg: /blog/css_512.png
img: /blog/landscape_1920.jpg
alt: CSS REM vs EM
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - CSS
  - web development
---

# EM과 REM에 대해서 알아봅시다.

**EM이란?**  
em은 어원적으로 M을 뜻합니다.  
해당 태그의 font-size의 영향을 받습니다.

**REM이란?**  
Root EM의 약자입니다.  
최상위 엘리먼트(html)의 폰트 크기에만 영향을 받습니다.

<br/>

> > 다음과 같은 내용을 알고 있으면 학습하시는데 도움이 될 것입니다.

- REM의 지원브라우저  
  IE 11, Edge 14 이후, Firefox 52 이후, Chrome 49 이후, Safari 10.1 이후, Opera 45버전 이후
- font-size는 상속됩니다.
- 기본 웹브라우저 폰트사이즈는 16px이며, 변경 가능합니다.
- REM과 EM 모두 'M'문자가 기준이 됩니다.

<br/><br/>

# EM

## 부모 폰트크기에 비례해야할 경우

```css
.box h2 {
  color: orangered;
  padding: 20px;
  font-size: 40px;
}

.box h2 span {
  font-size: 0.5em;
  vertical-align: super;
}
```

<br/>

## letter-spacting

```css
.box h2 span {
  font-size: 0.5em;
  vertical-align: super;
  letter-spacing: 0.3333em;
}
```

<br/>

## 가상클래스 before

```css
.box h2:before {
  content: '';
  display: inline-block;
  background-color: orange;
  width: 0.55em;
  height: 0.55em;
  margin-right: 0.33em;
  border-radius: 50%;
}
```

<br/>

## 버튼 시리즈 padding

```css
.button-wrapper {
  border-top: 2px solid #ddd;
  padding: 20px;
}

.button {
  background-color: #369;
  color: white;
  font-size: 30px;
  padding: 0.5em 1em;
  display: inline-block;
}

.button.small {
  font-size: 12px;
}
.button.large {
  font-size: 30px;
}
```

<br/><br/>

## 참고

[빔캠프](https://www.youtube.com/watch?v=47xHPFQ1Ll4)
