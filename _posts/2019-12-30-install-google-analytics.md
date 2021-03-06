---
title: "구글 애널리틱스 시작하기"
excerpt: "구글 애널리틱스를 설치해보자"
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - google-analytics
tags:
  - Google-Analytics
---

데이터로 세상을 바라보는 건 아주 좋은 일이기 때문에 구글 애널리틱스를 배우며  
하나씩 진짜 데이터로 바라보는 세상을 경험해보려고 한다.

> 세상은 정보화 시대에서 **데이터 시대**로 가고 있다.

---

## 왜 구글 애널리틱스인가?

몇 가지 이유를 나열해보자면

1. 무료다
2. 이용하기에 따라 과도도 되고, 소잡는 칼도 된다.
3. 많은 곳에서 사용한다.

이 세가지 이유면 충분하지 않을까?  
나는 단순히 오늘 몇명이 어디서 들어왔는 지 정도만 확인하던(전형적인 과도) 경우였는데  
이번 기회에 공부를 해서 소잡는 칼로 이용해보고 싶다.

## 사이트에 구글 애널리틱스 넣기

설치법은 아주 간단하다.  
![추적코드](https://drive.google.com/uc?id=1OQS_IZqHrs6nribrcSmHjpOQEJ85ELWm)  
가입해서 url 적고 진행해 나가다 보면 해당 이미지 처럼 추적코드가 나오는 것을 확인할 수 있다.
위에서 아래로 진행읽으니 **적절한 위치**에 삽입하면 아주 간단히 구글 애널리틱스를 적용가능하다.

```html
<head>
  <script></script>
  <script></script>
</head>
```

html 코드에는 메타 태그들을 정의하는 <head>태그가 있는데, 이 태그 바로 뒤에 추적코드를 넣으라고 설명해놨다.  
위에서 아래로 진행하다가 구글 애널리틱스의 스크립트를 읽고 다시 body 부분을 진행시키는게 역시 좋다고 생각해서 그렇게 했다고 생각한다.

해당 추적코드가 추가되면 바로 [구글 애널리틱스](https://analytics.google.com/analytics/web/)에서 확인이 가능하다.
