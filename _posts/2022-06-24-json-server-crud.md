---
title: "json-server로 1분만에 rest api 서버만들기 2"
excerpt: "json-server로 투두 리스트의 rest api를 만들어보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - 시리즈
tags:
  - Nodejs
  - REST api
  - json-server
---

## 해당 시리즈의 다른 편 확인하기

## 소스 코드 보러 가기

> [커밋 내역 확인하기](https://github.com/minhanPark/json-server-example/commit/358ef08788ff2c0ea8807e1712a9ab3221c7e03a)

## db.json 파일 작성 및 서버 띄우기

json-server는 json 파일을 만든 후에 해당 파일을 바라보게 하면 바로 작동한다.  
우선은 **루트 디렉토리에 db.json 파일**을 만들어 보자.

```json
// db.json

{
  "todos": [
    { "id": 1, "description": "json-server 공부하기", "isCompleted": false },
    { "id": 2, "description": "시리즈 읽어보기", "isCompleted": true }
  ]
}
```

위와 같이 적어주면 서버를 띄울 준비는 끝났다.  
이제 좀 더 편하게 사용하기 위에 script를 추가해보자.

```json
// package.json

{
  "scripts": {
    "server": "json-server --watch db.json"
  }
}
```

package.json 파일에서 script에 server라는 명령어를 추가해줬다.

```bash
npm run server
```

위의 명령어를 치면 json-server가 만들어질 것이다.

```bash
> json-server --watch db.json


  \{^_^}/ hi!

  Loading db.json
  Done

  Resources
  http://localhost:3000/todos

  Home
  http://localhost:3000

  Type s + enter at any time to create a snapshot of the database
  Watching...
```

우리가 적은 todos에 접근하려면 http://localhost:3000/todos 로 들어가면 된다.  
만약 todo 객체에 접근하고 싶다면 http://localhost:3000/todos/1 처럼 뒤에 아이디를 붙여주면 가능하다.

> 기본 port는 3000번 이지만 다른 포트로 설정해주고 싶다면 스크립트에 --port 옵션을 붙여주면 된다.  
> **json-server --watch db.json --port 4444** 로 수정하면 4444 포트에서 실행될 것이다.  
> 리액트의 기본 포트가 3000이니 다른 포트로 변경하도록 하자.

## CREATE 추가하기

우선 axios를 추가해보자. 해당 모듈로 crud 통신을 할 수 있다.  
todo를 추가하기 위해 App.js를 다음과 같이 수정해보자.

```js
import axios from "axios";
import React, { useState } from "react";

function App() {
  const [description, setDescription] = useState("");

  const handleSubmit = async () => {
    const { data } = await axios.post("http://localhost:4444/todos", {
      description,
      isCompleted: false,
    });
    alert(data.description + "이 추가되었습니다.");
    setDescription("");
  };

  return (
    <>
      <h1>1분만에 Rest api 만들기</h1>
      <div>
        <h2>추가하기</h2>
        <input
          placeholder="할 일"
          type="text"
          value={description}
          onChange={(e) => setDescription(e.target.value)}
        />
        <button onClick={handleSubmit}>추가하기</button>
      </div>
    </>
  );
}

export default App;
```

input을 컨트롤하기 위해 useState 훅을 추가했고, 버튼을 누르면 할 일이 추가되고, 알림창이 뜬다.  
추가되었다고 알림창이 뜨면 db.json을 확인해보자.

```json
// db.json

{
  "todos": [
    {
      "id": 1,
      "description": "json-server 공부하기",
      "isCompleted": false
    },
    {
      "id": 2,
      "description": "시리즈 읽어보기",
      "isCompleted": true
    },
    {
      "description": "할 일 추가하기2",
      "isCompleted": false,
      "id": 3
    }
  ]
}
```

db.json에 추가된 모습을 확인할 수 있다.

## Read, Update, Delete 구현

Read 부분을 구현하기 위해서는 페이지가 로드 되었을 때 데이터를 요청할 useEffect 훅과 받아온 데이터를 가지고 있을 state가 필요하다.

```js
// App.js

function App() {
  const [todoList, setTodoList] = useState([]);
  const readList = async () => {
  const { data } = await axios.get("http://localhost:4444/todos");
    setTodoList(data);
  };
  useEffect(() => {
    (async () => {
      await readList();
    })();
  }, []);
  return (
    ...
  );
}
```

get 요청은 반복 사용할 것같아서 함수로 만들었다.(readList)  
즉시 실행 함수로 페이지가 마운트 되었을 때 get 요청을 날리고 받아온 데이터를 todoList에 담는다.

```js
const toggleCompleteBtn = async (id, isCompleted) => {
  await axios.patch(`http://localhost:4444/todos/${id}`, {
    isCompleted: !isCompleted,
  });
  await readList();
};
const deleteTodoBtn = async (id) => {
  await axios.delete(`http://localhost:4444/todos/${id}`);
  await readList();
};
```

완료/미완료 버튼을 클릭하면 토글될 수 있도록 함수를 만들었고, 해당 아이디를 받아서 삭제할 수 있도록 만들었다.

```js
return (
  <>
    <h1>1분만에 Rest api 만들기</h1>
    <div>
      <h2>추가하기</h2>
      <input
        placeholder="할 일"
        type="text"
        value={description}
        onChange={(e) => setDescription(e.target.value)}
      />
      <button onClick={handleSubmit}>추가하기</button>
      <br />
      <h2>할 일 목록</h2>
      <ul>
        {todoList?.map((todo) => (
          <div key={todo.id}>
            <li key={todo.id}>{todo.description}</li>
            <button
              onClick={() => toggleCompleteBtn(todo.id, todo.isCompleted)}
            >
              {todo.isCompleted ? "완료" : "미완료"}
            </button>
            <button onClick={() => deleteTodoBtn(todo.id)}>삭제하기</button>
          </div>
        ))}
      </ul>
    </div>
  </>
);
```

해당 함수들을 사용할 수 있도록 리스트를 렌더링하도록 jsx를 수정해보자.

![화면](https://user-images.githubusercontent.com/29043491/175489228-5b92d8cb-f860-4025-9ee3-70861f6099f3.png)  
**아주 투박한 모습**이지만 CRUD를 사용할 수 있을 것이다.
