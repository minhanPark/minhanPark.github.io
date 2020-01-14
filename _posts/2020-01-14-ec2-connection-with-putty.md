---
title: "ec2로 만든 인스턴스를 putty를 통해 접속해보자."
excerpt: "putty를 통해서 우리가 만든 인스턴스에 접속해보도록 하겠습니다."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - DevOps
  - aws
---

인스턴스를 만드는데까지 성공했으니 이제 putty를 통해서 접속해보도록 하겠습니다.

## putty 다운받기

putty는 원격접속 프로그램입니다.  
물리적으로 떨어져 있는 서버에 우리가 접속할 수 있도록 도와주는 존재라고 할 수 있죠.

[putty 다운로드](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)  
해당 링크로 들어가면 putty를 다운로드할 수 있습니다. 자신의 사양에 맞게 다운로드 해주시면 됩니다.

## puttygen 실행하기

putty를 다운로드 했다면 puttygen도 설치되어 있습니다.

![puttygen 실행](https://drive.google.com/uc?id=1BR-0s9d6TGlVGMaGsoBs5GKDax9JLavn)

실행하면 해당 이미지처럼 보입니다.

이제 다운로드 받았던 **.pem 파일을 load** 시켜야 합니다. load 버튼을 누르고 다운로드했던 \*.pem 파일을 불러와주세요.

![private key](https://drive.google.com/uc?id=1dPulBPkXQu3J2NjrB99XNO3dLdHFZaAY)  
대충 지웠지만 로드했다면 각자의 키와 지문이 나타났을 겁니다.  
이제는 **Save private key**를 눌러주세요. 그러면 **.ppk** 파일을 어디에 저장할 지 묻습니다. 이름과 확장자를 적고 까먹지 않을 곳에 저장해주세요.  
putty로 접속하실 때 이 파일이 필요합니다.

## putty로 인스턴스 접속하기

이제는 ec2의 인스턴스 창으로 돌아와주세요.

![ec2 인스턴스 보기](https://drive.google.com/uc?id=1-oLIQBVnshkXBQ3YLEztIdKKIbGOrkQ5)  
위에 인스턴스 시작옆에 연결이라고 되어 있는 부분이 있습니다. **연결 버튼을 눌러주세요.**

![연결](https://drive.google.com/uc?id=1E-j0F4gzkK0Br56cOHEXf3ZksO023hqA)  
연결버튼을 누르면 해당 이미지처럼 생긴 창을 볼 수 있습니다.  
**독립실행형 ssh 클라이언트**를 선택하고 제가 지워놓은 부분 중에 **예: ~~~ .pem** 부분 뒤에 ec2~ 되어 있는 부분을 복사해주세요.  
해당 부분이 host name입니다.  
이제 putty를 실행하신 다음에 빈칸을 채워주시면 됩니다.  
![putty 실행](https://drive.google.com/uc?id=1oQ-X3JU4eDIdc4WRCObAi0OcaJ-6sjue)  
host name에는 방금 복사한 ec2~를 넣어주시고, 왼쪽 카테고리에서 **SSH > Auth**를 눌러주세요. 그러면 해당 모습의 창이 뜹니다.  
![Auth 창](https://drive.google.com/uc?id=1nNZG-kBfTv3BTepjVZvDc0Na5M1t3QZZ)  
이제는 **Browse**버튼을 눌러서 우리가 생성했던 **.ppk 파일**을 불러옵니다.  
마지막으로 open을 눌러주세요.  
![연결화면 창](https://drive.google.com/uc?id=1CF-N43ojPOmlS-THCZ3Bw4A2ipfYkYiL)  
연결이 성공하면 해당 창을 볼 수 있습니다.
