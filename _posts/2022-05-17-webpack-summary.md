---
title: "웹팩 개요"
excerpt: "웹팩이 무엇인 지 어떻게 사용하는 지 간략하게 알아보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - webpack
---

## 웹팩(webpack)이란?

웹팩은 모던 자바스크립트 애플리케이션을 위한 **모듈 번들러**이다.  
각 단어를 나누면 해당 의미를 가지게 된다.

> 모듈 : 기능 및 성격에 따라 파일을 나눈 것  
> 번들러 : 합쳐서 결과물을 보여주는 것

모던 자바스크립트에서는 기능에 따라 모듈화 해서 작업하는 것이 일상화되어있다.
하지만 나누어 작성했다고 해서 그대로 두는 것이 정답은 아니다.  
왜냐하면 브라우저마다 요청할 수 있는 횟수가 정해져 있고, 파일이 너무 세분화되어 있다면 사용자가 파일을 읽는데 더 오랜 시간이 걸리기 때문이다.  
또한 사용자의 브라우저가 읽지 못하는 부분은 읽을 수 있도록 변환되어지는 과정도 필요하기 때문이다.  
웹팩은 이러한 것들을 해결하면서 합쳐서 결과물로 보여주는 것이라고 할 수 있다.

## webpack.config.js

웹팩의 설정은 커맨드 라인에서 직접 파라미터로 설정이 가능하지만 보통은 설정 파일을 만들어서 웹팩 실행 시에 설정에 따라 실행되도록 한다.  
그리고 **설정을 담고 있는 파일이 webpack.config.js**이다.

기본적으로 Entry, Output, Loader, Plugin으로 구성되어 있다.

**entry는 진입점**이다.  
웹팩은 entry로부터 의존성이 있는 모듈을 모두 찾게된다.  
시작점이라고 할 수 있음

```js
// webpack.config.js

module.exports = {
  entry: "./path/to/my/entry/file.js",
};
```

**기본값은 /src/index.js**이지만 다른 값을 넣어주면 entry를 바꿀 수 있음.

**Output**은 번들링한 후 웹팩이 만들어 줄 결과물이다.

```js
const path = require("path");

module.exports = {
  entry: "/src/index.js",
  output: {
    path: path.resolve(__dirname, "build"),
    filename: "index.js",
  },
};
```

path를 통해서 번들의 내보낼 위치를 알려주고, filename을 통해서 내보낼 파일의 이름을 알려주었다.  
즉 웹팩을 실행하면 웹팩은 /src/index.js를 읽어서 /build/index.js로 내보내줄 것이다.  
**기본값은 /dist/main.js** 라는 것을 잊지말자.

**Loader**는 웹팩이 다른 유형을 파일을 처리할 수 있도록 해주는 역할을 한다.  
**기본적으로 웹팩은 Javascript와 json 파일만 이해**할 수 있다.  
그러나 타입스크립를 사용했다면? 또는 css가 사용되었다면? loader를 통해서 웹팩이 해당 파일들을 처리할 수 있도록 할 수 있다.

> loader는 module이라는 key로 들어간다는 것을 유의하자

```bash
# 우선은 css-loader와 ts-loader를 설치했다고 가정하자.
npm install --save-dev css-loader ts-loader
```

그리고 웹팩에 해당 모듈을 사용하도록 설정할 수 있다.

```js
module.exports = {
  entry: "/src/index.ts",
  module: {
    rules: [
      { test: /\.css$/, use: "css-loader" },
      { test: /\.ts$/, use: "ts-loader" },
    ],
  },
  output: {
    path: path.resolve(__dirname, "build"),
    filename: "index.js",
  },
};
```

해당 파일은 /src/index.ts 파일을 읽어서 만약 css가 import 되어있다면 css-loader를 처리하고, ts 파일은 ts-loader를 통해서 자바스크립트로 변환시키고, /build/index.js에 결과물을 내놓으면 된다는 뜻이 됩니다.

> **test** : 적용할 파일 유형, **use** : 적용할 로더의 이름

로더는 특정 유형의 모듈을 변환하는데 사용되지만 **플러그인은 번들을 최적화 하거나, 애셋을 관리하거나, 환경 변수 주입 등과 같은 광범위한 작업을 수행 및 결과물의 형태를 바꾸는 역할을 한다**

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");
const webpack = require("webpack"); //빌트인 플러그인에 접근하기 위함

module.exports = {
  plugins: [
    new webpack.ProgressPlugin(),
    new HtmlWebpackPlugin({ template: "./src/index.html" }),
  ],
};
```

HtmlWebpackPlugin은 빌드한 결과물(output으로 나오는 번들링한 결과물)이 포함된 HTML 파일을 생성해주고, ProgressPlugind은 빌드 진행율을 표시해주는 플러그인이다.
