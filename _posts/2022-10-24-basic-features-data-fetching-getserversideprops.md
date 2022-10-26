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

### getServerSideProps 또는 API Routes

서버로부터 데이터를 가져오고 싶을 때 api 라우트를 통해서 접근한다음 getServerSideProps로부터 api를 요청할 수도 있습니다. 이러면 불필요하고 비효율적인 접근으로, 추가 요청이 발생하게 됩니다.

다음의 예를 생각해보세요. CMS로부터 어떤 데이터를 가져오기 위해 API route가 사용됩니다. 그리고 그 api route는 직접 getServerSideProps로부터 호출됩니다. 이것은 추가적인 호출이 발생하여 성능을 감소시킵니다. 대신에 api 경로 내에서 사용되는 로직을 getServerSideProps로 직접 가져오세요. 이는 getServerSide 내부에서 직접 CMS, 데이터베이스 또는 기타 API를 호출하는 것을 의미할 수 있습니다.

## 클라이언트 측에서 데이터 가져오기

만약에 페이지의 데이터가 자주 업데이트 되고, 데이터를 미리 렌더링할 필요가 없다면, 클라이언트 측에서 데이터를 가져올 수도 있습니다.  
사용자별 데이터를 예시로 들 수 있습니다.

- 우선 데이터 없이 즉시 페이지를 보여줄 수 있습니다. 페이지의 어떤 부분들은 Static Generation을 통해서 미리 렌더링될 수 있습니다. 데이터를 기다리는 동안 로딩상태를 보여줄 수 있습니다.
- 그리고 클라이언트 측에서 데이터를 가져오고 준비가 되었을 때 보여줄 수 있습니다.

예를 들어 이런 접근은 유저 대시보드 페이지들에 적합합니다. 왜냐하면 대시보드 페이지는 프라이빗하고, 특정 사용자를 위한 페이지고, SEO가 상관없고, 미리 렌더링될 필요도 없기 때문입니다. 데이터는 자주 업데이트 되고, 데이터를 불러올 시간이 필요합니다.

## 요청시에 데이터를 가져오기 위해 getServerSideProps 사용하기

아래의 예제는 어떻게 요청 시에 데이터를 불러올 수 있는지와 결과를 미리 렌더링할 수 있는 지를 보여줍니다.

```js
function Page({ data }) {
  // 데이터를 렌더링합니다.
}

// 매 요청 시 실행됩니다.
export async function getServerSideProps() {
  // 외부 api로부터 데이터 가져오기
  const res = await fetch(`https://.../data`);
  const data = await res.json();

  // props를 통해 data 전달
  return { props: { data } };
}

export default Page;
```

## Server-Side 렌더링 캐싱하기(SSR)

동적 응답을 캐싱하기 위해 getServerSideProps 내부에 캐싱 헤더(Cache-Control)을 사용할 수 있습니다. 예를 들어 [stale-while-revalidate](https://web.dev/stale-while-revalidate/)를 사용할 수 있습니다.

```js
// 이 값은 10초 동안 새로운 것으로 간주됩니다. (s-maxage=10).
// 만약 요청이 10초 이내에 반복된다면 이전의
// 캐싱된 값은 여전히 새로운 것으로 간주됩니다.. 만약에 요청이 59초 이내에 반복된다면,
// 캐싱된 값은 여전히 오래된 것(stale)하지만 여전히 렌더링 됩니다. (stale-while-revalidate=59).
//
// 백그라운드에서 캐시를 채우기 위해 재검증 요청이 이뤄지고, 페이지를 새로고침하면 새 값을 보게됩니다.
export async function getServerSideProps({ req, res }) {
  res.setHeader(
    "Cache-Control",
    "public, s-maxage=10, stale-while-revalidate=59"
  );

  return {
    props: {},
  };
}
```

[캐싱](https://nextjs.org/docs/going-to-production)에 대해 더 배워보세요.

## getServerSideProps가 에러 페이지를 렌더링합니까?

만약에 getServerSideProps 내부에서 에러가 발생했다면, pages/500.js 파일을 보여줍니다. [500 페이지](https://nextjs.org/docs/advanced-features/custom-error-page#500-page)를 어떻게 만드는 지 배우려면 문서를 확인하세요. 개발 중에는 이 파일이 사용되지 않고, dev overlay가 보여집니다.
