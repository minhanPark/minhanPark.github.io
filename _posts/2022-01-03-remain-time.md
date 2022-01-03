---
title: "로그인 시간 연장 기능 추가"
excerpt: "Vue 프로젝트에서 로그인 시간 연장 기능 추가하기"
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

## 기능의 필요성

은행이나 카드사에 로그인 하면 헤더에 10분 등 로그인 유지 시간이 있다. 아무것도 하지 않으면 자동으로 로그아웃이 되도록 되어있고, 사용자가 검색/페이지 이동 등 특정 요청을 하면 다시 시간이 10분으로 채워진다.  
자사 솔루션의 경우에도 보안 상 10분의 제한 시간이 있는데, 사용자가 검색/페이지 이동 등을 하면 다시 10분으로 되돌리도록 수정할 필요가 있었다.

## 가장 쉽게 기능을 넣는 방법

우선 대부분의 api 요청을 axios를 통해서 하게 될 것이다. 그리고 공통적으로 사용하는 부분이기 때문에 instance로 만들어 사용할 것이다.

```js
const instance = axios.create({
  baseURL: "https://api.example.com",
});
```

해당 인스턴스를 이용하면 baseURL을 간편히 돌려서 사용할 수 있다. 그리고 또 인스턴스를 활용할 때 인터셉터를 활용하면 매 요청시 시간을 다시 채워줄 수 있다.

```js
// 인스턴스를 만들었다면 instance를 갖고와서 인터셉터를 설정해주면 된다.
axios.interceptors.request.use(
  function (config) {
    // 매 요청이 보내지기 전에 필요한 공통 로직
    return config;
  },
  function (error) {
    // 에러가 났을 경우 처리
    return Promise.reject(error);
  }
);

axios.interceptors.response.use(
  function (response) {
    // 2xx 응답이 오면 해당 응답이 도착하기 전에 필요한 공통 로직
    return response;
  },
  function (error) {
    // 2xx번대 코드가 아니면 발생할 에러 처리
    return Promise.reject(error);
  }
);
```

사용자가 요청을 하기만 했다면 시간을 복구시켜줄 지, 아니면 요청 후 200번대의 코드를 받아야 시간을 복구시켜 줄 지에 따라서 request나 response에 넣어주면 된다.  
페이지 이동 시엔 어떻게 걸어줄 수 있을까?

뷰 라우터에는 네비게이션 가드가 있다.  
페이지에 들어온 뒤 기존의 시간을 연장시켜주어야 하기 때문에 afterEach를 사용하면 된다.

```js
router.afterEach((to, from) => {
  // 여기에서 시간을 연장시켜준다.
});
```

## 문제점 처리

시간을 연장하는 함수를 reLogin이라고 하고, 위와 같은 부분들에서 처리했을 때 문제가 생기는 부분은 reLogin 함수가 여러번 실행될 수 있다는 것이다.  
페이지 이동 시 afterEach는 한번 호출 되고, reLogin은 잘 작동할 것이지만, 만약 페이지에서 데이터를 여러번 불러오는 경우가 있다고 생각하면 그때마다 reLogin이 불필요하게 호출되게 된다.

```
// Vue 프로젝트 내부에서
async created(){
   await api1();
   await api2();
   await api3();
}
```

위의 예시처럼 인터셉터를 통해서 api 요청마다 reLogin을 호출하기 때문에 해당 페이지에서 3개가 아닌 6개의 호출이 일어난다.

여기서는 전역 상태값에 timestamp를 넣어주고, 특정 시간 미만에는 호출되지 않도록 처리했다.  
[timestamp로 사용한 Date.now 함수 보기](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/now)

```js


const reLogin = () => {
    if(state.timestamp + 3000 > Date.now()){
        // timestamp를 확인해서 3초미만이면 실행이 되지 않도록 그냥 종료함
        return;
    }
    (...) // reLogin 로직 실행
    // 만약 3초가 넘어서 실행이 되었다면 다시 timestamp 값을 넣어준다
    state.timestamp = Date.now();
}
```

매번 해야하는 귀찮은 요청을 인스턴스의 인터셉터만 라우터 가드를 통해서 코드를 줄이며 실행해 보았다.
