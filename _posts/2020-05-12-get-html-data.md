---
title: "data-* 활용해서 html 데이터 가지고 오기"
excerpt: "html 태그를 통해서 data를 가지고 오자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - HTML
  - CSS
  - Javascript
---

## data 속성은 무엇인가?

html에는 속성이 있다. class라든지 id라든지 기타 속성들로 html에 접근해서 액티브한 기능을 줄 수도 있고, css를 통해서 꾸며줄 수도 있다.  
또한 태그의 특성에 맞게 가지는 속성들도 있다.

```html
<input type="text" value="" />
```

위의 인풋처럼 type이나 value가 태그의 특성에 맞게 가진 속성들이라고 할 수 있다.  
즉 정해진 값이 있다는 것이다. 이러한 속성을 통해서 사용자가 값을 정할 수 있을까?  
위에 대한 대답이 **data-** 속성이다.

```html
<div data-info="simple">이 태그는 데이터를 담고 있습니다.</div>
<div data-info-two="is good">이 태그도 데이터를 담고 있습니다.</div>
```

위와 같이 **data-정의할문자**로 속성을 정하고 속성값을 전달하면 된다.

## 자바스크립트로 데이터 값 가지고 오기

위에서는 정의하는 방법을 알아보았다.  
속성에 **data-** 형태로 넣으면 데이터를 저장할 수 있다. 각 단어는 -을 통해서 구분해주면 된다.  
위의 data-info-two가 그 예시이다.

```js
const info = document.getElementById("data");
const info2 = document.getElementById("data2");

console.log(info.dataset.info); // simple
console.log(info.getAttribute("data-info")); // simple

console.log(info2.dataset.infoTwo); // is good
console.log(info2.getAttribute("data-info-two")); // is good
```

위의 코드를 통해서 알 수 있듯이 가지고 오는 방법은 2개다.  
**getAttribute 메소드와 dataset.이름**을 통해서 가지고 올 수 있다.  
큰 차이라면 getAttribute 메소드를 활용할 때는 data- 부분까지 다 붙여줘야 한다는 점이 있다.  
또한 dataset을 활용할 때는 html 속성에서 -(하이픈)으로 구분한 것이 자동으로 camelCase로 바뀌어져 있다.  
**data-info-two를 dataset.infoTwo으로 갖고 온게 예시**이다.

이처럼 html 태그를 통해서 데이터를 넣고, 우리는 데이터를 활용할 수 있다.

## css로 활용해보기

해당 속성이 있는 태그에 값을 줄 수 있다.

```css
/* [data-info]라고만 적으면 data-info 속성을 지닌 태그에 다 효과를 준다. */
div[data-info="simple"] {
  color: red;
}
```

**data-** 속성을 준 뒤에 해당 속성을 가진 태그에만 css 효과를 적용할 수 있다.

## 주의사항

1. 어떤 데이터를 담아야 할 지 생각하는게 중요하다.
   > 왜냐하면 접근 보조 기술이 접근할 수 없고, 검색 크롤러도 데이터 속성 값을 찾지 못하기 때문이다.
2. 지원범위를 생각해야한다.
   > 인터넷 익스프롤러는 11 이상부터 dataset을 지원한다. 그래서 그 밑의 버전도 사용해야한다면 getAttribute 메소드를 활용해야한다.

주의사항에 맞게 사용하도록 하자!
