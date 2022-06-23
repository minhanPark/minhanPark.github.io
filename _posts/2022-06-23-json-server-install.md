---
title: "json-server로 1분만에 rest api 서버만들기 1"
excerpt: "파일을 정리하고 json-server 모듈을 다운로드 해보자."
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
---

## 해당 시리즈의 다른 편 확인하기

## 소스 코드 보러 가기

> [커밋 내역 확인하기](https://github.com/minhanPark/json-server-example/commit/0d3e0f5eca2ec8f43e4e0b829c05f26d2a2e1d4f)

## 1분 만에 가짜 rest api 서버 만들기

프론트엔드 개발을 하다보면 api 서버와 통신하는 부분을 구현할 때 테스트 등을 위해 간단한 형태로 직접 구현해야할 순간이 온다.  
express로 쉽게 만들수도 있고, nextjs 같은 경우엔 api 폴더에 넣기만 하면 api 통신을 할 수 있지만 그것보다 훨씬 더 간단하게 fake rest api 서버를 만들 수가 있다.  
해당 시리즈를 통해서 1분만에 가짜 rest api 서버를 만들어보자.  
꼭 아래처럼 리액트 프로젝트를 만들필요는 없고, 편하게 vue나 아니면 빈 디렉토리에서 시작해도 상관없다.

## CRA로 리액트 프로젝트 만들기

```bash
npx create-react-app json-server-example
```

해당 명령어로 json-server-example 이라는 리액트 프로젝트를 만들어보자.  
그리고 **src 폴더에서 불필요한 부분을 제거**해보자.

> **src/**  
> App.css  
> App.test.js  
> index.css  
> logo.svg  
> reportWebVitals.js  
> setupTests.js

src 폴더 내부에서 필요없는 부분들을 지웠다.  
또 해당 파일들을 지웠으니 해당 파일들을 import 해왔던 부분들을 수정하자.

```js
// App.js 파일

import React from "react";

function App() {
  return (
    <>
      <h1>1분만에 Rest api 만들기</h1>
    </>
  );
}

export default App;
```

필요없는 내용을 지우고 App.js를 위와 같이 수정해보자.

```js
// index.js 파일

import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

index.js 파일에서도 필요없는 내용을 지우고 위와 같이 수정해보자.

## json-server 설치하기

```bash
npm i -D json-server
```

해당 명령어로 devDependencies에 **json-server**를 설치해보자.  
package.json의 devDependencies를 보면 json-server가 설치되어 있는 것을 알 수 있다.

```json
{
  "devDependencies": {
    "json-server": "^0.17.0"
  }
}
```

모듈 설치는 끝났으니 다음 시리즈에 서버를 띄우고 CRUD를 구현해보도록 하겠다.
