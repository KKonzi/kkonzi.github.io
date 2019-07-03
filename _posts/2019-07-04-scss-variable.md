---
layout: post
title:  "[SCSS] 문법 정리 - 4. 변수"
date:   2019-06-20 22:30:00
categories: CSS
---

> SASS의 변수 선언과 사용에 관련된 기능들입니다.


### (1) 변수 선언

`$variable-name: variable-value;` 꼴로 변수를 선언하고 사용할 수 있다.

```scss
$primary-color: #333;

body {
  background-color: $primary-color;
}
```

변수를 선언 할 때, 변수를 global (전역) 하게 설정 할 때는 `!global` 플래그를 사용합니다.

**SCSS**

```scss
$primary-color: #333;

body {
  $primary-color: #eee !global;;
  background-color: $primary-color;
}

p {
  color: $primary-color;
}
```

**컴파일 후 CSS**

```scss
body { background-color: #eee; }
p { color: #eee; }
```

- `!default` 플래그는 해당 변수가 설정되지 않았거나 값이 null 일떄 값을 설정합니다

**Sass**

```scss
$primary-color: $eee !default;
$primary-color: #333;

p {
  color: $primary-color;
}
```

**컴파일 후 CSS**

```scss
p { color: #333; }
```

<br>

### (2) #{ } (Interpolation)

- CSS에서의 `/`는 나눗셈의 의미가 아니라 값을 분리하는 의미를 갖는다. 따라서 Sass의 `/` 연산자를 사용하기 위해서는 몇가지 조건이 필요하다.

```scss
div {
  $width: 1000px;

  width: $width / 2;            // 1. 변수에 대해 사용 width: 500px;
  height: (500px / 2);          // 2. 괄호 내에서 사용 height: 250px;
  margin-left: 5px + 8px / 2px; // 3. 다른 연산의 일부로서 사용 margin-left: 9px;
}
```

- `#{ }` 표현은 특정 문자열을 따로 처리하지않고 **그대로 출력** 할 때 사용된다
- `#{ }` 을 사용하면 문자열 내에 표현식의 결과를 넣거나, 다른 변수의 내용으로 치환하는 것이 가능하다. 속성값의 일부나 전체 모두 적용 가능하고 속성명과 셀렉터에 대해서도 적용 가능하다.

```scss
p {
  $font-size: 12px;
  $line-height: 30px;
  font: #{$font-size}/#{$line-height};  // 12px/30px
}
```

<br>

### (3) 변수 범위

- Sass 의 변수엔 변수범위가 있습니다.  변수를 특정 선택자 에서 선언하면 해당 선택자에서만 접근이 가능합니다.

**SCSS**

```scss
$primary-color: #333;

body {
  $primary-color: #eee;
  background-color: $primary-color;
}

p {
  color: $primary-color;
}
```

**컴파일 후 CSS**

```scss
body {
  background-color: #eee;
}

p {
  color: #333;
}
```