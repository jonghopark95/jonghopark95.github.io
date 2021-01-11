---
layout: post
title: React, TypeScript 프로젝트 초기 셋팅
summary: Katumm Admin 사이트를 위한 React, TypeScript, Webpack, Babel 초기 셋팅 과정을 기록해봅니다.
featured-img: katumm-admin
---

## 개요
Katumm-Admin 프론트엔드 페이지를 제작하게 되었다.
개발 환경으로 TypeScipt와 React를 선택하게 되었는데, 셋팅 후 기억 나지 않을 것을 대비하여 글을 남기게 되었다.

## 사용 라이브러리
React, TypeScript, Webpack, Babel, Eslint, Prettier

## Webpack

[Webpack official site](https://webpack.js.org/concepts/#entry)

`yarn add -D webpack webpack-cli webpack-dev-server`

webpack은 정적 모듈 번들러로서, 애플리케이션을 dependency graph를 만들어 프로젝트에 필요한 모든 모듈을 매핑하며, 하나 또는 다중의 bundle을 생성한다.

예제 프로젝트에 webpack, webpack-cli, webpack-dev-server를 설치한다.
webpack은 핵심 패키지 이며, webpack-cli는 터미널에서 webpack 커맨드를 실행할 수 있게 해주는 command line 도구이며 webpack-dev-server는 빠른 실시간 리로드를 위한 개발 서버이다.

해당 패키지들은 개발할 때에만 필요한 의존성이므로 -D를 사용한다.

webpack.config.js 파일에 다음과 같은 설정을 해준다.

```js
const path = require("path");

module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "./dist"), // __dirname tell you the absolute path of the directory
    filename: "bundle.js",
  },
  devServer: {
    contentBase: path.resolve(__dirname, "./dist"),
  },
};

```
설정에 의하여 빌드를 할 경우 webpack은 ./src/index.js 파일을 가져와 bundle.js 파일을 ./dist 경로에 만들 것이다.

devServer config는 dev server를 실행할 때 사용된다.

이후 package.json에 다음과 같이 추가한다.
```js
"scripts": {
  "dev":"webpack serve --open --hot",
  "build":"webpack --mode production --progress"
} 
```
webpack serve는 컴파일된 파일을 따로 저장하지 않기 때문에 속도가 빨라지게 만든다.

## React

`yarn add react react-dom`

가상 DOM과 컴포넌트를 사용하기 위하여 react를 추가해주고, 실제 DOM과 상호작용 하기 위하여(render) react-dom을 추가한다.

webpack은 단지 번들러이기 때문에 React JSX를 컴파일 할수는 없다. 따라서 Babel을 사용해주어야 한다.

## Babel
Babel은 컴파일러다. 인터프리터 언어인 JS에서 컴파일이 필요하다는 표현이 웃기지만, 빠르게 발전하는 프론트엔드 기술과 달리 브라우저의 지원은 이에 못미치는 경우가 많으므로 구형 브라우저를 위한 컴파일이 필요하다. 

또, 마찬가지의 이유로 polyfill이라는 기능이 필요하다. babel은 단지 문법을 변환해주는 역할만 할 뿐이다. polyfill은 프로그램이 처음 시작될 때 현재 브라우저에서 지원하지 않는 함수를 검사하여 각 Object Prototype에 붙여준다. 

즉, babel은 compile-time, babel-polyfill은 run-time에 실행된다.

`yarn add -D @babel/core babel-loader`   
@babel/core는 babel의 코어가 담겨져 있는 라이브러리이며, webpack과 Babel을 함께 사용하기 위해 babel-loader를 설치한다.

`yarn add -D @babel/preset-react`
React JSX를 컴파일 하기 위해 Babel config 셋팅을 해야 한다. 이를 위해 @babel/preset-react를 설치한다.

`yarn add -D @babel/preset-env core-js regenerator-runtime @babel/plugin-proposal-class-properties @babel/plugin-proposal-object-rest-spread`

앞서 말한 compile과 polyfill을 하기 위해 다음 모듈들을 설치해준다.

- @babel/preset-env : 구형 브라우저 지원
  - 설정 단계에서 useBuiltIns, corejs를 설정해주어야 한다.
  - corejs는 버전 2, 3중 선택할 수 있는데 유지보수를 이유로 3을 선택한다.
  - useBuiltIns은 어떻게 polyfill code를 add하는지 설정해준다.
    - usage, entry를 설정할 수 있는데 bundle size를 줄이기 위해 usage를 사용한다. 
- core-js : polyfill preset
- regenerator-runtime : generator function polyfill
- @babel/plugin-proposal-class-properties : class 지원
- @babel/plugin-proposal-object-rest-spread : spread 연산자 지원 

## TypeScript
타입스크립트를 추가하기 위해 두 가지 컴파일 옵션이 있다.
1. TypeScript compiler를 이용한다. TSC(Typescript compiler)는 type 체킹도 해준다.
2. Babel을 이용한다. 

Babel이 TSC에 비해 더 유동적인 transpile이 가능하기 때문에 두 번재 옵션을 사용한다. 

`yarn add -D typescript @types/react @types/react-dom`
우선 typescript와 react, react-dom type을 추가한다.

`yarn add -D @babel/preset-typescript`
typescript를 위한 babel preset이다.



## Reference

- [2020 Settings of React TypeScript Project with webpack and Babel](https://medium.com/swlh/2020-settings-of-react-typescript-project-with-webpack-and-babel-403c92feaa06)
- [webpack-dev-server 사용하기](https://velog.io/@adam2/webpack-dev-server-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0%EC%82%BD%EC%A7%88%ED%9B%84%EA%B8%B0#:~:text=webpack%2Ddev%2Dserver%EB%8A%94%20%EB%B9%A0%EB%A5%B8,%EC%A7%80%EC%A0%95%ED%95%98%EC%97%AC%20%EC%82%AC%EC%9A%A9%EC%9D%B4%20%EA%B0%80%EB%8A%A5)
- [webpack-dev-server does not work with webpack-cli v4](https://github.com/webpack/webpack-dev-server/issues/2759)
- [prettier, eslint 설정](https://simsimjae.medium.com/prettier%EC%99%80-eslint%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%84%A4%EC%A0%95-110dc8ab94b6)