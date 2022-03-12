---
title: "라즈베리파이에서 사용할 우분투 이미지 다운로드"
excerpt: "우분투 공식 홈페이지에서 라즈베리파이에서 사용할 우분분투 이미지를 다운로드 해보자"
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

## 기존 centos에서 ubuntu로 갈아탄 계기

centos는 원래 레드햇 엔터프라이즈 리눅스와 완벽하게 호환되는 무료 기업용 리눅스계 운영 체계라서 회사에서 많이 사용한다. 그러나 이제는 독자적인 행보를 가게 되었다. 그래서 centos stream으로 관리가 되어지는데, 기존의 centos가 아니라면 ubuntu로 바꾸는 것이 좋겠다고 생각해서 새로 라즈베리파이를 세팅하게 되었다. 최근에 회사에서 해킹당하는 모습도 보았기 때문에 내 작고 소중한 개인 서버에 이것저것 설정하는 것들을 기록으로 꾸준히 남길 예정이다.

## sd card 준비하기

우선은 micro sd 카드와 리더기를 준비하자.  
 ![마이크로sd카드 리더기](https://user-images.githubusercontent.com/29043491/158022065-ad3ef1da-31a9-47f5-a287-aefc17e8463d.png)

위의 이미지처럼 생긴것을 준비하면된다. 그래서 해당 마이크로 sd 카드에 우분투 이미지를 넣고 라즈베리파이에 마이크로 sd 카드를 꽂고 부팅시켜주면 된다.  
 용량은 넉넉한 것으로 준비하자.

## imager 설치하기

공식 홈페이지에서 사용방법이 친절히 나와있다.  
[설치 문서 보기](https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#2-prepare-the-sd-card)

![설치 목록](https://user-images.githubusercontent.com/29043491/158022261-19ca0ae9-182b-41db-89c3-ddcf85c7c361.png)  
자기 컴퓨터에 맞는 imager를 설치하자.

imager를 실행하면 아래와 같이 실행될 것이다.  
![imager 실행](https://ubuntucommunity.s3.dualstack.us-east-2.amazonaws.com/original/2X/1/1443a1624b2c3e4f48bc2ec18dbadbdfb052f636.png)

storage에는 마이크로 sd카드를 선택하면 되고, os에는 우분투 이미지를 선택하면 된다.  
CHOOSE OS에 erase 기능도 있지만 어차피 이미지를 넣기전에 알아서 다 지울 것이므로 아래 순서대로 우분투를 선택하자

> CHOOSE OS -> Other general-purpose OS -> Ubuntu -> 최신버전의 Ubuntu Server

나는 최신버전의 라즈베리파이4를 사용하고 있기 때문에 Ubuntu Server 20.04.4 LTS(RPi 3/4/400) 64-bit server OS for arm64 architecture를 다운로드했다.

시간은 20분 정도 걸리니 커피라도 한잔 마시자.

라즈베리파이를 부팅했다면 우선 **_아이디와 비밀번호_**를 입력하라고 할 것이다.

> id: ubuntu, pw: ubuntu

아이디와 비밀번호를 입력하면 바로 비밀번호를 바꿔야한다고 뜬다.  
안전한 비밀번호로 바꾸면 된다.  
다음편 부터는 하나씩 설정을 바꿔보도록 하자.
