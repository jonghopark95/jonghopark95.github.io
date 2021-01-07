---
layout: post
title: useReducer, Context API 이용하기
summary: 전역 상태 관리를 도와주는 useReducer, Context API에 대해 알아봅시다.
featured-img: reactjs
---

## useReducer

기존에 우리는 React.Component 클래스의 life cycle function이나 hook을 사용한 setState와 같은 로직으로 상태를 변경하였다. 두 상태 업데이트 로직 모두 컴포넌트 내부에서 선언해줬어야 했다. 근데 컴포넌트 외부에서 이를 관리하게 해줄 수 있는 훅이 있는데, 바로 useReducer이다.

useReducer는 (state, action) => newState 의 형태로 동작한다.
잘 와닿지 않을 수 있는데, 즉 상태와 그 상태를 변경시켜 주는 action을 받아 새로운 상태를 반환시켜준다는 것이다.

> const [state, dispatch] = useReducer(reducer, initialArg, init);

useReducer는 인자로 reducer, initialArg, init을 받는다.
reducer는 액션에 따라 상태를 변경시켜주는 함수이며 initialArg는 초기 인자, init은 초기화를 지연시키고 싶을 경우 (상황에 따라 state를 재설정 하기 위하여, 초기 state를 계산하기 위하여) init 함수를 세번 째 인자로 전달할 수 있다.

### 예제

아래는 useReducer를 사용한 카운터 예제이다.

```js
const initialState = { count: 0 };

const reducer = (state, action) => {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
};

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
    </>
  );
};
```

만약 useReducer에서 현재 state와 같은 값을 반환하는 경우 React는 자식을 re-rendering하거나 effect를 발생시키지 않는다.

## Context API

Context는 global 한 데이터를 공유할 수 있도록 고안된 방법이다.
예를 들어, 웹 사이트의 컨셉 컬러를 지정해놓고 해당 데이터를 전역으로 공유할 수 있는 경우가 있을 것이다.
기존의 방법으로는 props로 자식 컴포넌트에게 넘겨줬어야 했다.
이는 nesting이 많아질 경우 한도 끝도 없이 props를 전달받아야 한다.

중간에 있는 컴포넌트들은 불필요한 prop을 받아 하위 컴포넌트에게 전달해야 한다.
이 과정은 매우 귀찮고 관리하기 번거로울 수 있다. 이런 경우 Context API를 사용하면 효율적일 것이다.

### Context API를 사용해야 하는 경우

context를 사용하면 컴포넌트를 재사용하기 어려워질 수 있으므로, 필요한 경우를 잘 알아야 한다.
같은 데이터를 여러 레벨이 있는 많은 컴포넌트에게 주어지게 할 경우 (broadcast) context를 사용하는 것이 좋다.

### 예제

```js
const ThemeContext = React.createContext("light");

class App extends React.Component {
  render() {
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

- React.createContext
  ```js
  const ThemeContext = React.createContext("light");
  ```
  - createContext는 Context 객체를 만든다. 해당 Context 객체 안에는 Provier, Consumer 컴포넌트가 포함되어 있다. Context 객체를 subscribe하는 컴포넌트를 렌더링 할 경우 Provider로 부터 값을 읽는다.
  - Provider가 없을 경우 defaultValue를 갖게 된다. 위의 경우는 light를 갖게 될 것이다.
- Context.Provider
  ```js
  <ThemeContext.Provider value="dark">
  ```
  - Provider는 컴포넌트 계층 내에서 얼마나 nesting 되어 있던지 모든 하위 컴포넌트에서 state를 사용할 수 있게 한다. 변화가 일어날 경우 구독하는 모든 컴포넌트는 re rendering 된다. Provider는 value를 prop으로 전달한다. Provider부터 하위 comnsumer의 전달 과정에서 shouldComponentUpdate 메서드가 적용되지 않으므로, 상위 컴포넌트가 업데이트를 하지 않더라고 consumer는 업데이트 된다.
- Context.Consumer
  ```js
  <MyContext.Consumer>
  ```
  - context 변화를 구독하는 컴포넌트이다. Provider로부터 value를 받아 렌더링한다.
- Context.displayName
  ```js
  MyContext.displayname = "tester";
  ```
  - displayName 속성을 통해 context의 이름을 설정할 수 있다. 바뀐 이름은 React 개발자 도구에서 확인할 수 있다.
- Class.contextType
  ```js
  static contextType = ThemeContext;
  ```
  - React.createContext()로 생성한 context 객체를 contextType 으로 지정할 수 있다.
  - 이 클래스 안의 this.context를 이용하여 provider를 찾아 그 값을 읽을 수 있게 된다.

## useReducer + Context API

소규모 프로젝트에서 상태 변화를 관리하기 위하여 redux를 사용하는 것은 꽤나 무겁게 느껴질 수도 있다. 이 경우, 위에서 언급한 useReducer와 Context API를 중첩하여 사용할 수도 있다.
createContext()로 부터 만들어 지는 Context 객체의 Provider, Consumer 중 Consumer 컴포넌트 대신 useContext를 사용시켜주면 된다.

- [useReducer official documentation](https://reactjs.org/docs/hooks-reference.html#usereducer)
- [Context API official documentation](https://reactjs.org/docs/context.html)
