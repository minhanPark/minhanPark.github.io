---
title: "리액트의 hooks 살펴보기"
excerpt: "hooks를 통해 함수 컴포넌트에서 state를 사용해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - 자바스크립트
  - javascript
  - React
---

예전 리액트에서 함수 컴포넌트는 jsx를 렌더링 하는 역할에 주로 이용했다. 부모 컴포넌트를 통해서  
데이터(props)를 받고 태그들과 함께 뿌려주는 역할이라고 할까?  
하지만 **리액트 hooks가 추가된 후 이러한 개념이 바뀌어버렸다**

## 함수형 컴포넌트에서 state 바꿔보기

```js
import React, { useState } from 'react';

function App() {
  const [count, setCount] = useState(0);

  return (
    <div>{count}</div>
    <button onClick={() => setCount(count + 1)}></button>
  );
}
```

해당 코드는 버튼을 누를 때마다 div 태그 안에 count 값이 1씩 상승한다.  
여기서 중요한 것은 세가지이다.

1. useState를 불러온 것.
2. 값으로 0을 전달해 준 것.
3. useState를 통해서 count와 setCount 값을 받은 것.

우선 hooks에는 사용할 수 있는 여러가지가 있는데 그 중에서 state를 사용하는 것이 useState이다.  
그리고 0을 전달한 건 초기값이 0이면 되기 때문이다. 즉 1부터 시작하려면 1을 전달하고 객체가 필요하다면  
객체를 전달하면 된다. 즉 state는 여기서 count가 된다.
마지막으로 setState 함수의 역할을 setCount가 하게 되는 것이다.

## hooks를 사용함으로써 얻는 이점을 뭘까?

예전에는 state나 리액트의 라이프 사이클을 사용하려면 당연히 class를 사용해야 했지만 이제는  
필요한 부분만 함수형 컴포넌트에서 불러올 수 있게 되었다. 아주 간편하게 import만 하면 된다.  
또 필요한 부분만 불러와서 사용하니 코드도 훨씬 짧아지고 가독성도 좋아지게 변했다고 생각한다.  
이 얼마나 긍정적인 변화인가 !  
또 재사용하기도 엄청 편하다.

```js
const countFunction = initialState => {
  const [count, setCount] = useState(initialState);
  return { count, setCount };
};
```

위와 같이 초기값을 매개변수로 받는 함수로 만들어준다면 다양한 숫자로 시작하는 카운트를 만들 수 있다.  
그것도 아주 편하게
