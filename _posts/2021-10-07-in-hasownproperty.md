---
title: "자바스크립트에서 객체의 속성을 확인하는 in과 hasOwnProperty를 알아보자"
excerpt: "자바스크립트에서 객체의 속성을 확인할 수 있는 in 연산자와 객체의 hasOwnProperty 메소드를 알아보고 활용해보자"
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

## 객체에서 특정 프로퍼티(속성)을 확인하는 방법

자바스크립트 객체에서 특정 속성이 존재하는 지 확인해야할 때가 있다.  
아래와 같은 상황을 생각해보자.

```js
const human = {
  name: "Runningwater",
  job: "unemployed",
  age: "30",
};
```

위와 같은 human 객체가 있다. 여기에서 hobby 프로퍼티가 있으면 특정 로직을 실행할 때 아래와 같이 할 수 있다.

```js
if ("hobby" in human) {
  // hoby 프로퍼티가 있을 때 실행할 로직
} else {
  // hoby 프로퍼티가 없을 때 실행할 로직
}
```

이처럼 in 연산자를 통해서 객체에 값이 존재하는지에 따라서 true나 false를 반환받을 수 있다.

> **"속성" in 객체명**  
> 객체에 값이 존재하면 true 존재하지 않으면 false를 반환한다.

in 연산자와 같은 기능을 하는 것으로 hasOwnProperty 메소드가 있다.

```js
if (human.hasOwnProperty("속성명")) {
  // hoby 프로퍼티가 있을 때 실행할 로직
} else {
  // hoby 프로퍼티가 없을 때 실행할 로직
}
```

hasOwnProperty 메소드도 불리언 값을 반환한다.

> **객체명.hasOwnProperty("속성")**  
> 마찬가지로 객체에 값이 존재하면 true 존재하지 않으면 false를 반환한다.

## 둘의 차이점 및 중요한 점

둘의 차이점 및 특정 상황에 따라 조심해야 할 점은 **in 연산자는 프로토타입 체인을 확인한다는 점**이다.  
코드를 통해서 알아보자.

```js
"hasOwnProperty" in human;
// true
```

위에는 human 객체에서 hasOwnProperty 속성을 찾고 있다. 그리고 결과값은 true다.  
이는 human 객체는 프로토타입 체인에 기본 객체들이 가지고 있는 속성 및 메소드들을 가지고 있고, **in 연산자는 해당 프로토타입 체인까지 다 확인하고 true나 false를 반환**하기 때문에 가지게되는 결과다.

> 직관적으로 보기에는 in 연산자가 깔끔하고 쉬워보이지만 객체 자신이 가지는 속성을 확인할 때는 hasOwnProperty를 쓰는 것이 좋다.
