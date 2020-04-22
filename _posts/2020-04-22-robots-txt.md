---
title: "express에 robots.txt 삽입하기"
excerpt: "express 앱에 robots.txt를 삽입해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - nodejs
---

## robots.txt란 무엇인가?

포털사이트는 각 검색 로봇을 가지고 있다.  
검색로봇이 컨텐츠를 어디에서 어디까지 수집할 수 있는 지 허용하거나 제한하는 범위를 나타낸 것이 robots.txt 파일이다.  
더 많은 검색 노출과 상위 노출을 원한다면 당연히 해야할 작업이라고 할 수 있다.

대표적인 검색로봇을 예로 든다면 네이버는 Yeti 구글은 Googlebot이다.

## 작성방법

```
User-agent: *
Allow: /
```

일반적으로 위와 같이 작성한다.  
User-agent에 검색로봇이 들어가고, Allow나 Disallow에 페이지를 적어주면 된다.  
위에는 모든 검색로봇에게 모든 페이지에 대하여 수집을 허용한다고 알려주는 것이다.

```
User-agent: *
Disallow: /
```

위는 모든 검색로봇에게 수집을 허용하지 않는다고 알려주는 것이다.

```
User-agent: *
Allow: /
Disallow: /admin/
```

모든 로봇에게 /admin/ 디렉토리는 수집을 허용하지 않고, 나머지는 다 수집해도 된다고 알려준다.

## express에 삽입하기

텍스트 파일을 만들어서 삽입해도 되겠지만 미들웨어에 바로 삽입할 수도 있다.

```js
app.get("/robots.txt", (req, res) => {
  res.type("text/plain");
  res.send(
    "User-agent: *\nDisallow: /admin/\nDisallow: /store/orders/\nDisallow: /store/orders/kr/\nDisallow: /store/orders/jp/\n"
  );
});
```

구매가 가능한 사이트다 보니 주문내역도 수집되면 안되기 때문에 차단했다.

```
User-agent: *
Disallow: /admin/
Disallow: /store/orders/
Disallow: /store/orders/kr/
Disallow: /store/orders/jp/
```

사이트/robots.txt 에 접속해보면 위와 같이 나타나는 것을 확인할 수 있다.
