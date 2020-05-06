---
title: "문제 55 : 하노이의 탑"
excerpt: "하노이의 탑을 실행하는 코드를 완성하고 최소 원반 이동 횟수를 계산하라."
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

## 문제 55 : 하노이의 탑

```js
// 하노이의 탑은 다음 규칙을 만족해야한다.

// 1. 처음에 모든 원판은 A 기둥에 꽂혀 있다.
// 2. 모든 원판의 지름은 다르다.
// 3. 이 원반은 세 개의 기둥 중 하나에 반드시 꽂혀야 한다.
// 4. 작은 원반 위에 큰 원반을 놓을 수 없다.
// 5. 한 번에 하나의 원판(가장 위에 있는 원판)만을 옮길 수 있다.

const route = [];

function hanoi(num, start, end, temp) {
  //원판이 한 개일 때에는 바로 옮기면 됩니다.
  if (num === 1) {
    route.push([start, end]);
    return NaN;
  }

  //원반이 n-1개를 경유기둥으로 옮기고
  hanoi(/*내용을 채워주세요.*/);
  //가장 큰 원반은 목표기둥으로
  route.push(/*내용을 채워주세요.*/);
  //경유기둥과 시작기둥을 바꿉니다.
  hanoi(/*내용을 채워주세요.*/);
}

hanoi(3, "A", "B", "C");
console.log(route);
console.log(route.length);
```

## 풀이 1

```js
const route = [];

function hanoi(num, start, end, temp) {
  //원판이 한 개일 때에는 바로 옮기면 됩니다.
  if (num === 1) {
    route.push([start, end]);
    return NaN;
  }

  //원반이 n-1개를 경유기둥으로 옮기고
  hanoi(num - 1, start, temp, end);
  //가장 큰 원반은 목표기둥으로
  route.push([start, end]);
  //경유기둥과 시작기둥을 바꿉니다.
  hanoi(num - 1, temp, end, start);
}

hanoi(3, "A", "B", "C");
console.log(route);
console.log(route.length);
// 7
```
