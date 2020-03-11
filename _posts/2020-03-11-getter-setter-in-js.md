---
title: "자바스크립트의 getter와 setter"
excerpt: "자바스크립트에서 getter와 setter를 적용하는 방법을 알아보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - javascript
---

## getter와 setter의 쓰임새

이름을 갖고오는 함수와 설정하는 함수가 있다고 생각해보자.

```js
const getName = () => this.name;

const setName = name => {
  this.name = name;
};
```

아마 이런 식으로 표현이 가능할 것이다.  
getter와 setter를 단순히 표현한다면 name이라는 메소드를 역할을 나누어 정의할 수 있다는 것이라고 생각한다.

```js
get name(){
    return this.name;
}

set name(newName){
    this.name = newName;
}
```

**주의할점은 setter은 하나의 매개변수만 받는 다는 점이다. 변경할 단 하나**  
이를 주의하면서 사용하면 된다.

## class를 통한 예시 만들어보기

```js
class Human {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.fullName = firstName + " " + lastName;
  }
  get name() {
    return this.fullName;
  }
  set name(newFirstName) {
    this.firstName = newFirstName;
    this.fullName = this.firstName + " " + this.lastName;
  }
}
```

해당 class는 firstName과 lastName을 받아서 풀네임까지 저장한다.

```js
const Human1 = new Human("Running", "Water");
console.log("name is", Human1.name); // Running Water
Human1.name = "Beautiful";
console.log("name is", Human1.name); // Beautiful Water
```

**set으로 변경할 때를 잘 보길 바란다. 함수 형태로 변경하는 것이 아니다.**  
만약 setter를 쓰지 않았다면 firstname을 변경해줬을 때 fullName도 직접 변경해야 했지만,  
setter를 통해서 **일관성도 유지**할 수 있었다.
