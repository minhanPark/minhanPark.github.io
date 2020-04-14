---
title: "대문자 확인하기"
excerpt: "입력된 문자열이 대문자인지 아닌지를 확인해보자."
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

## 문제 29

```js
// 알파벳을 하나만을 입력하고 그 알파벳이 대문자이면 YES를 아니면 NO를 출력하는 프로그램을 만들자.
```

## 풀이 1

```js
const Big = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

const alphabet = prompt("알파벳을 입력하세요.");
if (Big.includes(alphabet)) {
  console.log("YES");
} else {
  console.log("NO");
}
```

배열이나 문자열은 **includes 메소드**를 활용하면 값이 포함되어 있는 지 없는 지 확인하기가 쉽다.

## 풀이 2

```js
const alphabet = prompt("알파벳을 입력하세요.");

if (alphabet === alphabet.toUpperCase()) {
  console.log("YES");
} else {
  console.log("NO");
}
```
