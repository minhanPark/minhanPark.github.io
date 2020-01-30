---
title: "리액트 프로젝트에 sass와 css module 적용하기"
excerpt: "리액트 프로젝트에 sass와 css module을 적용시켜 보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - javascript
  - React
---

리액트 프로젝트에 스타일을 더하는 방법은 styled-components를 이용하는 것 말고도 기본적인 css와 sass를 이용하는 방법도 있습니다.  
[styled-components 사용법 보러가기](https://minhanpark.github.io/today-i-learned/how-to-use-styled-components/)  
스타일 컴포넌트를 사용하는 방법은 위의 포스팅을 참고해주세요.

## sass 이용하기

리액트 프로젝트에 sass를 적용하는 방법은 매우 간단합니다. node-sass모듈만 설치해주시면 됩니다.

```
yarn add node-sass
```

설치가 완료되면 이름.scss 형태로 sass파일을 만드시면 됩니다.

```js
// Box.js

import "./Box.scss";

const Box = () => {
  return (
    <div className="wrapper">
      <div className="box red"></div>
      <div className="box blue"></div>
      <div className="box green"></div>
      <div className="box gray"></div>
    </div>
  );
};

export default Box;
```

sass가 적용되는 모습을 확인하기 위해 Box 컴포넌트를 만들었습니다.

```scss
// Constant.scss

$red: #e74c3c;
$blue: #2980b9;
$green: #27ae60;
$gray: #95a5a6;
```

scss는 프로그래밍 언어처럼 변수를 지정해서 적용시킬 수 있습니다. 해당 색들을 Constant.scss 파일에 정의했습니다.

```scss
// Box.scss

@import "./Constant.scss";

.wrapper {
  width: 100%;
  height: 150px;
  display: flex;
  .box {
    width: 25%;
    height: 100%;
  }
  .red {
    background-color: $red;
  }
  .green {
    background-color: $green;
  }
  .blue {
    background-color: $blue;
  }
  .gray {
    background-color: $gray;
  }
}
```

sass 내에서 다른 sass(위의 경우 변수가 들어있는 파일) 파일을 불러오실 땐 **@import**를 사용하면 됩니다.  
컴포넌트에 sass 파일을 불러오면 아래 그림과 같이 적용되는 모습을 확인할 수 있습니다.

![sass가 적용된 모습](https://drive.google.com/uc?id=1CbJOvwZDzPSZeR3BWDEEKJCFSOsd2hQK)

## css module 적용시키기

css 모듈은 왜 필요할까요? **클래스의 이름을 고유하게 바꿔주기 때문**입니다.  
지금은 box나 red 등 클래스 이름을 같이 사용하는 것이 없지만 프로젝트가 커지면 본의 아니게 같은 이름을 사용하는 경우가 생깁니다.  
그래서 각각의 이름을 고유하게 만들어서 본의아니게 겹치지 않도록 만들어주는 역할을 하는 것이 css module입니다.

**따로 설치할 필요없이 기존 sass 파일 사이에 module을 추가해주세요.**  
즉 현재는 Box.scss로 되어 있지만 이름을 **Box.module.scss** 형태로 바꿔주시면 됩니다.  
이제는 Box.js를 고쳐보도록 하겠습니다.

```js
import styles from "./Box.module.scss";
```

Box.module.scss에서 styles를 불러옵니다. 해당 객체를 콘솔에 찍어보면 각 고유한 값이 매칭되어 있는 것을 확인할 수 있습니다.  
![객체확인](https://drive.google.com/uc?id=1SIJywnssGASg45ln6BtRdoclCXlYi0Sq)

맨 처음에 클래스 이름으로 지어줬던 값이 "키"가 되고 고유한 이름이 "값"으로 이루어져 있는 객체의 모습을 확인할 수 있습니다.

```js
const Box = () => {
  return (
    <div className={styles.wrapper}>
      <div className={`${styles.box} ${styles.red}`}></div>
      <div className={`${styles.box} ${styles.blue}`}></div>
      <div className={`${styles.box} ${styles.green}`}></div>
      <div className={`${styles.box} ${styles.gray}`}></div>
    </div>
  );
};
```

객체로 이름이 들어오기 때문에 각각의 태그에 객체의 값을 전달해주면 됩니다.  
클래스이름이 2개 이상이 적용되는 것은 위의 형태처럼 \`${ 값 } ${ 값 }\` 과 같이 전달해주시면 됩니다.
