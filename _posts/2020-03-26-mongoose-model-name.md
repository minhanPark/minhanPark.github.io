---
title: "mongoose에서 model의 name이 지어지는 법"
excerpt: "mongoose에서 model의 name이 어떻게 형성되는 지 알아보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - Database
---

## mongoose에서 모델 생성하기

```js
import mongoose from "mongoose";

const ListSchema = new mongoose.Schema({
    (...)
});

const model = mongoose.model("List", ListSchema);
export default model;
```

일반적으로 mongoose로 모델을 생성하는 코드는 위와 같다.  
model 메소드의 첫번째 매개변수에 **단수형을 입력하면, "lists"와 같이 복수형으로 콜렉션**이 만들어진다.

## 모델을 이용해서 CRUD 하기

```js
import List from "../models/lists";

//controller 만들기
const getLists = async (req, res) => {
  const lists = await List.find();
  console.log(lists);
};
```

위와 같이 컨트롤러를 만들어서 lists에 대해서 생성된 데이터들을 가지고 올 수 있다.  
하지만 model 이름과 매칭되지 않는다면 올바른 값을 가져올 수 없다.

## 모델 이름 확인하기

model메소드에 List라고 전달하면 lists라는 컬렉션을 생성한다고 했다.  
즉 List.find 등의 메소드를 실행하면 lists라는 컬렉션에서 찾는 것이다.  
그러면 ListItem이라고 카멜 케이스로 이름을 짓는 다면 어떻게 될까?  
첫글자만 대문자에서 소문자로 바꾸는 것이 아니고, 전체 글자를 소문자로 바꾼 뒤 복수형태로 s를 붙이기 때문에  
listitems라는 컬렉션이 만들어진다. 얼핏 listItems라고 컬렉션이 만들어지지 않았을까 생각할 수도 있지만 조심하자!
