---
title: "리액트의 setState 함수를 알아보자"
excerpt: "클래스 컴포넌트에서 state 값을 바꾸는 setState 함수를 알아보자."
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

## setState의 역할

리액트는 함수형 컴포넌트와 클래스형 컴포넌트가 있습니다.  
이제는 hooks의 사용으로 인해서 클래스형 컴포넌트가 많이 사용되지는 않지만 그럼에도 불구하고 클래스형 컴포넌트를 사용해야할 순간이 있을까  
생각하면서 해당 포스팅을 작성하게 되었습니다.

가장 기본적으로 setState는 state의 값을 변경할 때 사용하는 함수입니다.

```js
this.state.item = this.state.item + 1;
```

위와 같은 형태처럼 리액트에서 직접 값을 바꾸지 말고 바뀐 값을 파악해서 render하는 등 리액트의 이점을 활용하려면 반드시 setState나 hooks의 세터 함수를 통해서  
값을 바꾸라고 합니다.

## 사용방법

```js
class App extends Component {
  state = {
    item: 0
  };
  render() {
    const { item } = this.state;
    return (
      <div>
        <h2>{item}</h2>
        <button onClick={this.handleBtn}>add</button>
      </div>
    );
  }
  handleBtn = () => {
    const { item } = this.state;
    this.setState({
      item: item + 1
    });
  };
}
```

해당 컴포넌트는 item의 개수와 그 개수를 1씩 더하는 버튼이 있습니다.  
위의 경우처럼 setState에 객체를 전달하면 거기에 맞게 state의 값이 변경됩니다.

> setState() does not always immediately update the component. It may batch or defer the update until later.

하지만 해당 형태의 문제는 리액트 docs에 나와있듯이 **setState는 지연되거나 병합될 수 있다는 것**입니다.

```js
handleBtn = () => {
  const { item } = this.state;
  this.setState({
    item: item + 1
  });
  this.setState({
    item: item + 1
  });
};
```

handleBtn 함수가 해당 형태처럼 변경된다면 item을 2씩 더해갈까요 ?  
위의 같은 경우는 병합되기 때문에 1씩 더해집니다.

## 함수 전달하기

객체가 아니라 함수를 전달할 수도 있습니다.

```js
handleBtn = () => {
  this.setState((prevState, props) => {
    return {
      item: prevState.item + 1
    };
  });
  this.setState((prevState, props) => {
    return {
      item: prevState.item + 1
    };
  });
};
```

함수를 전달할 때는 첫번째 인수엔 이전의 state를, 두번째 인수엔 props를 가져옵니다. 그래서 함수를 이용할 경우엔 병합되지 않고  
2씩 더해지는 것을 확인할 수 있습니다.(props를 사용하지 않을 경우는 비워두시면 됩니다)

## 콜백함수 전달하기

setState에는 두번째 매개변수로 콜백함수를 선택적으로 전달할 수 있습니다.

> The second parameter to setState() is an optional callback function that will be executed once setState is completed and the component is re-rendered

이 콜백함수는 setState과 완료되고, 컴포넌트가 다시 렌더되면 실행되는 함수 입니다.

```js
handleBtn = () => {
  this.setState(
    (prevState, props) => {
      return {
        item: prevState.item + 1
      };
    },
    () => console.log("add 1")
  );
  this.setState(
    (prevState, props) => {
      return {
        item: prevState.item + 1
      };
    },
    () => console.log("and add 1")
  );
};
```

위의 예시를 이용하면 해당 형태가 된다고 할 수 있습니다.  
대부분의 기능을 리액트 hooks에서 구현가능하다면 클래스 컴포넌트를 사용하지 않게될까요?  
useState의 세터함수는 두번째 콜백을 받지 않습니다.  
**즉 컴포넌트가 복잡해질 가능성이 있는 한 클래스 컴포넌트는 계속 중요할 것**이라는 말이죠.
