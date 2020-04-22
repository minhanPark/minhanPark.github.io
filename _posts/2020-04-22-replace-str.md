---
title: "문제 39 : 오타 수정하기"
excerpt: "입력받은 문자를 다른 문자로 바꿔보자."
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

## 문제 39 : 오타 수정하기

```js
// 문장이 입력되면 모든 q를 e로 바꾸는 프로그램을 작성하라

// 입출력

// 입력 : querty
// 출력 : euerty

// 입력 : hqllo my namp is hyqwon
// 출력 : hello my name is hyewon
```

## 풀이 1

문자열에는 replace라는 메소드가 있다. 해당 메소드는 **String.replace(searchValue, newValue)** 형태로 찾을 값을 첫번째 매개변수에, 바꿀 값을 두번째 매개변수에 넣어주면 된다.

```js
"querty".replace("q", "e");
// euerty
```

하지만 일반적으로 값을 입력하면 맨 처음 만나는 값만 변경한다. 즉 밑에 문장에서는 hqllo만 hello로 바꾸고 나머지는 유지하는 것이다.

```js
const str = prompt("문장을 입력하세요.");
const changeStr = str.replace(/q/g, "e");
console.log(changeStr);
```

위의 경우처럼 첫번째 매개변수에 정규표현식을 넣으면 문장에서 모든 값을 찾아서 바꿀 수 있다.

## 풀이 2

```js
const str = prompt("문장을 입력하세요.");

function replaceAll(str, searchStr, replaceStr) {
  return str.split(searchStr).join(replaceStr);
}

console.log(replaceAll(word, "q", "e"));
```
