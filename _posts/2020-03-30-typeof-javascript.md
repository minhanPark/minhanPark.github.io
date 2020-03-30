---
title: "typeof로 자바스크립트 타입 체크하기"
excerpt: "typeof 메소드를 통해서 자바스크립트의 타입을 확인해보고 나올 수 있는 값을 확인해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - Javascript
  - Algorithm
---

## 문제3

```js
// 다음 출력 값으로 올바른 것은?

var arr = [100, 200, 300];
console.log(typeof arr);
```

## 자바스크립트에서 타입을 확인할 수 있는 방법

```js
var arr = [100, 200, 300];

typeof arr; // object
typeof arr; // object
```

함수형태로 typeof를 사용해도 되고 아니면 그냥 typeof를 쓰고 타입체크할 데이터를 그 뒤에 놓아도 된다.

## 자바스크립트 자료형의 타입

```js
typeof undefined; // undefined
typeof null; // object
typeof true; // boolean
typeof 3; // number
typeof "RunningWater"; // string
typeof Symbol(); // symbol
typeof function() {}; // function

// 이 외에 나머지는 object임
```

## 풀이 1

정답은 object이다.

## 문제 4

```js
// 다음 변수 a를 typeof(a)로 넣었을 때 출력될 값과의 연결이 알맞지 않은 것은?

// 1) 입력: a=1, 출력: number
// 2) 입력: a=2.22, 출력: boolean
// 3) 입력: a='p', 출력: string
// 4) 입력: a=[1,2,3], 출력: object
```

## 풀이 1

typeof로 나올 수 있는 자료형의 타입을 위에서 적어놨지만, 2.22의 경우엔 number가 나온다.  
그래서 정답은 2번이다.
