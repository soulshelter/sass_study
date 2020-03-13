# SASS 정리 - 한눈에 보기

## SASS 란?

Sass(Syntactically Awesome Style Sheets)는 CSS의 단점을 보완하기 위해 만든 CSS 전처리기(CSS pre-processor)입니다.   
복잡한 CSS 작업을 보다 쉽게 할 수 있게 해주며 코드 재활용이 가능해 가독성을 높여주는 프로그래밍 언어입니다.

***

## SCSS 란?

SASS 문법은 기존 CSS의 문법과 달리 `;`를 사용하지 않고 `들여쓰기`를 사용하여 CSS 사용자들이 이해하기가 어려웠습니다.   
그래서 SASS 3버전 이상부터는 기존 CSS 문법에 사용되던 `중괄호{ }`를 사용한 SCSS(Sassy CSS)로 변경되었습니다.   
이로 인해 CSS만 사용하던 사람들 또한 보다 쉽게 사용할 수 있습니다.

    # SASS
    ul.list
        overflow:hidden
        li
            float:left
            width:200px

    # SCSS
    ul.list {
        overflow:hidden;
        li {
            float:left;
            width:200px;
        }
    }

***

## Variable (변수)

SASS는 변수가 사용 가능합니다.   
각 변수는 타입을 가지며 각 타입에 대한 연산도 가능합니다.   
변수를 선언할 때는 맨 앞에 `$`를 사용합니다. 
|타입|설명|
|---|---|
|숫자|숫자 리터널로 사용된 값, 크기를 나타낼 경우에도 숫자로 판단|
|문자열|문자의 경우 기본적으로 `따옴표''`여부에 상관없이 문자열로 취급|
|폰트|색상명이나 색상리터널로 표기된 값|
|색상|
|null|null
|list|리스트 내 각 변수는 동일한 타입일 필요는 없으며 `콤마,`나 공백으로 구분
|map|`콜론:`으로 `키:값`을 구분

    # SASS
    $primary : #000;

    # CSS
 
***

## Math Operators(수학연산자)

***

## Built-in Functions

***

## Nesting (중첩)

***

## Import (불러오기)

***

## Extend (상속)

***

## Mixin (믹스인)

***

## Function (함수)

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
