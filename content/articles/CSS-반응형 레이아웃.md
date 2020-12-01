---
title: CSS 반응형 레이아웃
description: CSS 반응형 레이아웃
renderimg: /blog/css_512.png
img: /blog/landscape_1920.jpg
alt: CSS 반응형 레이아웃
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - CSS
  - web development
---

## reset.css 적용

- \* 사용 지양(브라우저 렌더링 속도 저하)
- [참고](https://cssreset.com/scripts/eric-meyer-reset-css/)

## box-model

- content-box대신 border-box를 많이 사용
- border-box에서 width, height는 padding과 border가 포함되어 있음

## html 태그 공백문자도 실제로 width에 포함이 됨

- 해결방법은 부모태그에 font-size를 0으로 설정 후 각 태그별 font-size 설정

## inline, block, inline-block, none, flex, grid

- inline: width, height 등 block태그에서 설정할 수 있는 스타일을 설정 못함
- block
- inline-block: inline속성과 block 속성을 두개다 가지고 있음

## block태그를 좌우로 배치하고자 할 때

- display: inline-block 속성으로 설정
- float: left 설정

## width 100% 설정 시 x 스크롤 생성방지

- box-model이 무엇인지 먼저 생각해야 함
- 부모 컨테이너에 width값을 100% 설정 안해도 브라우저에서 자동 설정(추천)

## rem vs em

- rem: html(root)기준 상대적인 크기, 대부분을 이 단위로 사용
- em: 부모태그의 font-size기반으로 조정되는 상대적 크기, 특정 태그의 font-size에 달라지는 padding값 줄 때

## max-width: 최대 width값까지만 늘어남

## margin-left, margin-right: auto

- margin: 0 auto;
- 좌우 margin 자동 정렬

## float

- 컬럼 사이의 여백이 있을 경우 float의 값 left, right로 조정
- float 사용시 관계없는 태그들을 초기화 하려면 clear: both 설정
- 자식이 float 사용시 부모입장에서는 떠있는 것처럼 보이기에 height가 0

  - 가상엘리먼트 after 사용(추천)
  - 자식 태그 마지막 자리에 clear:both 추가

  ```css
  .container:after {
    content: '';
    display: block;
    clear: both;
    height: 0;
    visibility: hidden;
  }
  ```

## viewport 설정

- Device별 자연스러운 화면을 설정하기 위함

```html
<meta
  name="viewport"
  content="width=device-width, initial-scale=1.0,
maximum-scale=3.0"
/>
```

## vw, vh 기준은 하나만 설정

```css
.container {
  width: 50vw;
  height: 20vw;
}

/* or */

.container {
  width: 50vh;
  height: 20vh;
}
```

## 창 크기에 맞춰 조정되는 이미지

```css
img {
  max-width: 100%;
}

/* or */

img {
  width: 100%;
}
```

## 4:3 비율 동영상 크기 자동조절

```css
.container {
  position: relative;
  height: 0;
  padding-bottom: 75%;
}

iframe {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}
```

참고영상: [1분코딩](https://www.youtube.com/watch?v=Zny5Vxqk6Mk&t=1633s)
