---
layout: post
title: Sass, Scss
summary: CSS 전처리기인 SASS, SCSS에 대해 알아봅니다.
featured-img: sass
---

## 개요

CSS는 그 자체로도 물론 매우 훌륭하지만 기술의 발달과 함께 stylesheet는 커지고 복잡해지며 유지보수하기 힘들어지고 있다. 이러한 문제점을 전처리기(Preprocessor)에서 해결해줄 수 있다. CSS 전처리기 중 SASS, SCSS에 대해 알아본다.

## CSS Preprocessing

전처리기는 CSS에 없는 변수 선언, nesting, mixins, inheritance등 CSS 작성을 좀 더 편리하게 만들어 줄 수 있는 여러 방법을 제시해준다.

다만 브라우저에서는 전처리기로 작성한 코드를 해석할 수 없으므로 CSS로 컴파일하는 작업이 필요하다. 

보통 언급되는 전처리기로 Less, Sass(SCSS), Stylus가 있다.

## Sass? SCSS?

Sass(Syntactically Awesome Style Sheets)는 2006년 부터 시작된 가장 오래된 CSS 전처리기이다.
이후 기존의 Sass가 CSS와의 이질적인 차이 때문에 CSS 친화적인 문법이 나왔는데 이것이 SCSS이다.

### Sass
```sass
nav
  ul
    display:flex
    list-style-type: none
  li
    margin:40px
```

### SCSS
```scss
nav{
  ul {
    display: flex;
    list-style-type: none;
    li {
      margin: 40px; 
    }
  }
}
```

Sass와 SCSS의 뚜렷한 차이는 {}(bracket)와 ;(semicolon)이다.
또한, 서로의 확장자가 .sass, .scss로 다르다.

## Sass의 특성
예시 코드는 모두 SCSS로 작성했습니다.

### 변수 사용 가능
Sass에서는 $ 심볼을 사용하여 변수를 사용할 수 있다.   
미리 지정한 홈페이지의 컨셉 컬러 #565656을 디자이너가 마음에 안든다고 바꿔야 한다고 생각해보자.
CSS로 작성되어 있었다면 #565656을 일일히 찾아 디자이너가 요구한 색상으로 바꿔주어야 한다. 
Sass를 사용하면 간단히 수정할 수 있다.
```scss
$concept-color: #565656;

body{
  color: $concept-color;
}
```

### Nesting
```html
<nav>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>
</nav>
```
위 예시 코드와 같이 HTML에서는 계층적으로 element들이 구성되어 있음을 확인할 수 있다. 그러나 CSS에서는 이를 할 수 없다.
```css
nav{
  width:600px
}
nav ul{
  color:red;
}
nav li{
  font-size:15px;
}
```
위와 같이 CSS는 각 요소들을 접근할 때 계층적으로 접근할 수 없었다.

Sass는 HTML과 동일하게 CSS 선택자들을 계층적으로 nesting할 수 있다.
```scss
nav{
  width:600px;
  ul{
    color:red;
    li{
      font-size:15px;
    }
  }
}
```
이는 CSS를 직관적으로 알아보기 쉽고 유지보수 하기 편하게 만들어준다.

## Modules
Sass는 파일들을 partials로 나누어서 모듈화 할 수 있다.
partial에 해당하는 Sass 파일은 앞에 **_(underscore)**를 붙여주어야 한다. ***(_partials.scss)***   
underscore는 Sass에게 해당 파일이 partial 이고 CSS로 만들 필요가 없다고 말해준다.

이렇게 나눈 partials을 @use를 사용하여 모듈로 불러올 수 있다.
```scss
//_base.scss
$concept-color: #565656;

body{
  color: $concept-color;
}
```
```scss
// styles.scss
@use 'base';

h1{
  color: base.concept-color;
}
```

## Mixins
Mixin은 함수를 사용하는 것처럼 인자를 받아 CSS 선언 그룹을 사용할 수 있도록 한다. 이를 사용하면 같은 구문을 반복해야 하는 상황에서 하나의 Mixin을 만들어 재사용할 수 있다.
```scss
@mixin highlight($bg-color, $font-color:yellow){
  color: $font-color
  background-color: $bg-color;
}
.name {@include highlight(black)}
```
@mixin을 사용해 선언한 후 @include를 사용하여 mixin을 사용할 수 있다.

## Extend/Inheritance
소프트웨어 원칙 중 DRY(Don't Repeat Yourself) 원칙이라는 말이 있다.
동일한 코드가 반복되는 것을 경계해야 한다는 원칙인데, **extend**는 CSS에서 이러한 원칙을 지킬 수 있도록 만들어 준다.

``` scss
%shared-features{
  background-color:red;
  padding-left:10px;
}
.test1{
  @extend %shared-features;
}
.test2{
  @extend %shared-features;
  padding-left:20px;
}
.test3{
  @extend %shared-features;
  background-color:blue;
}
```

## Operators
Sass는 CSS 내에서 간단한 연산(+, -, *, /, %)을 할 수 있도록 만들어 준다.
```sass
width: 600px / 960px * 100%;  // on css, width:62.5%
```
단순히 62.5%가 아닌 수학적 연산을 통해 기술하는 것은 다른 사람이 해당 코드를 보았을 때, 심지어 자신이 작성한 코드를 오랜 시간이 지나 유지보수가 필요할 경우 왜 해당 값을 기술해 놓았는지 빠르게 파악할 수 있도록 도와준다.

## Reference

- [Sass 공식 문서](https://sass-lang.com/guide)
- [What is the difference between scss and sass](https://www.geeksforgeeks.org/what-is-the-difference-between-scss-and-sass/#:~:text=SASS%20is%20used%20when%20we,SAAS%20file%20extension%20is%20.)