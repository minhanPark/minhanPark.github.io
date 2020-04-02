---
title: "자바스크립트 문자열 합치기"
excerpt: "자바스크립트에서 주어진 문자열을 합쳐보자."
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

## 문제 9

```js
// 다음 소스코드를 완성하여 날짜와 시간을 출력하시오.

//데이터
var year = "2019";
var month = "04";
var day = "26";
var hour = "11";
var minute = "34";
var second = "27";

var result = console.log(result); // 빈칸을 채워주세요.

// 출력
// 2019/04/26 11:34:27
```

## 템플릿 문자열

백틱을 이용해서 템플릿 리터럴을 이용하면 쉽게 문자열을 합칠 수 있다.  
tab키와 숫자 1 키 사이에 있는 것이 백틱이다.

```js
`${여기에 표현식을 쓸 수 있다.}`
```

위의 예시와 같이 백틱 사이에 달러와 중괄호를 이용하면 표현식을 쓸 수 있다.

## 풀이 1

```js
var result = `${year}/${month}/${day} ${hour}:${minute}:${second}`;
// 출력
// 2019/04/26 11:34:27
```

## concat 메소드

배열이나 문자를 합칠 때 사용할 수 있는 concat메소드가 있다.

```js
var arr1 = [1, 2, 3];
var arr2 = [4, 5, 6];

arr1.concat(arr2); // [1, 2, 3, 4, 5, 6]

var str1 = "a";
var str2 = "b";

str1.concat("/", str2); // "a/b"
```

위의 문제도 concat을 활용하면 쉽게 풀 수 있다.

## 풀이 2

```js
var result = year.concat(
  "/",
  month,
  "/",
  day,
  " ",
  hour,
  ":",
  minute,
  ":",
  second
);
// 출력
// 2019/04/26 11:34:27
```
