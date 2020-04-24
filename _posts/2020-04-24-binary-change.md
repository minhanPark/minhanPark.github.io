---
title: "문제 43 : 10진수를 2진수로"
excerpt: "사용자에게 숫자를 입력받고 이를 2진수로 바꿔서 출력해보자."
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

## 문제 43 : 10진수를 2진수로

```js
// 사용자에게 숫자를 입력받고 이를 2진수로 바꾸어 값을 출력해보자.
```

## 풀이 1

자바스크립트에서 진수 변환을 할 때는 2가지 메소드를 알면 된다.  
바로 **parseInt와 toString 이다.**  
십진수에서 다른 진수로 변환 시킬때는 toString을 사용하면 되고, 다른 진수에서 십진수로 변환시킬때는 parseInt를 사용하면 된다.  
이진수에서 팔진수로 변환시킬 때는 위의 응용으로 이진수 -> 십진수 -> 팔진수 순서로 변환시키면 된다.

```js
// 예시
const num = 1384;
num.toString(2); // "10101101000"
// 십진수 1384를 이진수로 변환

parseInt("10101101000", 2).toString(16); //"568"
// 이진수 "10101101000"를 십진수 1384로 변경하고 다시 16진수로 변환
```

그래서 문제처럼 그냥 이진수로 변환하기만 하면 되면 toString을 사용하면 된다.

```js
const num = Number(prompt("숫자를 입력해주세요."));
console.log(num.toString(2));
```
