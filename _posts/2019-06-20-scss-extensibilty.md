---
layout: post
title:  "[SCSS] 문법 정리 - 2. @import/@extend/@mixin/@function"
date:   2019-06-20 22:30:00
categories: CSS
---

> SASS의 확장성과 관련된 기능들입니다.


### (1) @extend

특정 선택자를 상속할 때, `@extend 선택자` 를 상속받을 선택자의 최상위에 작성 함으로서 부모 선택자의 스타일을 상속받을 수 있다.

```scss
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  @extend .message;
  border-color: green;
}

.error {
  @extend .message;
  border-color: red;
}
```

**컴파일 후 CSS**

```css
.message, .success, .error {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success { border-color: green; }
.error { border-color: red; }
```

extend를 이용하면 공통되는 속성값에 대한 클래스를 `,` 구분으로 묶어서 출력해주고, 다른 속성값에 대해서만  따로 출력해준다. 이는 HTML에서 `<div class="message success">` 와 같이 클래스를 이중으로 써야 했던 작업들을 `<div class="success">` 하나로 축소시킬 수 있다.

<br>

### (2) @mixin - @include

공통적으로 많이 쓰이는 CSS 선언값들을 묶어서 믹스인으로 만들어 재사용이 가능하게끔 할 수 있습니다. 변수는 단일 값을 담을 수 있는 것에 비해, 믹스인은 여러 속성의 정의 및 셀렉터에 대한 속성 전체등 블럭 단위로 재사용할 수 있습니다. 또한, 믹스인을 정의할 때에는 파라미터를 받아 파라미터 값에 따른 가변적 속성 집합을 만들 수 있습니다. 

```scss
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
  -moz-border-radius: $radius;
  -ms-border-radius: $radius;
  border-radius: $radius;
}

.box { @include border-radius(10px); }
```

믹스인의 인자는 선언하는 만큼 사용할 수 있습니다. 만약 인자값에 디폴트를 적용하고 싶다면, 변수 선언과 같은 문법으로 인자변수의 초기값을 설정해 줄 수 있다.

```scss
@mixin dashed-box($color, $width: 2px) { .. }
```

@include 구문에서 인자값은 선언된 순서대로 써야 한다. 그러나 인자의 각 이름을 명시한 경우에는 순서가 바뀌어도 상관없다.

```scss
.box { @include dashed-box($width: 3px, $color: #eee) }
```

인자명에 `...` 을 붙이면 단일 값이 아닌 리스트로 인자를 받는 다는 의미이다. 이는 일련의 연속값을 속성으로 사용하는 경우에 활용할 수 있다.

```scss
@mixin box-shadow($shadows...) {
  -moz-box-shadow: $shadows;
  -webkit-box-shadow: $shadows;
  box-shadow: $shadows;
}

.shadows { @include box-shadows(0px 4px 5px #666, 2px 6px 10px #999); }
```

`... `표현은 리스트나, 맵을 개별 인자들로 분해해서 함수나 믹스인에 전달할 때 사용될 수 있다.

```scss
@mixin colors($text, $background, $border) {
  color: $text;
  background-color: $background;
  border-color: $border;
}

$values: #ff0000, #00ff00, #0000ff;
.primary { @include colors($values...); }
```

흔한 케이스는 아니지만, 블럭 자체를 믹스인에 넘겨줄 수 있다.  믹스인내에서 @content 지시어를 쓴 부분이 넘겨받은 블럭으로 치환된다.

```scss
@mixin code-inline {
  code {
    background-color: #cecece;
    padding: 2px;
    border-radius: @include border-raidus(4px);
    font-family: monospaces;
    @content
  }
}

p {
  @include code-inline {
    color: #33ccff;
    font-size: .8em;
  }
}
```

<br>

### (3) @function - @return

css 속성 정의의 모듈화와 재사용은 믹스인을 통해서 처리하면 된다. 함수는 어떤 값들을 사용해서 하나의 리턴값을 생성하는 용도로 사용하는 것이 좋다

```scss
$grid-width: 40px; 
$gutter-width: 10px; 

@function grid-width($n) {
   @return $n * $grid-width + ($n -1 ) * $gutter-width; 
} 

#sidebar { width: grid-width(5); } // 믹스인과 달리 @include를 쓰지 않는다.
```

<br>

### (4) @Import 

```scss
@import "path/filename.scss";
```

다른 scss 파일을 현재 파일로 가져와 사용할 수 있습니다. 만약 확장자 없이 `@import "filename";` 이라 하면 다음의 파일들이 있는지 검색합니다.

- filename.scss
- filename.sass
- filename.scss
- filename.sass

네 개 중 하나만 존재하면 그 파일을 가져오고, 여러 파일이 존재하면 변환 시 에러가 납니다.

> 만약에 .sass 파일이나 .scss 파일의 파일이름을  `_`(underscore) 로 시작하면 css 파일로 따로 컴파일되지 않습니다. HTML 에서 해당 css 파일을 불러올일이 없고, import 만 되는 경우에는이 기능을 사용합시다.