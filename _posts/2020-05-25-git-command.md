---
title: "깃의 기본 명령어 알아보기"
excerpt: "깃을 이용하는데 필요한 기본 명령어를 알아보자."
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

## 깃이란 무엇인가?

깃은 버전관리, 백업, 협업을 할 수 있는 도구로 깃을 통해서 어떤 부분이 변경되었는지도 확인할 수 있고, 다른 사람들과 같이 협업을 할 수도 있다.  
GUI를 제공해주는 데스크톱 버전도 있지만, 주로 커맨드를 통해서 많이 실행하기 때문에 커맨드 라인에서 사용할 수 있는 명령어를 정리했다.

## 깃의 기본 구조

깃으로 보여지는 파일의 상태는 여러가지가 있다. 우선 알아두어야 할 것은 **작업트리와 스테이지, 저장소**이다.  
**작업트리는 우리가 작업을 하고 있는 파일들**을 말한다. 즉 무언가를 수정했고, 아무것도 하지 않았다면 작업트리에 있다.  
**스테이지는 버전을 만들기 전에(저장소에 보내기 전에) 보낼 것들을 추려내는 것**이다. 즉 스테이지에 올린다는 것은 다음 버전에 기록된다는 뜻이다.  
**스테이지에서 저장소로 보내면 버전이 생긴다**.

이렇게 생긴 버전들을 통해서 코드를 관리할 수 있다.

## 등록하기

```
git add 파일이름
```

**해당 명령어로 파일을 스테이지 올린다.** 중요한건 아직 한번도 수정하지 않았다면 기본적으로 **git add 파일이름**을 통해서 깃이 추적할 수 있도록 해야한다.

```
git add .
```

.을 사용하면 작업트리에 있는 것들이 모두 스테이지에 올라간다.

```
git commit -m "메시지"
```

스테이지에 있는 파일들의 수정사항을 모아서 버전을 등록한다.  
**메시지**는 커밋을 보낼때 남기는 부분이고, 이 부분을 통해서 어디가 변경되었는 지 수정사항이 무엇인지 등을 확인할 수 있다.

```
git commit -am "메시지"
```

우선 add를 통해서 스테이지에 올리고, commit을 통해서 버전을 만들지만 위의 명령어를 통해서 add와 commit을 동시에 진행할 수 있다.

## 상태 확인하기

```
git status
```

해당 명령어를 통해서 작업트리와 스테이지에 어떤 파일들이 어떤 상태에 있는 지 확인할 수 있다.

```
git log
```

커밋이 되면 버전이 생기는데, 로그를 통해서 지금까지 commit 했던(버전을 만들었던) 기록을 살펴볼 수 있다.  
**git log --stat**처럼 --stat이라는 옵션을 붙이면 해당 버전에 수정사항이 어디 파일에 있는 지 확인할 수 있다.

```
git diff
```

작업트리 - 스테이지 또는 스테이지 - 최신 커밋(버전) 에서 어떤 부분이 변화되었는 지를 확인할 수 있다.

## 상태 되돌리기 및 취소하기

```
git checkout --파일이름
```

스테이지에 올리기 전에 작업트리에서만 변경이 기록된 파일을 넣으면 **수정한 내용을 취소**한다.

```
git reset HEAD 파일이름
```

스테이지에 올라가 있는 파일을 내린다. 즉 해당 파일을 스테이지에 올리기 전인 작업트리에 있는 상태로 되돌린다.

```
git reset HEAD^
```

최근에 커밋한 파일을 취소하고 스테이지 밑으로 내린다.(작업트리에 둔다.)

```
git reset --hard "되돌아갈 커밋해시"
```

HEAD를 해당 커밋으로 이동한 후(최신 커밋을 해당 커밋으로 이동한 후) 그 다음 commit 된 것들을 다 삭제한다.

```
git revert "취소할 커밋해시"
```

위에서 적은 commit hash 전의 최신상태로 돌아가고, 기존의 commit들은 삭제하지 않고 상태가 되돌아갔다는 commit이 하나더 추가된다.  
**커밋을 남기면 다시 돌아갈 수 있기 때문에 예전 버전으로 돌아가야할 상황이 생길수도 있다면 git revert를 사용해서 커밋을 남겨야 한다.**
