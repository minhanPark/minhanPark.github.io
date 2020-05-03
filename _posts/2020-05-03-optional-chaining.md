---
title: "자바스크립트의 optional chaining"
excerpt: "자바스크립트의 optional chaining을 알아보자."
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

## optional chaining이란?

"?."형태로 체인의 각 **참조가 유효한지 명시적으로 검증하지 않고, 연결된 객체 체인 내에 깊숙이 위치한 속성값을 읽는 것**이다.  
만약에 **참조가 null 또는 undefined라면 에러대신 undefined를 반환**한다.

이게 무슨 말일까?

```js
const animal = {
  cat: {
    name: "navi",
  },
};

console.log(animal.dog.name);
// Uncaught TypeError: Cannot read property 'name' of undefined
```

위에 코드는 에러가 난다. 왜일까?  
animal에 dog는 존재하지 않는다. 그래서 animal.dog는 undefined이다.  
그런데 undefined에서 name 값을 찾으려고 했으니 위와 같은 에러가 나오는 것이다.  
그렇다면 이렇게 깊이 중첩된 속성을 찾으려면 어떻게 해야할까?

```js
console.log(animal.dog && animal.dog.name);
// undefined
```

우선 name에 접근하기 전에 dog가 존재하는 지 부터 확인한다.  
위와 같은 경우 dog가 존재하지 않기 때문에 name을 확인하지 않는다. 그래서 에러대신 undefined가 출력된다.

## optional chaining을 활용해보기

위와 같은 경우처럼 값이 존재하는 지 부터 확인하기 전에 optional chaining을 활용할 수 있다.

```js
console.log(animal.dog?.name);
// undefined
```

"?."형태로 name을 참조하면 dog값이 존재하지 않을 때 에러가 발생되는 것이 아니라 dog의 값인 undefined를 반환한다.
