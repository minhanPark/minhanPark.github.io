---
title: "json에서 개행문자 사용하고 리액트에 삽입하기"
excerpt: "json에서 개행문자를 사용해보고, 리액트에서 개행문자를 그대로 갖고와서 사용해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - React
  - json
---

## json의 사용

일반적으로 다국어 처리를 할 때 json에 각각 매칭이 되도록 설정한다.

```json
// en/common.json

{
    "company": "Company"
}

// kr/common.json
{
    "company": "회사"
}
```

위와 같은 형태에서 company를 전달해주면 상황(locale)에 맞게 값을 바꿔준다.  
만약에 json에 개행문자가 들어간다면 리액트에서 어떻게 처리해줄 수 있을까?

## 리액트 jsx의 escape

기본적으로 React는 JSX에 삽입된 모든 값을 렌더링하기 전에 **이스케이프**한다.  
이런 특성이 있기 때문에 XSS(cross-site-scripting)에서 안전하지만 일반적으로 위와 같은 상황에서는 불편함을 겪는다.

```js
const Test = () => {
  const text = "첫번째 줄\n두번째줄\n세번째줄";
  return (
    <div>
      <p>{text}</p>
    </div>
  );
};
```

위와 같이 작성되었다면 개행되지 않고 한 줄로 나타날 것이다.

## escape 하기

두가지 방법으로 할 수 있다.

```js
const Test = () => {
  const text = "첫번째 줄\n두번째줄\n세번째줄";
  return (
    <div>
      <p style={{ whiteSpace: "pre-line" }}>{text}</p>
    </div>
  );
};
```

**css로 whiteSpace 값으로 pre-line을 주면 적용**되는 것을 확인할 수 있다.  
또한 **jsx에서 dangerouslySetInnerHTML**을 이용할 수도 있다.

```js
const Test = () => {
  const text = "첫번째줄<br>두번째줄<br>세번째줄<br>";
  return (
    <div>
      <p dangerouslySetInnerHTML={{ __html: text }} />
    </div>
  );
};
```

> 만약 html을 삽입하기로 했다면 className은 class로 해야한다.
> 또 코드에서 HTML을 설정하는 건 사이트 간 스크립팅 공격에 쉽게 노출 될 수 있으니 조심해야한다.
