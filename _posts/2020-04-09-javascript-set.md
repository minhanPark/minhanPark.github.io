---
title: "자바스크립트에서 Set 활용하기"
excerpt: "자바스크립트에서 Set을 활용해보자."
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

## 문제 21

```js
// 다음 중 set을 만드는 방법으로 올바른 것을 모두 고르시오.

// 1) var x = {1, 2, 3, 5, 6, 7};
// 2) var x = {};
// 3) var x = new Set('javascript');
// 4) var x = new Set(range(5));
// 5) var x = new Set();
```

## 자바스크립트의 Set

Set은 중복이 허용되지 않는 객체이다. 안에 이미 같은 값이 존재한다면 추가되지 않는다. 그래서 중복값을 허용하지 않아야 한다면  
Set을 이용하면 된다.

## Set 생성방법

```js
let x = new Set();
```

위와 같은 방식으로 Set을 만든다. 값은 배열값이 들어가면 된다.

```js
let x = new Set([1, 2, 3, 4, 5]);
//Set(5) {1, 2, 3, 4, 5}

let y = new Set("러닝워터");
//Set(4) {"러", "닝", "워", "터"}
```

## 활용 메소드

값을 추가하고, 삭제하고, 가지고 있는지 확인하고 등의 메소드를 활용할 수 있다.

```js
let x = new Set([1, 2, 3, 4, 5]);
//Set(5) {1, 2, 3, 4, 5}
x.add(6);
//Set(6) {1, 2, 3, 4, 5, 6}

x.add(6); // 값이 존재하기 때문에 추가 안됨
//Set(6) {1, 2, 3, 4, 5, 6}

x.delete(1);
//Set(5) {2, 3, 4, 5, 6}

x.has(2);
//true

x.clear();
//Set(0) {}

x.size;
// 0
```

add는 중복된 값이 없을 경우에 값을 추가한다.  
delete는 값을 삭제한다.  
has는 값이 존재하는 지 확인해준다.  
clear는 Set 내부의 모든 값을 삭제한다.  
size는 값의 수를 확인해준다(length역할)

## 풀이 1

기본적으로 range라는 함수는 자바스크립트 내부에서는 없기 때문에 정답은 3, 5번이다.
