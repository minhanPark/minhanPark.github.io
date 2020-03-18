---
title: "css로 반응형 정사각형 만들기"
excerpt: "css로 반응형에도 계속 정사각형이 유지되도록 만들어보자."
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

## 정사각형을 만들 때의 문제점

width나 height에 150px 등 고정값을 준다면 쉽게 정사각형을 만들 수 있다.  
하지만 반응형 웹페이지에서는 값을 %로 주기 때문에 문제가 생긴다.  
사용자가 보는 화면이 직사각형인데, 가로 길이가 30% 세로 길이가 30%라고 해도 정사각형이 되지 않기 때문이다.

## 해결방법

square라는 class이름을 가진 div태그가 있다고 가정해보자.

```css
.square {
  width: 50%;
}

.square:after {
  content: "";
  display: block;
  padding-bottom: 100%;
}
```

height 값을 제외하고, width 값만 %로 준뒤에 after라는 수도 클래스를 통해서 padding-bottom 값을 100%를 주었다.  
이 방법이 정사각형을 유지하는 이유는 **padding이 부모 엘리멘터의 width 값을 기준으로 계산되기 때문**이다.  
즉 padding-bottom이지만 width 값에 맞게 padding-bottom이 계산되어서 정사각형이 만들어지는 것이다. 그것도 반응형으로.

## 내부에 컨텐츠가 있다면.

```html
<div class="square">
  <div class="inner">내부 컨텐츠</div>
</div>
```

```css
.square {
  width: 50%;
  position: relative;
}

.square:after {
  content: "";
  display: block;
  padding-bottom: 100%;
}

.inner {
  position: absolute;
  width: 100%;
  height: 100%;
}
```

내부에 inner라는 컨텐츠가 있다고 한다면, 위처럼 부모 div.square에 position을 relative로 지정해주고  
inner의 position을 absolute, width와 height를 100%으로 해주면 문제없이 해결할 수 있다.
