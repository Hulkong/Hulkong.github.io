# CSS 가로정렬 inline-block과 float 중 어떤 것을 써야 할까?

```html
<!-- 아래와 같이 가독성을 위해 들여쓰기를 할 경우 -->
<div class="grid">
    <div class="grid-item a"></div>
    <div class="grid-item b"></div>
    <div class="grid-item c"></div>
</div>
```

```css
.grid {
    width: 500px;
    margin: 0 auto;
    background-color: white;
}

.grid-item {
    width: 100px;
    height: 100px;
}

.grid-item.a { background-color: orange; }
.grid-item.b { background-color: orangered; }
.grid-item.c { background-color: red; }
```

## inline-block
### html 작성 중 공백이 있을 경우 띄어쓰기로 인식

- **해결방법 첫 번째**: 컨테이너 'font-size: 0' 설정 후 각 자식 태그의 font-size 설정
- 위의 방법은 브라우저에 따라 적용이 안될 가능성 있음
```css
.grid {
    width: 500px;
    margin: 0 auto;
    background-color: white;
    font-size: 0;
}

.grid-item {
    width: 100px;
    height: 100px;
    display: inline-block;
}
```
- **해결방법 두번째**: 가독성이 안좋더라도 html 태그의 공백을 제거함
- 위의 방법은 prettier 등 협업에 굉장히 안좋음
```html
<div class="grid"><div class="grid-item a"></div><div class="grid-item b"></div><div class="grid-item c"></div></div>
```

### 하단에 공간이 생김
- inline 속성도 가지기 때문에 폰트의 가장 밑단도 포함되어야 하기때문에 공간이 생김
- 해결방법
```css
.grid-item {
    width: 100px;
    height: 100px;
    display: inline-block;
    vertical-align: top;
}
```

### 태그 안에 텍스트가 있을 경우 해당 텍스트 기준 동적 width값 설정가능
```css
.grid-item {
    /* width: 100px; */
    height: 100px;
    display: inline-block;
    vertical-align: top;
    padding: 0 30px;
}
```

## float
```css
.grid-item {
    width: 100px;
    height: 100px;
    float: left;
}
```

### float 설정시 부모 컨테이너의 height는 0으로 설정됨
- 해결방법: 'overflow: hidden'설정
```css
.grid {
    width: 500px;
    margin: 0 auto;
    overflow: hidden;
}
```

### float는 부모컨테이너 위에 붕 떠있다는 느낌으로 생각
- 확인
```html
<div class="grid">
    <div class="grid-item a"></div>
    <div class="grid-item b"></div>
    <div class="grid-item c"></div>
    <div class="contents">wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow wow  
    </div>
</div>
```

```css
.grid-item {
    width: 100px;
    height: 100px;
    float: left;
    opacity: 0.7;
}

.contents {
    border: 10px solid blue;
}
```

<br/>

[참고]
- [빔캠프](https://www.youtube.com/watch?v=8xKDSdHQ35U)