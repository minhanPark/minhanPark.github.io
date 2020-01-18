---
title: "styled-components를 사용해보자"
excerpt: "실전에서 사용할 수 있도록 styled-components의 주요 사용법들을 정리해봅시다. "
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - React
  - styled-components
---

스타일 컴포넌트는 아주 유용한 모듈 중의 하나입니다. 간단하게 리액트나 리액트 네이티브 환경에서  
자바스크립트 만으로 컴포넌트를 스타일링 할 수 있습니다. 그래서 styled-components의 주요 기능들을 정리해봤습니다.  

## styled-components 설치하기

프로젝트 안에서 아래의 명령어를 통해서 styled-components를 설치할 수 있습니다.

```
npm i styled-components
```

설치 부분에서 styled-components의 특징을 간략히 설명하자면  
1. 컴포넌트와 결합되어 있는 형태이기 때문에 css 코드를 관리하기 아주 편해집니다.
2. className 등도 알아서 관리해주기 때문에 아주 편해집니다.
3. vendor prefixing을 자동으로 해줍니다.

이러한 장점이 있기 때문에 리액트 프로젝트를 시작할 때 아주 편하게 환경을 구성할 수 있습니다.  

## 기본적인 사용방법

```js
import styled from "styled-components";

const StyledHeader = styled.h1`
  font-size: 40px;
  font-weight: 700;
`;

// 사용하실 땐
<StyledHeader>Hello World</StyledHeader>
```

위와 같이 styled에는 각 태그들이 정의되어 있습니다. ``을 이용해서 태그들에 css값을 전달해주면 됩니다.  
모든 css값들을 편하게 적으시면 되고, 클래스나 벤더 프리픽스에 대해서도 신경끄셔도 됩니다. 아주 편하죠?

## styled-components 확장하기

기존의 정의된 컴포넌트를 확장할 수도 있습니다.  위에선 Header 컴포넌트를 정의했습니다.  
색깔별로 여러 Header 컴포넌트가 만들어져야 한다고 생각해본다면 확장 기능을 쉽게 이해할 수 있습니다.

```js
const BlueHeader = styled(StyledHeader)`
  color: blue;
`;
```

기존의 StyledHeader에 파란색을 주고 BlueHeader라는 컴포넌트를 만들었습니다. 해당 컴포넌트는 기존의 StyledHeader이 파란색으로 나온다고 생각하시면 됩니다. 만약에 기존의 컴포넌트에 색상이 있었다면 확장한 컴포넌트의 css 값으로 덮어씌워져서 나오게 됩니다.

## props를 활용해서 상황에 따라 값주기

styled-components는 상황에 따라 값을 달리할 수 있습니다.
```js
const Header = () => <StyledHeader color={false}>Hello World</StyledHeader>;
```
위와 같은 코드가 있다고 생각해봅시다. 해당 StyledHeader에 color라는 props 값을 전달했습니다.  
styled-components를 정의할 때 ${함수}형태로 해당 값을 받아올 수 있습니다.  
```js
const StyledHeader = styled.h1`
  font-size: 40px;
  font-weight: 700;
  color: ${props => (props.color ? "red" : "grey")};
`;
```
해당 코드는 color의 불린값에 따라 색이 달라집니다.  
또다른 형태로는 **props 값을 통해 효과를 줄지 말지를 결정**할 수 있습니다.

```js
const Header = () => (
  <StyledHeader color={false} bg={true}>
    Hello World
  </StyledHeader>
);
```
해당 컴포넌트에 bg라는 props도 만들어서 값을 전달했습니다. 해당 값이 true면 배경을 주고, false면 배경을 주지 않는 컴포넌트를 만들려면 어떻게 해야할까요 ?

```js
import styled, { css } from "styled-components";

const StyledHeader = styled.h1`
  font-size: 40px;
  font-weight: 700;
  color: ${props => (props.color ? "red" : "grey")};
  ${props =>
    props.bg &&
    css`
      background-color: blue;
    `}
`;
```

위와 같은 형태로 styled-components에서 css를 불러와서 값을 주면 됩니다. 이제 StyledHeader는 bg값이 true일때 파란색 백그라운드를 갖게 됩니다.  

## 애니메이션 주기

keyframes도 불러와서 사용하면 됩니다.  
```js
import styled, { keyframes } from "styled-components";

const rotate = keyframes`
    from {
        transform: rotate(0deg);
    }

    to {
        transform: rotate(360deg);
    }
`;

const StyledHeader = styled.h1`
  font-size: 40px;
  font-weight: 700;
  color: ${props => (props.color ? "red" : "grey")};
  animation: ${rotate} 2s linear infinite;
`;
```

여기서는 rotate의 위치가 중요합니다. 먼저 정의한 후 밑에 컴포넌트에서 사용하셔야 합니다.  

## 전역으로 css 값 주기

컴포넌트를 스타일링 할뿐만 아니라 body 등 전역적으로 style을 줄 수도 있습니다.  

```js
import { createGlobalStyle } from "styled-components";

const GlobalStyle = createGlobalStyle`
  body {
    background-color: black;
  }
`;
```

해당 형태처럼 createGlobalStyle를 불러와서 안에 body의 css를 정의했습니다.  
```js
function App() {
  return (
    <>
      <GlobalStyle />
      <div className="App">
        <Header />
      </div>
    </>
  );
}
```
그리고 App 컴포넌트에 이렇게 GlobalStyle 컴포넌트를 전달해주면 body에 배경이 검은색이 된 것을 확인할 수 있습니다.  

## 리액트 네이티브에서 사용하기

styled-components는 리액트 네이티브에서도 사용할 수 있습니다.

```js
import styled from 'styled-components/native'
```

위와 같은 형태로 불러와서 사용하시면 됩니다.  
불러온 다음에는 리액트 네이티브의 View나 Text 등을 그대로 사용하시면 됩니다.
```js
const StyledText = styled.Text`
    font-size: 20px;
`;

const StyledView = styled.View``;
```
css 값을 줄 때는 자바스크립트 변수 형태(fontSize)가 아니라 기존의 css 형태(font-size)로 주시면 됩니다.

~~꼭 /native를 붙이지 않고 import styled from 'styled-components'; 로 해도 잘 적용됩니다.~~

