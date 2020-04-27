---
title: "문제 50 : 버블정렬 구현하기"
excerpt: "버블정렬을 구현해보자."
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

## 문제 50 : 버블정렬 구현하기

```js
// 버블정렬은 두 인접한 원소를 검사하여 정렬하는 방법이다.
// 버블정렬을 완성해보자.

function bubble(arr) {
  let result = arr.slice();

  for (let i = 0; i < result.length - 1; i++) {
    for (/*빈칸을 채워주세요.*/) {
      if (result[j] > result[j + 1]) {
         //빈칸을 채워주세요.
      }
    }
  }
  return result;
}

const items = prompt('입력해주세요.').split(' ').map((n) => {
  return parseInt(n, 10);
});

console.log(bubble(items));
```

## 풀이 1

```js
function bubble(arr) {
  let result = arr.slice(); // 들어온 배열을 복사한다.

  for (let i = 0; i < result.length - 1; i++) {
    for (let j = i + 1; j < result.length; j++) {
      if (result[j] > result[j + 1]) {
        //빈칸을 채워주세요.
      }
    }
  }
  return result;
}

const items = prompt("입력해주세요.")
  .split(" ")
  .map((n) => {
    return parseInt(n, 10);
  });

console.log(bubble(items));
```
