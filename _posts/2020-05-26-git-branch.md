---
title: "깃의 브랜치 관련 명령어 알아보기"
excerpt: "깃을 이용하는데 필요한 브랜치 관련 명령어를 알아보자."
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

## 브랜치란?

"나뭇가지"라는 뜻을 생각하면 브랜치가 무엇인 지 유추하기 쉽다. 버전은 어느 순간 다양화 될 수 있다.  
A-B-C 로 나아가다가 C'로 가야하는 순간이 있고, D로 처리해야하는 순간이 있다. 그러면 버전이 두가지가 된다.  
A-B-C-C'와 A-B-C-D이다. 이럴 때 C까지는 공통으로 가지고 있고, 달라지는 부분에서 branch를 만들면 각각의 버전을 가질 수 있을뿐만 아니라  
만약에 에러를 처리해야할 부분이 있다면 브랜치로 만들어서 처리하고, 다시 병합할 수도 있다. 해당 포스팅에서 브랜치에 관련된 명령어들을 정리했다.

## branch 확인 및 생성하기

```
git branch
```

해당 명령어를 입력하면 branch 목록을 확인할 수 있다.  
처음 시작하면 master 브랜치만 있고, 여러 branch를 만들면 해당 명령어를 통해서 확인할 수 있다.

```
git branch 이름
```

해당 명령어를 통해서 branch를 생성할 수 있다.

git log 명령어를 입력했을 때 commit hash뒤에 (HEAD -> master)와 같은 형태로 되어 있을 것이다.  
이때 HEAD가 가르키는 부분이 내가 현재 있는 branch이다.  
생성한 branch로 이동은 어떻게 할 수 있을까?

```
git checkout 이동할 브랜치
```

해당 명령어로 branch를 이동할 수 있다.

각각의 브랜치가 무슨 commit을 가리키고 있는 지 확인하는 방법은 log를 보면 된다.

```
git log --branches --oneline --graph
```

**oneline 옵션은 한줄로 나타나게 하고, graph는 그래프 형태로 보여준다. branches 옵션을 통해서 각각의 브랜치가 어디 commit을 가리키고 있는 지 까지 확인**이 가능하다.

```
git log master..브랜치이름
```

해당 명령어는 master를 기준으로 branch에 무엇이 달라졌는 지를 확인할 수 있다.

## branch 병합

에러가 발생해 branch를 만들고, 해당 부분을 수정했다고 하면 이제 필요한 것은 그 branch를 master에 합치는 것이다.  
이것을 병합(merge)이라고 한다.  
master로 합쳐야 하므로 HEAD가 master로 향해있는 지 확인해야한다.

```
git branch
```

해당 명령어를 통해서 확인할 수 있다. 만약 master가 아니라면

```
git checkout master
```

위의 명령어를 통해서 master를 가리키도록 해야한다.

```
git merge 병합할 브랜치
```

해당 명령어를 입력하면 병합이 된다.  
**병합할 때 주의할 사항은 충돌이 일어날 수 있다는 것**이다.

만약 다른 파일을 수정했다면, 또는 다른 부분을 수정했다면 깃은 알아서 합쳐줄 것이다.  
그러나 같은 부분을 다르게 수정했다면 깃은 무엇을 기준으로 고쳐야할 지 모르기 때문에 충돌이 일어난다.  
이때에는 충돌이 일어난 파일을 직접 수정한 뒤에 commit을 해줘야한다.

```
직접 수정한 뒤에
git commit -am "message"
```

## branch 삭제

병합까지 했다면 이제 branch가 필요 없어졌다고 할 수 있다.

```
git branch -d 브랜치이름
```

해당 명령어를 통해서 branch를 삭제할 수 있다.  
만약에 병합하지 않은 branch를 삭제하려고 하면 오류가 뜨는데 -D 옵션을 주면 강제로 삭제가 가능하다.

```
git branch -D 브랜치이름
```

> 삭제한 브랜치는 같은 이름으로 다시 브랜치를 만들면 예전에 작업했던 내용이 그대로 나타난다.
> 즉 브랜치를 삭제한다는 것은 저장소에서 없애는 것이 아니라 감추는 것이라고 할 수 있다.