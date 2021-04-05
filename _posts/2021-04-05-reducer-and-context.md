---
title: "useReducer와 useContext를 사용해서 전역 상태 관리를 해보자."
excerpt: "next app에 useReducer와 useContext를 사용해서 전역으로 상태 관리를 해보자"
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - React
  - Next
  - React Hooks
---

## useReducer와 useContext를 같이 사용해보자.

왜 둘을 같이 사용해야할까?

> 전역 상태 값을 좀 더 세분화해서 다룰 수 있기 때문이다.

useReducer만으로도 충분한 경우가 있지만 애플리케이션이 커지다 보면 좀 더 세분화해서 다뤄야하는 경우가 이럴 때 useReducer와 useContext를 같이 사용하면 더 편하게 관리할 수 있다.

## 프로젝트 설정

root 디렉토리에 reducer 폴더를 만들어보자. 그리고 그 밑에 index.js 파일과 action.js 파일을 만들어보자.

```js
// reducer/action.js

export const PLUS = "add";

export const MINUS = "minus";
```

```js
// reducer/index.js

import { PLUS, MINUS } from "./action";

export const initialState = {
  num: 0,
};

const reducer = (state, action) => {
  switch (action.type) {
    case PLUS:
      return {
        num: state.num + 1,
      };
    case MINUS:
      return {
        num: state.num - 1,
      };
    default:
      return;
  }
};

export default reducer;
```

**action.js에서는 액션을 정의하고 index.js에서는 초기값과 리듀서를 정의**했다.  
만약 useReducer만을 사용하면 어떻게 될까?

```js
import React, { useReducer } from "react;
import reducer, {initialState} from "./reducer;

const Counter = () => {
    const [ state, dispatch ] = useReducer(reducer, initialState);
    (...)
}
```

위와 같이 useReducer 훅을 이용해서 reducer와 initialState를 전달해서 state와 dispatch를 갖고 올 수 있다. 위와 같은 경우는 state에는 num이 있고, dispatch에는 두가지 타입(PLUS, MINUS)가 있다.

만약 여러 곳에서 사용한다면 계속 저렇게 불러와야할까?  
단순히 num만 있는 것이 아니라 여러 객체가 있다면, 그 객체들을 계속 불러와야할까?  
**Context를 통해서 상황을 더 깔끔하게 분리할 수 있다.**

## context 추가하기

```js
// context.js

import React, { createContext, useReducer, useContext } from "react";
import reducer, { initialState } from "./reducer";

const Context = createContext();

const ContextProvider = ({ children }) => {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <Context.Provider value={{ state, dispatch }}>{children}</Context.Provider>
  );
};

export const useDispatch = () => {
  const { dispatch } = useContext(Context);
  return dispatch;
};

export const useGlobalState = () => {
  const { state } = useContext(Context);
  return state;
};

export default ContextProvider;
```

**컨텍스트를 만들어서 value에 state와 dispatch를 전달**했다.  
그리고 그 밑에 dispatch 부분과 state 부분을 나누었다.  
만약 상황별로 state를 더 나누고 싶다면 useGlobalState 부분을 더 자세히 나눠서 사용하면 된다.

마지막으로 next app의 경우엔 \_app.jsx에 컴포넌트를 감싸주면 된다.

```js
// _app.jsx

function MyApp({ Component, pageProps }) {
  return (
    <ContextProvider>
      <Component {...pageProps} />
    </ContextProvider>
  );
}
```

이제 간단히 사용할 수 있다.

```js
const Test = () => {
    const globalState = useGlobalState();
    const dispatch = useDispatch();
    (...)
}
```

위와 같이 state만 필요할 땐 state만 갖고오고, dispatch가 필요할 땐 dispatch만 들고오면 더 깔끔하게 사용할 수 있다.
