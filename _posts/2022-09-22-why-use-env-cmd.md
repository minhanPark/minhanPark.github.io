---
title: "env-cmd로 빌드 시 환경변수 분기 나누기"
excerpt: "빌드 시에 환경변수 분기를 왜 나눠야 하는 지 어떻게 나눌 수 있는 지 알아보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - 시리즈
tags:
  - env
---

## env 파일이 적용되는 순서

기본적으로 환경은 두가지로 나눠질 수 있다.  
개발 환경과 배포 환경이다.  
이 두가지 환경을 사용할 때 환경 변수는 아주 쉽다.

[create-react app env 설정](https://create-react-app.dev/docs/adding-custom-environment-variables/)

> nextjs에서도 똑같은 상황이다.

.env.development 파일이라면 개발환경에서 적용될 것이고, .env.production 파일이라면 배포 환경에서 적용될 것이다.  
.env 파일은 (배포 및 개발 환경에)다 적용 되겠지만 우선 순위가 낮아서 만약 같은 값이 있다면 위의 파일들에 덮어씌어질 것이다.

문제점은 개발환경이 좀 더 세분화되서 나눠질 수 있다는 것이다.  
만약 staging이라는 단계가 있어서 staging의 환경 변수를 넣어야 한다면 어떻게 해야할까?

```json
"scripts": {
    "build:staging": "???"
  }
```

이렇게 좀 더 환경 변수를 세분화 해야할 때 사용하는 것이 **env-cmd**이다.

## 적용 방법 및 원리

위와 같이 스크립트 실행 시 **환경 변수이름을 직접 넣어주면 환경변수들 중에서 가장 우선 순위를 가지게 된다.**

env-cmd는 해당 역할을 스크립트 상에서 해주게 된다.

```json
"scripts": {
    "build:staging": "env-cmd -f .env.staging react-scripts build"
  }
```

위와 같이 정의했다면 npm run build:staging을 실행하면 .env.staging의 환경 변수들이 적용될 것이다.

즉 기본적으로 react나 next에서 제공하는 development/production/test 같은 상황들 말고도 환경변수를 좀 더 세분화해서 사용할 수 있게 된다.
