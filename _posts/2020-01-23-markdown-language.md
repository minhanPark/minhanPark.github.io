---
title: "마크다운의 문법을 사용해보자."
excerpt: "md 파일에서 자주 사용하는 마크다운의 문법을 알아보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - git
  - md
---

깃허브 블로그를 사용하다보니 자연스럽게 마크다운의 사용이 늘어나는 것 같습니다.  
그래서 제가 자주 사용하는 마크다운 문법을 정리합니다.

## 마크업과 마크다운

마크업 랭귀지의 대표적인 예는 HTML입니다. 데이터를 태그를 통해서 구조적으로 나타내는 모습이 마크업 랭귀지의 정의라고 할 수 있습니다.  
마크다운은 거기서 완전히 반대되는 말은 아니고, HTML로 변환이 되면서 좀 더 쉽게 텍스트를 나타낼 수 있다고 생각하시면 됩니다.  
지금 보고 있는 .md 파일 등에서 쓰이며 md도 markdown의 줄임말입니다.

## 제목 나타내기

#~######을 사용하면 h1~h6태그 처럼 제목을 나타낼 수 있습니다.

```md
### h3태그 입니다.
```

### h3태그 입니다.

## 인용문 사용하기

```md
> 인용문입니다.
>
> > 중첩인용문 입니다.
```

> 인용문입니다.
>
> > 중첩인용문 입니다.

이처럼 >를 사용하면 인용문을 나타낼 수 있습니다. 그리고 위의 예시처럼 >밑에 >>등을 사용해주면 인용문을 중첩해서 사용할 수 있습니다.

## 줄바꿈

기본적으로 제가 엔터를 통해서 줄바꿈을 하더라도 마크다운은 줄바꿈이 되지 않습니다. 그래서 줄바꿈을 하고 싶으시면 하고 싶은 부분에서 2번 space를 해주셔야 합니다.

```md
줄바꿈하기
줄바꿈하기
```

스페이스  
2번

스페이스
1번

## 목록 나열하기

목록은 기본적으로 순서가 있는 목록과 순서가 없는 목록이 있습니다. 순서가 있는 목록은 1. 형태로 해주시면 됩니다.

```md
1. 첫번째
2. 두번째
3. 세번째
```

1. 첫번째
2. 두번째
3. 세번째

순서가 없는 목록은 \*, - 을 이용하면 됩니다.

```md
- 사과

* 바나나

- 귤
```

- 사과

* 바나나

- 귤

## 코드 나타내기

코드는 \`\`\` \`\`\`을 이용해서 나타낼 수 있습니다.

\`\`\`js  
console.log("Hello World");  
\`\`\`

위와 같이 표현하면 아래와 같이 나타납니다.

```js
console.log("Hello World");
```

js는 어떤 언어인 지를 표시해서 하이라이트 처리해줌니다. 각 언어에 맞게 써주시면 됩니다.

## escape

코드를 작성할 때나 아니면 글을 쓸 때 escape가 필요한 순간이 있습니다. 그럴땐 **\ (백슬래쉬)**를 사용해주세요.  
또한 코드 상에 {% raw %}{}{% endraw %} 등 빈 객체 부분이 들어갈 때 표시가 되지 않는 부분이 있습니다.  
이건 jekyll에서 이 코드를 실행하기 때문입니다. 그래서 { }는 &#123;% raw %&#125; &#123;% endraw %&#125; 사이에 넣어주셔야 합니다.

## 수평선 남기기

공간을 나눌 땐 수평선을 사용하면 아주 편합니다.

---

위 처럼 수평선을 남기려면 \-\-\- 을 사용하시면 됩니다.

```md
나

---

눔
```

나

---

눔

### 링크 남기기

링크는 []()의 형태로 남깁니다.

```md
[대체텍스트](링크 "링크에 마우스 올리면 보이는 타이틀")
```

타이틀은 생략되어도 됩니다.

[깃허브 보러가기](https://github.com/minhanPark?tab=repositories)  
[깃허브 보러가기](https://github.com/minhanPark?tab=repositories "러닝워터 깃허브")  
위의 링크는 주소가 보이고 아래의 링크는 "러닝워터 깃허브"라는 타이틀이 보이는 것을 확인할 수 있습니다.

```md
링크: <링크>
```

깃허브 링크: <https://github.com/minhanPark?tab=repositories>

그리고 <>을 이용하면 url을 통해 링크를 그대로 남길 수 있습니다.

## 텍스트 강조하기

```md
_이텔릭체_  
**강조하기**
~~취소선~~
```

_이텔릭체_  
이텔릭체는 \_으로 감싸주시면됩니다.

**강조하기**  
~~취소선~~

가장 많이 쓰는 강조와 취소선입니다. \*\*와 ~~을 이용하시면 됩니다.

## 이미지 남기기

이미지 남기는 방법은 링크와 비슷합니다.

```md
![이미지 대체텍스트](링크)
```

위와 같은 형태로 남기면 이미지를 남길 수 있습니다.

![프로필](https://drive.google.com/uc?id=1Lq6eU1HprMP5ffAPAy44-m-vyd4DNHrz)
