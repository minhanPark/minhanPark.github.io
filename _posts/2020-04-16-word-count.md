---
title: "문제 32 : 문자열 만들기"
excerpt: "문자열을 입력받으면 split을 통해서 나눠보자"
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

## 문제 32 : 문자열 만들기

```js
// 문자열을 입력받으면 단어의 갯수를 출력하는 프로그램

// 입출력
// 입력 : 안녕하세요. 저는 제주대학교 컴퓨터공학전공 혜림입니다.
// 출력 : 5
```

## 풀이 1

**split 메소드는 전달한 구분자를 기준으로 나눠서 배열에 담아준다.** 그래서 split 메소드를 활용하면 쉽게 갯수를 파악할 수 있다.

```js
const str = prompt("문자열을 입력하세요.");
console.log(str.split(" ").length);
```
