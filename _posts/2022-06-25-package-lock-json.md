---
title: "package.json과 package-lock.json"
excerpt: "package-lock.json을 꼭 커밋하세요 배포시에도 꼭 필요합니다."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - package.json
  - package-lock.json
  - 배포
---

## package.json이란?

프로젝트를 시작하면 package.json 파일이 만들어진다.  
**npm init** 이나 **npx create-react-app 프로젝트명** 등을 통해서 보통 작업의 시작은 다르지만 자바스크립트 프로젝트의 경우 package.json 파일을 가지게 된다.  
package.json의 예시를 보자.

```json
{
  "name": "프로젝트명",
  "version": "0.1.0",
  "dependencies": {},
  "devDependencies": {},
  "scripts": {}
}
```

기본적인 구조는 이렇게 된다.  
프로젝트의 이름, 버전, 다운 받은 모듈, 프로젝트에서 사용할 스크립트(명령어) 등이다.  
즉 **package.json은 프로젝트의 정보를 담고 있다**고 생각하면 된다.

## 모듈의 버전 정보와 버전 범위

모듈의 버전은 어떻게 나타낼까?  
npm의 모듈은 Semactic Versioning을 따른다. 줄여서 SemVer라고 하는데 세가지 숫자로 버전을 나타내게 된다.

> Major.Minor.Patch  
> ex) 1.2.5에서 Major는 1, Minor는 2, Patch는 5가 된다.

각 자리에 대해서 설명하자면 Patch는 하위 호환성이 유지되면서 버그를 고치는 등의 작업을 했을 때 올리게 되고, Minor는 하위 호환성이 유지되면서 기능이 추가되는 등의 작업이 있었다는 것이다.  
Major는 하위호환성을 안될 정도로 큰 변경이 있는 작업을 했다는 것이다.  
즉 버전에 의하면 A 모듈을 다운로드 했을 때 내가 1.2.5를 사용하나 1.9.5를 사용하나 이 모듈은 많은 기능들이 추가되었을 뿐 하위호환성이 있을 가능성이 높다.

위치에 따른 버전의 의미를 파악했으니 ~와 ^를 붙여서 버전 범위를 알아보자.

> 만약 버전이 Major나 Minor까지만 있다면 뒷 부분은 0이라는 의미를 가진다.  
> 즉 1은 1.0 이 되고, 1.2는 1.2.0이 된다.

### 틸드(~)

모듈이 아래처럼 기록되어 있다고 생각하자.

```
"A모듈": "~1.2.0",
```

틸드는 **현재 지정한 버전의 마지막 자리 내의 범위에서 자동으로 업데이트** 한다.  
즉 저 모듈은 **1.2.0 =< A모듈 버전 < 1.3.0 의 범위**를 가지게 되는 것이다.  
만약 A모듈이 1.2.1 버전이었다면 1.2.1 =< A모듈 버전 < 1.3.0 의 범위를 가질 것이다.  
A모듈이 버전이 1이라면 1.0 =< A모듈 버전 < 2.0 의 범위를 가질 것이다.

### 캐럿(^)

캐럿은 SemVer의 규약을 따른다는 것을 신뢰한다는 가정하에서 동작한다.  
그래서 Minor나 Patch 버전은 하위호환성이 보장된다고 생각해서 업데이트를 진행한다.

```
"A모듈": "^1.2.5"
```

즉 **1.2.5 =< A 모듈 버전 < 2.0.0 까지의 범위**를 가지게 된다.  
하위호환성이 보장된다고 생각해 더 minor까지도 설치가 가능하게 되는 것이다.  
중요한건 Major 버전이 0일 때 예외가 있다는 것이다.  
이떄에는 정식 release상황이 아니기 때문에 업데이트가 자주 일어난다. 그래서 Major 버전이 0일 땐 아래와 같은 예외가 일어난다.

> ^0.0.1 이라고 되어있으면 0.0.1 버전을 설치한다.  
> ^0.1.2 이라고 되어있으면 0.1.2 =< 버전 < 0.2 의 범위를 가진다.  
> ^0 일땐 0.0 =< 버전 <1.0 의 범위를 가진다.

## package-lock.json이란?

이제 버전이 범위를 가지고 해당 모듈들을 설치한다는 것을 우린 알 수 있다.  
여기서 package-lock.json의 역할이 나온다.  
해당 파일은 파일이 생성되는 시점의 의존성 트리를 가진다. 그리고 의존성 트리를 가지기 때문에 개발 상황에서 모듈을 다운로드 했을 시 에러가 나지 않았다면 해당 모듈을 사용해서 배포해서도 문제가 없도록 만든다.  
좀 더 상황을 풀어서 생각해보자.  
보통 package.json이 있는 루트에서 npm install이라는 명령어를 사용하면 dependency에 있는 모듈들을 모두 설치할 것이다.  
그런데 내가 개발 과정에서 있다가 배포할 때 다시 설치하게 되면 설치된 버전이 달라져 오류를 발생시킬 수 있게 되는 것이다. 그래서 결론은 아래와 같다.

> 배포 시 또는 깃 등에 push 할땐 package-lock.json도 꼭 보내자.