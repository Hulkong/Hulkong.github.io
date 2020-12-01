# REM vs EM

## font-size는 상속

## 기본 웹브라우저 폰트사이즈는 16px

## 'M'문자를 기준

## EM

- 부모 폰트크기에 비례해야할 경우

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

- letter-spacting

```css
.box h2 span {
  font-size: 0.5em;
  vertical-align: super;
  letter-spacing: 0.3333em;
}
```

- 가상클래스 before

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

- 버튼 시리즈 padding

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

참고영상: [빔캠프](https://www.youtube.com/watch?v=47xHPFQ1Ll4)
