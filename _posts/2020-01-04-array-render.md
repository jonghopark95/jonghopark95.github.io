---
layout: post
title: 배열 렌더링하기
summary: 배열에 element를 추가, 수정, 삭제하는 법과 react에서 배열을 렌더링 하는 법에 대해 알아봅니다.
featured-img: reactjs
---

## 개요

```js
const a = 아;
const b = 맥주마렵다;
```

리액트에서 a, b를 렌더링 하고 싶다면 단순히 {a}, {b}를 포함시키면 될겁니다. 하지만 요소가 동적으로 늘어난다면, 꽤나 큰 수로 늘어난다면 이는 전과 같은 방식으로 추가하기 매우 힘들어 질 것입니다.

이를 해결 하기위해 JS의 내장함수를 활용해 여러 요소들을 렌더링 할 수 있습니다.

## JS array method

JS의 Array class는 여러 내장 메모리를 가지고 있습니다. 이를 사용해 배열에 추가, 제거, 요소 변경들을 원활히 할 수 있습니다.

- 추가
  - Array의 끝 부분에 요소 추가
  ```js
  let newLength = fruits.push("Orange");
  // ["Apple", "Banana", "Orange"]
  ```
  - Array의 첫 부분에 요소 추가
  ```js
  let newLength = fruits.unshift("Strawberry"); // add to the front
  // ["Strawberry", "Banana"]
  ```
- 제거

  - Array의 끝 요소 제거

  ```js
  let last = fruits.pop(); // remove Orange (from the end)
  // ["Apple", "Banana"]
  ```

  - Array의 첫 요소 제거

  ```js
  let first = fruits.shift(); // remove Apple from the front
  // ["Banana"]
  ```

  - 인덱스 포지션의 요소 제거

  ```js
  let vegetables = ["Cabbage", "Turnip", "Radish", "Carrot"];
  console.log(vegetables);
  // ["Cabbage", "Turnip", "Radish", "Carrot"]

  let pos = 1;
  let n = 2;

  let removedItems = vegetables.splice(pos, n);
  ```

- 변경
  - map
  ```js
  let arr = [1, 2, 3, 4, 5];
  arr = arr.map((val) => val * 2);
  // arr = [2, 4, 6, 8, 10]
  ```
  - forEach
    forEach를 사용하면 배열 안의 요소를 순회할 수는 있지만 해당 배열의 요소를 변경시키진 않는다.

## map()

JS list의 내장함수인 map()은 기존 배열의 element를 순회하고 조건에 맞게 값을 변형시켜 새로운 배열을 반환합니다.

```js
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
console.log(doubled);
```

다음 코드에서 map은 numbers의 값을 순회하여 [2, 4, 6, 8, 10]의 새로운 배열을 만들어냅니다.

## 여러 개의 element 렌더링

map() 메소드를 사용하여 여러 개의 JSX 요소들을 렌더링 할 수 있습니다.

```js
import React from "react";
import "./styles.css";

const Test = () => {
  const arr = ["종호", "취업", "마렵다"];
  return arr.map((i) => <li>{i}</li>);
};

export default function App() {
  return (
    <div className="App">
      <Test />
    </div>
  );
}
```

위의 코드를 실행하면

> **Warning: Each child in a list should have a unique "key" prop. See https://reactjs.org/link/warning-keys for more information.**

~~다음과 같은 오류를 만나게 됩니다. 이 오류를 해결하기 위해서는 리스트의 각 항목에 Key를 할당하면 됩니다.~~
```js
return arr.map((i, index) => <li key={index}>{i}</li>);
```

오류가 아닌 경고 메시지입니다. 경고 메시지가 출력되도 실행은 됩니다. 다만 React에서 Key는

(http://admin.katumm.com/)