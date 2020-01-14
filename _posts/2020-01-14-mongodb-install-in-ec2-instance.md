---
title: "ec2로 만든 인스턴스에 mongodb를 설치해보자."
excerpt: "ec2로 만든 인스턴스에 mongodb 설치하는 것을 따라해봅시다."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - aws
---

ec2로 만든 인스턴스에 mongodb를 설치하려고 했는데, 일반 우분투 서버(기존에는 라이트세일을 통해서 서비스했었던지라..)처럼 하면 설치가 안되길래  
이렇게 다음에 헤매지 않으려고 흔적을 남깁니다.

## 설치방법 보기

[공식문서 보기](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-amazon/)  
우선 공식문서를 보는게 제일 좋습니다. 여기를 따라할 예정이고 버전도 업그레이드 된다면 바로바로 공식문서에서도 보기 좋게 변하기 때문입니다.  
우선 자신의 아마존 리눅스가 무슨 버전인지 확인부터 하셔야합니다. 저는 프리티어의 아마존 리눅스 2를 사용하고 있어서 해당 방법을 따라하였습니다.

## 설치 따라하기

> Create a /etc/yum.repos.d/mongodb-org-4.2.repo file so that you can install MongoDB directly using yum

우선 이렇게 적혀 있으니 해당 설명처럼 만들어보겠습니다.

```
sudo vi /etc/yum.repos.d/mongodb-org-4.2.repo
```

를 입력해주세요.  
![vi 에디터 화면](https://drive.google.com/uc?id=1uUwWsTSM0EdrHe22wsGjKj-O4DmZLPbi)  
해당 명령어를 실행하면 vi 에디터를 통해서 (존재하지 않는다면)해당 파일을 만들고 편집창을 열어줍니다.  
이제 입력하려면 명령어들을 몇개 입력해야 합니다. 해당 커맨드 창에서 **i**를 눌러주세요.  
![insert 화면](https://drive.google.com/uc?id=1W4jTS6bDynLaO68nU97cbO7E-Vr0aRbl)  
그러면 이렇게 밑부분이 **INSERT**라고 바뀐 것을 확인할 수 있습니다.  
이렇게 뜬다면 이제 입력할 수 있습니다.

```
[mongodb-org-4.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/amazon/2/mongodb-org/4.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.2.asc
```

공식 문서에 보면 이렇게 나와 있는 것을 확인할 수 있습니다. 해당 부분을 copy해서 붙여넣어 주세요.  
공식문서에서 **COPY**버튼을 통해 간편하게 복사하고, 커맨드창에서 마우스로 오른쪽을 클릭해주세요.  
그럼 간단하게 복사 붙여넣기가 됩니다.  
이제 **esc**를 누르면 입력모드가 종료됩니다. 입력모드를 종료했으니 파일을 저장하고 끄시면 됩니다.

```
:wq
```

위의 명령어를 입력하고 엔터를 누르면 파일을 저장하고 끄는 것을 확인할 수 있습니다.  
![명령어 확인](https://drive.google.com/uc?id=1tpj08-ZysEdEqks2Y-bakthbA3wIHvkm)  
명령어를 입력하면 이렇게 밑부분에서 확인할 수 있습니다.

```
sudo yum install -y mongodb-org
```

해당 명령어를 복사해서 붙여넣어주세요. 공식문서에서 복사해서 붙여넣으시면 편합니다.  
그러면 설치하고 **Complete**라는 말을 볼 수 있습니다.

```
sudo service mongod start
```

해당 명령어를 입력하면 mongod가 실행됩니다.  
![mongo 실행](https://drive.google.com/uc?id=1ulx7G59zRzrlmIttdRLCK7BQb90vOvDE)  
이제 커맨드창에 **mongo**라고 입력하시면 위와 같이 mongoDB가 실행되는 것을 확인할 수 있습니다.
