---
title: "open-color에서 색을 사용해보자."
excerpt: "open-color 라이브러리에서 색을 추출해서 사용해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - CSS
---

## 색상 고민 말고 open-color 라이브러리 이용하기

[깃허브 보러가기](https://github.com/yeun/open-color)  
[문서 확인하기](https://yeun.github.io/open-color/)

해당 링크를 통해서 open-color에 대한 자료들을 확인할 수 있다.

```
npm install open-color

or

yarn add open-color
```

위의 명령어를 통해서 다운로드 할 수 있다.

```scss
@import "path/open-color";

.body {
  background-color: $oc-gray-0;
  color: $oc-gray-7;
}
```

scss의 경우엔 위처럼 "\$oc-color-번호" 형태로 불러와서 사용하면 된다.

## 직접 추출해서 사용하기

위와 같이 다운로드 받는다면 사용하지 않는 색까지 다 불러오게 된다.  
자기가 필요한 색만 가지고 오고 싶다면 색을 추출해서 lib 폴더 등에 넣어두고 사용한다면 편리하다.

```js
const palette = {
  gray: [
    "#f8f9fa",
    "#f1f3f5",
    "#e9ecef",
    "#dee2e6",
    "#ced4da",
    "#adb5bd",
    "#868e96",
    "#495057",
    "#343a40",
    "#212529"
  ]
};

export default palette;
```

위 처럼 색을 다 가지고 온 뒤에 내보내주면 **palette.gray[숫자]** 형태로 사용할 수 있다.
