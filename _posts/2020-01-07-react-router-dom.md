---
layout: post
title: react-router-dom
summary: React에서 Router 관리를 도와주는 react-router-dom 라이브러리에 대해 알아봅니다.
featured-img: reactjs
---

## React Router 를 사용해야 하는 이유
SPA가 퍼지기 전, 우린 새로운 페이지를 요청하기 위해 서버에게 해당 주소의 파일을 요청하고 그 파일이 로드될 때까지 빈 화면을 응시했어야 했다.

그러나 SPA 아키텍처가 등장하며 새 콘텐츠가 필요할 때마다 서버에 요청할 필요가 없어졌다. 초기 애플리케이션을 로드할 때 모든 콘텐츠를 load하고 URL 경로 이름에 따라 페이지에 콘텐츠를 동적으로 표현하게 되었다.

이 때 라우팅을 활용하여 URL 경로의 이름을 분석하고 이 이름과 관련된 콘텐츠를 분석하게 된다. 해당 관련 콘텐츠는 서버가 아닌 메모리에 저장되며 애플리케이션 내에서 페이지를 스왑하게 된다.

## react-router vs react-router-dom vs react-router-native vs react-router-redux
React Router는 라우터 기능으로, native 모바일 앱을 만들기 위한 react-router-native와 react-router-dom의 코어 기능을 가지고 있다.

react-router-redux는 redux store와 함께 사용지며, store.dispatch()를 경유하여 navigate 하는 등 state와 함께 router를 관리할 수 있다.

react-router-dom은 react-router에 &lt;BrowserRouter>, &lt;Link> 등 browser의 window.history와 상호작용할 수 있는 요소들을 가지고 있다. 우리는 웹 개발을 할 것이므로 react-router-dom을 사용하면 된다.


## &lt;a>가 아니다 &lt;Link>다 !!
리액트는 SPA(Single Page Application) 아키텍처를 사용한다. 이름에서 보다시피 여러 개의 페이지가 아닌 단 한개의 페이지를 사용한다.

하나의 페이지에서 렌더링에 필요한 데이터만을 가져오는 것이다. 

기존에 다른 경로로 이동하기 위해 사용하던 a 태그는 페이지를 이동시키며 아예 새로운 페이지를 불러오기 때문에 리액트가 가진 상태들과 함수, 컴포넌트가 모두 초기화되며 re-rendering 된다.

따라서 Link 컴포넌트를 사용하며 Link 컴포넌트는 경로와 관련된 콘텐츠를 스왑하게 된다.

## 라우팅 방법
1. history(Browser History)
   - window.history.pushState API를 활용하여 페이지를 로드하지 않고 URL을 탐색한다.
2. hash (Hash History)
   - url hash를 사용하여 전체 url을 simulate하며, url이 변경될 때 페이지가 다시 로드되지 않는다. 보통 url에 #이 붙는다.
   - 주로 정적 페이지에서 앵커 이동 시 #을 붙이는 방법으로 사용된다.

## 주요 컴포넌트

### ***Routers***
웹 프로젝트에서 react-router-dom은 &lt;BrowserRouter> 와 &lt;HashRouter>를 제공해준다.

두 개의 차이는 다음과 같다.
* Browser Router
  * 깔끔한 URL을 가지게 해준다.
  * window.history.pushState API를 활용하여 페이지를 로드하지 않고 URL을 탐색한다.

* Hash Router
  * url hash를 사용하여 전체 url을 simulate하며, url이 변경될 때 페이지가 다시 로드되지 않는다. 보통 url에 #이 붙는다. 
  * 주로 정적 페이지에서 앵커 이동 시 #을 붙이는 방법으로 사용된다.
  * 검색 엔진에서 해당 링크를 읽지 못하는데, # 값 때문에 서버가 페이지의 유무를 알지 못하기 때문이다.

라우터를 사용하려면 다음과 같이 루트 element에 wrapping하면 된다.
```js
ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById("root")
);
```

### ***Route Matchers***
두 개의 Router Matchers가 있는데, 이는 &lt;Switch> 와 &lt;Route>이다.

Switch는 자신의 하위 Route Element중 url과 맞는 path를 가진 route 를 찾는다.
해당 라우트를 찾게 되면 다른 모든 것은 무시하게 된다. Switch를 사용하지 않을 경우 URL에 해당하는 모든 요소들이 렌더링 될 수 있다.

이러한 특성으로 짧은 라우트를 나중에 쓰게 되고 긴 라우트를 상단에 위치하게 한다.

```js
<Switch>
  <Route path="/about">
    <About />
  </Route>

  // /contact/:id가 앞에 오도록 하여 개인 contact를 라우팅 할 때 contact가 안 보여지게 한다.
  <Route path="/contact/:id">
    <Contact />
  </Route>
  <Route path="/contact">
    <AllContacts />
  </Route>

  //path="/"는 모든 라우트와 매치 되기 때문에 마지막에 있어야 한다.
  <Route path="/">
    <Home />
  </Route>
</Switch>
```
하지만 개인적으로 메인 페이지가 제일 뒤에 오면 별로라고 느껴진다.
이 때 사용할 수 있는 것이 **exact** 이다. &lt;Route exact path="/">는 전체 URL이 /인 url만 true로 받는다.

### ***Navigation or Route Changers***
React Rotuer는 &lt;Link> 컴포넌트를 제공해준다. 앞서 설명했듯이, navigation으로 a태그가 아닌 Link 태그를 사용해야 한다. Link태그를 사용하면 HTML document로 a 태그가 렌더링된다.

```js
<Link to="/">Home</Link>
```

내비게이션을 강제하고 싶다면 Redirect 태그를 사용하면 된다.
```js
<Redirect to="login" />
```



- [react-router-dom official documentation](https://reactrouter.com/web/guides/quick-start)
- [Dev. DY님의 Vanilla JS에서 SPA 라우팅 시스템 구현하기)](https://kdydesign.github.io/2020/10/06/spa-route-tutorial/)
- [What is difference between react-router 4.0, react-router-dom and react-router-redux?](https://stackoverflow.com/questions/43185222/what-is-difference-between-react-router-4-0-react-router-dom-and-react-router-r/50003311#:~:text=No%20need%20to%20use%20react,uses%20react%2Drouter%20at%20core.&text=Extra%20Benefit%20of%20react%2Drouter,in%20sync%20with%20application%20state.)
