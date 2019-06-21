---
layout: post
title:  "[SCSS] 문법 정리 - 3. @if/@for/@each/@while"
date:   2019-06-21 23:50:00
categories: CSS
---

> SASS의 제어 구문과 관련된 기능들입니다.

### (1) @if - @else if - @else 

```scss
$area: mountain; 
p { 
    @if $area == ocean { 
        color: blue; 
    } @else if $area == mountain { 
        color: red; 
    } @else if $area == city { 
        color: green; 
    } @else { 
        color: black; 
    } 
}
```

**컴파일 후 CSS**

```css
p { color: red; }
```

<br>

### (2) @for

`for $i from n through m` 의 형태로 n이상 m이하의 숫자 범위에 대해 각 **정수**값에 대해 순회한다. `:nth-` 가상 클래스와 결합되었을 때 유용하다.

```scss
@for $i from 1 through 3 { // 1이상 3이하에 대해 반복
  li:nth-child({$i}) { width: 2em * $i; } 
    // li:nth-child(1}) { width : 2em; }
    // li:nth-child(2) { width : 4em; }
    // li:nth-child(3) { width : 6em; }
} 
```

<br>

### (3) @each

`each $변수 in 리스트` 형태로, 주어진 리스트의 각 원소에 대해 순회하며 `$변수` 에 값이 차례대로 저장된다.

```scss
// 리스트 내 각 문자열 원소에 대해서
@each $animal in cat, dog, bird` {
  .#{$animal}-icon {
    background-image: url('/image/#{$animal}.png');   
  }
}
```

**컴파일 후 CSS**

```css
.cat-icon { background-image: url('/image/cat.png'); }
.dog-icon { background-image: url('/image/dog.png'); }
.bird-icon { background-image: url('/image/bird.png'); }
```



map에 관해서도 적용할 수 있다

```scss
$map: ( 
    key: #aaa, 
    value: #bbb 
) 
    
@each $key, $value in $map { 
    .section-#{$key} { background-color: $value; } 
}
```

**컴파일 후 CSS**

```css
.section-key { background-color: #aaa; } 
.section-value { background-color: #bbb; }
```

<br>

### (4) @while

```scss
// 6, 4, 2번 아이템에 대해서 
$i : 6; 
@while $i > 0 {
  .item-#{$i} { width: 2em * $i }
  $i: $i - 2; 
}
```