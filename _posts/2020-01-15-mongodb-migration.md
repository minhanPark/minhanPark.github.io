---
title: "aws의 lightsail에서 ec2 인스턴스로 mongodb를 옮겨보자."
excerpt: "lightsail에서 mongodb 데이터를 ec2의 인스턴스로 옮겨보는 작업을 정리한 것입니다."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - aws
  - database
---

우선 aws와 mongodb 등 제가 지식이 부족한 부분이 많아서 엄청 헤매고, 일반적인 방법이 아닌, 해당 방법으로 성공했기 때문에  
정리해서 올리는 글입니다.  
에러가 난 것을 다 표시할 순 없었지만 더 좋은 방법이 있다면 댓글로 남겨주시면 정말 매우 감사할 것 같습니다!

## 문제점

회사에서 웹개발을 혼자 하다보니 맨처음은 서비스를 lightsail로 서비스 했다가, 상황이 바뀌어서 ec2로 옮겨야 하는 상황이 문제의 시작이었습니다.  
기존에 있던 mongodb의 데이터를 이전하는 방법을 찾다가... **mongoexport도 mongoimport도 또 scp도 다 권한이 없다**고 하고,  
~.pem 파일이 보이는 어딘가에 있어야 하는건지, 아니면 aws는 다른 방식으로 연결하는 건지.. 분명히 chmod 400 ~.pem을 했는데도,  
진짜 아무것도 안되서 이렇게 했습니다. 저처럼 헤매신다면 해당 방법으로도 옮길 수 있다는 것을 알려드릴려고 기록합니다.

## lightsail 인스턴스에 dump 파일 만들기

![ssh를 사용하여 연결](https://drive.google.com/uc?id=14T08pYsjxOIw1r5IugA9uK6CPGQ2-YnK)  
우선 라이트세일의 인스턴스를 눌러보면 해당 화면이 먼저 보이게 됩니다. ssh를 사용하여 연결을 눌러주세요.

그러면 콘솔창이 뜬 것을 확인할 수 있습니다. 거기서 우선은 파일 및 폴더의 목록들을 확인해주세요.

```
ls -a
```

위의 명령어를 입력하면 볼 수 있습니다.

```
mongodump --port 27017
```

이라는 명령어를 입력하면 현재 데이터베이스에 있는 데이터가 덤핑됩니다.  
**기본 포트를 사용하지 않는다면 포트번호를 바꿔주셔야 합니다.**

![dump 폴더 확인하기](https://drive.google.com/uc?id=1IbO8md1OSb0iAGrh3ORaXeoiVFnzRBjC)  
다시 **ls -a** 명령어를 입력하시면 dump폴더가 생긴 것을 확인할 수 있습니다.  
이제 우리가 해야할 건 **이 dump 폴더를 새로운 인스턴스에 옮기는 것**인데 제가 문제가 되었던 점이 리눅스 기반에 대해서도, aws에 대해서도, 문외한이라  
**chmod 400 ~** 할 건 다했는데도 안되서 파일질라를 이용해서 옮겨보도록 하겠습니다.

## 라이트 세일의 ~.pem 다운로드하기

라이트세일의 인스턴스를 클릭하고 밑에 부분을 보시면 아래와 같습니다.  
![프라이빗키 발급받기](https://drive.google.com/uc?id=1TPHqBb4HGkDJNCK5q8lPmnA6P5bCJ2vx)  
읽어보시면 기본갑(ap-northeast-2)키 페어를 사용하도록 이 인스턴스를 구성했고, 계정페이지에서 다운로드 할 수 있다고 나옵니다.  
링크를 눌러 **계정페이지**로 이동해주세요.  
![키 다운로드](https://drive.google.com/uc?id=16tSC7IjoGWr00_2thXL2lsifoQoo-Rpx)  
기본값중에 서울을 다운로드 받겠습니다.  
그러면 **LightsailDefaultKey-ap-northeast-2.pem** 이라는 비슷한 형태의 .pem을 파일을 다운로드합니다.

## 파일질라 다운로드 및 연결하기

[파일질라 다운로드 하기](https://filezilla-project.org/download.php?type=client)  
해당 링크를 클릭하면 파일질라는 다운로드 할 수 있습니다.  
![파일질라 선택화면](https://drive.google.com/uc?id=1PMFUtr3jwErKW2Karw0y8Ihyy-pmBtLS)  
다운로드를 클릭하면 해당 화면이 뜨는데 제일 왼쪽에 있는 기본을 다운받으시면 됩니다.  
인스톨 하시면 **중간에 광고가 나오는데 조심**하셔서 설치하시면 됩니다.  
![파일질라 실행](https://drive.google.com/uc?id=1EqQkCNL7WiR-clx_oYNh0owu7ZwWJpTK)  
실행하시면 해당 화면을 볼 수 있습니다.  
**로컬사이트**는 자신의 컴퓨터를 나타내고, **리모트 사이트**는 연결한 서버를 나타내는데 현재는 연결된 곳이 없기 때문에 이렇게 비워있습니다.  
이미지에 **파일** 밑에 본체 3개 이미지를 누르면 **사이트 관리자** 뜹니다.  
![사이트 관리자 화면](https://drive.google.com/uc?id=1QW1-f2j0StcH-am5S6DzyHPj0UOEyg-Q)  
**New site**를 통해서 새로운 사이트를 만들고 **프로토콜은 SFTP - SSH File Transter Protocol**로 설정하고,  
**호스트는 라이트세일 인스턴스의 퍼블릭 IP**, **포트는 22** ,**로그온 유형은 키파일**, **사용자는 퍼블릭 IP 위에 있던 (보통)bitnami**를 입력해주세요.  
마지막으로 다운로드 받았던 키파일을 찾아보기를 통해서 로드해주세요.  
이제 리모트 사이트에 해당 라이트세일 인스턴스의 파일들이 보이는 것을 확인할 수 있습니다.  
여기서 **dump 파일을 다운로드** 해주세요.  
그리고 이 접속 과정을 ec2로 만든 인스턴스에도 똑같이 한 뒤에 **ec2의 인스턴스에 dump 파일을 올려주세요.**

![putty로 접속](https://drive.google.com/uc?id=18e6A_Kw3q3a5wJIsney3ECnDZ7tp-fHP)  
ec2의 인스턴스에 putty로 접속해서 **ls -a** 명령어를 입력해보시면 dump 폴더가 잘 올라와져 있는 것을 확인할 수 있습니다.  
[putty로 ec2 인스턴스 접속하시는 방법을 모르시면 여기를 눌러주세요.](https://minhanpark.github.io/today-i-learned/ec2-connection-with-putty/)

## mongodb 데이터 복구하기

이제 dump 폴더를 통해 데이터를 복구해보겠습니다.

```
mongorestore -h 127.0.0.1 ~/dump/
```

로컬에 mongoDB가 있고, 해당 명령어를 입력하면 우리가 옮기려고 했던 데이터가 옮겨져 있는 것을 확인할 수 있습니다.
