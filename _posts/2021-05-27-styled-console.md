---
title: "console.log에 스타일 입히기"
excerpt: "console.log에 스타일을 입혀서 예쁘게 만들어 보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - Javascript
---

## 콘솔에 스타일을 입혀보자

콘솔창에 스타일을 입힐 수 있다.  
굳이 입혀야하나... 라고 생각할 수도 있고, 나도 그랬지만 몇몇 사이트 들을 보면서 나도 만들어야 겠다고 생각을 했다.  
각 사이트에 접속해서 f12를 눌러보자.  
놀랍게도 뭔가 멋있다...

![쿠팡](https://user-images.githubusercontent.com/29043491/119785569-4699e100-bf0a-11eb-8794-56a7ea9d7493.png)  
**쿠팡**

![티스토리](https://user-images.githubusercontent.com/29043491/119785910-a2646a00-bf0a-11eb-8f87-653c72f417d3.png)  
**티스토리**

![티몬](https://user-images.githubusercontent.com/29043491/119786390-1b63c180-bf0b-11eb-9e0e-f08d89ff623d.png)  
**티몬**

대부분의 홈페이지에는 없지만 위의 기업들을 보면 만들어보고 싶지 않은가 ?

## 콘솔에 스타일 입히기

기본적으로는 스타일을 입힐 부분을 지정하고, 스타일을 전달해주면 된다.

```js
const style = {
  blackAndWhite: "color: white; background-color: black;",
};

console.log("%cI am Happy", style.blackAndWhite);
```

**%c** 부분부터 스타일이 들어간다. 만약 다른 지정한 부분이 없다면 해당 부분부터 쭉 들어가게 된다.

![스타일1](https://user-images.githubusercontent.com/29043491/119787282-02a7db80-bf0c-11eb-8415-12d180e94018.png)  
여러줄을 지정하는 것도 방법은 같다.

```js
const style = {
  blackAndWhite: "color: white; background-color: black;",
  blueAndRed: "color: red; background-color: blue;",
};

console.log(
  "%cI am Happy %cAnd You also happy",
  style.blackAndWhite,
  style.blueAndRed
);
```

![스타일2](https://user-images.githubusercontent.com/29043491/119788075-c9bc3680-bf0c-11eb-9351-717cf25411d0.png)

여러개의 스타일을 넣을 때는 %c에 각각 값을 넣어주면 된다.  
즉 첫번째 %c는 console.log의 2번째 매개변수의 스타일이 적용되고, 두번째 %c는 세번째 매개변수의 스타일이 적용된다.  
한땀한땀 열심히 맞추다보면 나름 재미있게 만들수가 있다.

![회사로고](https://user-images.githubusercontent.com/29043491/119788894-81e9df00-bf0d-11eb-8456-ec3ff4b94a6e.png)  
그래서 **완성한 작품**이다.  
회사 로고 까지 만들어 넣고 나니 너무 마음에 든다.
