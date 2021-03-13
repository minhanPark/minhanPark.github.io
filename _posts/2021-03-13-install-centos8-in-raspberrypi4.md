---
title: "라즈베리파이4에 centos 설치하기"
excerpt: "라즈베리파이4에 centos8 설치해서 개인 서버를 만들어 보자"
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - centos
  - Raspberry-pi
---

## 개인용 서버 만들기

개인용 서버를 만들기 위해서 **라즈베리파이4**를 구매했다.  
해당 환경에서 회사에서 많이 사용하는 **centos**를 설치하고 여러 환경을 설정하고, 도커 환경을 구축해 개인 서버를 만들어 보는중이다.  
회사에서는 도커 스웜을 사용하고 있는데, 언젠가는 라즈베리파이 4를 2대 더 구매해서 스웜 환경을 제대로 구축해보는 것도 재밌을 것 같아서 기대가 된다.

## 라즈베리파이 선택하기

역시나 최신 버전이 좋다고 생각하기 때문에 8GB 라즈베리파이4를 구매했다.  
처음 사보는 것이고 정확히 어떤 것들이 필요한 지 모르기 때문에 스타터 키트를 구매했다.  
(micro sd 카드 리더기, 미니 HDMI 케이블 등이 필요한 지 몰랐기 때문에 스타터 키드로 구매한 것이 좋았다고 생각하고, 혹시 구매를 할 생각이라면 스타터 키트 구매하는 것이 좋을듯하다.)

## centos 설치하기

맨처음엔 회사에서 사용하는 centos7을 다운로드 하려고 했지만 도커를 설치하려고 하면 자꾸 **Not Found**라고 뜨며 설치가 되지 않았다. 그래서 centos 8을 설치하고 도커를 되는 것을 확인한 후에 해당 글을 쓰는 것이기 때문에 여기서는 8을 기준으로 설명을 하게 되었다.

버전 이름에 **Minimal-4**가 들어가 있는 것이 라즈베리파이4의 centos 이다.  
[centos8-minimal4 다운로드 링크 가기](https://people.centos.org/pgreco/CentOS-Userland-8-stream-aarch64-RaspberryPI-Minimal-4/)  
해당 버전을 다운로드 받고 압출을 풀자.

이미지를 다운로드 했다면 이제 필요한 것은 micro sd 카드에 centos 이미지를 심는 일이다.  
sd 카드 리더기에 micro sd 카드를 넣고, 컴퓨터에 연결하자.  
이미지를 심는데 사용한 프로그램은 balenaEtcher이다.  
[balenaEtcher 사이트 가기](https://www.balena.io/etcher/)  
해당 사이트에서 자신의 컴퓨터 환경에 맞게 설치를 하자.

![설치 후 화면](https://user-images.githubusercontent.com/29043491/111022569-c7baff00-8416-11eb-9efd-d4f879dc1c0c.png)  
설치하고 나서는 해당 화면을 볼 수 있다.  
압축을 푼 이미지 파일을 넣고, 타겟에는 우리가 연결한 micro sd카드를 선택해주자.

## mciro sd 카드에 내용 지우기

만약 sd 카드에 이미지를 지워야 한다면 sd 카드 formatter를 사용해서 이미지나 내용물을 지울 수 있다.

[sd card formatter 다운로드 사이트](https://www.sdcard.org/downloads/formatter/)

## 장착하기

라즈베리파이4는 우리가 위에서 작업한 micro sd 카드를 꽂고, 전원을 켜면(충전기를 연결하면)
해당 os를 바로 설치하고 일반적으로 계속 그 sd 카드에 데이터들을 저장하는 형태가 된다. (윈도우 처럼 부팅 usb가 따로 있는 형태가 아님)  
![라즈베리파이4](https://user-images.githubusercontent.com/29043491/111022934-5b8dca80-8419-11eb-909e-7a235024acaa.jpeg)
