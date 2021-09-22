---
title: "ORM이란?"
excerpt: "orm이 무엇인지, 왜 사용하는지를 알아보자"
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - orm
  - database
---

## orm이란 무엇인가?

ORM은 Object Relational Mapping 에서 앞자리만 따온 것이다.  
해당 뜻을 더 설명하기 전에 우리가 **데이터베이스의 테이블의 데이터 다루는 방법**을 생각해보자.  
간단히 **직접 sql 문을 사용하면 생성, 읽기, 수정, 삭제 등을 실행**할 수 있다.

하지만 sql을 직접 사용할 필요 없이 내가 사용하는 언어(가령 자바스크립트)로 위와 같이 생성, 읽기, 수정, 삭제 등을 실행할 수 없을까? 라는 생각에서 나온 것이 orm이다.

> 즉 직접 sql 쿼리를 사용하는 것이 아니라 내가 선호하는 언어의 객체를 활용해  
> 데이터베이스의 테이블을 다루는 것이다.

## 왜 ORM을 사용해야 할까?

> 1. 우선 우리가 친숙한 언어로 작성할 수 있다.

가장 익숙한 언어(자바스크립트나 자바, 파이썬 등)를 사용해서 작성할 수 있다.

> 2. MYSQL이나 postgreSQL 등 어떤 데이터베이스인지 상관없이 연결 및 전환이 쉽다.

ORM은 데이터베이스에 종속적이지 않다.

MYSQL을 사용할 거면 MYSQL의 드라이버를 다운로드 하면되고, postgreSQL을 사용하려면 postgreSQL의 드라이버를 다운로드해서 연결만 해주면 된다.

```cmd
npm install --save pg pg-hstore # Postgres를 사용할 때
npm install --save mysql2 # MYSQL을 사용할 때
```

종속적이지 않기 때문에 만약 mysql에서 postgreSQL로 바뀌더라도 더 공수가 적다.  
설정을 바꾸고, 기존의 orm 코드로 작성되어있다면 다시 사용하면 되기 때문이다.

> 3. 더 적은 코드로 실행할 수 있음

```js
// sql 문을 직접 사용했을 시
pool.query(`SELECT * FROM MODEL`);

// ORM 중에 하나 인 Sequelize 사용 시
Model.findAll({});
```

1번 이유와 비슷하지만 더 익숙한 형태로 더 짧게 구현이 가능하다.

## 꼭 ORM을 사용해야할까?

ORM을 사용하는게 항상 좋은 것은 아니다.  
ORM을 사용하면 아래와 같은 단점도 생기게 된다.

> 1. orm의 사용방법을 배워야 한다.

가령 express에서 많이 사용하는 Sequelize에 대해서 생각해보자.  
Sequelize를 사용하려면 문서를 보고 사용방법을 배워야 한다.

[Sequelize 공식 문서](https://sequelize.org/master/manual/getting-started.html) 를 보고 제공하는 것이 무엇인 지 연결 방법은 무엇인 지 등을 파악해야만 사용할 수 있다.

> 2. 복잡한 기능은 직접 쿼리문을 사용해야한다.

orm에서는 모든 기능을 제공하고 있는 것은 아니다. 복잡한 구현이라면 sql문을 직접 사용해야할 수도 있다. 보통 **Sequelize와 같은 모듈이 제공하는 기능 안에 쿼리문을 직접 사용할 수 있도록 구현**이 되어있다.

**_편한만큼 장단점을 잘 생각해서 사용_**하자
