---
title: "자바스크립트의 깊은 복사와 얕은 복사"
excerpt: "깊은복사와 얕은 복사의 차이점을 알아보자"
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - javascript
---

## 참조데이터의 특성을 다시 보자

참조데이터는 값이 아니라 주소를 전달한다고 이전 포스팅에서 다루었다.

```js
let o = { name: "runningwater", like: "coffee" };
let o2 = o;
o2.like = "water";

console.log(o, o2); // {name: "runningwater", like: "water"} {name: "runningwater", like: "water"}
```

위의 예시를 통해서 설명했었는데 다시 얘기하면 값자체가 복사되는게 아니라 주소가 복사되기 때문에 위와 같은 경우엔  
o와 o2가 둘다 바뀌게 된다.

**위의 특성이 깊은 복사와 얕은 복사의 차이점을 만든다.**

## 깊은 복사와 얕은 복사의 차이점

우선 복사라는 말은 똑같은 값을 만든다는 말이다. 간단히 말해서 **얕은 복사는 한단계 아래의 값만 복사하는 것이고, 깊은 복사는 내부의 모든 값들을 복사**하는 것이다.

```js
const arr = {
  a: 1,
  b: {
    c: null,
    d: "runningwater",
    e: [1, 2, 3]
  }
};
```

arr이라는 객체를 복사한다고 생각해보자. 우선 가장 쉬운 복사방법은 나머지 연산자를 활용하는 것이다.

```js
const arr2 = { ...arr };
// arr2 = {
//  a: 1,
//  b: {
//    c: null,
//    d: "runningwater",
//    e: [1, 2, 3]
//   }
// };
```

만약 arr2의 b값의 d를 anonymous로 바꾼다면 어떻게 될까?

```js
arr2.b.d = "anonymous";
// "anonymous"

console.log(arr.b.d, arr2.b.d);
// "anonymous" "anonymous"
```

arr2의 b.d를 바꿨는데 arr의 b.d도 바뀐 것을 확인할 수 있다.  
이게 **바로 아래 단계의 값만 복사하는 얕은 복사**이다. 참조형 데이터가 포함되어 있으면 그 주솟값만을 복사하기 때문에 위의 예시처럼 원본(arr)의 값까지 바뀌게 된 것이다.

## 깊은 복사 해보기

```js
const copyDeep = target => {
  let result = {};
  if (typeof target === "object" && target !== null) {
    for (let prop in target) {
      result[prop] = copyDeep(target[prop]);
    }
  } else {
    result = target;
  }
  return result;
};
```

깊은 복사를 하기 위해선 값 중에 참조형 데이터가 있다면 **재귀함수**로 다시 복사를 해줘야 한다.(또는 **라이브러리를 이용**해야한다.)  
위의 코드는 깊은 복사를 실행해주는 함수이다.

```js
const arr3 = copyDeep(arr);

arr3.b.d = "RH";
// "RW"

console.log(arr.b.d, arr3.b.d);
// "anonymous" "RW"
```

## 다른 방법으로 깊은 복사해보기

객체를 JSON 문자열로 바꿨다가 다시 JSON 객체로 바꾸면 깊은 복사를 실행할 수 있다.

```js
const copyDeepJson = target => {
  return JSON.parse(JSON.stringify(target));
};
```

해당 함수는 target 객체를 JSON 문자열로 바꿨다가 다시 JSON객체로 바꾸어서 리턴한다.  
그러니 저런 코드가 등장했다면 깊은 복사를 하려고 하는구나라고 생각하자.  
**~~성능면에서는 굉장히 느리다고 한다.~~**
