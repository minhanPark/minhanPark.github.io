---
title: "mongoose에서 메서드 생성하기"
excerpt: "mongoose에서 메서드를 만들어서 이용해보자."
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

## 스키마 정의하기

일단은 데이터 타입이 어떻게 되는 지 스키마를 정의해 보겠습니다.

```js
import mongoose, { Schema } from "mongoose";

const UserSchema = new Schema({
  username: String,
  cryptoPassword: String
});

const User = mongoose.model("User", UserSchema);
export default User;
```

보통 위와 같이 스키마를 설정할 수 있습니다.  
이때 password를 그대로 넣는 것이 아니라 변경된 상태로 넣어야 한다고 가정해봅시다.  
그럴 경우 메소드를 통해서 해당 역할을 수행할 수 있습니다.

## 인스턴스 메소드 만들기

```js
(...)

UserSchema.methods.setPassword = function(password){
    const crypto = password + "123456789";
    this.cryptoPassword = crypto;
}

const User = mongoose.model('User', UserSchema);
(...)
```

**인스턴스 내부의 this는 만들어진 인스턴스를 뜻합니다. 그래서 화살표함수를 사용하면 안됩니다.**

```js
const user = new User({
  username
});
user.setPassword(password);
await user.save();
```

위의 코드 처럼 사용하면 됩니다.

## 스태틱 메소드 만들기

```js
UserSchema.statics.findByUsername = function(username) {
  return this.findOne({ username });
};
```

해당 함수는 유저네임을 통해서 아이디를 찾습니다.

```js
// 사용할 땐
const existeduser = await User.findByUsername(username);
```

**인스턴스 메소드 내부의 this는 생성된 인스턴스, 스태틱 메소드의 this는 User 모델**을 나타냅니다.
