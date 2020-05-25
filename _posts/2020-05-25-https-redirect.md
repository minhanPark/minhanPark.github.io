---
title: "http 프로토콜을 https 프로토콜로 리다이렉트 시키기"
excerpt: "http 프로토콜을 https로 리다이렉트 시켜보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - Backend
  - javascript
---

## protocol 확인하기

```js
const protocol = req.headers["x-forwarded-proto"] || req.protocol;
// https or http
```

해당 값으로 http인지 https인지 확인할 수 있다.

```js
if (protocol !== "https") {
  // https로 리다이렉트
}
```

프로토콜을 판단한 후 위와 같은 형태로 리다이렉트 시켜주면 된다.

## 실제 코드

나의 경우는 test.com으로 접속한다면 www.test.com으로 보내는 코드도 포함해서 미들웨어를 만들었다.

```js
export const middleware = (req, res, next) => {
  const host = req.get("host");
  const protocol = req.headers["x-forwarded-proto"] || req.protocol;
  if (host === "test.com" || protocol !== "https") {
    res.redirect(301, `https://www.test.com${req.url}`);
  } else {
    next();
  }
};
```

req.get("host")를 통해서 host를 받아온다. (test.com으로 접속하면 test.com이 host다)  
그리고 접속한 protocol을 가져온다.  
그리고 protocol이 http거나 host가 test.com이라면 **https://www.test.com**로 리다이렉트 시킨다.
