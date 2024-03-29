---
title: "base64 인코딩을 사용하는 이유"
excerpt: "base64는 무엇인 지 base64 인코딩을 사용하는 이유는 무엇인 지 알아본다."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - etc
---

## base64란

base64는 64진법이라는 뜻이다. **_아스키 코드의 일부_**를 매칭되는 문자열로 단순 치환한다.

> 문자열 => ASCII Binary로 바꿈 => 6bit씩 자름 => base64로 encoding

위와 같은 형태로 치환이 된다.  
예시로 a가 어떻게 변환되는 지 알아보자.  
우선 a를(문자열) 아스키로 바꾸면 **97**이된다.  
97의 바이너리는 **01100001** 이다.  
이걸 6bit씩 자르면 **011000 010000**이 된다.

> 6비트씩 자르고 부족한 부분은 0을 채우게 된다.

해당 부분을 다시 십진수로 돌리면 **24 16**이 된다.  
아래 표에서 24, 16을 찾으면 YQ가 된다.  
우리가 빈부분에 0을 채워넣었던 것을 기억하는가?  
그 빈 부분을 표시하기 위해 패딩처리를 해준다. (빈 공간이 두개 있어서 ==을 넣어줌)
![base64 테이블](https://www.woolha.com/media/2020/12/base64-table.png)

즉 콘솔에서 atob("a")를 해보면 **YQ==**이 나오는 것을 확인할 있다.

## 자바스크립트에서 base64로 변경하거나, 다시 ascii로 변경하는 법

btoa 함수는 문자열을 base64로 encoding 해준다.  
atob 함수는 base64로 encoding된 것을 다시 문자열로 되돌려준다.

```js
window.btoa("a");
// => 'YQ=='

window.atob("YQ==");
// => a
```

이름의 유래는 알 수 없지만 b는 base64를 나타내고, a는 ascii를 나타내지 않을까 생각한다.  
중간에 to 부분에 의미가 반대로 되긴 하지만..

## 왜 base64로 인코딩을 할까?

아스키 코드가 맨처음 소개되었을 땐 영어 텍스트만 있었다. 그리고 시간이 지나서는 벤더사들이 아스키 코드를 확장했고, 나중에 벤더사, 언어 및 국가에 따라서 아스키 코드중에서 확장된 부분은 다르게 되었다. 즉 시스템간 데이터를 전달하기에 오류가 발생할 수도 있는 부분이기에 문자 코드에 영향을 받지않는 공통 ASCII인 64개의 안전한 출력 문자만 사용해서 인코딩해 전달하는 것이다.

> 비록 encoding 함으로써 전달할 데이터가 더 길어지긴 하지만 데이터가 왜곡되지 않기 때문이다.
