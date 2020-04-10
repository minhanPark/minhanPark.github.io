---
title: "자바스크립트에서 대문자 소문자로 바꾸기"
excerpt: "자바스크립트에서 나누기를 해보자"
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

## 문제 24

```js
// 이름이 입력되면 전부 대문자로 출력되는 프로그램을 만들어주세요.

// 입출력
// 입력 : mary
// 출력 : MARY
```

## 풀이 1

문자열을 대문자로 바꿔주는 함수는 str.toUpperCase()가 있다.

```js
"RunningWater".toUpperCase();
// "RUNNINGWATER"
```

그리고 소문자로 바꿔주는 함수는 toLowerCase가 있다.

```js
"RUNNINGWATER".toLowerCase();
// runningwater
```

해당 함수를 이용하면 문제를 풀 수 있다.

```js
let name = prompt("이름을 입력하세요.");
console.log(name.toUpperCase());
```
