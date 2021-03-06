---
title: HTML 브라우저 캐시 삭제 및 초기화
description: HTML 브라우저 캐시 삭제 및 초기화
renderimg: /blog/html_512.png
img: /blog/landscape_1920.jpg
alt: HTML 브라우저 캐시 삭제 및 초기화
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - HTML
  - web development
---

> > expires 속성은 브라우저 캐쉬를 핸들링. 즉, 캐쉬의 파기시간을 정의

- 개발시 캐쉬가 있으면 수정사항이 바로 반영이 안되기 때문에 불편함을 줄이기 위해 캐쉬삭제
- 운영시 페이지 속도 최적화를 위해 캐쉬사용

<br/><br/>

> > 방법1

## **[HTML]**

```html
<!--  명시된 날짜 이후가 되면 페이지가 캐싱되지 않는다.(1990년 이후 쭉 ) -->
<meta http-equiv="Expires" content="Mon, 06 Jan 1990 00:00:01 GMT" />

<!-- 캐시된 페이지가 만료되어 삭제되는 시간을 정의하나 특별한 경우가 아니면 -1로 설정 -->
<meta http-equiv="Expires" content="-1" />

<!-- 페이지 로드시마다 페이지를 캐싱하지 않는다.(HTTP 1.0) -->
<meta http-equiv="Pragma" content="no-cache" />

<!-- 페이지 로드시마다 페이지를 캐싱하지 않는다.(HTTP 1.1) -->
<meta http-equiv="Cache-Control" content="no-cache" />
```

## **[JSP]**

```jsp
<%
response.setHeader("Pragma","no-cache");
response.setDateHeader("Expires",0);
response.setHeader("Cache-Control", "no-cache");
%>
```

**'Pragma: no-cache'는 캐싱을 방지하는 메타태그**  
**'Expires: -1'은 캐시된 페이지를 즉시 만료**

<br/><br/>

> > 방법2

## **[HTML]**

```html
<script src="filename.js?version = 1.0"></script>
```

## 출처

- [나만의 기록들](https://mine-it-record.tistory.com/294)
- [geeksforgeeks](https://www.geeksforgeeks.org/how-to-clear-cache-memory-using-javascript/)
- [넥스트티](https://www.next-t.co.kr/blog/%EA%B2%80%EC%83%89%EC%97%94%EC%A7%84%EC%B5%9C%EC%A0%81%ED%99%94-SEO-%ED%85%8C%ED%81%AC%EB%8B%88%EC%BB%ACSEO-%EB%A9%94%ED%83%80%ED%83%9C%EA%B7%B8-metatags-expires-%EC%86%8D%EC%84%B1)
- [알찬 올라가용](https://gahyun-web-diary.tistory.com/16)
- [stackoverflow](https://stackoverflow.com/questions/1922910/force-browser-to-clear-cache)
