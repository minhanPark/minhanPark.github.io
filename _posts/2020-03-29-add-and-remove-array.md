---
title: "자바스크립트에서 배열의 아이템을 추가하거나 삭제해보자."
excerpt: "자바스크립트의 push, unshift, pop, shift를 활용해보자."
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

## 자바스크립트 100제를 발견하다.

[자바스크립트 100제](https://www.notion.so/JS-100-94d97d294dd14c9b911a02c840fa9f2d)  
무료로 자바스크립트 100제와 해설을 제공해주는 자료를 찾았다.  
해당 문제들을 풀어가며 나중에는 좀 더 고급 알고리즘에 대해서 배우고 포스팅해보고 싶다.  
무언가 코드를 작성하면서 느낀건 좀 더 좋은 코드를 작성하기 위해서는 더 기준을 세우고 생각해야 한다는 것이다.  
그 기준이 알고리즘이라고 생각한다.

## 문제 1

```js
// 다음 배열에서 400, 500을 삭제하는 code를 입력하세요.

var nums = [100, 200, 300, 400, 500];
```

## 배열의 추가 및 삭제 메소드 알아보기

배열에는 아이템을 추가하고 삭제하는 방법이 있다.  
추가에는 push와 unshift, 제거에는 pop과 shift이다.

```js
var arr = [100, 200, 300];
arr.push(400); // [100, 200, 300, 400]
```

push는 맨뒤에 추가한다.

```js
var arr = [100, 200, 300];
arr.unshift(0); // [0, 100, 200, 300]
```

unshift는 맨 앞에 추가한다.

```js
var arr = [100, 200, 300];
arr.pop(); // [100, 200]
```

pop은 배열의 맨 뒤 아이템을 제거한다.

```js
var arr = [100, 200, 300];
arr.shift(); // [200, 300]
```

shift는 배열의 맨 앞 아이템을 제거한다.

즉 해당 메소드를 이용하면 배열에서 아이템을 삭제하거나 추가할 수 있다.

## 풀이 1

```js
var nums = [100, 200, 300, 400, 500];
nums.pop(); // [100, 200, 300, 400]
nums.pop(); // [100, 200, 300]
```

## 특정값이 있는 지 확인하고 삭제해보기

400과 500은 뒤에 있었기 때문에 pop을 통해서 삭제할 수 있었다. 그렇다면 특정 값을 삭제하려면 어떻게 해야할까?

```js
let newNums;

if (nums.includes(400)) {
  // 배열에 400이 들어있다면 true 아니면 false
  const index = nums.indexOf(400); //400의 인덱스를 찾는다
  newNums = [...nums.slice(0, index), ...nums.slice(index + 1)]; // index를 기준으로 number를 제거하고 새배열을 만든다.
}

console.log(newNums);
```

특정값이 있는 지 확인한 후에 삭제하려면 위의 코드를 참고해서 작성하면 된다.
