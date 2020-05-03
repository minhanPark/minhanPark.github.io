---
title: "Redux Toolkit 흝어보기"
excerpt: "Redux Toolkit을 흝어보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - javascript
  - Redux
---

## Redux Toolkit이란?

리덕스를 사용하면 코드가 늘어나기 마련이고, 그에 따라서 약간 복잡해진다.  
**리덕스 툴킷은 으레 추가해야하 하는 코드들을 상당히 줄여준다**.  
하지만 코드를 너무 줄이면 또한 이해가 어려워지는 것도 있기 때문에 자기가 이해할 수 있는 범위내에서 줄이는 것이 맞다고 생각한다.  
해당 글을 작성하는 이유는 redux-actions을 사용하는 것과 비슷한 점이 많고 어느정도 익숙한 모습들이 보이기 때문에 정리하게 되었다.

## 설치방법

```
npm install @reduxjs/toolkit

또는

yarn add @reduxjs/toolkit
```

해당 명령어로 존재하는 앱에 설치할 수 있다.  
만약 리액트 프로젝트를 새로 시작하는 입장이라면 redux 템플릿을 선택해서 다운로드 할 수 있다.

```
npx create-react-app my-app --template redux
```

## createAction

리덕스에서는 액션을 정의한다.

자주 예시로 사용되는 increament를 예로 들어보자.

```js
const INCREMENT = "counter/increment"; // 액션

// 액션 크리에이터 함수
function increment(amount) {
  return {
    type: INCREMENT,
    payload: amount,
  };
}

const action = increment(3);
// { type: 'counter/increment', payload: 3 }
```

dispatch를 통해서 액션 크리에이터 함수를 전달하면 그 만큼 값이 증가할 것이다.  
액션이 추가될 때마다 액션과 액션크리에이터 함수들이 계속 늘어난다고 생각해본다면 상당한 코드의 증가라고 할 수 있다.

```js
import { createAction } from "@reduxjs/toolkit";

const increment = createAction("counter/increment");
```

createAction으로 액션을 전달하기만 하면 액션과 액션 크리에이터 함수가 만들어진다.  
만약에 인수를 전달해야 한다면 그냥 increment에 값을 넣어서 전달하면 알아서 payload에 값을 담아서 전달한다.

```js
increment(3);
// {type: 'counter/increment', payload: 3}
```

기본적으로 타입이 전달되고 인수를 전달하면 payload에 담겨서 전달되니 아주 편해진다.  
더 재밌는 점은 **prepareAction을 넣을 수 있다는 점**이다.

```js
function createAction(type, prepareAction?)

// 위 처럼 두번째 인수에 prepareAction을 넣을 수 있다.
```

payload에 값을 넣기 전에 추가적으로 값을 더해야 할 순간들이 있다. payload 객체에 {text, id}를 같이 전달할 수 있지만  
prepareAction을 통해서 id를 생성할 수 있다.

공식문서의 예를 보자.

```js
const addTodo = createAction("todos/add", function prepare(text) {
  return {
    payload: {
      text,
      id: v4(),
      createdAt: new Date().toISOString(),
    },
  };
});
```

payload에 text를 전달하기 전에 {text, id, createdAt}이라는 속성들이 담긴 객체를 만들어서 전달한다.  
이렇게 되면 미리 값을 만들어서 전달하는 것이 아니라 createAction을 통해서 간단하게 전달할 수 있다.

## createReducer

```js
function counterReducer(state = 0, action) {
  switch (action.type) {
    case "increment":
      return state + action.payload;
    case "decrement":
      return state - action.payload;
    default:
      return state;
  }
}
```

보통 리듀서는 switch문을 통해서 처리한다.  
하지만 createReducer를 통해서 코드를 줄일 수 있다.

```js
const increment = createAction("increment");
const decrement = createAction("decrement");

const counterReducer = createReducer(0, {
  [increment]: (state, action) => state + action.payload,
  [decrement.type]: (state, action) => state - action.payload,
});
```

createAction으로 만들어진 객체를 전달하면 된다.  
맨 **처음 인수는 initialState다(예시에서는 0)**이고, 두번째 인수는 리듀서를 정의한다.  
정의할 때 기존의 state를 변형시킬 수도 있고, 새로운 값을 반환할 수도 있다는 특징이 있다.

```js
const todosReducer = createReducer([], {
  [addTodo]: (state, action) => {
    const todo = action.payload
    return [...state, todo]
  },
  (...)
})
```

위의 코드를 보면 todo를 받아와서 기존에 state 값을 다 넣은 후에 새로운 todo를 넣은 배열을 반환했다.  
더 풀어서 쓴다면 기존의 state가 [{text: "코딩하기", id:1}, {text: "카페가기", id:2}] 였다면  
새로운 todo인 {text: "잠자기", id:3} 추가할 때 이전의 state를 spread 연산자를 통해서 나열하고 새로운 todo를 추가해서 새로운 배열을 반환했다.  
왜냐하면 리덕스를 이전 값을 참조해야하기 때문에 새로운 배열을 반환해야 하기 때문이다.

하지만 **createReducer에는 새로운 값을 반환하려면 return을 사용해주고, 기존 배열을 변형하려면 return 없이 수행**하면 알아서 해준다.

```js
const todosReducer = createReducer([], {
  [addTodo]: (state, action) => {
    //push는 기존의 배열에 todo를 추가한다.
    // 즉 배열을 mutate하지만 사용가능하다.
    const todo = action.payload
    state.push(todo)
  },
  (...)
})
```

새로운 state를 반환하지 않고 **기존의 state를 변형한다면(return 없이)immer를 통해서 알아서 처리해주기 때문에 가능**하다.

## configureStore

configureStore를 사용하면 아주 간단하게 기본세팅을 도와준다.

```js
import { configureStore } from "@reduxjs/toolkit";

import rootReducer from "./reducers";

const store = configureStore({ reducer: rootReducer });
```

기본적으로 리듀서만 전달하면 Redux의 DevTools Extension이 켜지고, redux-thunk가 추가된다.  
사용할 미들웨어를 지정할수도 있는데 middleware의 값을 배열로 전달하면 된다.

```js
const store = configureStore({
  reducer: rootReducer,
  middleware: [thunk, logger],
});
```

해당 배열에 있는 미들웨어만 적용이 된다.  
기본으로 제공하는 미들웨어에 logger를 추가하는 방법은 **getDefaultMiddleware**를 사용하자.

```js
const store = configureStore({
  reducer: rootReducer,
  middleware: [...getDefaultMiddleware(), logger],
});
```

getDefaultMiddleware를 통해서 기본적인 것은 가져오면서 미들웨어를 추가할 수 있다.

자주 사용할 것 같은 configureStore, createReducer, createAction을 정리해봤다.
