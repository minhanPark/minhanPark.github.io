---
title: "node-schedule 간단한 정리"
excerpt: "node-schedule 설치와 실행 및 취소를 간단히 정리해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - nodejs
  - cron
---

## cron을 사용해야하는 이유

자바스크립트에서 제공하는 setInterval을 사용하면 특정 시간마다 함수를 실행시킬 수 있다.  
하지만 **"실행시켜야 하는 시간"**이 복잡해지면 관리하기가 어려워진다.  
그런 경우에는 node-schedule 등의 라이브러리를 통해서 실행한다면 문제를 해결할 수 있다.

## 설치 방법 및 구성

```cmd
npm install node-schedule
```

위의 명령어를 통해서 설치할 수 있다.  
설치한 후에는 모듈을 불러오자.

```js
const schedule = require("node-schedule");
```

## 실행 및 종료방법

scheduleJob 메소드를 통해서 작업을 지정할 수 있고, cancel 메소드를 통해서 작업을 종료할 수 있다.  
scheduleJob의 첫번째 인수에는 시간이 들어간다.

```
*    *    *    *    *    *
┬    ┬    ┬    ┬    ┬    ┬
│    │    │    │    │    │
│    │    │    │    │    └ day of week (0 - 7) (0 or 7 is Sun)
│    │    │    │    └───── month (1 - 12)
│    │    │    └────────── day of month (1 - 31)
│    │    └─────────────── hour (0 - 23)
│    └──────────────────── minute (0 - 59)
└───────────────────────── second (0 - 59, OPTIONAL)
```

각 자리에 알맞은 숫자를 넣어주면 된다.  
매 시간의 1분마다 실행하려고 한다면 어떻게 하면 될까?  
"1 \* \* \* \*"을 입력하면 매 시간의 1분마다 실행한다.(1시 1분, 2시 1분 3시 1분...)
초를 지정하는 부분은 생략할 수 있기 떄문에 생략하고, 실행할 시간 1분이라고 입력했기 때문이다.

이와 비슷하게 1분마다 실행하게 하려면 어떻게 해야할까?  
"/"을 이용하면 된다.  
"_/1 _ \* \* \*"을 입력하면 1분마다 실행이 된다. (1시 1분, 1시 2분, 1시 3분...)

위를 바탕으로 예시를 만들어 본다면 아래와 같이 된다.

```js
const job1 = schedule.scheduleJob("1 * * * *", function () {
  console.log("매 시간의 1분마다 실행(1시 1분, 2시 1분 3시 1분...)");
});

const job2 = schedule.scheduleJob("*/1 * * * *", function () {
  console.log("1분 마다 실행(1시 1분, 1시 2분, 1시 3분...)");
});
```

job1이나 job2에 변수로 할당한 이유는 취소를 시키기 위함이다.

```js
job1.cancel();
```

위와 같이 cancel 메소드를 통해서 스케쥴을 종료할 수 있다.
