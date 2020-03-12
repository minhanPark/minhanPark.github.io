---
title: "mongoose에서 objectId 체크하기"
excerpt: "mongoose의 objectId가 유효한 지 유효하지 않은 지를 확인해보자. "
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

## objectId 형태 확인하기

mongoose의 id는 "5e68a34880ccfb5b78a85e3b"와 같은 형태이다.  
해당 형태로 넘어오는 아이디 값을 체크해야할 경우가 생기는데, mongoose에서는 이를 위한 메소드를 지원한다.

```
{
    "data": {
        "_id": "5e68a34880ccfb5b78a85e3b",
        "title": "HelloWord",
        "_v":0
    }
}
```

불러온 데이터의 형태는 위와 같다.

## 유효성 체크하기

```js
import mongoose from "mongoose";

const { ObjectId } = mongoose.Types;

ObjectId.isValid(id); // 값이 맞다면 true, 맞지 않다면 false를 리턴한다.
```

isValid 메소드를 통해서 값의 유효성을 체크할 수 있다.

## express에서 사용할 수 있게 미들웨어로 만들어보기

```js
import mongoose from "mongoose";

const { ObjectId } = mongoose.Types;

export const checkObjectId = (req, res, next) => {
  const { id } = req.params;
  if (!ObjectId.isValid(id)) {
    res.status(400).send("no!");
  }
  return next();
};
```

params를 통해서 전달되는 id를 받아서, 값이 유효하지 않다면 status를 400으로 하고, no!라는 글을 보여준다.  
그리고 반대로 값이 유효하다면 next를 통해서 다음 미들웨어를 실행한다.
