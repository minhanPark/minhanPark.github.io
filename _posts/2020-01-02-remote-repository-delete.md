---
title: "원격 저장소 삭제하기"
excerpt: "원격 저장소를 삭제하고 새로운 원격저장소를 입력해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - git
---

기존에 있던 홈페이지에 기능을 추가하기 전에 로컬에서도 확인을 하지만 실제 서비스 되고 있는  
상황에서도 테스트해야할 게 생겨서 새로운 원격 저장소를 만들었다.

## 원격저장소 연결 확인하기

대부분 원격 저장소 이름을 origin으로 넣는다.

```
git remote add origin <.git>
```

이렇게 하다보니 이름이 origin으로 되어있을 것이다. 만약을 위해 아래 명령어로 확인해보자

```
git remote
```

이러면 origin 이라고 뜨는 것을 확인할 수 있다.

## 저장소 삭제하기

원격저장소를 삭제하는 명령어는 git remote rm이다.

```
git remote rm origin
```

만약 origin으로 되어있다면 해당 명령어가 원격저장소를 삭제한다.  
다시 **git remote**를 입력하면 origin이 삭제된 것을 확인할 수 있다.

## 다시 연결하기

삭제했다면 다시 새로운 원격저장소에 연결해보자.

```
git remote add origin https://github.com/---.git
```

해당 명령어를 통해서 다시 연결하고 원격저장소에 가서 새로고침을 해보면 새로운 원격저장소에 파일이 올라와 있는 것을 확인할 수 있다.  
혹시 의심이 되면 README.md를 수정해서 확인해보자!
