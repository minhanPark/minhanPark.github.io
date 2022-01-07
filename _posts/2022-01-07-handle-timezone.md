---
title: "프론트엔드에서 타임존(timezone) 다루기"
excerpt: "타임존에 대해서 알아보고 프론트엔드에서 어떻게 타임존을 다룰 지 알아보자"
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - javascript
---

## 왜 프론트엔드에서 타임존을 생각해야 하나?

우리가 글을 써서 인터넷에 올리면 다른 사람들이 볼 수 있다. 만약 내가 한국에서 글을 올린 시간이 2022년 1월 7일 17:00라고 하자.  
그리고 나서 바로 미국 샌프란시스코에 있는 내 친구가 내 글을 읽는다고 했을 때 그 포스트에서 나타나는 시간은 어떻게 되어야 할까?  
한국이 17:00 였기 때문에 한국과 17시간 차이나는 미국에선 포스팅이 올라간 시간이 2022년 1월 7일 00:00시 일 것이다.
왜 프론트엔드에서 타임존을 다루어야 하는 지 느낌이 오는가?  
만약에 서비스가 글로벌을 생각한다면 데이터의 시간은 utc로 저장하는 것이 좋고, 데이터를 받아오고 나서 프론트엔드에서 타임존을 고려해서 시간을 보여줘야 한다.  
즉 위에 상황처럼 글을 쓰더라도 저장되는건 utc로는 2022-01-07T08:00:00Z이고 한국은 UTC+9 이기 때문에 타임존에 맞게 날짜를 보여주면 되는 것이다.

## 그래서 타임존은 무엇인가?

타임존은 동일한 로컬 시간을 따르는 지역을 의미하고, 주로 해당 국가에 의해 법적으로 지정된다.  
하나의 국가에서 여러 타임존이 있을 수도 있고, 아주 넓은 국가라도 하나의 타임존을 사용하기도 한다.(_중국의 경우가 그렇다_)  
아까 한국과 미국의 샌프란시스코는 17시간 차이가 난다고 했다. 그러면 그냥 그 시간만 빼면 되는 것이 아닐까?  
문제점은 단순히 그렇게 빼기엔 변수가 너무 많다는 것이다. 외국에는 서머타임이 있다. 그래서 하절기에 표쥰시를 원래 시간보다 한 시간 앞당긴 시간으로 이용하기도 하고, 있다가 사라진 나라의 경우도 있을 것이다.(국가나 지역에서 제정해 사용하는 것이라 다 알수도 없을 것)  
그래서 이러한 시간에 대해서 데이터베이스가 필요하고 가장 정확하다고 하는 데이터가 [IANA time zone database](https://www.iana.org/time-zones)이다.

## 일단은 moment.js를 사용해보자

moment.js를 활용하면 위의 상황을 쉽게 풀 수 있다.  
타임존을 사용하기 위해 확장된 버전인 moment-timezone을 다운로드해보자.

```bash
npm install moment-timezone --save   # npm
yarn add moment-timezone             # Yarn
```

다운로드를 받았다면 이제 간단히 타임존에 맞게 시간을 보여줄 수 있도록 함수를 하나 만들어보자.

```js
import moment from "moment-timezone";

export const replaceWithTimezone = (date) => {
  const localtz = moment.tz.guess(true);

  const dateOnTimeZone = moment.utc(date).tz(localtz);

  const formattedDate = dateOnTimeZone.format("YYYY-MM-DD HH:mm:ss");
  return formattedDate;
};
```

코드를 하나씩 보자면 tz.guess를 활용하면 사용자의 타임존을 갖고온다.  
그것을 localtz에 담았고, 들어온 시간을 해당 타임존에 맞게 시간을 바꾼게 dateOnTimeZone이다.  
그리고 리턴되는 시간 포맷을 맞춰줄 수 있는데 'YYYY-MM-DD HH:mm:ss'라고 적으면 시분초까지 다 나올 것이고, 'YYYY-MM-DD'라고만 적으면 연월일 까지만 나오게 될 것이다.

[moment-timezone 문서 보러 가기](https://momentjs.com/timezone/docs/)

하지만 이 좋은 모듈도 지금은 해당 버전까지 유지만 하고 최신버전은 나오지 않는다. 왜그럴까? 포스팅의 2편에서 더 적어보도록 하겠다.
