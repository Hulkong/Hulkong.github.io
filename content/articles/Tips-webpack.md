---
title: Tips webpack 설정
description: Tips webpack 설정
renderimg: /blog/tips_512.png
img: /blog/landscape_1920.jpg
alt: Tips webpack 설정
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - Tips
  - web development
---

> > 소스 맵

- 소스 맵(Source Map)이란 배포용으로 빌드한 파일과 원본파일을 서로 연결시켜주는 기능
- 압축하여 배포한 파일에 에러 발생시 디버깅에 필요

```javascript
// webpack.config.js
module.export = {
  devtool: 'cheap-eval-source-map'
}
```

> > extract-text-webpack-plugin

- 확장성을 높이기 위해 css를 분리

```bash
# webpack4와의 호환을 위해 @next 붙임
npm i extract-text-webpack-plugin@next --save-dev
```

```html
<!-- 분리된 css파일을 index.html에서 읽어오도록 함 -->
<link ref="stylesheet" href="dist/style.css" />
```

```javascript
// webpack.config.js
var path = require('path')
var ExtractTextPlugin = require('extract-text-webpack-plugin') //변경부분

module.exports = {
  mode: 'development',
  entry: './app/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        //use: ["style-loader", "css-loader"] //변경부분2
        use: ExtractTextPlugin.extract({
          //변경부분3
          fallback: 'style-loader',
          use: 'css-loader'
        })
      }
    ]
  },
  plugins: [new ExtractTextPlugin('styles.css')]
}
```

```bash
# webpack 실행
npx webpack
```

# 출처

- [joshua1988](https://joshua1988.github.io/webpack-guide/devtools/source-map.html#%EC%86%8C%EC%8A%A4-%EB%A7%B5)
- [Negabaro`s Blog](https://negabaro.github.io/archive/webpack-extract-text-webpack-plugin)
