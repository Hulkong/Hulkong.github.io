---
title: CSS 비디오 꽉찬 화면 설정
description: CSS 비디오 꽉찬 화면 설정
renderimg: /blog/css_512.png
img: /blog/landscape_1920.jpg
alt: CSS 비디오 꽉찬 화면 설정
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - CSS
  - web development
---

> > object-fit 사용

```css
.video-wrapper {
  width: 100%;
  height: 100%;
}

.video-wrapper video {
  object-fit: cover;
  width: 100%;
  height: 100%;
}
```

<br/><br/>

> > object-fit은 IE에서 사용 못함

## 해결방법

## **[HTML]**

```html
<!-- for IE 버튼 부분 -->
<div class="button-wrap last button-IE">
  <button class="rad-button">for IE</button>
</div>

<!-- 이미지가 들어있는 컨테이너 및 이미지 태그 -->
<div class="img-container">
  <img class="target" src="https://bit.ly/2GZUits" />
</div>
```

## **[Javascript]**

```javascript
function forIE() {
  var t = $('.target'), // 이미지 태그
    s = 'url(' + t.attr('src') + ')', // 이미지 태그의 src를 가져옴.
    p = t.parent(), // 부모 컨테이너 '.img-container'
    d = $('<div class="backGround"></div>') // div를 하나 만듦.

  t.hide() //이미지는 숨기고.
  p.append(d) //부모div에 생성한 div를 붙임.

  d.css({
    height: 300,
    'background-size': 'cover',
    'background-repeat': 'no-repeat',
    'background-position': 'center',
    'background-image': s,
    border: '1px solid red'
  })
}
```

# 출처

- [stackoverflow](https://stackoverflow.com/questions/20127763/video-100-width-and-height#comment77429639_39668310)
- [.Blow](http://error404.co.kr/dev/2019/05/04/CSSinIE/#object-fit%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%A0-%EB%95%8C-ie%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%ED%95%A0%EA%B9%8C)
- [Journey](http://blog.naver.com/PostView.nhn?blogId=seri313&logNo=221396791807&parentCategoryNo=&categoryNo=20&viewDate=&isShowPopularPosts=true&from=search)
