---
title: "문제 14: 3의 배수인지 파악하기"
excerpt: "숫자를 입력받아서 해당 숫자가 3의 배수라면 짝, 아니라면 숫자를 그대로 출력해보자."
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

## 문제 14

```js
// 3의 배수라면 '짝'이라는 글자를, 3의 배수가 아니라면 n을 그대로 출력하시오.

// 입출력

// 입력: 3
// 출력: 짝

// 입력: 2
// 출력: 2
```

## 풀이 1

나머지 값을 확인하면 배수인지를 쉽게 확인할 수 있다.  
1을 3으로 나누면 나머지는 1이다. 2를 3으로 나누면 나머지는 2이다. 3을 3으로 나누면 나머지는 0이다. 4를 3으로 나누면 나머지는 1이다.  
즉 3으로 나누기 시작하면 나오는 값은 0, 1, 2가 된다.  
자바스크립트에서 **나머지 값은 "%"을 이용**하면 된다.

```js
let n = prompt("숫자를 입력하세요.");

if (n % 3 == 0) {
  console.log("짝");
} else {
  console.log(n);
}
```
