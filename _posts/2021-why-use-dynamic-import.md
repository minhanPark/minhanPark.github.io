---
title: "Dynamic import 사용하기"
excerpt: "dynamic import를 왜 사용하는 지 어떻게 사용하는 지 알아보자"
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

## import의 기본 사용 예제

import문은 다른 모듈에서 내보낸 바인딩을 가져올 때 사용한다.  
일단 아래에서 모듈을 내보내는 예제를 살펴보자.

```js
//기본값으로 내보내기 - default
export default (a, b) => {
  return a + b;
};

//이름을 가지고 내보내기 - named
export const sum = (a, b) => {
  return a + b;
};
```

내보내는 방식에는 두 가지가 있다.  
**이름을 가지고 내보내기**와 **기본값으로 내보내기**다.

내보내는 방식에 따라서 import할 때에도 다르게 사용한다.

```js
//기본값으로 내보내기 - default
import defaultSum from "모듈";

// 이름을 가지고 내보내기 - named
import { sum } from "모듈";
```

기본적으로 파일 상단에 import 형식으로 모듈을 많이 불러오는데 해당 방식으로 불러오는 방법을 **정적(static) import** 라고 한다.

## 동적 import를 사용하는 이유

기본적으로는 정적 import를 사용하는데 왜, 어떤 경우에 동적 import를 사용하게 될까?

> 조건에 따라서 모듈을 불러와야하는 상황에 동적 import를 사용하게 된다.

```js
import {sum} from "모듈";

(...)
const temp = sum(1, 2);
(...)
```

정적 import를 쓴다면 해당 모듈을 메인 모듈에 포함시키게 되고 불필요한 자원 낭비를 일으킨다.

## 동적 import 사용 예제

일반적인 함수 형태처럼 import("path")로 사용하면 된다.

```js
import(모듈)
  .then((모듈) => {
    // 모듈 가지고 할 것
  })
  .catch((e) => {
    // 모듈을 불러올 때 에러가 났을 때 할 것
  });
```

> 프로미스(promise)를 리턴한다는 것을 잊지말자

### 즉시 실행함수랑 함께 사용하기

프로미스를 반환하기 때문에 async/await와 함께 사용하면 더 효과적이다.

```js
(async function () {
  const sumModule = await import("모듈");
})();
```

await를 통해서 반환하는 객체값을 변수에 담을 수 있다.

### 구조분해

위의 예제에서 sumModule에는 어떤값이 들어있을까?  
export에는 두가지 방식이 있다고 설명했듯이(named, default) 내보내는 방식에 따라서 들어가는 값이 다르다.  
만약에 named의 경우엔 해당 named로, default를 선언한 경우엔 default 값으로 들어가 있을 것이다.

즉 구조분해를 해본다면 아래와 같다.

```js
(async function () {
  const {
    default: sumDefault, //default
    sum, // named
  } = await import("모듈");
})();
```

중요한 것은 default 부분이다.

> 자바스크립트에서 default는 예약어이기 때문에 그냥 사용하면 에러가 날 것이다. 그래서 변수명을 변경해줘야 사용가능하다.
