# SASS 정리 - 한눈에 보기

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

***

## Import (불러오기)

***

## Extend (상속)

***


## Function (함수)

***

## 조건 / 반복문

### if
### for
### each
### while
### 

***

## 시작하기
    # 프로젝트 생성
    npx create-react-app [프로젝트명]
    
    # SASS 설치
    yarn add node-sass

    # 컴파일 방법 추가

***


## Available Scripts

### `yarn start`

Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

### `yarn test`

Launches the test runner in the interactive watch mode.<br />
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `yarn build`

Builds the app for production to the `build` folder.<br />
It correctly bundles React in production mode and optimizes the build for the best performance.

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `yarn eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: https://facebook.github.io/create-react-app/docs/code-splitting

### Analyzing the Bundle Size

This section has moved here: https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size

### Making a Progressive Web App

This section has moved here: https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app

### Advanced Configuration

This section has moved here: https://facebook.github.io/create-react-app/docs/advanced-configuration

### Deployment

This section has moved here: https://facebook.github.io/create-react-app/docs/deployment

### `yarn build` fails to minify

This section has moved here: https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify
