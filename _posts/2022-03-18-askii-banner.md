---
title: "우분투 로그인 시 커스텀 배너를 보여주자"
excerpt: "우분투 로그인에 성공했을 때 커스텀한 배너를 보여주자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - ubuntu
  - Raspberry-pi
---

## 배너의 필요성?

사실 딱히 필요없다. 자기만족일 뿐.

![기본 배너](https://user-images.githubusercontent.com/29043491/158949904-05c671bb-c779-4d76-a233-363344d1bade.PNG)

우분투 서버에 접속하면 기본적으로 이런 화면이 보일 것이다.  
우리는 여기에 약간의 커스텀을 추가해서 더 접속하고 싶도록 만들어보자.

## modt 파일 추가

기본적으로 /etc/motd 파일을 수정하면 저 로그들 아래에 커스텀한 내용을 넣을 수 있다.

> 파일이 없다면 생성하자.(/etc/motd)

사용하고 싶은 텍스트나 그림을 아스키 코드로 해서 만들면되는데, 하나하나 만들기보다 있는 것을 갖고오면 편하고 좋다.

[텍스트 만들기](https://patorjk.com/software/taag/#p=display&h=0&v=0&f=Big&t=Type%20Something%20)

위에 접속해서 사용하고 싶은 텍스트를 쓰고 폰트 등을 선택하자. 모든 폰트를 확인하고 싶을 때는 타이핑한 후 **_Test All_**을 눌러서 전체적인 모습을 봐도 좋다.

![타이핑 확인](https://user-images.githubusercontent.com/29043491/158952393-3b61c3e8-563a-4942-92c6-634ebeaf3dcd.png)

[아스키 코드 아트](https://www.asciiart.eu/)

위의 사이트에서는 아스키 아트를 가지고 오자.  
각각을 복사해서 motd 파일에 넣어주면 필요한 것은 준비가 끝난 것이다.  
저장 후에 putty나 커맨드 창을 하나 더 켠 다음 ssh 접속을 해보자.

![내 화면](https://user-images.githubusercontent.com/29043491/158952683-d37a472d-e3ff-4704-bacc-5b26b689c2b8.png)

위와 같은 훌륭한(?) 배너를 확인할 수 있다.
