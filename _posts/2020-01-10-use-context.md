---
title: "useContext를 알아보자"
excerpt: "hooks의 useContext를 알아보고 사용해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - 자바스크립트
  - javascript
  - React
---

state값을 전달할 때 부모 컴포넌트에서 자식 컴포넌트로 값을 전달할 수 있지만 이 경우 컴포넌트가 너무 깊어지면  
전달하는데 아주 복잡한 상황이 생길 수 있다.  
가령 그 값을 사용하지 않는 경우에도 부모-자식-손자의 형태라면 부모에서 손자로 전달할 때 자식이 쓰지도 않지만 받게되는 경우를 들 수 있다.  
이때 사용할 수 있는게 리액트의 context이다.

## context 생성하기

```js
export const context = React.createContext();
```

위와 같은 형태로 context를 만들 수 있다. 해당 context는 상황에 따라서 userContext나 countContext로 이름을 지어주면 된다.

```js
const contextProvider = ({ children }) => {
  return (
    <context.Provider value={{ }}>
      {children}
    </context.Provider>
  );
```

context를 통해서 provider를 만들 수 있다.
그리고 value에 값을 전달해주면 각 컴포넌트에 연결해주면 사용할 수 있다.

```js
export const countContext = React.createContext();

const CountContextProvider = ({ children }) => {
  const [count, setCount] = useState(0);
  return (
    <CountContext.Provider value={{ count, setCount }}>
      {children}
    </CountContext.Provider>
  );
};
```

해당 형태로 useContext를 사용할 수 있다.

```js
function App() {
  return (
    <UserContextProvider>
      <Screen />
    </UserContextProvider>
  );
}
```

App.js에 이렇게 프로바이더를 제공해주거나 index.js에 제공해준 뒤

```js
const Counter = () => {
    const {count, setCount}= Context(UserContext);
    return (
        ...
    )
}
```

다시 useContext를 불러온 뒤 거기에 countContext를 전달해주면 value를 받을 수 있다.
