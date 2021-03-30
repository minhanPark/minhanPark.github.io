---
title: "Next app의 기본 구조"
excerpt: "next app의 기본 구조를 보면서 커스터마이징을 해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - React
  - Next.js
---

## Next.js의 구조

기본적으로 넥스트 앱을 만들면 아래와 같은 폴더와 파일이 중요하다.

```cmd
.
└── pages
|     └── _app.jsx
|     └── _document.jsx
└── public
```

해당 포스팅에서 Next의 앱에서 구조적으로 중요한 pages 폴더, \_app.jsx, \_document.jsx, public 폴더를 알아보자.

## pages 폴더

React와 달리 Next는 **pages 폴더를 통해서 라우팅을 설정**한다.  
이게 무슨 뜻이냐면 기본적으로 pages/index.js나 pages/index.jsx 파일이 "/"에 해당한다.  
그리고 pages/company.jsx가 있다면 해당 파일은 "/company"가 된다.  
좀 더 복잡하다면 어떻게 해야할까?

```cmd
.
└── pages
      └── company
        └── index.jsx
        └── detail.jsx
```

pages 폴더 안에 company 폴더를 만들고 다시 그 안에 index.jsx와 detail.jsx를 만들었다고 가정하자.  
이렇게 되면 company/index.jsx가 "/company"가 되고, company/detail.jsx가 "/company/detail"이 된다.

## 동적 라우팅

가령 "/movie/11223" 과 같은 형태처럼 movie 다음에 동적으로 라우팅이 되어야 하는 경우엔 어떻게 구성해야할까?  
답은 **[id].jsx와 같은 형태로 구성**하면 된다.

```cmd
.
└── pages
      └── movie
        └── [id].jsx
```

폴더를 위와 같이 구성하고 id 값을 받아와서 페이지를 렌더링해주면 된다.

```js
// pages/movie/[id].jsx


(...컴포넌트 처리 부분)

export const getServerSideProps = async ({params}) => {
    // params를 확인하면 id 부분의 값을 가져올 수 있다.
}
```

## \_app.jsx

기본적으로 라우팅하는 방법을 알았다면 이제는 \_app.jsx을 알아보자.

```js
import "../styles/globals.css";

function MyApp({ Component, pageProps }) {
  return (
    <>
      <Component {...pageProps} />
    </>
  );
}

export default MyApp;
```

기본 형태는 위와 같을 것이다.  
제일 먼저 실행되며 레이아웃을 잡거나 **공통 로직을 처리할 때 사용**할 수 있다.

```js
function MyApp({ Component, pageProps }) {
  return (
    <>
      <Header />
      <Component {...pageProps} />
      <Footer />
    </>
  );
}
```

헤더나 푸터를 위와같이 처리해주면 모든 컴포넌트에서 헤더와 푸터를 자동적으로 불러와서 사용할 수 있다.  
또한 pageProps는 Component(해당 페이지에서 불러오는 컴포넌트)에서 사용하는 props인데 이때 공통적으로 사용할 props 등을 처리해서 넘겨줄 수도 있게된다.

## \_document.jsx

공통적으로 사용하는 메타 태그 등을 삽입할 수 있는 파일입니다.

```js
// _document.jsx

import Document, { Html, Head, Main, NextScript } from "next/document";
class MyDocument extends Document {
  render() {
    return (
      <Html>
        <Head />
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}

export default MyDocument;
```

기본 형태는 위와 같다. 해당 파일을 통해서 기본 Document을 오버라이드 할 수 있다.  
주의할 점은 해당 파일에서는 **이벤트 핸들러를 사용할 수 없고, Main 컴포넌트 밖의 컴포넌트들은 브라우저에 의해서 초기화되지 않을 것이라는 것**이다. 그렇기 때문에 공통 로직은 여기가 아니라 \_app.jsx에 작성되어야 하고, css 파일도 여기가 아니라 \_app.jsx에 작성해야한다.

## public 폴더

해당 폴더에서 **정적파일을 제공**해줄 수 있다. 이미지를 넣으면 "/이미지이름"형태를 가지기 떄문에 정적파일을 나눠서 넣을 수 있다. robots.txt나 폰트 등의 여러 정적파일을 넣어주면 된다.(파비콘, 구글 사이트 확인 등을 할 때도 사용가능)

기본적으로 해당 형태들을 기본으로 잡고, 커스터마이징을 할 수 있다.
