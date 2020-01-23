---
title: "자바스크립트의 래퍼객체를 활용해보자"
excerpt: "래퍼객체를 활용해보고 값도 바꿔보자"
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

다른 프로그래밍 언어와 마찬가지로 자바스크립트도 여러 자료형으로 구분되어서져 있다.  
그 중 큰 범위로 두개를 나눈다면 원시타입과 참조타입이라고 할 수 있다.  
이번 포스팅에서 원시타입의 래퍼 객체에 대해서 보고 약간의 활용을 정리해보려고 한다.

## 원시타입이 메소드를 가지고 있다.

```js
let str = "runningwater";

console.log(typeof str);
// "string"

console.log(str.slice(0));
// "runningwater"
```

str의 타입은 문자열이다.  
하지만 메소드를 가지고 있는 이유는 해당 메소드를 실행할 때 그 원시타입에 맞는 래퍼 객체가 생성되서 감싼다음  
메소드를 실행하기 때문이다.  
**number도 마찬가지이다.**

## 문자, 숫자, 불린, 심볼로 바꿔보기

또 하나의 특징은 Number, String, Boolean, Symbol을 통해서 값을 바꿀 수 있다는 것이다.

```js
const strNum = String(1);
typeof strNum;
// "string"
```

해당 경우처럼 String()에 값을 넣으면 모두 문자열로 만들어준다. 숫자를 바꿀 땐 Number, true나 false로 바꿀땐 Boolean을 이용하면 된다.
