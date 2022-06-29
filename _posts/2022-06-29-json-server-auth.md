---
title: "json-server로 1분만에 rest api 서버만들기 3"
excerpt: "json-server-auth로 회원가입, 로그인 기능까지 완성해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - 시리즈
tags:
  - Nodejs
  - REST api
  - json-server
  - json-server-auth
---

## 해당 시리즈의 다른 편 확인하기

- [json-server로 1분만에 rest api 서버만들기 1](https://minhanpark.github.io/%EC%8B%9C%EB%A6%AC%EC%A6%88/json-server-install/)
- [json-server로 1분만에 rest api 서버만들기 2](https://minhanpark.github.io/%EC%8B%9C%EB%A6%AC%EC%A6%88/json-server-crud/)

## 소스 코드 보러 가기

> [커밋 내역 확인하기](https://github.com/minhanPark/json-server-example/commit/36efec77e3f53294889951035184f0fa23399cc8)

## db.json 파일 작성 및 서버 띄우기

로그인 및 회원가입 기능을 처리해주는 가짜 rest api 서버는 없을까?  
json-server에 회원가입 및 로그인 기능을 더한 json-server-auth를 활용하면 간단히 만들 수 있다.

```
npm i -D json-server-auth
```

해당 명령어로 설치하고, db.json을 아래처럼 바꾸자.

```json
{
  "users": [],
  "todos": []
}
```

json-server-auth는 **기본적으로 users에 사용자를 추가**한다.  
이제 script의 명령어를 아래처럼 바꾸면 json-server-auth가 실행된다.

```json
"scripts": {
    "server": "json-server-auth --watch db.json --port 4444"
  }
```

지금까지 작동 방식은 json-server와 똑같았다. 이제 서버를 실행시키고 나서 회원가입과 로그인 요청을 해보자.

## 회원 가입 및 로그인 요청

서버가 실행되면 아래 3가지 요청으로 회원가입을 요청을 할 수 있다.

- POST /register
- POST /signup
- POST /users

포스트맨을 통해서 /register에 요청을 보내면 어떤일이 일어나는 지 확인해보겠다.

![회원가입 요청 화면](https://user-images.githubusercontent.com/29043491/176357656-8a70c45d-c403-4325-bd12-80e62617778d.png)  
기본적으로 email과 password값이 필요하다.

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImZpcnN0bWFuQGVtYWlsLmNvbSIsImlhdCI6MTY1NjQ4MDAxNiwiZXhwIjoxNjU2NDgzNjE2LCJzdWIiOiIxIn0.9WMn7l9cE4c2MRhccBpo15g0uD1Zx24X28_Dy1aa3lk",
  "user": {
    "email": "firstman@email.com",
    "id": 1
  }
}
```

jwt 토큰과 user의 정보가 응답값으로 왔다.  
이제 db.json을 확인해보면 어떻게 저장되어 있을까?

```json
{
  "users": [
    {
      "email": "firstman@email.com",
      "password": "$2a$10$uT0E.SM7RvUvwC3OKzgcHOFOSCW2fV9d4bBcYpdacgNsoOjwnP0GG",
      "id": 1
    }
  ],
  "todos": []
}
```

비밀번호는 암호화되어 저장되어 있고, 유저가 추가되어 있는 모습을 확인할 수 있다.  
이제는 가입한 아이디를 통해서 로그인 요청을 해보자.

- POST /login
- POST /signin  
  두 가지 요청으로 로그인을 진행할 수 있다.

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImZpcnN0bWFuQGVtYWlsLmNvbSIsImlhdCI6MTY1NjQ4MDY0NiwiZXhwIjoxNjU2NDg0MjQ2LCJzdWIiOiIxIn0.vRFm2P1ELCZ-lQ6R2izzpqdp_Y5OfFCDdaioGd5QCD0",
  "user": {
    "email": "firstman@email.com",
    "id": 1
  }
}
```

로그인 요청을 하면 똑같이 jwt 토큰과 유저의 정보를 던져주는 것을 알 수 있다.

## 라우트 가드

로그인의 유무는 저 토큰을 같이 던지느냐 마냐의 차이일 뿐이다.  
![로그인 후 요청](https://user-images.githubusercontent.com/29043491/176360173-388c5b3b-4501-47a3-936b-9c1a4534d264.png)  
위의 이미지 처럼 요청을 할 때 Headers의 Authorization의 값을 Bearer + 토큰 해서 보내주면 로그인한 유저인 것이다.  
그렇다면 내가 적은 투두만 볼 수 있도록 라우트 가드를 어떻게 사용할 수 있을까?  
우선 jwt의 토큰 안에 무슨 값이 있는 지 파악해보자.

```json
{
  "email": "firstman@email.com",
  "iat": 1656480646,
  "exp": 1656484246,
  "sub": "1"
}
```

토큰에는 발행시간, 만료시간(토큰은 1시간 짜리), email(아이디)과 sub라는 값이 들어있다.  
여기서 중요한 것은 sub가 해당 유저의 id라는 것이다.  
즉 요청을 보냈을 때 해당 resource의 userId가 sub와 같다면 그 resource는 유저의 것이 된다.

db.json에 직접 넣거나 포스트맨을 통해서 userId가 포함된 투두를 만들어보자.

```json
"todos": [
    {
      "userId": 1,
      "description": "todos check",
      "id": 1
    }
  ]
```

이제 라우트 가드를 걸어보자.
routes.json 파일을 만들고 난 후 아래 내용을 넣는다.

```json
{
  "users": 600,
  "todos": 600
}
```

json-server-auth는 숫자로 권한을 표시하는데 읽기는 4, 쓰기는 2를 준다. (6은 읽기+쓰기 둘다 가능) **첫번째 숫자는 리소스의 주인, 두번째 숫자는 로그인한 유저, 세번째 숫자는 로그인하지 않은 퍼블릭 유저**이다.  
즉 투두에 본인이 쓴것만 확인할 수 있도록 가드를 걸었다.

```json
"scripts": {
    "server": "json-server-auth --watch db.json --port 4444 -r routes.json"
  }
```

이제 라우트를 적용 시킬 수 있게 명령어를 수정해주고 다시 서버를 작동시킨 후에 새로 아이디를 만들어서 투두스 요청을 해보자.

```
GET /600/todos 403
```

403(권한없음)을 받는 것을 확인할 수 있을 것이다.
