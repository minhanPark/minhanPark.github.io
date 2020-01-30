---
title: "리액트 프로젝트에서 componentDidCatch를 통해 에러를 처리해보자"
excerpt: "componentDidCatch를 통해서 에러가 발생했을 때 빈 화면이 아니라 에러 컴포넌트를 보여주자."
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

부모 컴포넌트에서 에러가 발생한다면 그 안의 자식 컴포넌트 들은 다 렌더링 되지 못합니다.  
그런 형태로 전체 앱을 중단하는 상황이 발생합니다. componentDidCatch를 통해서 해당 경우에  
어떻게 fallback ui를 보여줄 수 있을 지 정리해보겠습니다.

## 컴포넌트에서 에러 발생 시 모습

리액트 앱에서 에러가 발생하면 어떻게 될까요?

![에러 발생 모습](https://drive.google.com/uc?id=1RHQHd9g1Xhh6xJ2xPlgmtlVWSyXTF1Vo)  
해당 이미지처럼 에러가 발생한 모습을 보여줍니다. 해당 모습은 개발 환경에서의 모습이고 오른쪽 상단의 "x"를 누른다면 빈 화면이 보이게 됩니다. 이게 사용자들이 보게될 화면입니다.

> A JavaScript error in a part of the UI shouldn’t break the whole app. To solve this problem for React users, React 16 introduces a new concept of an “error boundary”.

docs의 설명에서도 나와있듯이 UI 부분의 에러가 전체 앱을 중단해서는 안되기 때문에 해당 경우를 해결하기 위해서 componentDidCatch가 추가되었습니다.

## 에러 컴포넌트 만들기

```js
//ErrorComponent.js

import React, { Component } from "react";

class ErrorComponent extends Component {
  state = {
    error: false
  };

  componentDidCatch(error, info) {
    this.setState({
      error: true
    });
    console.log("error is", error);
    console.log("error info is", info);
  }

  render() {
    const { error } = this.state;
    if (error) {
      return <div>Something Wrong :(</div>;
    }
    return this.props.children;
  }
}

export default ErrorComponent;
```

해당 컴포넌트는 componentDidCatch를 통해서 자식컴포넌트 트리에 있는 에러와 에러 로그들을 보여주고, 컴포넌트가 전체가 무너지지 않게 fallback UI를 보여주게 됩니다.  
코드로 좀 더 설명을 하자면 에러가 발생하면 state에 있는 error의 값이 false에서 true로 바뀌고,  
렌더링 시에는 **에러가 있을 때는 Something Wrong이라는 fallback UI를 보여주고, 없을 시엔 그대로 컴포넌트를 보여주게 됩니다.**

## 실제 사용 해보기

에러를 발생시키는 Simple 컴포넌트를 만들어보세요.

```js
const Simple = () => {
  throw new Error("critical hit");
  return <div>Hello world</div>;
};
```

해당 컴포넌트는 실행하면 반드시 에러가 발생합니다.

```js
// App.js

class App extends Component {
  render() {
    return (
      <div>
        <ErrorComponent>
          <Simple />
        </ErrorComponent>
      </div>
    );
  }
}
```

에러를 처리하기 위해 만든 ErrorComponent로 Simple 컴포넌트를 감싸면 됩니다. 그러면 자식컴포넌트 트리의 에러를 보여주고 fallback UI를 보여주게 됩니다.  
![폴백ui적용모습](https://drive.google.com/uc?id=1_ywqcLzLH5em_opactDbO4t-7c9USPv-)  
이젠 에러가 발생해도 앱 전체가 무너지지 않고, 폴백 ui를 보여주는 것을 확인할 수 있습니다.
