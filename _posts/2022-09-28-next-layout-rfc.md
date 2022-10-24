---
title: "Next.js Layout RFC"
excerpt: "Next.js 블로그에 올라와있는 Layouts RFC에 대한 번역글입니다."
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

# Layouts RFC

> 해당 글은 Next.js에서 Layout RFC에 대한 번역본입니다.  
> [원문 보러 가기](https://nextjs.org/blog/layouts-rfc)

이 RFC(Request for Comment)는 Next.js가 2016년에 도입된 이후로 가장 큰 업데이트를 간략하게 설명합니다.

- **중첩된 레이아웃(Nested Layouts)**: 중첩 라우트들(nested routes)로 복잡한 앱을 만들어보세요.
- **서버 컴포넌트를 위한 설계**: 하위 트리 탐색에 최적화되었습니다.
- **향상된 데이터 가져오기**: 폭포(waterfalls)를 피하면서 레이아웃에서 데이터를 가져오세요.
- **React 18의 기능들을 사용하기**: Streaming, Transition과 Suspense.
- **클라이언트와 서버 라우팅**: SPA와 같이 동작하는 서버 중심 라우팅.
- **100% 점진적으로 적용가능**: 큰 변화를 주지 않고, 점진적으로 적용할 수 있습니다.
- **향상된 라우팅 패턴**: 평행 라우팅(parallel routes), 라우팅 가로채기(intercepting routes)와 기타 등등

새로운 Next.js의 라우터는 최근에 출시된 React 18의 기능 위에 구축됩니다. 쉽게 적용하고 이번에 잠금해제된 기능들의 효과를 누리기 위해서 우리는 기본값과 규약(conventions)을 도입할 계획입니다.

## 목차

- 동기
- 용어
- 현재의 라우팅 작동 방식
- app 디렉토리
- 라우팅 정의하기
- 레이아웃
- 페이지들
- 리액트의 서버 컴포넌트
- 데이터 가져오기
- 경로 그룹(new)
- 서버 중심 라우팅(new)
- 즉각적인 로딩 상태(new)
- 에러 다루기(new)
- 템플릿(new)
- 향상된 라우팅 패턴(new)
- 결론

## 동기

우리는 깃허브, 레딧과 개발자 설문조사에서 현재 Next.js의 라우팅의 한계에 대해서 피드백을 모아왔고, 아래와 같은 사실들을 발견했습니다.

- 레이아웃을 만드는 것이 개발자 경험을 향상 시킬수 있고, 레이아웃은 중첩되고 경로들을 통해서 공유되기 때문에 만들기 쉬워야 하고, 상태값도 보존할 수 있어야 한다
- 많은 Next.js의 애플리케이션들이 대시보드 또는 콘솔이며 향상된 라우팅 솔루션들의 이점을 누릴 수 있다.

현재의 라우팅 시스템은 Next.js를 시작한 이후로 잘 작동해 왔지만, 우리는 개발자들이 성능이 뛰어나고 기능이 풍부한 웹 애플리케이션을 만들기를 원합니다.

프레임워크 관리자로서 우리는 이전 버전과 호환되고, React의 미래와 일치하는 라우팅 시스템을 구축하고자 합니다.

## 용어

이번 RFC는 새로운 라우팅 협약과 문법을 소개합니다. 용어는 React와 웹 플랫폼 표준을 기반으로 하고 있습니다. RFC 전반에 아래 정의와 연열된 용어들을 볼 수 있습니다.

- **Tree**: 계층구조를 시각화 하기 위한 협약입니다. 예를들면 부모와 자식 컴포넌트를 가진 컴포넌트 트리나 폴더 구조 등이 있습니다.
- **Subtree**: 뿌리(처음)에서 시작하여 잎(마지막)에서 끝나는 나무의 일부.

![트리 구조](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Fterminology.png&w=1920&q=75)

- **URL 경로(Path)**: 도메인 뒤에 오는 URL의 일부.
- **URL Segment**: 슬래시(/)로 구분된 URL 경로의 일부.

![url구조](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Furl-anatomy.png&w=1920&q=75)

## 현재의 라우팅 작동 방식

오늘날, Next.js는 파일 시스템을 사용하여 **Pages 폴더** 안에 있는 폴더들과 파일들을 URL들을 통해 접근할 수 있는 경로와 연결합니다. React 컴포넌트로 부터 나온 각 페이지 파일은 자신의 파일이름에 기반해서 경로를 가지고 있습니다.  
예를들면:

![router구조](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Frouting-today.png&w=1920&q=75)

- **동적 라우팅**: Next.js는 **[param].js**, **[...param].js**, **[[...param]].js** 규칙들을 통해서 (모든 변형을 포함하는) 동적 라우팅을을 지원합니다.
- **레이아웃**: Next.js는 [컴포넌트 기반](https://nextjs.org/docs/basic-features/layouts)의 레이아웃을 지원합니다. 걱 페이지는 컴포넌트의 속성을 이용해서 레이아웃을 줄 수 있고, 단일 레이아웃을 줄 수도 있습니다.
- **데이터 가져오기**: Next.js는 페이지(또는 route) 단계에서 사용할 수 있는 데이터를 갖고오는 방법(**'getStaticProps'**, **'getServerSideProps'**)이 있습니다. 이 방법들은 페이지를 정적으로 생성(**'getStaticProps'**)해야 하는지 서버측에서 렌더링(**'getServerSideProps'**)되어야 하는지 여부를 따라 사용됩니다. 게다가 [Incremental Static Regeneration(ISR)](https://nextjs.org/docs/basic-features/data-fetching/incremental-static-regeneration)도 사이트 빌드 후에 정적 파일들을 업데이트 하거나 생성하는데 사용할 수도 있습니다.
- **렌더링**: Next.js는 Static Generation, Server-side Rendering, Client side Rendering이라는 세가지 렌더링 방식을 제공합니다. 기본적으로 페이지들은 데이터 가져오는 것을 막는 것('getServerSideProps')이 없다면 정적으로 생성됩니다.

## 'app' 폴더 소개

새로운 기능들이 점진적으로 적용되고, 큰 변화를 피하기 위해서 우리는 'app'이라는 폴더를 새로 만들었습니다.  
![app 디렉토리](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Fapp-folder.png&w=3840&q=75)

'app' 폴더는 'pages'폴더와 함께 작동합니다. 당신은 점진적으로 애플리케이션의 부분들을 'app' 폴더로 옮겨서 새로운 기능들의 혜택을 받을 수 있습니다. 이전 버전과의 호환을 위해서, 'pages' 폴더는 계속 유지될 것이고, 앞으로도 꾸준히 지원될 것입니다.

## 라우팅 정의하기

라우팅을 하기 위해 'app' 디렉토리 내부에서 폴더 구조를 사용할 수 있습니다. 경로(route)는 단일 경로(path)이며 루트 폴더에서 아래에 있는 최종 리프(leaf) 폴더 구조를 따라갑니다.  
![라우팅정의](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Froutes.png&w=3840&q=75)  
예를 들어, **'/dashboard/settings'**라는 새로운 경로를 추가하기 위해서는 'app' 디렉토리 안에 두개의 새 폴더를 중첩하면 됩니다.(dashboard와 settings)

Note:

- 이 시스템을 사용하면, 폴더를 사용해 라우팅을 하고 파일을 통해 UI를 보여줄 수 있습니다.(새로운 파일 컨벤션인 layout.js, pages.js와 loading.js)
- 또한 이런 구조는 'app' 디렉토리 내부에 프로젝트 파일(UI 컴포넌트, 테스트 파일, 스토리 등)들을 통합할 수 있게 해줍니다. 'app' 디렉토리를 활용하지 않으면 현재 통합은 [pageExtensions config](https://nextjs.org/docs/api-reference/next.config.js/custom-page-extensions#including-non-page-files-in-the-pages-directory)를 통해서만 가능합니다.

## 라우트 영역(Route Segment)

서브트리에 있는 각 폴더는 라우트의 영역을 나타냅니다. 각 라우트의 영역(Segment)는 URL 경로의 영역과 연결됩니다.  
![라우트 영역](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Froute-segments.png&w=3840&q=75)  
예를 들어, **'/dashboard/settings'**는 3가지 영역으로 구성되어 있습니다.

- **'/'** 루트 영역
- **'/dashboard'** 영역
- **'/settings'** 영역

> **경로 영역(route segment)**라는 이름은 URL 경로(paths)에 존재하는 용어를 선택했습니다.

## 레이아웃

지금까지 폴더를 사용해서 애플리케이션의 라우팅을 정의했습니다. 그러나 빈 폴더는 아무것도 하지 못합니다. 이제는 어떻게 새로운 컨벤션을 사용해서 UI를 보여줄 수 있을 지 얘기해보겠습니다.

레이아웃은 서브트리에 있는 라우트 영역 사이에 공유되어지는 UI 입니다. 레이아웃은 URL 경로에는 영향을 주지 않고, 사용자가 형제 경로(segment)를 탐색할 때 다시 렌더링 되지 않습니다.(React의 상태값이 유지됩니다)

레이아웃은 **'layout.js'**파일에 리액트 컴포넌트로 기본적으로 정의할 수 있습니다. 레이아웃 컴포넌트는 **'children'** prop를 허용해서 레이아웃 컴포넌트가 감싸고 있는 부분(Segment)을 채울 수 있어야 합니다.

두 가지 레이아웃이 있습니다.

- **루트 레이아웃**: 모든 라우트에 적용됩니다.
- **일반 레이아웃**: 특정 라우트에 적용됩니다.

레이아웃을 중첩시키기 위해서 두개 또는 더 많은 레이아웃을 함께 사용할 수 있습니다.

## 루트 레이아웃

애플리케이션의 모든 라우트에 적용될 수 있도록 루트 레이아웃을 만들기 위해서는 **'app'** 폴더의 내부에 **'layout.js'**파일을 만드세요.  
![루트 레이아웃](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Froot-layout.png&w=3840&q=75)

> - 루트 레이아웃은 모든 경로에 적용되므로 custom App('\_app.js')와 custom Document('\_document.js')의 필요성을 교체합니다.  
>   루트 레이아웃을 사용해서 초기 문서의 shell(예를 들어 html 태그나 body 태그)을 사용자 정의로 지정할 수 있습니다.  
>   루트 레이아웃(다른 레이아웃에서도)에서 데이터 갖고오기(fetch data)를 할 수 있습니다.

## 일반 레이아웃

특정 폴더 내부에서 **'layout.js'**를 만들어서 애플리케이션의 한 부분에 적용될 수 있는 레이아웃을 만들수도 있습니다.

![일반 레이아웃](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Fregular-layouts.png&w=3840&q=75)

예를 들어 **'layout.js'** 파일을 **'dashboard'** 폴더 안에 만들면 **'dashboard'** 폴더의 내부에 있는 경로들로만 적용될 것입니다.

## 레이아웃 중첩하기

레이아웃은 기본적으로 **중첩**됩니다.

![레이아웃 중첩](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Fnested-layouts.png&w=3840&q=75)

예를들어 위와 같이 두 개의 레이아웃을 결합하면, 루트 레이아웃(**'app/layout.js'**)는 **'dashboard'**와 내부에 있는 경로(**/dashboard/\***)에도 레이아웃이 적용됩니다.

![중첩 레이아웃](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Fnested-layouts-example.png&w=3840&q=75)

## 페이지

새로운 파일 협약: **pages.js**  
페이지는 경로에 고유한 UI입니다. 폴더 내부에 **page.js** 파일을 만들어서 페이지를 생성할 수 있습니다.  
![페이지 구조](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Fpage.png&w=3840&q=75)

예를들어, '/dashboard/\*' 에 맞는 페이지를 만들려면 각 폴더에 'page.js' 파일을 만들면 됩니다. 유저가 '/dashboard/settings'에 접속하면 Next.js는 'settings' 폴더에 있는 'page.js' 파일을 레이아웃에 감싸서 보여줄 것입니다.
![페이지](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Fpage-example.png&w=3840&q=75)

또한 '/dashboard'에 접속했을 때 보여줄 수 있도록 dashboard폴더에 직접 'page.js' 파일을 만들 수 있습니다.
![대시보드 페이지](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Fpage-nested.png&w=3840&q=75)

이 경로는 2 개로 구성되어 있습니다.

- 루트('/') 경로
- 대시보드('dashboard') 경로

> 경로가 유효하려면 리프 부분(leaf segment)에 페이지가 있어야 합니다. 그렇지 않으면 경로에 오류가 발생합니다.

## 레이아웃과 페이지의 작동 방식

- **'js|jsx|ts|tsx'** 파일 확장자들이 페이지와 레이아웃에 사용될 수 있습니다.
- 페이지 컴포넌트는 'page.js'에서 default export 되어야 합니다.
- 레이아웃 컴포넌트는 'layout.js'에서 default export 되어야 합니다.
- 레이아웃 컴포넌트는 'children' props를 사용해야 합니다.

레이아웃 컴포넌트가 렌더링 될 때 'children' props는 서브트리의 레이아웃이나 페이지로 채워지게 됩니다.

예제:  
![렌더링구조](https://nextjs.org/_next/image?url=%2Fstatic%2Fblog%2Flayouts-rfc%2Fbasic-example.png&w=3840&q=75)

```jsx
// 루트 레이아웃 (app/layout.js)
// - 모든 라우트에 적용됨
export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <Header />
        {children}
        <Footer />
      </body>
    </html>
  )
}

// 일반 레이아웃 (app/dashboard/layout.js)
// - app/dashboard/* 에만 적용됨
export default function DashboardLayout({ children }) {
  return (
    <>
      <DashboardSidebar />
      {children}
    </>
  )
}

// 페이지 컴포넌트 (app/dashboard/analytics/page.js)
// - `app/dashboard/analytics` 경로에 위차한 ui
// - `acme.com/dashboard/analytics` URL 경로에 접속하면 보여집니다.
export default function AnalyticsPage() {
  return (
    <main>...</main>
  )
}
```

위의 레이아웃과 페이지 조합은 아래 컴포넌트 구조처럼 렌더링됩니다.

```jsx
<RootLayout>
  <Header />
  <DashboardLayout>
    <DashboardSidebar />
    <AnalyticsPage>
      <main>...</main>
    </AnalyticsPage>
  </DashboardLayout>
  <Footer />
</RootLayout>
```
