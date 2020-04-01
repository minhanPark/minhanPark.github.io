---
title: "객체의 키 이름이 중복될 때"
excerpt: "객체의 키 이름이 중복된다면 자바스크립트는 어떤 값을 보여줄까 확인해보기"
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

## 문제 8

```js
// 자바스크립트 객체를 다음과 같이 만들었다.
// 출력값을 입력하시오.(출력값은 공백을 넣지 않습니다.)

var d = {
  height: 180,
  weight: 78,
  weight: 84,
  temperature: 36,
  eyesight: 1
};

console.log(d["weight"]);
```

## 풀이 1

키 내부에서 중복되는 이름이 있을 경우 자바스크립트는 맨 뒤에 적은 것을 값으로 연결해준다.  
즉 순서상 **정답은 84**이다.
