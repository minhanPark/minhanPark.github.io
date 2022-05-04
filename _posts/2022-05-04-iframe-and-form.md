---
title: "ifram으로 form 응답값 받기"
excerpt: "form의 action이 post일 때 페이지를 이동하지 말고, iframe에서 응답을 받아보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - html
---

## 코드 구조

```html
<div>
  <form action="/post-handler" method="post" target="intro__frame">
    <input name="nickname" value="runningwater" />
    <button type="submit">제출</button>
  </form>
</div>
<iframe
  name="intro__frame"
  width="400px"
  height="400px"
  title="iframe 타이틀"
></iframe>
```

우선 form 태그를 살펴보면 action(데이터를 보내는 url)은 /post-handler로 되어 있고, post 방식으로 보내고 **target은 iframe의 name**이 들어가 있다.

form의 데이터(여기서는 nickname)를 처리하는 코드는 아래와 같다.

```js
router.post("/post-handler", function (req, res) {
  const { nickname } = req.body;
  // nickname을 가지고 무언가 처리
  res.send("처리가 완료되었습니다.");
});
```

위와같이 되면 **iframe에 처리가 완료되었습니다 라는 문구**가 뜨게된다.

> 즉 페이지를 이동시키지 않고 form의 결과값이 iframe에서 보여지는 형태가 됨. 페이지를 이동하지 않으니 부모 페이지의 함수도 호출할 수 있다.
