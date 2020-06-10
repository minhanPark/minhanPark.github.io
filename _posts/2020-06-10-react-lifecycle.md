---
title: "React의 라이프 사이클을 알아보자."
excerpt: "리액트의 라이프 사이클이 실행해보자."
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

## 리액트의 라이프 사이클이란?

리액트의 클래스 컴포넌트는 **생명주기(라이프 사이클)**을 가지고 있다.  
이 생명주기마다 함수를 가지는데, 해당 함수들을 이용해서 특정 시점에 원하는 동작을 하도록 만들 수 있다.

> 1.  생성 과정
> 2.  갱신 과정
> 3.  소멸 과정

위와 같이 크게 3가지 과정이 있다.  
생성 과정은 컴포넌트 생성부터 생성 완료까지를 나타내고, 갱신 과정은 생성 완료부터 갱신 완료까지를 나타낸다. 소멸 과정은 갱신 완료부터 소멸 완료 까지를 나타낸다.

## 코드로 작성해보기

```js
import React, { Component } from "react";

class Life extends Component {
  static getDerivedStateFromProps() {
    console.log("getDerivedStateFromProps 호출");
    return {};
  }

  constructor(props) {
    super(props);
    this.state = {};
    console.log("constructor 호출");
  }

  componentDidMount() {
    console.log("componentDidMount 호출");
    // this.setState({ update: true });
  }

  componentDidUpdate() {
    console.log("componentDidUpdate 호출");
  }

  componentWillUnmount() {
    console.log("componentWillUnmount 호출");
  }

  getSnapshotBeforeUpdate() {
    console.log("getSnapshotBeforeUpdate 호출");
    return {};
  }

  shouldComponentUpdate() {
    console.log("shouldComponentUpdate 호출");
    return true;
  }
  render() {
    console.log("render 호출");
    return null;
  }
}

export default Life;
```

해당 컴포넌트를 <App />에 넣고 실행해보면 함수가 실행되는 순서를 확인할 수 있다.  
생성 과정에서는 constructor - getDerivedStateFromProps - render - componentDidMount 가 실행이 된다.

```js
componentDidMount(){
    ...
    this.setState({update: true});
}
```

위와 같이 componentDidMount 함수에 setState를 통해서 state 값을 변경시키면 갱신과정을 볼 수 있다.  
갱신 과정에서는 getDerivedStateFromProps - shouldComponentUpdate - render - getSnapshotBeforeUpdate - componentDidUpdate 가 실행이 된다.

소멸 과정은 컴포넌트가 사라질 때의 과정을 얘기한다.

```js
class App extends Component {
  constructor(props) {
    super(props);
    this.state = { hasDestroyed: false };
  }

  componentDidMount() {
    this.setState({ hasDestroyed: true });
  }

  render() {
    return (
      <div>
        <div>{this.state.hasDestroyed ? null : <Life />}</div>
      </div>
    );
  }
}

export default App;
```

위의 라이프 컴포넌트는 맨 처음에는 렌더링 되었다가 setState 안의 hasDestroyed의 값이 변경됨에 따라 사라진다.  
이때 콘솔창을 확인해보면 소멸 과정을 확인할 수 있다.

소멸 과정에서는 componentWillUnmount가 실행이 된다.
