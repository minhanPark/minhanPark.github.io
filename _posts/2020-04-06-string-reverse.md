---
title: "문자열 거꾸로 만들기"
excerpt: "입력받은 문자열을 거꾸로 만들어서 반환해보자."
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

## 문제 16

```js
// 문장이 입력되면 거꾸로 출력하는 프로그램을 만들어봅시다.

// 입력 : 거꾸로
// 출력 : 로꾸꺼
```

## 풀이 1

array의 메소드 중에 reverse가 있다. 배열 요소들을 반대로 뒤집어주는 역할을 하는데, 해당 메소드를 활용하면 쉽게 뒤집을 수 있다.  
즉 문자열을 받아서 split를 사용하면 각 글자가 원소로 들어가는 배열이 만들어지고 그것을 reverse를 이용해 뒤집는다.  
그리고 다시 join을 통해서 문자열로 만들어 반환하면 된다.

```js
const p = prompt("문장을 입력하세요."); // 거꾸로

const splitedP = p.split(""); // ["거", "꾸", "로"]
const reversed = splitedP.reverse(); // ["로", "꾸", "꺼"]
console.log(reversed.join("")); // "로꾸꺼"
```
