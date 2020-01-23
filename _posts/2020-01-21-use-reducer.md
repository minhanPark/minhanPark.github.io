---
title: "useReducer를 알아보자"
excerpt: "hooks의 useReducer를 알아보고 사용해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - javascript
  - React
---

리듀서는 어떨 때 사용할까요?  
리액트의 경우 부모 컴포넌트에서 자식컴포넌트로 데이터가 전달됩니다.  
하지만 중간 중간 데이터를 전달만 하지 사용하지 않는 컴포넌트들이 생기게 됩니다.  
이럴 경우에 사용하는 것이 **상태관리 라이브러리**입니다.  
상태관리 라이브러리를 사용하면 전역적으로 state를 관리하고 필요한 컴포넌트에 전달할 수 있도록 할 수 있습니다.  
리액트 hooks의 useReducer를 정리하며 좀 더 알아보도록 하겠습니다.

## 액션 정의하기

우선은 액션을 정의하는게 필요합니다. 액션은 말그대로 **행동**입니다.  
ADD(더하기), MINUS(지우기), DELETE(삭제하기) 등을 생각하시면 됩니다.

```js
// reducer/actions.js

export const INCREMENT = "increment";
export const DECREMENT = "decrement";
```

액션들은 위와 같이 따로 파일을 통해 모아두셔도 되고, 아니면 reducer.js 등 리듀서함수가 정의된  
파일에 같이 넣으셔도 됩니다.  
저는 import시 너무 길이가 길어질까봐 위와 같이 따로 액션들을 정의했습니다.

## 리듀서 함수 정의하기

```js
// reducer/index.js

import { INCREMENT, DECREMENT } from "./actions.js";

export const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case INCREMENT:
      return { count: state.count + 1 };
    case DECREMENT:
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

export default reducer;
```

위와 같이 리듀서 함수를 정의할 땐 우선 초기상태값을 정하는게 좋습니다.  
예시에서는 react의 공식문서처럼 count를 정의했습니다.  
그 다음에 switch문을 통해서 액션의 타입에 따라서 어떤식으로 상태값을 바꿀 지 정해주면 됩니다.

## switch문 작성시 주의사항

switch문은 일반적으로는 if~else문과 동일합니다.  
해당 케이스에 맞게 실행시키기 때문입니다. 하지만 중요한 차이를 든다면 2가지가 있습니다.

1. 비교값은 상수가 와야 한다.
2. 같은 블록에 있으니 스코프를 주의해야한다.  
   우선 첫번째 부터 확인해볼까요?
   num이 10일 때 아래와 같은 switch문이 있다고 생각해보겠습니다.

```js
const num = 10;

switch (num) {
  case num > 10:
    console.log("num is > 10");
    break;
  case num <= 10:
    console.log("num is <= 10");
    break;
  default:
    console.log("no no");
    break;
}
```

해당 값은 nono가 나옵니다. 값 자체가 === 인지 비교를 하는 것이지, 조건에 따른 비교를 하는게 아니기 때문입니다. 그래서 원시값이 와야합니다.

두번째의 스코프를 주의해야 한다는건 무엇일까요?

```js
switch(){
    // 같은 블록
}
```

스위치 문은 같은 블록 안에서 정의됩니다. 그래서 let이나 const등 같은 블록 안에서 같은 변수명으로 정의되면 에러가 발생합니다.  
이때에는 블록을 따로 지정해주면 해결가능합니다.

```js
switch (action) {
  case "say_hello": {
    let message = "hello";
    console.log(message);
    break;
  }
  case "say_hi": {
    let message = "hi";
    console.log(message);
    break;
  }
  default: {
    console.log("Empty action received.");
    break;
  }
}
```

위의 예시는 MDN에서 가져온 예시인데 해당 예시처럼 중괄호를 쳐주면 블록이 나눠져서 똑같은 변수명이라도 스코프가 달라져 정의가 가능합니다.

## 리듀서 사용방법

```js
function App() {
  const [state, dispatch] = useReducer(reducer, initialState);
}
```

사용할때는 reducer함수와 초기 상태값을 불러와서 useReducer hooks에 넣어주시면 됩니다.  
그러면 state와 state를 변경할 수 있는 dispatch함수를 반환합니다.

```js
Count: {% raw %}{state.count}{% endraw %}
<button onClick={() => dispatch({type: DECREMENT, payload:id})}>-</button>
```

state와 dispatch를 사용할 때는 위의 예시처럼 사용하시면 됩니다.  
dispatch에서 중요한건 type말고 다른 값도 전달되어야 할때 payload(이름 상관 없음)처럼 값이나 객체를 전달해주면 리듀서 함수에서 상황에 맞게 사용할 수 있습니다.
