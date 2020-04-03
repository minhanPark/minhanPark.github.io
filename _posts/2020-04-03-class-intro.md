---
title: "자바스크립트의 클래스를 이용해보자"
excerpt: "자바스크립트의 클래스를 통해서 출력값이 같도록 만들어보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - Javascript
  - Algorithm
---

## 문제 12

```js
// 데이터
// <여기에 class를 작성하세요.>

const x = new Wizard(545, 210, 10);
console.log(x.health, x.mana, x.armor);
x.attack();

// 출력
// 545 210 10
// 파이어볼
```

## 클래스 만들기

자바스크립트에서 클래스를 만드는 방법은 예약어인 class와 앞머리가 대문자인 이름이다.

```js
class Human {
  // 여기에 정의할 것들을 정의한다.
}
const me = new Human();
```

위와 같이 new 를 붙이면 클래스를 통해서 새로운 객체가 만들어진다.  
객체를 만들때 값을 넣어서 초기화 시키고 싶다면 생성자 함수를 사용하면 된다.

```js
class Human {
  constructor(name) {
    this.name = name;
  }
}

const me = new Human("러닝워터");
```

**constructor 함수(생성자 함수)**는 name이라는 매개변수를 가지고, 내부에서 this를 통해서 연결한다.  
생성자 함수 안에서 **this는 앞으로 만들어질 인스턴스(객체)를 가리키고 있다.** 즉 밑에 만든 "me"라는 객체를 가리킨다.  
그래서 me.name은 러닝워터가 된다.

## 풀이 1

```js
class Wizard {
  constructor(health, mana, armor) {
    this.health = health;
    this.mana = mana;
    this.armor = armor;
  }
  attack = () => "파이어볼";
}

const x = new Wizard(545, 210, 10);
console.log(x.health, x.mana, x.armor);
// 545 210 10
x.attack();
// 파이어볼
```
