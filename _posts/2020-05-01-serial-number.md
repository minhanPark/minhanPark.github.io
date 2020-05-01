---
title: "문제 54 : 연속되는 수"
excerpt: "입력되는 숫자가 연속수인 지 아닌 지를 파악하라"
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

## 문제 54 : 연속되는 수

```js
// 숫자가 연속수인지 아닌지를 파악해서 YES와 NO를 출력하라.

// 입력1
// 1 2 3 4 5

// 출력1
// YES

// 입력2
// 1 4 2 6 3

// 출력2
// NO
```

## 풀이 1

```js
const numbers = prompt("스탬프의 숫자를 입력하세요.")
  .split(" ")
  .map((n) => parseInt(n, 10))
  .sort((a, b) => a - b);
// 숫자를 오름차순으로 정렬했음
let result = true;
for (let i = 0; i < numbers.length - 1; i++) {
  if (numbers[i] + 1 !== numbers[i + 1]) {
    result = false;
  }
}
console.log(`${result ? "YES" : "NO"}`);
```
