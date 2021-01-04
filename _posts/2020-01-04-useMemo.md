---
layout: post
title: useMemo, useCallback
summary: React Hooks 중 useMemo와 useCallback을 사용해보았습니다.
featured-img: reactjs
---

## 개요

평소 훅을 사용 할 때 useState, useEffect 정도만 자주 사용하였고 좀 더 나아가 useRef 와 함께 만족스러운 리액트 라이프를 즐겼다.

하지만 이는 훅의 일부분일 뿐이며 더 알려고 하지 않는 것은 그대로 고여 있기 딱 좋기에 BCSD 과제의 도움으로 useMemo, useCallback을 공부해보았다.

## 메모이제이션 (Memoization)

메모이제이션은 DP를 구현하는 방법 중 한 종류로 한 번 구한 결과를 메모리 공간에 메모해두고 같은 식을 호출하면 메모한 결과를 그대로 가져오는 기법이다.

## 왠 메모이제이션??

useMemo와 useCallback의 주요 identity가 메모이제이션이기 때문이다. useMemo는 메모이제이션 된 **_값_**을 반환하며 useCallback은 메모이제이션 된 **_함수_**를 반환한다.

## 예제

예제 추가하기

## useMemo와 useCallback 을 사용해야하는 시점

useMemo와 useCallback은 리액트의 렌더링 성능 최적화를 위한 hook이기 때문에 필수적으로 사용할 필요는 없다.

[When to usememo and useCallback](https://kentcdodds.com/blog/usememo-and-usecallback/)

윗 글에 따르면 사용해야 하는 상황을 두 가지로 말하고 있다.

1. Referential Equality

```js
function CountButton({ onClick, count }) {
  return <button onClick={onClick}>{count}</button>;
}
function DualCounter() {
  const [count1, setCount1] = React.useState(0);
  const increment1 = () => setCount1((c) => c + 1);
  const [count2, setCount2] = React.useState(0);
  const increment2 = () => setCount2((c) => c + 1);
  return (
    <>
      <CountButton count={count1} onClick={increment1} />
      <CountButton count={count2} onClick={increment2} />
    </>
  );
}
```

위 코드의 경우 count1, count2 또는 increment1, increment2가 DualCounter가 렌더링될 때마다 새롭게 만들어 지므로 CountButton이 하나만 눌려도 두 버튼 모두 새롭게 렌더링된다.

이를 막기 위해 useMemo, useCallback을 사용할 수 있다.

2. 고비용의 복잡한 연산

useMemo는 리액트 hook에 내장되어 있는데, 이는 연산에 비용이 많이 드는 함수가 있을 때 특정 값이 변경되었을 때만 호출되도록 하기 위함이다.

## 참고

- [React 공식 문서](https://ko.reactjs.org/docs/hooks-reference.html#usecallback)
- [useCallback과 useMemo를 제대로 사용하는 법](https://atercatus.github.io/react/2020-01-07-useMemo-useCallback)
- [When to usememo and useCallback](https://kentcdodds.com/blog/usememo-and-usecallback/)
