---
title: "express에서 사용자의 ip 주소 받기"
excerpt: "사용자의 ip주소를 받아보자."
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

## 필요성

아임포트를 이용하면 webhook을 추가하라고 한다.  
내가 이해하기론 특정 요청이 있을 때 다시 우리쪽으로 데이터를 동기화하는 과정이 있다는 것인데,  
해당 요청은 **52.78.100.19와 52.78.48.223**으로만 요청이 온다.  
즉 해당 ip에 대해서만 처리를 하고 나머지 요청에 대해서는 무시하면 되기때문에  
ip를 알아내는 과정이 필요했다.

## express에서 ip 가지고 오기

```js
const ip = req.header["x-forwarded-for"] || req.connection.remoteAddress;
```

결론부터 말하자면 저렇게 ip주소를 가지고 올 수 있다.  
로드밸런스 등을 이용한다면 사용자의 ip 주소를 req.header["x-forwarded-for"]에 넣어서 보내고, 아니라면 req.connection.remoteAddress에서 ip를 확인할 수 있다.

그리고 해당 ip값을 확인해보면 "::ffff:xx.xx.xx.xxx" (물론 x는 숫자다.) 형태로 들어오는 것을 확인할 수 있다.  
왜일까?

## 주의할 점

```js
server.listen([port][, hostname][, backlog][, callback])
```

맨처음 server를 listen할 때 **hostname을 생략했다면 IPv6을 사용할 수 있을 때 IPv6을 사용**한다.  
그래서 "::ffff:xx.xx.xx.xxx" 형태로 들어온 것이다.

나같은 경우엔 slice 메소드를 활용해서 앞부분을 잘라서 사용했다.(아임포트의 요청인지만 확인하면 되므로.)
