---
title: "자바스크립트에서 거짓으로 판단되는 값"
excerpt: "자바스크립트에서 거짓으로 판단되는 값인 falsy들을 알아보자."
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

## 문제 6

```js
// 다음은 자바스크립트 문법 중에서 False로 취급하는 것들 입니다.
// 앗, False로 취급하지 않는 것이 하나 있네요! True를 찾아주세요.

// 1) NaN
// 2) 1
// 3) ""
// 4) 0
// 5) undefined
```

## 자바스크립트의 falsy

자바스크립트에서는 모든 값들이 참과 거짓으로 판단될 수 있다.

```js
if (true) console.log("true!");
if (1) console.log("also true!");
```

위와 같이 true나 false가 아닌데, **true나 false로 판단되는 것들을 truthy와 falsy**라고 한다.

```js
if (null) {
  console.log("null");
} else if (undefined) {
  console.log("undefined");
} else if (0) {
  console.log("0");
} else if (0) {
  console.log("");
} else {
  console.log("Bye");
}
```

위의 조건문은 "Bye"가 실행이 된다.  
왜냐하면 null, undefined, 0, ""(빈문자열) NaN은 false로 계산되기 때문이다.

## 자바스크립트의 truthy

거창하게 제목을 하나 더 팠지만, truthy는 간단하다. falsy가 아닌 것들은 다 true가 된다.  
**빈 객체와 빈 배열도 true로 판단되는 것을 잊지말자**

## 풀이 1

그래서 정답은 2번인 1이다.
