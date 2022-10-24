---
title: "Next.js Data Fetching - getServerSideProps"
excerpt: "Next.js의 공식문서에 Basic Features의 'Data Fetching'의 getServerSideProps 부분을 번역한 글입니다."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - docs
tags:
  - nextjs
  - docs
---

## getServerSideProps

만약에 'getServerSideProps'(Server-Side 렌더링) 함수를 페이지로부터 export하면 Next.js는 'getServerSideProps'에 의해서 리턴되는 데이터를 통해서 이 페이지에서 각 요청마다 미리 렌더링할 것입니다.

```js
export async function getServerSideProps(context) {
  return {
    props: {}, // props로 페이지 컴포넌트에 전달됩니다.
  };
}
```

> 렌더링 타입에 관계없이, 어떤 props든 페이지 컴포넌트에 전달되고, 클라이언트 측의 초기 HTML에 보여집니다. 이것은 페이지가 정확하게 수화(hydrated)된 것입니다.  
> 클라이언트 측의 props에 이용될 수 있는 민감한 정보는 넘기지 않도록 하세요.

## 언제 getServerSideProps가 작동하는가?

'getServerSideProps'는 서버측에서만 작동하고, 브라우저에서는 작동하지 않습니다.  
만약 페이지가 'getServerSideProps'를 사용한다면:

- 만약에 해당 페이지를 요청할 때 'getServerSideProps'는 요청 시 실행됩니다. 그리고 해당 페이지는 함수에서 반환된 props를 가지고 미리 렌더링하니다.
- 해당 페이지에서 'next/link'나 'next/router'를 통해 클라이언트 측 페이지 전환을 요청하면 Next.js는 'getServerSideProps'를 실행하는 서버에 api 요청을 보냅니다.

'getServerSidePrips'는 페이지를 렌더링할 때 사용할 JSON을 반환합니다. 모든 작업은 자동으로 Next.js에 의해서 실행되고, getServerSideProps를 정의하기만 하면 됩니다.

[next-code-elimination tool](https://next-code-elimination.vercel.app/)을 사용해서 Next.js가 클라이언트 측의 번들에서 제거하는 부분을 확인할 수 있습니다.

'getServerSideProps'는 페이지에서만 불러올 수 있고, 페이지가 아닌 파일들에서는 불러올 수 없습니다.

반드시 getServerSideProps를 독립적인 형태로 불러야하며, 만약 페이지 컴포넌트의 속성으로 getServerSideProps를 추가하면 작동하지 않습니다.  
[getServerSideProps api 참조](https://nextjs.org/docs/api-reference/data-fetching/get-server-side-props) 에서 모든 파라미터와 props를 확인할 수 있습니다.

## 언제 getServerSideProps 사용해야 하는가?

요청 시 데이터를 가져와야 하는 페이지를 렌더링할 때 사용해야합니다. 이는 데이터 또는 요청의 속성(권한 부여 또는 geo) 때문일 수 있습니다.  
getServerSideProps를 사용하는 페이지들은 요청 시에 서버측에서 렌더링되며, [cache-contorl headers가 설정되었을 때](https://nextjs.org/docs/going-to-production#caching) 캐싱될 수 있습니다.

만약 데이터를 요청 시 불러올 필요가 없다면 클라이언트 측 또는 getStaticProps를 고려해야합니다.
