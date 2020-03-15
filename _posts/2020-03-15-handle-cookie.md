---
title: "express에서 쿠키로 값 저장하기"
excerpt: "익스프레스에서 쿠키를 통해서 정보를 저장해보자"
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

## express에서 쿠키 사용하기

우선 cookie-parser를 다운로드 해야 한다.  
**npm install cookie-parser**  
또는  
**yarn add cookie-parser**

```js
import cookieParser from "cookie-parser";

(...)
app.use(cookieParser(process.env.COOKIE_SECRET));
```

여기서 process.env.COOKIE_SECRET은 쿠키의 secret을 넣은 것이다. 비워도 상관없지만 secret을 넣어주면 세션쿠키를 분석해서 매치시켜준다. 즉 세션을 사용하고 있다면, 고정된 secret 값을 넣어주는 것이 좋다.

## 쿠키 설정하기

위와 같이 설정을 했다면 쿠키를 사용할 수 있다.

```js
const myName = "RunningWater";
res.cookie("Name", Myname);
```

**res.cookie('name', 'value', {options})**를 통해서  
설정한다는 것을 잊지말자!
