---
title: "react-router-dom을 활용해 SPA를 만들어보자."
excerpt: "싱글 페이지 애플리케션을 만들 수 있게 react-router-dom의 사용방법을 알아봅시다."
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

SPA는 싱글 페이지 애플리케이션의 줄인말로 한 개의 페이지로 이루어진 애플리케이션입니다.  
하지만 싱글 페이지라고 해서 하나의 페이지만 보여주는 것이 아니라 react-router-dom을 통해서 여러 페이지를 보여줄 수 있습니다.

## 설치방법

```
yarn add react-router-dom
```

해당 명령어를 프로젝트 내에서 실행해주세요. 현재는 5.x 버전입니다.

## 라우터 불러오기

라우터로 App 컴포넌트를 감싸면 컴포넌트 내에서 사용이 가능합니다.

```js
// index.js

import { BrowserRouter as Router } from "react-router-dom";

ReactDOM.render(
  <Router>
    <App />
  </Router>,
  document.getElementById("root")
);
```

## Route로 컴포넌트 연결하기

이제 Route로 이동할 컴포넌트들을 만들어주시면 됩니다.

```js
// Home.js

const Home = () => {
  return (
    <div>
      <h1>홈</h1>
    </div>
  );
};
```

```js
// About.js

const About = () => {
  return (
    <div>
      <h1>어바웃</h1>
    </div>
  );
};
```

```js
// App.js

function App() {
  return (
    <div>
      <Route path="/" component={Home} exact={true} />
      <Route path={["/about", "/info"]} component={About} />
    </div>
  );
}
```

Route의 props를 확인해보겠습니다. path는 주소를 나타냅니다. "/"은 기본주소를 , "/about"은 "도메인/about"을 나타내는 것입니다.  
배열을 전달한 이유는 무엇일까요? **path에 배열을 전달하면 해당 배열의 주소들에 같은 컴포넌트들을 연결**시켜줍니다.  
exact의 역할은 무엇일까요? "/about"에도 "/"이 포함되어 있기때문에 그런 경우 만든 컴포넌트 둘다가 화면에 렌더링 됩니다. 그것을 막기위해  
**정확히 같은 주소일 때만 렌더링 되게 만들어주는 역할**을 합니다.

## Link와 NavLink 사용하기

주소를 입력하지 않고 페이지 이동은 어떻게 할 수 있을까요?  
a태그를 사용하면 페이지가 이동할 때마다 다시 처음부터 렌더링 되기 시작합니다. 그래서 react-router-dom에서는 Link와 NavLink 컴포넌트를 제공해줍니다.

```js
import { Link, NavLink } from "react-router-dom";

<ul>
  <li>
    <Link to="/">홈</Link>
  </li>
  <li>
    <Link to="/about">소개</Link>
  </li>
</ul>;
```

위와 같이 to에 주소를 전달하면 페이지를 처음부터 렌더링하지 않고, 주소를 바꾸어줍니다.  
그럼 NavLink는 어떨 때 사용할 수 있을까요? 네비게이션에는 보통 선택된 페이지에 있을 때 네비게이션에 부분적으로 효과를 줍니다.  
NavLink는 선택된 컴포넌트일 때 부분적으로 효과를 주게 됩니다.

```js
import { Link, NavLink } from "react-router-dom";

const activeStyle = {
  color: "red"
};

<ul>
  <li>
    <NavLink activeStyle={activeStyle} to="/">
      홈
    </NavLink>
  </li>
  <li>
    <NavLink activeStyle={activeStyle} to="/about">
      소개
    </NavLink>
  </li>
</ul>;
```

위와 같이 activeStyle에 css값을 전달해주면 됩니다. 또 NavLink의 경우 선택되었을 때(액티브 되었을 때) active라는 클래스를 가지게 됩니다.  
className을 바꾸고 싶을 때는 activeClassName에 새로운 이름을 전달해주시면 됩니다.

## 파라미터와 쿼리 확인하기

주소에서 /about/fitness 등 "/" 형태로 오는 것을 파라미터라고 하고, ?nickname=runningwater 형태로 오는것을 쿼리라고 합니다.  
Route에 전달된 컴포넌트들은 props를 확인해보시면 **history, location, match**가 전달됩니다.  
location에서는 쿼리를, match에서는 파라미터를 확인할 수 있습니다.

```js
<Route path="/about/:category" component={Category} />
```

위와 같이 ":"을 통해서 파라미터 변수를 넣으면 해당 컴포넌트로 이동했을 때 **match.params** 객체에 값이 있는 것을 확인할 수 있습니다.  
쿼리는 location.search에서 값을 확인할 수 있습니다.

## 만약 Route에 연결되어 있지 않는 컴포넌트에서 history, location, match를 사용하고 싶다면

Route에 전달되는 컴포넌트는 기본적으로 위의 세개가 주어집니다. 하지만 다른 컴포넌트에서도 사용하고 싶다면 어떻게 해야할까요?  
바로 withRouter를 사용하면 됩니다.

```js
import { withRouter } from "react-router-dom";

const Sample = props => <div>sample</div>;

export default withRouter(Sample);
```

위의 경우처럼 withRouter로 컴포넌트를 감싸서 내보내고, 사용하시면 history와 location, match를 사용할 수 있습니다.
