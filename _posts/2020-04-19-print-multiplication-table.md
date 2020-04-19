---
title: "문제 36 : 구구단 출력하기"
excerpt: "구구단 결과를 한줄에 출력해보자."
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

## 문제 36 : 구구단 출력하기

```js
// 1~9까지의 숫자중 하나를 입력하면 그 단의 구구단 결과를 한 줄에 출력해라.

// 입출력
// 입력 : 2
// 출력 : 2 4 6 8 10 12 14 16 18
```

## 풀이 1

```js
let str = "";
const num = prompt("숫자를 입력하세요.");
for (let i = 1; i < 10; i++) {
  str += String(num * i) + " ";
}

console.log(str);
```
