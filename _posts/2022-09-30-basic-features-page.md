---
title: "Next.js Pages"
excerpt: "Next.js의 공식문서에 Basic Features의 'Pages' 부분을 번역한 글입니다."
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

# Pages

Next.js에서 페이지는 pages 폴더에 있는 js, jsx, ts, tsx 파일에서 내보낸 리액트 컴포넌트 입니다. 각 페이지는 파일 이름에 기반해서 라우팅 되어 있습니다.

**예시:** 만약에 아래와 같이 리액트 컴포넌트로 내보내는 'pages/about.js' 파일을 만든다면 '/about' 페이지에서 접근할 수 있습니다.

```jsx
function About() {
  return <div>About</div>;
}

export default About;
```

## Pages with Dynamic Routes(동적 라우팅이 되는 페이지)

Next.js는 동적 라우팅을 지원합니다. 예를 들어 만약 **'pages/posts/[id].js'** 파일을 만든다면 posts/1 또는 posts/2 등과 같이 접근할 수 있게 됩니다.

## Pre-rendering(미리 렌더링 하기)

기본적으로, Next.js는 모든 페이지들을 미리 렌더링합니다. 이것이 의미하는 바는 Next.js는 클라이언트 측의 자바스크립트로 모든 작업을 처리하는 대신 각 페이지를 위한 HTML을 미리 생성한다는 것입니다. 미리 렌더링 하는 것은 더 나은 퍼포먼스와 SEO를 가져올 수 있습니다.

각 생성된 HTML 파일은 페이지에 필요한 최소한의 자바스크립트 코드와 연결됩니다. 그리고 페이지가 브라우저에 의해서 로드되었을 때 자바스크립트 코드가 실행되고, 페이지를 완전히 상호작용하도록 만들어줍니다.( 이 과정을 hydration(수화 과정) 이라고 부릅니다. )

## Two forms of Pre-rendering(미리 렌더링하기의 두 가지 형태)

Next.js에는 두 가지 형태의 미리 렌더링하는 방법이 있습니다. 그 방법은 **Static Generation**과 **Server-side Rendering** 입니다.

- Static Generation (추천): HTML 파일이 **빌드할 때** 생성되고, 각 요청마다 재사용됩니다.
- Server-side Rendering: HTML이 **각 요청마다** 생성됩니다.

중요한건 Next.js에서는 각 페이지마다 사용하고 싶은 미리 렌더링하는(pre-rendering) 형태를 선택할 수 있다는 것입니다.  
당신은 대부분의 페이지를 Staic Generation을 사용하고, 나머지 부분을 Server-side Rendering을 사용해서 "하이브리드"한 Next.js 앱을 만들 수 있습니다.

우리는 성능 상의 이유로 Server-side Rendering 보다는 Static Generation을 사용하기를 추천합니다. 물론 경우에 따라서는 Server-side Rendering이 유일한 선택사항일 수 있습니다만 정적으로 생선된 페이지는 성능 향성을 위한 추가 설정 없이 CDN에서 캐시될 수 있습니다.

또한 Static Generation과 Server-side Rendering과 함께 Client-side에서 data 가져오기도 가능합니다. 이것은 어떤 페이지들은 전체적으로 클라이언트 측에서 자바스크립트에 의해서 렌더링 될 수 있다는 것을 의미합니다. 해당 부분을 더 배우려면 [데이터 갖고오기](https://nextjs.org/docs/basic-features/data-fetching/client-side) 부분을 보세요.

## Static Generation (추천)

만약에 페이지가 **Static Generation**을 사용하면, HTML은 빌드할 때 생성됩니다. 이것이 의미하는 바는 페이지의 HTML 파일이 **'next build'**를 실행했을 때 생성되었다는 것입니다. 이 HTML 파일은 각 요청마다 재사용 될 것이고, CDN에 의해서 캐싱될 수 있습니다.

Next.js에서 데이터가 있거나 없어도 정적으로 페이지를 생성할 수 있습니다. 아래에서 각각의 경우를 보겠습니다.

## 데이터 없이 Static Generation

기본적으로 Next.js는 데이터를 가지고 오지 않고 Static Generation을 사용해 페이지를 미리 렌더링합니다.

```jsx
function About() {
  return <div>About</div>;
}

export default About;
```

이 페이지는 미리 불러올 외부 데이터가 필요하지 않다는 것을 주목하세요. 이러한 경우에 Next.js는 빌드하는 동안 각 페이지 마다 단일 HTML 파일을 만듭니다.

## 데이터를 가지고 Static Generation 사용

몇몇 페이지는 미리 렌더링을 하기 위해서 외부 데이터를 가져오는 것이 필요합니다. 여기엔 두가지 시나리오가 있는데, 하나 또는 둘다 적용될 수 있습니다. 각각의 경우에 Next.js가 제공하는 기능들을 사용할 수 있습니다.

- 페이지 컨텐츠가 외부 데이터에 의존한다면 **'getStaticProps'**를 사용하세요.
- 페이지 경로가 외부가 데이터에 의존한다면 **'getStaticPaths'**를 사용하세요.(보통 **'getStaticProps'**도 같이 사용합니다)

## 시나리오 1 : 페이지 컨텐츠가 외부 데이터에 의존하고 있음

**예시**: 당신의 블로그 페이지가 CMS(content management system)로 부터 블로그 포스트의 리스트를 가지고 와야할 수도 있습니다.

```jsx
// TODO: 이 페이지가 미리 렌더링 되기 전에 (api를 이용해서) 포스트 데이터들을 들고 오기
function Blog({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li>{post.title}</li>
      ))}
    </ul>
  );
}

export default Blog;
```

미리 렌더링 시 posts 데이터를 불러오기 위해서 Next.js는 같은 파일에서 **'exports'**와 **'getStaticProps'**라는 **'async'** 함수를 이용할 수 있습니다. 이 함수는 빌드할 때 불려지고, 미리 렌더링할 때 필요한 페이지의 **props**에 불러온 데이터를 전달합니다.

```jsx
function Blog({ posts }) {
  // 포스트를 관련 ui 렌더링
}

// 이 함수는 빌드할 때 불려집니다.
export async function getStaticProps() {
  // posts를 불러오기 위해서 외부 api를 부릅니다.
  const res = await fetch("https://.../posts");
  const posts = await res.json();

  // { props: {posts} }를 리턴함으로써, Blog 컴포넌트는 posts라는 prop를 빌드할 때 받게 됩니다.
  return {
    props: {
      posts,
    },
  };
}

export default Blog;
```

어떻게 **'getStaticProps'**가 작동하는지에 대해서 더 배우려면 데이터 갖고오기 문서를 확인하세요.

## 시나리오2: 페이지의 경로가 외부 데이터에 의존할 때

Next.js는 동적 라우팅을 통해서 페이지를 만들 수 있도록 해줍니다. 예를들어, **'id'**를 기반으로 블로그 포스트를 보여줄 수 있도록 **'pages/posts/[id].js'** 파일을 만들 수 있습니다. 이것은 사용자가 **'posts/1'**에 들어왔을 때 **'id: 1'**인 블로그 포스트를 보여줄 것입니다.

> 동적 라우팅에 대해서 더 배우려면 [동적 라우팅 문서](https://nextjs.org/docs/routing/dynamic-routes)를 확인하세요.

그러나 빌드할 때 미리 렌더링할 id는 외부 데이터에 따라 다를 수 있습니다.

**예시:**: 만약 데이터베이스에 (id가 1인) 블로그 포스트 1개를 추가했다고 생각해보세요. 이런 경우에 당신은 빌드할 때 'posts/1'만 미리 렌더링하기를 원할 것입니다.  
후에, id가 2인 포스트를 추가했습니다. 그러면 'posts/2'더 미리 렌더링되기를 원할 것입니다.  
즉 미리 렌더링할 페이지 경로는 외부 데이터에 의존하고 있습니다. 이것을 다루기 위해서 Next.js는 동적인 페이지(위와 같은 경우엔 **'pages/posts/[id].js'**)에서 **'getStaticPaths'**라는 **'async'** 함수를 **'export'**할 수 있습니다. 이 함수는 빌드할 때 실행되고, 미리 렌더링하고 싶은 경로(path)들을 지정할 수 있습니다.

```jsx
// 빌드할 때 사용됩니다.
export async function getStaticPaths() {
  // posts를 얻기 위해서 외부 api를 요청합니다.
  const res = await fetch("https://.../posts");
  const posts = await res.json();

  // Get the paths we want to pre-render based on posts
  // posts에 기반해서 미리 렌더링할 경로를 가져옵니다.
  const paths = posts.map((post) => ({
    params: { id: post.id },
  }));

  // 빌드할 때 경로들만 미리 렌더링합니다.
  // { fallback: false } means other routes should 404.
  // { fallback: false }은 맞는 아이디가 없으면 404를 던진다는 의미입니다.
  return { paths, fallback: false };
}
```

또한 **'pages/posts/[id].js'**에서 **getStaticProps**도 사용해서 id에 맞는 포스트 데이터를 불러올 수 있고, 페이지도 미리 렌더링할 수 있습니다.

```jsx
function Post({ post }) {
  // post 렌더링
}

export async function getStaticPaths() {
  // ...
}

// 이 함수도 빌드할 때 불러집니다.
export async function getStaticProps({ params }) {
  // params는 포스트의 'id'를 담고 있습니다.
  // 만약에 route가 /posts/1이라면 params의 id는 1입니다.
  const res = await fetch(`https://.../posts/${params.id}`);
  const post = await res.json();

  //props를 통해서 페이지에 post 데이터를 전달합니다.
  return { props: { post } };
}

export default Post;
```

> 어떻게 **'getStaticPaths'**가 작동하는 지에 대해 더 배우려면 [데이터 가져오기 문서](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths)를 확인하세요.

## 언제 Static Generation을 사용하나요?

우리는 (데이터가 있든 없든) 가능하면 Static Generation을 사용하기를 추천합니다. 왜냐하면 페이지가 일단 만들어진 후에 CDN에 의해 제공될 수 있게 되면 매 요청마다 페이지를 서버에서 제공하는 것보다 훨씬 빠르기 때문입니다.

아래와 같이 많은 타입의 페이지에서 Static Generation을 사용할 수 있습니다.

- 마케팅 페이지들
- 블로그 포스트나 포트폴리오
- 이커머스 제품 리스트들
- 도움말 및 문서 페이지

자신에게 물어보세요. "유저가 요청하기 전에 이 페이지를 미리 렌더링할 수 있을까?" 만약 대답이 'yes'라면 Static Generation을 선택해야합니다.

반면에 유저의 요청 전에 미리 페이지를 렌더링할 수 없다면 Static Generation은 좋은 생각이 아닙니다. 당신의 페이지가 자주 업데이트된 데이터를 보여주거나 매 요청마다 페이지의 컨텐츠가 바뀐다면 말입니다.

이런 경우들엔 아래에서 하나를 선택할 수 있습니다.

- Static Generation과 함께 **클라이언트 측에서 데이터 갖고오기**를 사용하세요. 어떤 부분들은 미리 렌더링하는 것을 건너뛰고, 클라이언트 측에서 채워넣으세요. 여기에 대해서 좀 더 배우려면 데이터 가져오기 문서를 참고하세요.

- Server-Side Redering을 사용하세요. Next.js는 각 요청마다 페이지를 미리 렌더링합니다. 페이지를 CDN으로 캐싱할 수 없어서 느려지지만 미리 렌더링된 페이지는 항상 최신상태입니다. 아래에서 더 얘기하겠습니다.

## Server-side Rendering

> "SSR"이나 동적 렌더링(Dynamic Rendering)이라고 언급되기도 합니다.

만약에 페이지가 Sever-side Rendering을 사용한다면, 페이지의 HTML은 매 요청시 마다 생성됩니다.

페이지에서 Server-side Rendering을 사용하려면 **'getServerSideProps'**라는 **'async'** 함수를 **'export'**해야합니다. 이 함수는 매 요청마다 서버에서 불려집니다.

예를 들어 (외부 api로부터 가져오는) 자주 업데이트되는 데이터를 페이지에서 미리 렌더링하기 위해서 **'getServerSideProps'**를 사용해 데이터를 갖고오고 아래처럼 **'Page'** 컴포넌트에 전달할 수 있습니다.

```jsx
function Page({ data }) {
  // data에 대한 것들 렌더링
}

// 매 요청마다 불러집니다.
export async function getServerSideProps() {
  // 외부 api로부터 데이터를 가져옵니다.
  const res = await fetch(`https://.../data`);
  const data = await res.json();

  // props를 통해서 Page 컴포넌트에 data를 전달합니다.
  return { props: { data } };
}

export default Page;
```

보다시피 **'getServerSideProps'**는 **'getStaticProps'**와 비슷하지만, 차이점은 **'getServerSideProps'**는 빌드할 때가 아니라 매 요청시에 실행됩니다.

## 요약

Next.js에서 미리 렌더링하는 두가지 형태에 대해서 얘기했습니다.

- **Static Generation (추천)**: HTML 파일이 빌드할 때 생성되고 매 요청시마다 재사용됩니다. 페이지가 Static Generation을 사용하도록 만들려면, 페이지의 컴포넌트를 export 하거나 'getStaticProps'를 export 해야합니다(필요하다면 'getStaticPaths'도 export 해야합니다.)  
  유저의 요청 전에 미리 렌더링할 수 있는 페이지에 좋고, 추가적인 데이터를 불러오기 위해서 Client-side Rendering을 함께 사용할 수 있습니다.

- **Server-side Rendering**: HTML 파일이 각 요청마다 생성됩니다. 페이지가 Server-side Rendering을 사용하도록 하기 위해선 'getServerSideProps'를 export 해야합니다. Server-side Rendering은 Static Generation보다 더 낮은 퍼포먼스를 초래하기 때문에 필수적인 경우에만 사용하세요.
