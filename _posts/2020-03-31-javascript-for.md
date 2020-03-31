---
title: "자바스크립트의 for문에 대해서 알아보자."
excerpt: "for문의 예제를 통해서 반복문을 알아보자."
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

## 문제 5

```js
// 다음 코드의 출력값으로 알맞은 것은?

var a = 10;
var b = 2;

for (var i = 1; i < 5; i += 2) {
  a += i;
}

console.log(a + b);
```

## for 문 흝어보기

for의 괄호안에는 초기값, 평가식, 반복후 표현식 들어간다.  
그리고 각 구분을 ;(세미콜론)을 통해서 하게된다.

위의 문제에서 "var i=1"이 초기값, "i<5"가 평가식, "i+=2"가 반복후 표현식이 된다.

1. i가 1이 되고, 5보다 작은지 확인한다.
2. a값(10)에 1을 더한다.
3. i += 2한다 (기존의 i값에 2를 더한다)
4. i가 5보다 작은지 확인한다.
5. a값(11)에 i값(3)을 더한다.
6. i += 2한다 (기존의 i값에 2를 더한다)
7. i가 5보다 작은지 확인한다.
8. 5보다 작지 않기 때문에 for문을 종료한다.

초기값, 평가식, 반복후 표현식을 적절히 사용하면 0부터 시작하는 것이 아니라 10부터 시작해서 0까지 진행할 수도 있고 다양한 방법이 있다.

## 스코프 주의하기

var는 함수 스코프를 가진다. 즉 저 문제에서 전역변수가 된다.

```js
var a = 10;
var b = 2;

for (var i = 1; i < 5; i += 2) {
  a += i;
}

console.log(a + b);
console.log(i); //5
```

즉 i를 콘솔에서 확인하면 5가 된다.  
즉 **이벤트를 연결한다고 하면 var를 통해서 연결하는 것이 아니라 블록으로 스코프를 생성하는 let이나 const를 사용**해야한다.

```js
let a = 10;
let b = 2;

for (let i = 1; i < 5; i += 2) {
  a += i;
}

console.log(a + b); // 16
console.log(i); // i is not defined
```

## 풀이 1

for문 흝어보기처럼 한 단계식 생각해본다면 정답은 16이 된다.
