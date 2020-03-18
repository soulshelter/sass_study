# SASS 정리 - 한눈에 보기

[SASS-GUIDELIN](https://sass-guidelin.es/ko/)   
[SASS(SCSS) 완전 정복!](https://heropy.blog/2018/01/31/sass/)   

이 문서는 개인학습을 위해 위 페이지를 재구성한 문서입니다.


***

## SASS 란?

Sass(Syntactically Awesome Style Sheets)는 CSS의 단점을 보완하기 위해 만든 CSS 전처리기(CSS pre-processor)입니다.   
복잡한 CSS 작업을 보다 쉽게 할 수 있게 해주며 코드 재활용이 가능해 가독성을 높여주는 프로그래밍 언어입니다.

***

## SASS와 SCSS의 차이점은?

SASS 문법은 기존 CSS의 문법과 달리 `세미콜론;`를 사용하지 않고 `들여쓰기`를 사용하여 CSS 사용자들이 이해하기가 어려웠습니다.   
그래서 SASS 3버전 이상부터는 기존 CSS 문법에 사용되던 `중괄호{ }`를 사용한 SCSS(SCSSy CSS)로 변경되었습니다.   
이로 인해 CSS만 사용하던 사람들 또한 보다 쉽게 사용할 수 있습니다.

```
# SCSS
.list
    overflow:hidden
    li
        float:left
        width:200px
        color:red
# SCSS
.list {
    overflow:hidden;
    li {
        float:left;
        width:200px;
            color:red;
        }
    }
}
```
***

## Variable (변수)

SASS는 변수가 사용 가능합니다.   
각 변수는 타입을 가지며 각 타입에 대한 연산도 가능합니다.   
변수를 선언할 때는 맨 앞에 `$`를 사용합니다. 

### Variable Type (변수타입)
| 타입 | 설명 | 예시 |
|---|---|---|
| Numbers| 숫자, 단위 사용 가능 | `1`, `.10`, `10px`
| Strings | 문자, `따옴표''`여부에 상관없이 문자열로 취급 | `bold`, `"/image/logo.png"`
| Colors | 색상명이나 색상리터널로 표기된 값 | `black`, `#FFFFFF`, `rgba(255,255,255, 0.3)`
| Nulls | null, 컴파일 하지 않음 | `null`
| Booleans | 논리 자료형 | `true`, `false`
| Lists | 리스트, 각 변수는 동일한 타입일 필요는 없으며 `콤마,`나 `공백`으로 구분 | `(value1, value2, value3)`,` value1 value2`
| Maps | `콜론:`으로 `키:값`을 구분, `괄호( )`를 사용 |  `(key1:value1, key2:value2, key3:value3)`


```
#SCSS:

$color-primary: #FFFFFF;
$image-logo: "./image/logo.png";

.app {
    background: $color-primary url($image-logo);
}

#CSS:
    
.app {
    background: #FFFFFF url("./image.logo.png");
}
```

### Scope (유효범위)
변수는 사용 가능한 유효범위가 있습니다.
선언된 `블록{ }` 내에서만 유효합니다.

```
#SCSS:

.app1 {
    $color-primary: #FFFFFF;
    background: $color-primary;
}

.app2 {
    background: $color-primary;     //error
}
```

### !global (전역 설정)
!global를 사용하여 변수를 전역변수로 설정 할 수 있습니다.
```
#SCSS:

.app1 {
    $color-primary: #FFFFFF !global;
    background: $color-primary;
}

.app2 {
    boackground: $color-primary;
}

#CSS: 
.app1 {
    background: #FFFFFF;
}

.app2 {
    background: #FFFFFF;
}

```

### !default
!default는 할당되지 않는 변수의 초기값을 설정합니다.
이미 할당되어 있는 변수에는 적용되지 않습니다.
```
#SCSS:

$color-primary: black;
.app {
    $color-primary: white !default;
    background: $color-primary;
}

#CSS:
.app {
    background: black;
}
```
***
## Operators(연산자)
SASS에서는 연산자를 사용가능합니다. 사용 가능한 연산자는 다음과 같습니다.

#### 산술 연산자 
|종류|설명|
|:---:|---|
|`+`|더하기|
|`-`|빼기|
|`*`|곱하기|
|`/`|나누기|
|`%`|나머지|

#### 비교 연산자
|종류|설명|
|:---:|---|
|`==`|같다|
|`!=`|다르다|
|`>`|크다|
|`<`|작다|
|`>=`|크거나 같다|
|`<=`|작거나 같다|

### 논리 연산자
|종류|설명|
|:---:|---|
|`and`|그리고|
|`or`|또는|
|`not`|부정|

### 숫자 연산
기본적으로 숫자 연산은 `px`단위로 합니다. 상대적 단위(`%`,`em`,`vw`등)의 연산은 CSS의 내장함수인 calc()를 사용해야 합니다. 

#### 나누기 연산 주의사항
CSS에서는 속성 값의 숫자를 분리하는 방법으로 `/`을 허용하기 때문에 정상적으로 연산이 되지 않을 수 있습니다.
`/`를 나누기로 사용하기 위해선 다음과 같은 조건을 충족해야 합니다.
- 값 또는 그 일부가 변수에 저장되거나 함수에 반환되는 경우
- 값이 `괄호 ( )`로 묶여 있는 경우
- 다른 산술 연산자와 함께 사용하는 경우
``` 
width: 10px + 10px;         // 정상
width: 50% - 20px;          // error
width: calc(50% - 20px);    // 정상

#나누기
$x: 100px;
width: $x / 2;   // 변수에 저장된 값을 나누기
height: (100px / 2);   // 괄호로 묶어 사용
font-size: 10px + 2px / 3;   // 다른 산술 연산자와 함께 사용
```

### 문자 연산
문자 연산은 `+`를 사용합니다. 문자 연산의 결과는 첫번째 피연산자를 기준으로 합니다.
첫 번째 피연산자가 `따옴표 " "`가 있다면 결과 또한 `따옴표 " "`가 있습니다.
첫번째 피연산자가 `따옴표 " "`가 없다면 결과 도한 `따옴표 " "`가 없습니다.

```
SCSS:
.app {
    text1: "hello" + world;
    text2: hello + " SCSS";
}

CSS:
.app {
    text1: "hello world";
    text2: hello SCSS;
}
```

### 색상 연산
RGB, RGBA값 또한 연산이 가능합니다.
```
SCSS:
.app {
    $color-primary: #00FF00 + #FF00FF;
    // R: 00 + FF = FF
    // G: FF + 00 = FF
    // B: 00 + FF = FF
    background: rgba(50, 50, 50, .2) + rgba(10, 10, 10, .2);
    // R: 50 + 10 = 60
    // G: 50 + 10 = 60
    // B: 50 + 10 = 60
    // A: Alpha값이 같아아 연산이 가능
}

CSS:
.app {
    color-primary: #FFFFFF;
    background: rgba(60, 60, 60, .2);
}
```

### 논리 연산
논리 연산은 `and`, `or`, `not`이 있으며 다른 언어들과 유사하게 사용 가능합니다.
```
SCSS:
$width: 90px;
.app  {
    @if not ($width > 100px) {
        height: 300px;
    }
}

CSS:
.app {
    height: 300px;
}
```

***

## Nesting (중첩)
SASS는 서브 루틴안에 다른 서브루틴을 사용할 수 있습니다.
중첩을 통하여 반복을 피하고 좀 더 가독성 좋게 구조를 만들 수 있습니다.

```
SCSS:
.app {
    width: 100%;
    .list {
        padding: 10px;
        li {
            margin: 10px;
        }
    }
}

CSS:
.app {
    width: 100%;
}
.app .list {
    padding: 20px;
}
.app .list li {
    margin: 10px;
}
```

### 상위선택자 치환
서브루틴 안에 `&`를 사용하면 상위 선택자로 치환됩니다.
```
SCSS:
a {
    text-decoration: none;
    &:hover {
        text-decroation: underline;
    }
}

CSS:
a { text-decoration: none;}
a:hover { text-decoration: underline; }
```

#### 다음과 같이 `&`를 응용하여 사용 가능합니다.
```
.fs {
    &-small { font-size: 10px;}
    &-medium { font-size: 12px;}
    &-large { font-szie: 14px;}
}
```
### 중첩속성 사용
```
SCSS:
.box {
    font : {
        weight: bold;
        size: 10px;
        family: sans-serif;
    };
    margin: {
        top: 10px;
        left: 10px;
    };
    padding: {
        bottom: 20px;
        right: 20px;
    }
}

CSS:
.box {
    font-weight: bold;
    font-size: 10px;
    font-family: sans-serif;
    margin-top: 10px;
    margin-left: 10px;
    padding-bottom: 20px;
    padding-right: 20px;
}
```

***

## Mixin (믹스인)
Mixin은 공통적으로 많이 쓰이는 CSS의 선언값들을 묶어 재사용할 수 있게 만드는 기능입니다.
`@mixin`로 선언하고 `@include`로 사용합니다.

### @mixin
@mixin 지시어를 사용하여 스타일을 정의합니다.
```
SCSS:

@mixin large-text {
    font-size: 10px;
    font-weight: bold;
    color: black;
}
```
#### @mixin 은 선택자 포함이 가능하여 `&`가 사용 가능합니다.
```
SCSS:
@mixin large-text {
    font: {
        size: 10px;
        weight: bold;
    }
    color: orange;

    &::after {
        content: "..";
    }
}
```
### @include
@mixin을 사용하기 위해서는 @include를 사용해야 합니다.
```
SCSS:
h1 {
    @include large-text;
}
div {
    @include large-text;
}

CSS:
h1 {
    font-size: 10px;
    font-weight: bold;
    color: black;
}
h1::after {
    content: "..";

}
div {
    font-size: 10px;
    font-weight: bold;
    color: black;
}
div::after {
    content: "..";
}

```

### 매개변수
mixin은 `함수(funtion)`와 같이 매개변수를 가질 수 있습니다.
```
SCSS:
@mixin dash-line($width, $color) {
    border: $swidth dashed $color;
}

.box1 { @include dash-line(1px, red);}
.box2 { @include dash-line(2px, blue);}

CSS:
.box1 {
    border: 1px dashed red;
}
.box2 {
    border: 2px dashed blue;
}
```

#### 매개변수 기본값 설정
매개변수는 기본값을 가질 수 있습니다.
@include를 사용할 때 별도의 인수를 전달하지 않으면 기본값으로 적용됩니다.
```
SCSS:
@mixin dash-line($width: 1px, $color: red) {
    border: $swidth dashed $color;
}

.box1 { @include dash-line;}
.box2 { @include dash-line(2px, blue);}

CSS:
.box1 {
    border: 1px dashed red;
}
.box2 {
    border: 2px dashed blue;
}
```

### 키워드 매개변수
전달 할 매개변수를 키워드로 지정하여 사용할 수 있습니다.   
@include에서 사용할 때 키워드를 사용하면 매개변수 순서에 상관없이 사용가능합니다.   
```
SCSS:
@mixin positon(
    $p: absolute,
    $t: null,
    $b: null,
    $l: null,
    $r: null
) {
    positon: $p;
    top: $t;
    bottom: $b;
    left: $l;
    right: $r;
}

.absolute {
    @incude positon($b: 10px, $r: 10px);
}
.fixerd {
    @include position(
    fixed,
    $t: 30px,
    $r: 40px
    );
}

CSS:
.absolute {
    positon: absolute;
    bottom: 10px;
    right: 10px;
}
.fixed {
    positon: fixed;
    top: 30px;
    right: 40px;
}
```

### 매개변수 리스트
매개변수의 개수를 알수 없을때는 `arglist`를 사용할 수 있습니다.
`arglist`는 임의의 수의 매개변수를 mixin이나 함수에 전달할 때 암묵적으로 사용되는 SASS의 데이터 유형이며 `...`로 사용 할 수 있습니다.
```
SCSS:
@mixin shadows($shadows...) {

}
@include shadows(shadowsA, shadowsB, shadowsC);
```
```
SCSS:
@mixin bg($width, $height, $bg-values...) {
    width: $width;
    height: $height;
    background: $bg-values;
}

div {
    //bg_values는 개수 상관없이 전달
    @include bg(
        100px,
        200px,
        url("/image/a.png") no-repeat 10px 20px,
        url("/image/b.png") no-repeat,
        url("/iamge/c.png")
    );
}

CSS:
div {
    width: 100px,
    height: 200px,
    background: url("/image/a.png") no-repeat 10px 20px,
                url("/image/b.png") no-repeat,
                url("/iamge/c.png");
}
```
인수를 전달 할때도 `arglist`를 사용할 수 있습니다.
```
SCSS:
@mixin font(
    $style: normal,
    $weight: normal,
    $size: 16px,
    $family: sans-serif
) {
    font: {
        style: $style;
        weight: $weight;
        size: $size;
        family: $family;
    }
}

div{
    // 매개변수 순서에 맞게 전달
    $font-values: italic, bold, 16px, sans-serif;
    @include font($font-values...);
}
span {
    //필요한 값의 키워드 매개변수를 변수에 담에 전달
    $font-values: (style: italic, size: 22px);
    @include font($font-values...);
}
a {
    // 필요한 값만 키워드 매개변수로 전달
    @include font((weight:900, family: monospace)...);
}

CSS:
div {
    font-style: italic;
    font-weight: bold;
    font-size: 16px;
    font-family: sans-serif;
}
span {
    font-style: italic;
    font-weight: normal;
    font-size: 22px;
    font-family: sans-serif;
}
a {
    font-style: normal;
    font-weight: 900;
    font-size: 16px;
    font-family: monospace;
}
```

### @content
`@content`를 사용하여 `스타일 블록`을 전달 할 수 있습니다.
이 기능을 사용하여 @mixin에 설정된 선택자나 속성 외에 선택자나 속성을 추가할 수 있습니다.
```
SCSS:
@mixin icon($url) {
    &::after {
        content: $url;
        @content;
    }
}
.icon1 {
    //기존 mixin의 기능만 사용
    @include icon("/image/a.png");
}
.icon2 {
    //content를 이용하여 스타일 블록을 추가
    @include icon("/image/b.png") {
        position: absolute;
    }
}

CSS:
.icon1::after {
    content: "/image/a.png";
}
.icon2::after {
    content: "/image/b.png";
    positon: absolute;
}
```
***
## Extend (상속)
SACC는 `@extend`를 통해 모듈식 CSS를 작성할 수 있습니다.
다만 `@extend`는 득보단 실이 많을 수 있는 까다로운 개념입니다. 해당 기능을 사용하기전에 다음과 같은 문제를 고려해야됩니다.
- 내 현재 선택자가 어디에 첨부될 것인가?
- 원치 않은 부작용이 초래될 수도 있는가?
- 이 한번의 확장으로 얼마나 큰 CSS가 생성되는가?

이점을 고려하면 `@extend`는 유익할 수도 있지만 큰 부작용을 초래할 수도 있습니다.
그렇기에 `@extend`를 피하고 위의 `@mixin`의 사용을 권장합니다.
```
SCSS:
.btn {
    padding: 10px;
    margin: 10px;
    background: blue;
}
.btn-danger {
    @extend .btn;
    background: red;
}

CSS:
.btn, .btn-danger {
    padding: 10px;
    margin: 10px;
    background: blue;
}
.btn-danger {
    background: red;
}
``` 
***

## Import (불러오기)
`@import`로 외부에서 가져온 SASS파일을 단일 CSS파일로 병합합니다.
또한 `@import`한 파일에서 정의된 모든 변수와 `@mixin` 등을 사용 가능합니다.

`@import`는 SASS파일을 가져오지만 아래 규칙으로 사용 될 경우 CSS `@import`로 사용됩니다.
- 파일 확장자가 `.css` 일 경우
- 파일 이름이 `http://`로 시작하는 경우
- `url()`을 사용할 경우
- 미디어쿼리가 있는 경우
```
@import "helloworld.css";
@import "http://helloworld.com";
@import url(helloworld);
@import "helloworld" screen;
```

### 여러 파일을 @import 하기
하나의 `@import`로 여러 파일을 가져올 수 있습니다.
`,`로 구분합니다.
```
@import "hello", "world";
```

### 파일 분활
SASS의 Partials 기능을 사용하여 SASS를 기능별로 세분화하여 나눌 수 있습니다.   
또한 파일 이름앞에 `_`을 사용하여 @import 가져오면 `_`로 시작하는 파일은 CSS로 컴파일 하지 않습니다.

```
SCSS 구조 :
//index.scss @import "header", "footer"
SCSS
 |-_header.scss
 |-_footer.scss
 `-index.scss

CSS 컴파일:
CSS
 `-index.css // index + header + footer
SCSS
 |-header.scss
 |-footer.scss
 `-index.scss
```

***

## 조건 / 반복문

### if(함수)
조건의 값에 따라 두 개의 표현식 중 하나만 반환합니다.
삼항 연산자와 비슷하게 사용 가능합니다.

조건의 값이 `true`면 `표현식1` 을,
조건의 값이 `false`면 `포현식2` 를 실행합니다.
```
if(조건, 표현식1, 표현식2)
```
```
SCSS:

$width: 555px;
div {
    width: if($width > 300px, $width, 100px);
}

CSS:
div {
    width: 555px;
}
```
### @if (지시어)
`@if` 지시어는 조건에 따른 분기 처리가 가능합니다.   
같이 사용가능한 지시어는 `@else`, `@else if` 가 있습니다.   
추가 지시어를 통하여 보다 복잡한 조건문을 완성할 수 있습니다.   

```
// @if
@if (조건) {
    // ...
}

// @if @else
@if (조건) {
    // ...
} @else {
    // ...
}

// @if @else if @else
@if (조건) {
    // ...
} @else if (조건) {
    // ...
} @else {
    // ...
}
```
조건이 참일 경우에는 `( )`를 생략할 수 있습니다.
```
$bg: true;
div {
    @if $bg {
        background: url("/image/a.png");
    }
}
```
조건에 논리 연산자인 `and`, `or`, `not`을 사용할 수 있습니다.   
조건에서 거짓을 사용할 때는 `false`나 `null` 대신 `not`을 사용하는 것을 권장합니다. 
```
// not
@if not index($list, $item) {
    // ...
}

// and
// @return을 사용할 때는 조건문 블록 밖에서도 @return을 명시
@function limitSize($size) {
    @if $size >= 0 and $size <= 200px {
        @return 200px;
    }
    @return 800px;
}
```

## @for
@for는 스타일을 반복 출력이 필요할 때 사용합니다.   
`through`를 사용하는 형식과 `to`를 사용하는 형식 2가지가 있습니다.   
두 형식은 종료 조건이 해석되는 방식이 다릅니다.
```
// through
// 종료까지 반복
@for $변수 from 시작 through 종료 {
    // ...
}

// to
// 종료 전까지 반복
@for $변수 from 시작 to 종료 {

}
```
```
SCSS:
// 1번부터 3번까지 3번 반복
@for $i from 1 through 3 {
    .through:nth-child(#{$i}) {
        width: 20px * $i
    }
}

// 1번부터 2번까지 2번 반복
@for $i from 1 to 3 {
    .to:ntn-child(#{$i}) {
        width: 20px *i
    }
}

CSS:
.through:ntn-child(1) { width: 20px; }
.through:ntn-child(2) { width: 40px; }
.through:ntn-child(3) { width: 60px; }

.to:ntn-child(1) { width: 20px }
.to:ntn-child(2) { width: 40px }
```

일반적으로 `to`보다는 `throught`를 사용하는 것을 권장합니다.

### @each
`@each`는 `List`나 `Map`데이터를 반복할 때 사용합니다.
```
@each $변수 in 데이터 {
    // ...
}
```
`@each`를 이용하여 List를 반복 출력하겠습니다. 
```
SCSS:
// List Data
$fruits: (apple, orange, banana, mango);

.fruits {
    @each $fruit in $fruits {
        li.#($fruit) {
            background: url("/images/#{$fruit}.png");
        }
    }
}

CSS:
.fruits li.apple {
    backgrund: url("/images/apple.png");
}
.fruits li.orange {
    backgrund: url("/images/orange.png");
}
.fruits li.banana {
    backgrund: url("/images/banana.png");
}
.fruits li.mango {
    backgrund: url("/images/mango.png");
}
```
`index( )` 내장함수를 통해 index값을 불러올 수 있습니다.
```
SCSS:
// List Data
$fruits: (apple, orange, banana, mango);

.fruits {
    @each $fruit in $fruits {
        $i: index($fruits, $fruit);
        li:nth-child(#{$i}{
            left: 50px * $i;
        }
    }
}

CSS:
.fruits li:nth-child(1) {
    left: 50px;
}
.fruits li:nth-child(2) {
    left: 100px;
}
.fruits li:nth-child(3) {
    left: 150px;
}
.fruits li:nth-child(4) {
    left: 200px;
}
```
동시에 여러 개의 List 데이터를 반복 처리 할 수 있습니다.   
단, 각 데이터의 Length가 같아야 합니다.
```
SCSS:
$apple: (apple, korea);
$orange: (orange, china);
$banana: (banana, japan);

@each $fruit, $country in $apple, $orange, $banana {
    .box-#{$fruit} {
        background: url("/images/#{country}.png);
    }
}

CSS:
.box-apple {
    background: url("/images/korea.png");
}
.box-orange {
    background: url("/images/china.png");
}
.box-banana {
    background: url("/images/japan.png");
}
```
`Map`데이터를 반복할 경우에는 두개의 변수(`key`, `value`)가 필요합니다.
```
@each $key, $value in 데이터 {
    // ...
}
```
```
SCSS:
$fruits-data: (
    apple: korea,
    orange: china,
    banana: japan
);

@each $fruit, $country in $fruits-data: {
    .box-#{$fruit} {
        background: url("/images/#{$country}.png")
    }
}

CSS:
.box-apple {
    background: url("/image/korea.png");
}
.box-ornage {
    background: url("/images/china.png");
}
.box-banana {
    background: url("/images/japan.png");
}
```
### @while
`@while`은 조건이 `false`가 될때까지 반복합니다.
```
@while 조건 {
    // ...
}
```
```
SCSS:
$i: 6;

@while $i > 0 {
    .item-#{$i} {
        width: 2px * $i;
    }
    $i: $i -2;
}

CSS:
.item-6 { width: 12px; }
.item-4 { width: 8px; }
.item-2 { width: 4px; }
```

***

## 시작하기
    # 프로젝트 생성
    npx create-react-app [프로젝트명]
    
    # SASS 설치
    yarn add node-sass

    # 컴파일 방법 추가

***

## Learn More
[SASS-GUIDELIN](https://sass-guidelin.es/ko/)

[SASS(SCSS) 완전 정복!](https://heropy.blog/2018/01/31/sass/)
