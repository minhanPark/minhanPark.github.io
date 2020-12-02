---
title: "input 값을 검증해보자."
excerpt: "vue router에서 제공하는 가드를 활용해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - javascript
  - Vue
---

## 폼 검증하기

form 태그 안의 input 창의 값을 검증해야할 때가 있다. 이메일을 받아야 하는데 이메일의 형태가 아니라면 백엔드까지 요청을 보낼 필요도 없이 프론트에서 경고창 등을 띄워줄 수 있다. 그래서 서버 자원의 낭비와 사용자와의 상호작용을 늘릴 수 있다.

## 패키지 활용하기

직접 폼 검증을 하는 함수를 만들수도 있지만 패키지를 활용하면 간단하게 구현할 수 있다.

### vuelidate 설치하기

```bash
npm install vuelidate
```

해당 명령어를 입력하면 vuelidate를 다운받을 수 있다.

```js
//main.js
(...)
import Vuelidate from "vuelidate";

Vue.use(Vuelidate);

new Vue({
  (...)
}).$mount("#app");
```

프로젝트의 main.js에 vuelidate를 등록해준다.  
위처럼 등록했다면 이제 사용할 준비가 끝났다.

### 컴포넌트 안에서 직접 사용해보기

```js
<template>
  <div>
    <form @submit.prevent="handleSubmit">
      <div>
        <input v-model="form.name" type="text" />
      </div>
      <div>
        <button>등록하기</button>
      </div>
    </form>
  </div>
</template>

<script>
import { required, minLength, email } from "vuelidate/lib/validators";
export default {
  data() {
    return {
      form: {
        name: "",
      },
    };
  },
  validations: {
    form: {
      name: { required, minLength: minLength(6), email },
    },
  },
  methods: {
    handleSubmit() {
      console.log(this.$v.form.name);
      if (!this.$v.form.name.required) {
        alert("이름은 필수값입니다.");
      } else if (!this.$v.form.name.minLength) {
        alert("6자리 이상 입력해야 합니다.");
      } else if (!this.$v.form.name.email) {
        alert("이메일을 입력해주세요.");
      }
      return;
    },
  },
};
</script>

<style>
(...)
</style>

```

위의 예제는 하나의 input창을 가지고 있다. input 값이 비워져있으면 안되고, 6자리 이상이면서 이메일 형식이어야 한다고 하면 **_vuelidate/lib/validators_**에서 검증 조건을 가지고 오면 된다.  
[vuelidate에서 사용할 수 있는 validator들](https://vuelidate.js.org/#sub-builtin-validators)

validator를 들고와서 실제로 적용시키는 부분은 validations에 있다.

```js
export default {
  data() {
    return {
      form: {
        name: "",
      },
    };
  },
  validations: {
    form: {
      name: { required, minLength: minLength(6), email },
    },
  },
  (...)
};
```

validations를 정의하고, 해당 객체 안에 폼검증을할 **데이터 값을 그대로 적어준 후에 조건을 넣어주면 된다.**

### 값 확인하기

```js
methods: {
    handleSubmit() {
      console.log(this.$v.form.name);
      if (!this.$v.form.name.required) {
        alert("이름은 필수값입니다.");
      } else if (!this.$v.form.name.minLength) {
        alert("6자리 이상 입력해야 합니다.");
      } else if (!this.$v.form.name.email) {
        alert("이메일을 입력해주세요.");
      }
      return;
    },
  }
```

메소드 부분은 위와 같이 정의했었다. **validator가 추가되면 validator 이름을 가진 불린값을 가지게 된다**. 그래서 form.name 값에는 required, minLength, email이 추가되어 있다. 해당 값이 충족되었을 때는 true를, 충족되지 못했을 때는 false를 가지게 된다.

> 중요한 부분은 this.$v의 값을 확인해보면 anyError나 error값을 가지는 것을 확인할 수 있는데 validator에 충족하지 않는 값이 있을 때(예제에서는 값이 비워져있거나, 이메일 형태가 아니거나 6자리 이상이 아닐때) true의 값을 가지게 된다. 지금처럼 인풋이 하나일때는 안되지만 여러 인풋이 form 안에 있다면 해당 값 중에서 조건이 false를 가지게 되면 error나 anyError는 true가 되기 때문에 해당 값으로 문제가 있다고 사용자에게 알려줄 수 있다.
