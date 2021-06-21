---
title: "권한에 따른 vue router 접근 제한 걸기"
excerpt: "vue router에서 제공하는 가드를 활용해서 어드민, 클라이언트가 접근할 수 있는 페이지를 구분해보자."
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

## 권한에 따른 접근이 필요한 경우

각각의 페이지들을 상상해보자.  
로그인 페이지("/login"), 메인 페이지("/"), 관리자 페이지("/admin"), 404 페이지("/not-found) 등이 있을 때 회원, 비회원을 구분해야할 뿐만 아니라  
관리자 권한을 가진 사람만 관리자 페이지 접근할 수 있어야 한다.  
위와 같은 상황일 때 Vue에서는 어떻게 처리할 수 있을까?

필요한 것은 아래와 같다.

> 1. routes의 meta 속성
> 2. 라우터의 라우터가드(beforeEach)

## routes의 meta 속성

라우터에는 메타 속성을 정의해줄 수 있다.  
admin과, client라는 두가지 권한이 있다면 어떤 권한을 가져야 해당 라우터에 접근할 수 있는지를 메타 속성에 정의해주면 된다.

> 1. []는 유저라면 누구나 접근가능(client, admin)
> 2. ["admin"]는 어드민 권한을 가져야 접근가능
> 3. ["client"]는 클라이언트 권한을 가져야 접근가능
> 4. meta 속성에 따로 정의해주지 않았다면 아무나 접근가능

크게 위 처럼 나눌 수 있다.

```js
const router = new VueRouter({
  routes: [
    {
      path: "/",
      component: () => import("../component/Main.vue"),
      meta: { authorization: [] },
    },
    {
      path: "/login",
      component: () => import("../component/Login.vue"),
    },
    {
      path: "/admin",
      component: () => import("../component/Admin.vue"),
      meta: { authorization: ["admin"] },
    },
    {
      path: "*",
      component: () => import("../component/Notfound.vue"),
    },
  ],
});
```

## 라우터 가드에서는 무엇을 할까?

각각에 등록된 라우터가 실행되기 전에 실행해주는 메소드가 있다.  
바로 **beforeEach**이다.  
beforeEach에는 3가지 매개변수가 들어오는데 **to, from, next**이다.

> to는 어디로 갈지를 나타내는 객체, from은 어디에서 왔는 지를 나타내는 객체, next는 그대로 진행하게 해주는 메소드이다.

beforeEach에서 유저의 권한과 라우터의 메타 속성을 비교해서 처리해주면 된다.

우선 유저의 권한을 가져오려면 vuex 등 유저의 권한이 저장된 곳에서 값을 가져와야 한다.

```js
import { store } from "../store/index";

router.beforeEach((to, from, next) => {
  //authenticationState는 유저가 로그인이 되어있는지 아닌지 값을 가져와 판별해준다.
  const authenticationState = store?.state?.authentication;
  //authorization에서는 라우터에서 메타 속성을 정의해준 값이 담겨진다.
  // undefined, [], ["admin"], ["client"]가 올 수 있다.
  const { authorization } = to.meta;

  if (authorization) {
    if (!authenticationState?.isLogin) {
      // 로그인이 되어있지 않으면 로그인 페이지로 보낸다.
      return next({ path: "/login" });
    }

    if (
      authorization.length &&
      !authorization.includes(authenticationState?.role)
    ) {
      // 권한이 없는 유저는 not-found 페이지로 보낸다.
      return next({ path: "/not-found" });
    }
    // authorization가 빈 객체인 라우터들은 로그인만 되어있으면 다 접근할 수 있기 때문에
    // 맨 밑의 next를 통해서 그대로 통과된다.
  }
  next();
});
```

해당 조합을 통해서 **권한에 따른 페이지 접근권한**을 설정할 수 있다.
