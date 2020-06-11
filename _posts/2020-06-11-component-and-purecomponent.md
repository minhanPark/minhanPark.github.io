---
title: "React의 component와 pure component를 알아보자."
excerpt: "리액트 클래스 컴포넌트의 두 종류인 component와 pure component를 알아보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - javascript
  - React
---

## 깊은 비교와 얕은 비교

우선 깊은 비교는 "==="이다.

```js
const obj = { name: "rw" };
const list1 = [1, 2, 3, obj];
const list2 = [1, 2, 3, obj];
const list3 = [1, 2, 3, { name: "rw" }];
```

만약 list1과 list2를 "==="을 통해서 비교하면 어떻게 될까?

```js
list1 === list2;
// false
```

왜냐하면 해당 배열의 주소를 비교하기 때문이다. list1과 list2는 각각 선언되었고, 다른 메모리 주소가 할당되었다. 그래서 둘이 같은 값을 가졌다고 하더라도 비교하면
false가 나오는 것이다.

```
yarn add shallow-equal
```

해당 명령어 등으로 shallow-equal 모듈을 다운로드 하고 shallowEqual을 통해서 얕은 비교를 할 수 있다.  
얕은 비교를 하면 list1과 list2가 같다고 반환하는 것을 확인할 수 있다.  
하지만 얕은 비교가 값이 같아보인다고 해서 다 true를 반환하는 것은 아니다. list3의 경우 값은 같을 지도 모른다. 하지만 {name: 'rw'}처럼 새로운 객체를 선언했다.  
이 경우엔 메모리 주소가 달라지기 때문에 false를 반환한다.

## Component와 PureComponent 차이

```js
import React, {Component, PureComponent} from "react";

class Example1 extends Component {
    ...
}

class Example2 extends PureComponent {
    ...
}
```

그러면 둘의 차이는 무엇일까?  
라이프 사이클 메소드 중에 shouldComponentUpdate가 있다. PureComponent는 해당 메소드에서 shallowEqual를 메소드를 통해서 얕은 비교를 진행한다.  
그래서 데이터의 변경이 있으면 화면을 새로 출력하고, 없다면 새로 출력하지 않는다.

더 간단히 설명하면 state에 count:1 이라는 값이 있다고 생각해보자.

```js
state = {
  count: 1,
};
```

이때 다시 count에 1을 넣었을 때 Component는 render를 다시하지만 PureComponent는 render를 하지 않는다.
