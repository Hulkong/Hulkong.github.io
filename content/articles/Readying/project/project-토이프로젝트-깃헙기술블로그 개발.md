---
title: project 토이프로젝트-깃헙 기술블로그 개발-1일차
description: project 토이프로젝트-깃헙 기술블로그 개발-1일차
renderimg: /blog/project_512.png
img: /blog/landscape_1920.jpg
alt: project 토이프로젝트-깃헙 기술블로그 개발-1일차
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - project
  - web development
---

만 4년차 개발자로서 오픈소스 프로젝트와 기술블로그를 제작 및 관리해야 겠다는 생각이 들었다. 사실 신입 때부터 준비할 생각은 가졌었지만, 게으른 관계로 지금까지 미뤘다. 다만, Jekyll나 Tistory 등 블로그 프레임을 사용하지 않고, 직접 개발부터 배포, 관리까지 모두 진행해 보기로 다짐했다. 이렇게 생각한 이유는 NuxtJS, Markdown, Typescript를 공부하고 싶었고, 디자인까지 작업해 보고 싶었기 떄문이다. 작업환경은 아래와 같다.

|     유형     |        스택        | 비고 |
| :----------: | :----------------: | :--: |
|     IDE      |       Vscode       |      |
|   Frontend   | NuxtJS, Typescript |      |
| Hosting Site |       Github       |      |
|   Library    |                    |      |

<br/><br/>

## Todo-List

1. github 프로젝트 생성
1. git clone
1. npm 설치
1. NuxtJS 프로젝트 생성
1. 프로젝트 디렉토리 구조 생성
1. 커스텀 라이브러리 import
1. Font 추가
1. favicon.ico 추가
1. 디폴트 이미지 추가
1. [레이아웃 참고 사이트](https://vszhub.github.io/not-pure-poole/2020/10/02/testing-mathjax/) 조사
1. 기능명세서 작성
1. 화면설계(Sketch이용)
1. 프로젝트 옵션설정
   - .gitignore
   - .eslintrc.js 설정
   - .prettierrc 설정
   - .babelrc 설정
   - nuxt.config.js 설정
   - tsconfig.json 설정
1. 퍼블리싱
1. 기능구현
1. 테스트
1. 배포

<br/>

### 1. NuxtJS 프로젝트 생성

```bash
# 선택사항: PWA, Typescript
npm init nuxt-app <project-name>
cd <project-name>
npm install
```

### 2. 기능명세서

- 카테고리: 날짜별, 주제별
- 댓글: Disqus Library 사용
- 링크: Instagram, Facebook, Github
- 메일
- Markdown 문법 적용
- code highlighting
- robots.txt 적용
- meta tag 정의
