---
title: "Vue에서  v-slot 활용하기"
excerpt: "Vue에서 v-slot을 활용해보자"
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

## Slot이란 무엇일까?

컴포넌트의 모든 부분을 다 정해 놓는 것이 아니라 **다른 컴포넌트에서 빈 부분을 채워놓을 수 있도록 설정** 해놓을 수가 있다.  
가령 내부의 컨텐츠는 달라지지만 외부의 레이아웃은 동일하다고 생각해보자.  
이럴 경우 외부의 레이아웃과 관련된 코드를 반복할 필요 없이 하나만 정의해두고 반복해서 사용한다면 확장성이나 편이성 등이 엄청나게 상승할 것이다.  
**_리액트에서는 children 이라는 props의 속성을 통해서 이런 확장을 할 수 있고, vue에서는 slot을 이용해서 확장을 할 수 있다._**  
일단은 밑의 예시를 보면서 더 이해해보자.

## 예제 코드로 slot 활용해보기

우선 App.vue에 아래와 같은 코드가 있다고 해보자

```js
<template>
  <div id="app">
    <div class="container">
      <header>
        <h1>헤더 1</h1>
      </header>
      <main>
        <p>본문 1</p>
      </main>
    </div>
    <div class="container">
      <header>
        <h1>헤더 2</h1>
      </header>
      <main>
        <p>본문 2</p>
      </main>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
};
</script>

<style>
#app {
  padding: 10px;
}
.container {
  width: 500px;
  border: 1px solid black;
  margin: 10px;
}
</style>

```

여기서 우리가 중요하게 봐야할 점은 **container라는 클래스 안에 내용만 다르고(헤더 1- 본문 1 && 헤더 2 - 본문2) 똑같은 구조로 되어 있는 태그**이다.

> 공통된 부분을 하나만 정의해두고 바뀌는 부분만 전달하면 수정할 때나 추가할 때 아주 편해지지 않을까?

이럴때 v-slot을 사용하면 공통된 부분을 처리할 수 있다.

```js
// src/components/TestSlot.vue

<template>
  <div class="container">
    <header>
      <h1>
        <slot name="header">
          헤더
        </slot>
      </h1>
    </header>
    <main>
      <p>
        <slot name="contents">
          본문
        </slot>
      </p>
    </main>
  </div>
</template>

<script>
export default {
  name: "TestSlot",
};
</script>

<style scoped>
.container {
  width: 500px;
  border: 1px solid black;
  margin: 10px;
}
</style>

```

slot을 통해서 만들어 본 컴포넌트다.

형태가 다른 부분이 무엇일까?

```js
(...)
<slot name="header">
  헤더
</slot>

<slot name="contents">
  본문
</slot>
(...)
```

위와 같이 생긴 부분일 것이다. 해당 부분이 다른 컴포너트에서 컴포넌트의 부분을 교체할 수 있도록 해준다.

> **slot 태그 안에 부분은 기본값**이다.  
> 해당 name(위의 예제에서는 header 또는 contents)의 slot 태그에 전달되는 값이 없으면 기본값을 보여준다

이제 App.vue는 어떻게 변할까?

```js
// App.vue

<template>
  <div id="app">
    <test-slot>
      <template v-slot:header>헤더 1</template>
      <template v-slot:contents>본문 1</template>
    </test-slot>
    <test-slot>
      <template v-slot:header>헤더 2</template>
      <template v-slot:contents>본문 2</template>
    </test-slot>
    <test-slot></test-slot>
  </div>
</template>

<script>
import TestSlot from "./components/TestSlot.vue";
export default {
  name: "App",
  components: {
    TestSlot,
  },
};
</script>

<style>
#app {
  padding: 10px;
}
</style>

```

test-slot 컴포넌트를 불러와 값을 전달하는 형태로 바뀌었고, 아무것도 전달하지 않았다면 기본값이 나오게 될 것이다.

> **template 태그에 v-slot:slot이름** 형태로 각각을 연결했다는 것을 주의깊게 보자.

### v-slot의 축약형

자주 쓰는 형태에는 보통 축약형이 존재하는데 v-slot에도 축약형이 존재한다.  
해당 부분을 **샵(#)** 으로 바꿔보자

```js
<template>
  <div id="app">
    <test-slot>
      <template #header>헤더 1</template>
      <template #contents>본문 1</template>
    </test-slot>
    <test-slot>
      <template v-slot:header>헤더 2</template>
      <template v-slot:contents>본문 2</template>
    </test-slot>
    <test-slot></test-slot>
  </div>
</template>
```

**샵(#)** 으로 바꾼 부분도 문제 없이 작동하는 것을 확인할 수 있을 것이다.

## 범위가 있는 슬롯(scoped slot)

v-slot을 이해할 때 중요한 부분이 아직 한 가지 남았다.  
그건 컴포넌트에 데이터 값에 접근할 수 있다는 것이다.  
예시를 보며 또 이해해보자

```js
// src/components/Profile.vue

<template>
  <div>
    <h5>
      <slot name="profile">
        {{ profileData.nickname }}
      </slot>
    </h5>
  </div>
</template>

<script>
export default {
  name: "Profile",
  data() {
    return {
      profileData: {
        nickname: "RunningWater",
        bio: "Hello RW",
      },
    };
  },
};
</script>

<style></style>

```

위의 컴포넌트에서 **중요한 부분은 기본값으로 내부 데이터를 이용**했다는 것이다.  
만약 아무것도 입력하지 않는다면 profileData.nickname인 "RunningWater"라는 값이 입력될 것이다.

```js
// App.vue

<template>
  <div id="app">
    <profile>
      <template v-slot:profile>
        TWICE
      </template>
    </profile>
  </div>
</template>

<script>
import Profile from "./components/Profile.vue";

export default {
  name: "App",
  components: {
    Profile,
  },
};
</script>

<style>
#app {
  padding: 10px;
}
</style>

```

App.vue에서는 현재 TWICE라는 값을 전달하고 있고, 또 화면에도 TWICE라는 값이 나올 것이다.  
그리고 만약 template태그를 전달하지 않는다면 RunningWater가 화면에 보여질 것이다.  
위에서 다룬 부분이니 혹시 이해가 가지 않는다면 다시 한번 천천히 따라와주길 바란다.

**만약 App.vue에서 Profile.vue의 데이터인 profileData에 접근해서 "bio"속성의 값을 전달할 수 있을까?**

```js
<template>
  <div id="app">
    <profile>
      <template v-slot:profile>
        {{ profileData.bio }}
      </template>
    </profile>
  </div>
</template>
```

위와 같은 형태로 말이다.  
하지만 **App.vue에는 profileData가 정의되지 않았기 때문에 화면엔 아무것도 나타나지 않을 것**이다.

해당 부분을 처리하기 위해서는 아래와 같이 slot 태그에 값을 바인드 시켜주면 된다.

```js
<template>
  <div>
    <h5>
      <slot v-bind:data="profileData" name="profile">
        {{ profileData.nickname }}
      </slot>
    </h5>
  </div>
</template>

<script>
export default {
  name: "Profile",
  data() {
    return {
      profileData: {
        nickname: "RunningWater",
        bio: "Hello RW",
      },
    };
  },
};
</script>

<style></style>

```

중요한 부분은 **slot 태그에 data라는 속성으로 바인드 된 것**이다.  
이렇게 바인드 되었다면 App.vue에서 속성을 받아올 수 있다.

```js
<template>
  <div id="app">
    <profile>
      <template v-slot:profile="props">
        {{ props.data.bio }}
      </template>
    </profile>
  </div>
</template>

<script>
import Profile from "./components/Profile.vue";

export default {
  name: "App",
  components: {
    Profile,
  },
};
</script>

<style>
#app {
  padding: 10px;
}
</style>

```

> **v-slot:profile="props가 아니더라도 다른 편한 이름으로 해도 된다"**

내부에서 data라는 속성으로 연결 했기 때문에 props.data.bio라고 접근을 했고, 새로고침을 하면 데이터가 잘 렌더링 되는 것을 확인할 수 있을 것이다.

해당 형태는 라이브러리 등을 사용할 때 아주 많이 사용하는 형태니 꼭 이해해두자.

### 구조분해 형태가 가능하다.

props라고 받는 부분은 내가 편한 이름을 아무거나 넣으면 된다고 했다.  
props의 값을 확인해보면 아래와 같을 것이다.

```js
{data: {nickname: "RunningWater", bio: "Hello RW",}}
```

즉 구조분해 형태로도 접근이 가능하다는 뜻이다.

```js
<template v-slot:profile="{ data }">
  {{ data.bio }}
</template>
```

template 태그 부분을 적어보면 위와 같은 형태가 될 수 있다.
