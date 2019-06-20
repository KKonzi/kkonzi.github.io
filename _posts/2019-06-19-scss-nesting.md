---
layout: post
title:  "[SCSS] 문법 정리 - 1. 네스팅"
date:   2019-06-19 00:40:00
categories: CSS
---

> Nesting : 선언의 중첩, 내포

### (1) 셀렉터 네스팅

scss 파일 만으로도 html  구조를 알 수 있으며, 타고 들어가는 것이 많을 때 유용하다.

```scss
// CSS
.div p {color:skyblue}

// SCSS
.div {
  p {
    color:skyblue;
  }
}
```



### (2) 속성 네스팅

축양형으로 묶일 수 있는 속성들 끼리 네스팅이 가능하다. 예를 들어 `font`의 경우 `font-family`, `font-size`, `font-weight`등이 있는데 아래와 같이 속성의 하위 형태로 작성할 수 있다.

```scss
.content {
  p {
    font: {
      family: "Noto Serif CJK KR", serif; // font-family
      size: 15px; // font-size
      weight: 400; // font-weight
    }
  }
}
```



### (3) 상위 요소 치환

어떤 선택자의 { } 안에서 &를 쓰면 중괄호를 취하는 선택자 자신을 의미한다. `div { &{} }`의 경우 `&는` `div`다. 본인에게 속성을 준 후 `:hover`나 `:target`등을 이어서 줄 때 유용하다.

```scss
a {
  color:#666;
  &:hover { color:yellow; } // a:hover
}
```

글자 그대로 `&`가 선택자 자신의 글자로 치환되는 것이기 때문에 그 뒤로 글자를 이어 써서 선택자 클래스명 이름을 완성할 수도 있다.

```scss
.btn {
   display:inline-block; padding:3px 10px; border:1px solid #000;
   &_blue { background-color:blue; border:0; } // .btn_blue
   &_yellow { background-color:yellow; border:0; } // .btn_yellow
}
```