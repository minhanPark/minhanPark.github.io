---
title: "express에 pug 템플릿 사용하기"
excerpt: "express에 pug를 연결해보고 사용법을 알아보자"
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - Nodejs
  - express
  - pug
---

## 템플릿이란?

express 등 으로 웹페이지를 만들면 사용자의 요청에 따라 html을 보여줘야 한다.  
이때 템플릿이 사용자에게 보여줄 html을 생성해주는 역할을 한다고 생각하면 된다.  
express와 연동해서 **많이 사용하는 것들은 pug(예전 jade)나 ejs 등**이 있는데 자기가 사용하고 싶은 것을 사용하면 되지만 나에겐 문법적으로 좀 더 간략해보이는 pug가 친숙하게 느껴져 사용법을 요약해서 적어보도록 하겠다.  
(ejs는 html 태그 형태 그대로를 사용하기 때문에 처음 배우는 입장에서는 더욱 친숙하게 느껴질 수도 있다.)

## pug와 express 연동하기

우선 express는 설치했다는 가정하에 시작하겠다.

```bash
npm install pug --save
```

일단 pug를 설치하자.  
우선 생각해야할 것은 pug 말고도 다양한 template 언어가 있다는 것이다. 그리고 문법도 다 다르기 때문에 **express에게 pug를 사용한다고 알려줘야한다**.

```js
// app.js
import express from "express";

const app = express();

app.set("view engine", "pug");
```

위와 같은 형태로 적어주면 된다.  
또한 pug 파일들을 모아둔 디렉토리를 알려주면 render 시에 express가 가져올 수 있다.

```js
// app.js
import path from "path";
// 위에 부분은 동일

app.set("views", path.join(__dirname, "views"));
```

app.js가 존재하는 디렉토리에 views 디렉토리를 만들고 index.pug 파일을 만들어보자.
이제 해당 views 디렉토리에서 express는 파일들을 가져와 사용자에게 렌더링해줄 것이다.

## pug 기본 문법

pug는 **기본적으로 태그 이름만 사용**하면 된다.

```
h1 제목입니다.
==> <h1>제목입니다.</h1>
```

위에처럼 태그이름만 쓰고 한칸을 띄운 다음 글자를 쓰면 express가 사용자에게 보여줄 땐 해당 태그로 감싸서 보여지게 된다.  
속성은 어떻게 사용할 수 있을까?

```
h1(id="h1-id", class="h1-class") 제목입니다.
==> <h1 id="h1-id" class="h1-class">제목입니다.</h1>
```

속성을 줄 때는 태그 이름 옆에 **소괄호를 준 뒤 속성을 정의**해주면 된다.

가장 많이 사용하는 tag인 div에는 간편한 축약형이 존재하는데 만약 #div-id, .div-class 와 같은 형태로 쓰면 해당 id나 class를 가진 div가 된다.

```
#div-id 축약형
==> <div id="div-id">축약형</div>

.div-class 축약형
==> <div class="div-class">축약형</div>
```

위와 같은 형태로 변경되게 된다.

## 변수에 값 전달하기

웹서버에서 데이터를 받아서 보여주는 **동적인** 형태이기 때문에 변수의 활용이 중요하다.  
가령 footer에 저작권 관련 부분이 있다고 하자.  
2022라는 년도를 정적으로 그대로 적기보다는 자바스크립트 문법을 사용해 이번 해가 몇년도인지 가져오고, 해당 값을 전달해줄 수 있는 것이다.

```
// index.pug

.footer 지금은 #{year} 이다.
```

변수를 #{} 형태로 사용하였다.  
해당 변수에 값을 전달하는 방식은 2가지가 있는데 해당 페이지에서 전달해주거나 렌더링할 때 전달해주는 방식이 있다.

```
// 해당 페이지에서 전달
- var year = new Date().getFullYear();
.footer 지금은 #{year} 이다.
==> <div class="footer">지금은 2022 이다.</div>
```

-(하이픈) 를 사용하고, 변수를 선언하고 변수에 값을 전달해주면 된다.

```js
router.get("/", (req, res) => {
  res.render("index", { year: new Date().getFullYear() });
});
```

위와 같이 **index 페이지를 렌더링 시 변수값을 전달**해주면 index.pug에 전달되서 사용자에게 보여진다.

## layout을 사용해서 코드를 줄이자

만약 공통적인 부분을 놔두고 달라지는 부분만 코드를 작성할 수 있으면 훨씬 효율적일 것이다. 그리고 그런 장점을 가진 것이 템플릿 언어이다.

```
// layout.pug
html
  head
    title MyPage - #{title}
    link(rel='stylesheet', href='style.css')
  body
    block content
    block footer
        - var year = new Date().getFullYear();
        .footer 지금은 #{year} 이다.

```

위와 같이 공통적인 부분으로 레이아웃을 구성할 수 있다(파일의 이름이 중요한건 아님)  
그리고 해당 **레이아웃을 이용하려면 사용할 페이지에서 extends를 사용**하면 된다.

```
// index.pug
extends layout.pug

block content
    h1 블록이 교체

block append footer
    .footer2 이것은 어떻게 렌더링 될까요.
```

**block 블록이름** 형태를 주목해보자. 레이아웃에 block과 block 이름을 지정해두고  
사용할 페이지에서 block과 block 이름을 적어두면 해당 내용으로 블록이 교체된다.

> layout.pug에 block footer 부분을 보자  
> 여기에는 기본적으로 내용이 적혀 있는데, 해당 값은 기본값이 될 것이다.  
> 즉 index.pug 등 다른 페이지에서 block footer 값을 정의해주지 않았다면 해당 기본값이 사용될 것이다.

기본값은 교체지만 append와 prepend도 있다.  
index.pug를 정의해둔 부분을 보면 append footer라고 정의했는데 해당 부분으로 footer block의 뒤에 해당 코드가 나타날 것이다.
