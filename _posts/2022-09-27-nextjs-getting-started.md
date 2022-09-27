---
title: "Getting Started"
excerpt: "Next.js의 공식문서에 'Getting Started' 부분을 번역한 글입니다."
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

# Getting Started

Next.js 문서에 온 것을 환영합니다.

만약에 Next.js가 처음이라면 [learn course](https://nextjs.org/learn/basics/create-nextjs-app) 부터 시작하는것을 추천합니다.  
퀴즈로 상호작용할 수 있는 강의가 Next.js에서 당신이 알아야 할 모든것을 알려줄 것입니다.

만약에 Next.js에 관련된 어떤 질문이라도 있다면, [Github Discussions](https://github.com/vercel/next.js/discussions)에 있는 커뮤니티에 언제든지 질문 할 수 있습니다.

### 시스템 요구 사항

- Node.js 12.22.0 또는 그 이상
- MacOS, Windows(WSL을 포함한), and Linux

## Automatic Setup

모든 것을 자동으로 세팅해주는 '**create-next-app**'을 사용해서 새로운 Next.js 앱을 만드는 것을 추천합니다.  
프로젝트를 만들기 위해서는 아래 명령어를 실행하세요.

```cmd
npx create-next-app@latest
# or
yarn create next-app
# or
pnpm create next-app
```

만약 타입스크립트 프로젝트로 시작하고 싶다면 **--typescript**플래그를 사용할 수 있습니다.

```cmd
npx create-next-app@latest --typescript
# or
yarn create next-app --typescript
# or
pnpm create next-app --typescript
```

설치가 완료된 후:

- '**npm run dev**' 또는 '**yarn dev**' 또는 '**pnpm dev**'를 입력해서 'http://localhost:3000'에 개발 서버를 실행시키세요.
- 당신의 앱을 보기 위해선 'http://localhost:3000'에 방문하세요.
- '**pages/index.js**'를 수정하고 브라우저에서 업데이트된 결과를 보세요.

'create-next-app'을 사용하는 방법에 대한 더 많은 정보를 보고 싶으면 문서를 참고하세요.

## Manual Setup

'next', 'react', 'react-dom'을 프로젝트에 설치하세요.

```bash
npm install next react react-dom
# or
yarn add next react react-dom
# or
pnpm add next react react-dom
```

'**package.json**' 파일을 열고 아래 'scripts'를 추가하세요.

```cmd
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint"
}
```

아래 스크립트들은 애플리케이션을 개발하는 여러 단계들을 참조합니다.

- **'dev'** - Next.js의 개발 모드를 시작하면 **'next dev'**를 실행하세요.
- **'build'** - 프로덕션 모드 사용을 위해서 빌드하기 위해서는 **'next build'**를 실행하세요.
- **start** - Next.js의 프로덕션 서버를 시작하기 위해서 **'next start'**를 실행하세요.
- **lint** - Next.js에 내장된 ESLint 설정을 설정하기 위해서 **'next lint'**를 실행하세요.

애플리케이션 root 폴더에 **'pages'**와 **'public'** 폴더를 만들어주세요.

- **'pages'** 해당 폴더는 파일 이름에 기반해서 라우팅을 해줍니다. 예를 들어 **'pages/about.js'**는 '/about' 페이지와 맵핑 됩니다.
- **'public'** - 해당 폴더는 이미지, 폰트 등의 정적인 assets들을 저장할 수 있습니다. 'public' 파일들은 코드에서 base URL('/')로 시작하면 참조할 수 있습니다.

Next.js는 페이지 개념으로 구축되었습니다.  
페이지는 pages 폴더에 있는 js, jsx, ts, tsx 파일에서 내보낸 리액트 컴포넌트 입니다.  
심지어 동적으로 라우팅된 파라미터들을 파일이름으로 사용할 수도 있습니다.

시작하기 위해서는 **'pages'** 폴더 안에 **'index.js'** 파일을 추가해주세요. 이 페이지는 유저가 당신의 애플리케이션의 루트 페이지에("/") 방문할 때 렌더링 됩니다.

**'pages/index.js'**를 아래 컨텐츠로 채워주세요.

```jsx
const HomePage = () => {
  return <div>Welcome to Next.js!</div>;
};

export default HomePage;
```

설정이 완료된 후:

- '**npm run dev**' 또는 '**yarn dev**' 또는 '**pnpm dev**'를 입력해서 'http://localhost:3000'에 개발 서버를 실행시키세요.
- 당신의 앱을 보기 위해선 'http://localhost:3000'에 방문하세요.
- '**pages/index.js**'를 수정하고 브라우저에서 업데이트된 결과를 보세요.

'create-next-app'을 사용하는 방법에 대한 더 많은 정보를 보고 싶으면 문서를 참고하세요.
