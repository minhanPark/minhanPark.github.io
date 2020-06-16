---
title: "자바스크립트의 커링을 알아보자."
excerpt: "함수를 재활용할 수 있는 자바스크립트의 커링을 알아보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - javascript
---

## 커링이란 무엇일까?

함수를 반환하는 함수이다. 간혹 본적이 있을 것이다. func()()와 같은 형태를 말이다.  
이것이 커링이다.  
**함수를 재활용 할 수 있기 때문에** 커링을 알아두면 많은 도움이 된다.

## 커링 만들어보기

```js
const add = (a, b) => a + b;

const addX = (x) => (a) => add(x, a);

const addTwo = addX(2);

addTwo(4);
//6
```

위와 같은 함수가 커링 함수라고 할 수 있다.  
더하기예제에서도 볼 수 있듯이 인자를 나누어서 받아 함수를 실행할 수 있기 때문에 함수의 재활용에 좋고, 함수들을 쉽게 유지보수 할 수 있다.

## 함수조합과 compose 함수 만들어보기

x에 2를 곱하고, 거기에 3을 곱하고 거기에 4를 더한다고 생각해보자.  
((x*2)*3)+4라고 나타낼 수 있다. 커링으로 해당 함수들을 만들어보면 어떻게 될까?

```js
const add = (a, b) => a + b;
const addX = (x) => (a) => add(x, a);
const addFour = addX(4);

const multiple = (a, b) => a * b;
const multipleX = (x) => (a) => multiple(x, a);
const multipleTwo = multipleX(2);
const multipleThree = multipleX(3);

const formula = (x) => addFour(multipleThree(multipleTwo(x)));
```

위처럼 나타낼 수 있다.  
우리는 2를 곱하고 3을 곱하고 4를 더한다고 말했지만 함수는 정확히 반대방향으로 해석해야한다.  
우리가 읽은 순서대로 함수를 조합하기 위해서 compose 함수를 만들어 볼 수 있다.

```js
function compose(){
    const funcArr = Array.prototype.slice.call(arguments);
    return funcArr.reduce(
        function(prevFunc, nextFunc){
            return funcion(x){
                return nextFunc(prevFunc(x));
            }
        },
        function(k){return k;}
    )
}

const formulaWithCompose = compose(multipleTwo, multipleThree, addFour);
//우리가 정확히 실행하고 싶은 순서대로 넣으면 된다.
```

해당 함수를 해석해보자.  
arguments는 함수의 인자값을 유사배열로 가져온다. 그래서 유사배열을 배열로 바꾸주었다.(funcArr)  
reduce는 배열의 값들을 하나로 합쳐준다. 즉 이전의 함수와 다음 함수들을 계속해서 합쳐줄 것이다.

```js
function(k){return k;}
```

초기값으로 전달해준 함수는 무슨 역할을 할까?  
바로 k를 받아서 k를 리턴하고 x를 전달해주면 즉시실행함수가 된다.

저 익명함수와 첫번째 인수로 전달한 multipleTwo 함수의 조합을 보자.

```js
function(value){
    return multipleTwo((k => k)(x));
}
```

즉 계산하는 것은 그대로 우리가 말한것의 역순이 되지만 compose 함수를 통해서 우리는 순서대로 인식할 수 있는 것이다.

## 인수가 하나가 아닌 compose 함수 만들기

```js
function compose(...funcArr) {
  return funcArr.reduce(
    function (prevFunc, nextFunc) {
      return function (...args) {
        return nextFunc(prevFunc(...arg));
      };
    },
    function (k) {
      return k;
    }
  );
}
```
