---
title: "div 태그, img 태그 이미지 비율 맞추기"
excerpt: "div태그와 img태그에 이미지를 비율에 맞게 넣어보자."
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

## 이미지 넣는 방법

내가 생각하기에 이미지를 넣는 방법은 크게 2가지가 있다.  
하나는 div 태그에 background를 통해서 넣는 것이고, 하나는 어떤 태그를 감싸든 img 태그를 사용하는 것이다.  
자기가 만들고 싶은 형태가 분명 있을 것이지만 나는 보통 정사각형을 많이 사용하기 때문에 정사각형을 사용해 보겠다.  
[반응형 정사각형 만들기](https://minhanpark.github.io/today-i-learned/css-responsive-square/)  
위의 링크를 통해서 반응형 정사각형을 만들어 응용해도 좋을 것이다.

## background-image를 통해서 넣는 경우

```html
<div class="wrapper"></div>
```

위와 같이 div.wrapper 가 있다.  
여기에 배경으로 아무 이미지를 넣어보자.

```css
.wrapper {
  width: 500px;
  height: 500px;
  background-image: url(https://upload.wikimedia.org/wikipedia/commons/thumb/7/7e/Jezero_no.svg/1200px-Jezero_no.svg.png);
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}
```

각각의 역할을 보자.  
background-repeat은 이미지 크기가 맞지 않을 때 반복되는 것을 결정한다. 값중에 no-repeat을 통해서 반복을 막았다.  
background-size는 주어진 컨테이너(여기서는 div.wrapper)에 백그라운드 이미지를 어떻게 맞출지를 결정하는 역할을 한다. cover 값을 주어서 주어진 컨테이너 크기게 맞췄다.  
background-position은 배경 이미지의 위치를 조절해준다. 여기서는 center를 통해서 가운데로 맞추었고, 다른 값을 줘서 보이는 부분이 어디가 될 지를 결정할 수 있다.

이처럼 3개의 css 속성을 통해서 원하는 비율에, 배경 이미지의 크기를 맞출 수 있다.

## img 태그를 사용하는 경우

```html
<div class="wrapper">
  <img
    src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7e/Jezero_no.svg/1200px-Jezero_no.svg.png"
  />
</div>
```

```css
.wrapper {
  width: 500px;
  height: 500px;
}
img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

위에서 중요한건 object-fit이다. 해당 속성을 통해서 이미지를 컨테이너에 맞출 수 있다.
