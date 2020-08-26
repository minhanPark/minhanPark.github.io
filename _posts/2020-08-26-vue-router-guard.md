---
title: "vue에서 전역 가드, 라우터 가드, 컴포넌트 가드를 사용해보자."
excerpt: "vue router에서 제공하는 가드를 활용해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - javascript
  - Vue
---

## 가드

vue-router는 가드를 제공합니다. 주로 라우터를 통해서 이동할 시 리디렉션하거나 취소해서 네비게이션을 보호하는 역할을 합니다.  
연결하는 방법에는 전역, 라우트, 컴포넌트에 연결할 수 있습니다.

## 가드의 종류

### 전역가드

1. beforeEach
   > 라우터에 들어가기 전에 실행되는 가드입니다.
2. beforeResolve
   > 컴포넌트 가드가 끝난 후에 실행되는 가드입니다.
3. afterEach
   > 라우트 이동이 해결된 후에 실행되는 가드입니다.

### 전역가드 등록하기

```js
// src/routes/index.js

import Vue from "vue";
import VueRouter from "vue-router";

Vue.use(VueRouter);

export const router = new VueRouter({
    routes: [
        (...)
    ]
});
```

기본적인 라우터의 구조가 위와 같다고 하자.  
전역 가드는 바로 밑에 등록해주면 된다.

```js
// 위의 파일은 동일함

router.beforeEach((to, from, next) => {
  // 로직이 들어감.
});

router.beforeResolve((to, from, next) => {
  // 로직이 들어감.
});

router.afterEach((to, from) => {
  // 로직이 들어감
});
```

### to, from, next 파라미터 값 알아보기

전달하는 파라미터는 각각 어떤 값이 담기게 될까?  
**to는 우리가 이동할 라우트 객체의 정보**를 담고 있다. **from은 이동하기 전 라우트 객체의 정보**를 담고 있습니다. next는 다음 단계로 진행하기 위한 함수이다.  
조건에 맞지 않으면 next("/login") 형태로 로그인 페이지로 보내버릴수도 있고, next() 와 같은 형태로 값을 전달하면 네비게이션이 승인되고, 다음 단계로 이동하게 된다.

### 라우터 가드 등록하기

```js
export const router = new VueRouter({
  routes: [
    {
      path: "/",
      name: "indexPage",
      component: IndexView,
      beforeEnter: function (to, from, next) {
        // 로직이 들어감
      },
    },
  ],
});
```

라우터 가드는 위와 같이 정의해주면 된다.  
라우터 가드가 중요한 이유는 beforeEnter부터 this(vue 인스턴스)에 접근할 수 있기 때문이다.

해당 라우터에서 스토어에 접근하는 코드를 작성해보자.

```js
// routes/index.js
(...)
import { store } from "../store/index";

export const router = new VueRouter({
  routes: [
    {
      path: "/",
      name: "indexPage",
      component: IndexView,
      beforeEnter: function (to, from, next) {
        if(store.state.isLogin){
            next();
        } else {
            next("/login");
        }
      },
    },
  ],
});
```

store를 불러온 후 store.state로 뷰엑스에 저장된 state에 접근할 수 있다.  
위의 코드는 isLogin의 값을 확인해서 다음단계로 보낼 지 아니면 로그인 페이지로 보낼지를 정의한 것이다.

### 컴포넌트 가드 등록하기

1. beforeRouteEnter

   > 네비게이션 이동이 확정된 후 컴포넌트가 만들어 지기 전에 실행되는 가드입니다.

2. beforeRouteUpdate

   > 같은 컴포넌트 아래서 새로운 라우트가 불러졌을 때 실행되는 가드입니다.

3. beforeRouteLeave
   > 라우트를 떠날 떄 실행되는 가드입니다.

```js
export default {
  beforeRouteEnter(to, from, next) {
    // 로직이 들어감
  },
  beforeRouteUpdate(to, from, next) {
    // 로직이 들어감
  },
  beforeRouteLeave(to, from, next) {
    // 로직이 들어감
  },
};
```

위와같이 컴포넌트 내부에 가드를 등록하면 된다.

## 가드의 순서에 주의하자.

**실행되는 순서가 있기 때문에** next를 잘 이용하면 견고하게 가드를 만들 수 있습니다.  
전역 가드인 **beforeEach**가 가장 먼저 실행되고, 라우터 가드인 **beforeEnter**가 실행된다. 그리고 컴포넌트 가드 부분들이 **beforeRouteEnter**, **beforeRouteUpdate**가 실행되고,
전역가드인 **beforeResolve**가 실행된다. 그리고 마지막으로 **afterEach**가 실행이 됩니다.  
그리고 다른 라우터로 이동할 때 컴포넌트 가드인 **beforeRouteLeave**가 실행이 됩니다.
