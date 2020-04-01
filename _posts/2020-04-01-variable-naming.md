---
title: "자바스크립트 변수명으로 사용하지 못하는 것 알아보기"
excerpt: "자바스크립트에서 변수명으로 사용하지 못하는 것을 알아보자."
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

## 문제 7

```js
// 다음 중 변수명으로 사용할 수 없는 것 2개를 고르시오.

// 1) age
// 2) &age
// 3) let
// 4) _age
// 5) 1age
```

## 자바스크립트에서 변수명을 지을 때 유의할 것

**예약어**라는 개념이 있습니다. 변수를 선언할 때 사용하는 let이나 const 라던지, class와 같이 자바스크립트 내부에서 이미 사용하고 있는 것을 예약어라고 하고 해당 이름은 변수의 이름으로 사용할 수 없습니다.  
또한 **맨 앞에 숫자가 올 수 없습니다.** 항상 문자로 시작해야합니다.  
특수문자의 경우엔 **\_(언더바)와 \$(달러) 표시만 변수명으로 사용**할 수 있습니다.  
[사용가능한 변수명 확인하기](https://mothereff.in/js-variables#work)

## 풀이 1

그래서 **정답은 2번 &age와 3번 let, 5번 1age** 입니다.
