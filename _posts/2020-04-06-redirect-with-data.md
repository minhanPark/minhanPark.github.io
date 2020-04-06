---
title: "데이터를 가지고 리다이렉트 하기"
excerpt: "프론트엔드와 백엔드에서 각각 데이터를 가지고 리다이렉트 하는 방법을 알아보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - Backend
  - javascript
---

## 데이터를 가지고 리다이렉트하기

결제 기능을 연동하는 중에 결제 성공후에 데이터를 가지고 리다이렉트 해야할 필요성을 느꼈다.  
비회원이 주문정보를 확인하려고 하면 "이메일"과 "주문번호"가 필요한데 결제자의 이메일은 결제후 보여주지 않더라도 알고있겠지만,  
**주문번호는 "날짜-랜덤문자-번호"의 형태로 결제후에 알 수 있기 때문**이다.

## 프론트엔드 부분에서 데이터를 가지고 리다이렉트

```js
window.location.href = "이동할 주소";
```

위와 같이 설정하면 해당 코드를 만났을 때 내가 설정한 주소로 리다이렉트를 한다. 하지만 이땐 데이터를 담지 못한다.  
그래서 form 태그를 만들어서 데이터를 담아서 보내야 한다.

```js
// 결제 성공 후 콜백에서

let form = document.createElement("form");
form.action = "데이터 보낼 주소";
form.method = "POST";

let dataInput = document.createElement("input");
dataInput.type = "hidden";
dataInput.name = "데이터 받을때 이름";
dataInput.value = "담을 데이터";

form.appendChild(dataInput);
document.body.appendChild(form);
form.submit();
```

위와 같이 **input tag의 type을 "hidden"으로 해서 생성한 후에 form을 통해서 데이터를 보내면**  
원하는 곳으로 리다이렉트도 할 수 있고, 데이터도 같이 보낼 수 있다.

## 백엔드 부분에서 데이터를 가지고 리다이렉트

```js
try {
  const lists = await List.find({});
  req.session.lists = lists;
  res.redirect("/lists");
} catch (e) {
  console.log(e);
}
```

위와 같이 session에 데이터를 담아서 리다이렉트 시킨다.

```js
app.get("/lists", (req, res) => {
  const passedLists = req.session.lists;
  req.session.lists = [];
  return res.render("list", { passedLists });
});
```

데이터를 받는 쪽에서는 session을 통해서 보낸 데이터를 받고, **반드시 해당 값을 비워준다.**  
그리고 가지고온 데이터를 내가 사용할 부분에 넣어주면 된다.

정리하자면 그냥 일반적으로 리다이렉트 하는 것이 아니라 **데이터를 가지고 리다이렉트 할 때는 프론트엔드에서는 form태그를 이용하고, 백엔드에서는 session**을 통해서 하면 된다.
