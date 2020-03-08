---
title: "프로토타입 기반의 언어 자바스크립트의 상속"
excerpt: "프토로타입 기반의 언어 자바스크립트의 상속하는 방법과 class도 알아보자"
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

## 상속이란 무엇일까?

자식 클래스가 부모 클래스의 기능을 받아쓰는 것을 상속이라고 한다.  
왜 상속이 생겼을까?  
**코드가 줄어들기 때문이다.**  
아무리 생각해도 이 이유가 제일 큰 것 같다.  
A라는 객체와 B라는 객체는 메소드 하나가 차이난다면,  
코드 상으로 같은 것을 반복하기 보다는 B에서 A에 있는 기능을 그대로 쓰면서 메소드 하나만 추가하는게 편하지 않겠는가?  
그래서 나는 상속이라는 개념이 생겨났다고 생각한다.(+기타 여러 기능들도)

## 프로토타입 기반의 언어 자바스크립트

우선 배열의 속을 한번 들여다보자.

```js
const arr = [1, 2, 3];
console.dir(arr);
```

해당 코드를 실행시킨다면 아래 이미지와 같은 것을 볼 수 있다.

![__proto__ 확인하기](https://drive.google.com/uc?id=1VBGvjutahsCUrA5sIsmgweBOCWiWlu1J)

단순히 말하면 객체들은 자신의 prototype이 있고, 그게 자식에게는 **proto**라고 전달되는 것이다.  
class가 명시되지 않았을 때는 이 프로토타입을 통해서 상속을 했다.

## 기존의 상속보기

```js
function Person(name) {
  this.name = name;
}
Person.prototype.sayName = function() {
  return this.name;
};
```

Person이라는 생성자 함수를 만들었다. 그리고 prototype에 sayName이라는 함수를 추가해주었는데,  
이를 통해서 만들어진 인스턴스는 sayName이라는 메소드를 사용할 수 있게된다(**proto**속성에 있기때문이다. )

```js
const developer = new Person("RunningWater");
console.log(developer.sayName()); // RunningWater
```

Person을 기반으로 Man을 만든다고 한다면 어떻게 해야할까?

```js
function Man(name, age) {
  Person.call(this, name);
  this.age = age;
}
Man.prototype = new Person();

const boy = new Man("mike", 8);
```

Person 함수를 사용하기 위해 call을 사용해서 불러오고(super의 역할) 새로 age라는 속성을 넣어주었다.  
마지막으로 prototype을 새로 할당해준 것은 왜일까?  
바로 자바스크립트는 객체를 전달할 때 참조를 하기 때문이다.  
즉 참조를 통해서 전달된 기존 생성자 함수가 변경될 수 있기때문에 새로 할당해준 것이다.

## 클래스를 통한 상속

```js
class Person {
  constructor(name) {
    this.name = name;
  }
  sayName = () => {
    return this.name;
  };
}
```

class를 통해서 정의하는 방법은 위와 같고, 똑같이 new 생성자를 통해서 객체를 만들 수 있다.

```js
class Man extends Person {
  constructor(name, age) {
    super(name);
    this.age = age;
  }
}
```

extends를 통해서 상속받을 수 있고, super를 통해서 부모 클래스에 접근할 수 있다.  
MDN을 보면 **class를 구현하였지만 자바스크립트는 여전히 프로토타입 기반으로 남아있다**라는 글을 읽을 수 있는데,  
즉 작동방식은 예전과 그대로이지만 class에 익숙한 개발자들을 위한 문법적 슈가라고 할 수 있다.
