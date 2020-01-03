---
title: "자바스크립트에서 참으로 판단되는 값(truthy)과 거짓으로 판단되는 값(falsy)을 알아보자."
excerpt: "자바스크립트에서 모든 값들은 참과 거짓으로 판단될 수 있다. 이번 포스팅에서 truthy와 falsy를 알아보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - 자바스크립트
  - javascript
---

자바스크립트에서는 모든 값들은 참과 거짓으로 판단될 수 있다.  
단순히 불린값으로 표현하지 않아도 조건문에 값을 넣으면 판단되는 것을 알 수 있는데  
참으로 판단되는 값(truthy)과 거짓으로 판단되는 값(falsy)을 알아보자.

## 거짓으로 판단되는 값

```javascript
if (false) {
  console.log("Hello World");
}
```

해당 코드는 절대 실행되지 않을 것이다. 조건문에 false가 전달되었으니 말이다.  
그런데 다른 값을 전달해도 실행이 되지 않을 수 있다.

```javascript
if (null) {
  console.log("null");
} else if (undefined) {
  console.log("undefined");
} else if (0) {
  console.log("0");
} else if (0) {
  console.log("");
} else {
  console.log("Bye");
}
```

해당 코드는 Bye가 실행될 것이다. 자바스크립트에서 **null, undefined, 0, NaN, 빈문자열**은 거짓으로 판단되는 값이기 때문이다.

## 참으로 판단되는 값

간단히 말하면 거짓으로 판단되는 값이 아니면 모두 참으로 판단이 된다.

```javascript
if ([]) {
  console.log("[]");
}
```

해당 코드는 실행이 된다. 왜냐하면 빈배열은 참으로 판단되기 때문이다.  
내가 이 부분을 다시 정리하는 이유이기도 한데, 빈배열이 false로 판단될 줄 알고 하다가 착각했다는 걸 뒤늦게 깨달은 것이다.  
정말 바보 같았다.  
정리하자면 **[]와 같은 빈배열, {}와 같은 빈객체를 포함한 거짓으로 판단되지 않는 모든 값은 참으로 판단된다.**  
혹시 빈배열을 falsy로 활용하려면 length를 이용해서 판단해야한다.

## and와 or

위에 정리한 것을 응용하는 방법은 ||과 &&이 있다.  
or과 and를 안다고 생각하고 코드를 일단 보도록 하자.

```javascript
let cat = 0 || "meow";
console.log(cat);
// meow
let dog = 0 && "bark";
console.log(dog);
// 0
```

항상 값을 따지는게 이런 상황에서 나오는 경우가 많다. 말하고 싶은 것은 값을 따져서 값을 넣는데 중요한 역할을 하는 값이 변수에 할당된다는 것이다.  
위에서 말했듯이 0은 거짓으로 판단되고 or의 경우 **뒤의 값에 따라 이 식이 거짓인 지 참인지가 판단**되어 진다. 그래서 뒤에 meow가 cat에 할당된다.  
dog의 경우는 **0이 이미 거짓이기 때문에 and로 묶어져 있어 이 값은 항상 거짓이 된다.** 그래서 0이 dog에 할당되어 진다.  
반대의 경우도 생각해보면 쉽다. **참 || 참** 이라는 형태라면 앞 부분에 있는 참의 값이 변수에 할당될 것이다. 이미 앞의 값이 참이면 그 식은 참이 되기 때문이다.
