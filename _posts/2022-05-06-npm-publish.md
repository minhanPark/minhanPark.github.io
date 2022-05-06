---
title: "npm 모듈 만들기 - npm 가입 후 배포해보기"
excerpt: "npm 모듈을 만들기 위해 npm에 가입해보고 배포를 경험해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - npm
  - javascript
---

## npm 이란?

npm(Node Package Manager)은 자바스크립트의 패키지 매니저이다.  
쉽게 말하면 모듈을 만들어서 버전을 관리하고 사람들은 설치하고 사용하는 공간이라고 할 수 있다.  
대략 80만개 이상의 모듈이 npm에 올라와있고, 아래 명령어로 쉽게 다운로드할 수 있다.

```bash
npm i <module_name>
```

Node.js를 설치하면 기본적으로 같이 설치된다.

## npm 회원가입하기

[npm 접속하기](https://www.npmjs.com/)  
해당 페이지로 가서 회원가입을 하자.  
회원가입 시엔 username, password, email이 필요하다.  
로그인 시 email로 오는 otp가 필요하니 자기가 사용하는 email을 쓰는것도 중요하다.  
![otp 메일](https://user-images.githubusercontent.com/29043491/167087500-bfb635ac-a528-4849-832c-1b26702901fa.png)

메일로 받은 otp 까지 입력하면 가입이 완료된다.

## package.json 만들기

이제는 배포하기 위한 파일을 만들어야 한다. npm에 배포시 가장 중요한 파일은 package.json이다.  
디렉토리를 하나 만들고 거기에 package.json 파일을 만들어보자.

```json
{
  "name": "@scope/module",
  "version": "0.0.1"
}
```

가장 **필수적으로 들어가야 하는 부분은 name부분과 version**이다.  
우선 이름에 주목해보자.  
@scope라고 적은 부분을 생각해보자. 저부분에 팀명이나 회사명, 내 닉네임 등을 적으면 npm 검색 시에 해당 scope에 대한 것들을 모아서 볼 수 있다. 사실 scope는 없어도 된다. 추가한 이유는 scope를 지정하지 않으면 이름이 겹쳐서 올라가지 않을 확률이 높기 때문이다.  
(위에서 적었듯이 80만개 이상의 모듈이 올라가 있다.)

진행을 위해 scope에는 닉네임이나, 팀명, 회사명을 적어보자.  
@types를 확인해보면 적절한 예시를 볼 수 있다.([@types 검색 결과 보러가기](https://www.npmjs.com/search?q=%40types))

이제는 버전에 집중해보자.  
0.0.0 형태의 버전을 **SemVer**라고 말한다. Semactic Versioning이라는 의미인데,
왜 의미론적인 버전 붙이기라고 할까?  
이유는 각각의 버전에 의미를 담았기 때문이다.  
위치에 따라 아래처럼 이름이 있다.

> MAJOR.MINOR.PATCH

MAJOR는 각 모듈이 호환이 안될 때 사용한다. 즉 1.0.0에서 2.0.0은 크게 바뀌어서 호환이 되지 않는다는 뜻이다.  
MINOR는 호환성은 있되, 기능이 추가되는 등의 업데이트가 일어나면 사용하면 된다.  
PATCH는 말그대로 간단한 수정사항 및 패치 때 사용하면 된다.

지금 만들어보는 모듈은 정식버전도 아니고 완성이랄 것도 없기 때문에 0.0.1이라는 버전을 붙였다.

## 배포하기

우선 해당 커맨드창을 켜서 npm 로그인을 하자.

```bash
npm login
```

해당 명령어를 실행하면 username과 password, email을 물어보고 해당 email에 다시 otp를 보낸다.  
otp 까지 입력해서 로그인을 하자.

```bash
npm whoami
```

위의 명령어는 내가 어떤 유저로 로그인 되어 있는 지 확인할 수 있는 명령어이다.

npm에는 public과 private 형태로 모듈을 배포할 수 있는데, private 형태로 배포하려면 달마다 7$를 내야한다. 간편하고, 공익적으로 퍼블릭으로 배포해보자.

```bash
npm publish --access=public
```

위의 명령어를 입력하면 npm이 배포된다.  
성공적으로 배포되면 감사(?)하다는 메일을 확인할 수 있다.  
그리고 npm 검색창에 내 모듈을 검색해보면 발견함과 동시에 작은 기쁨을 느낄 수 있을 것이다.
