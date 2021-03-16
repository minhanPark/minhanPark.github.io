---
title: "라즈베리파이4에 도커 설치하기"
excerpt: "라즈베리파이4에 도커를 설치해보자"
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
  - Docker
---

## 로그인하기

일단 앞에 포스트에 적었던대로 성공적으로 다운로드를 받았다면 로그인하라고 뜰 것이다.

```cmd
localhost login
id: root
password: centos
```

위와 같이 아이디에는 ID에는 root, Password에는 centos를 적어주면 된다.

## wifi 로그인 하기

도커를 설치하려면 일단 인터넷에 연결되어야 한다. 우선 확인할 것은 wifi에 연결할 수 있는 지를 확인하는 것이다.

```cmd
nmcli d
```

해당 명령어를 치면 wifi를 인식하는 지 확인할 수 있다.  
![wifi인식](https://user-images.githubusercontent.com/29043491/111324773-6c0aa300-86ae-11eb-9105-1140bc8a870d.png)  
아직 wifi에 연결하기 전이라면 이미지 같은 상태가 아니라

```cmd
DEVICE  TYPE  STATE          CONNECTION
wlan0   wifi   disconnected  --
```

위와 같은 형태가 된다.

wlan0이 인식되는 것 자체가 wifi가 인식된다는 것이므로 이제 자기의 wifi에 연결하면 된다.

```cmd
nmcli dev wifi list
# 와이파이 리스트 확인
nmcli --ask dev wifi connect wifi이름
# 해당 이름의 wifi에 연결
```

비밀번호까지 쳐서 와이파이에 연결 했다면 **ifconfig** 명령어로 내부 ip를 확인할 수도 있다.

## 도커 설치하기

[공식 문서 확인하기](https://docs.docker.com/engine/install/centos/)
공식문서를 확인하면 상황에 따라 더 정확히 설치할 수 있다.  
아래의 내용도 공식문서를 참고해서 적었다.

```cmd
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

우선 오래된 버전이 남아있을수도 있으니 지우면 된다.

```cmd
sudo yum install -y yum-utils

 sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

yum-utils를 설치하면 yum-config-manager를 사용할 수 있다.  
그리고 repo를 추가하자.

그러면 /etc/yum.repos.d 에 docker-ce.repo가 추가된다.  
추가했다면 해당 repo를 통해서 필요한 것을 설치하면 된다.

```cmd
sudo yum install docker-ce docker-ce-cli containerd.io
```

설치한 후에는 도커를 시작하자.

```cmd
sudo systemctl start docker
```

도커가 잘 설치되었는 지 확인하려면

```cmd
docker ps
```

위와 같이 명령어를 입력해보자. docker ps 는 도커의 컨테이너 목록을 확인하는 명령어이다.  
![docker ps](https://user-images.githubusercontent.com/29043491/111329175-467f9880-86b2-11eb-83dc-7ea743c7b826.png)  
에러가 나지 않고 위의 이미지 같이 실행되었다면 성공적으로 도커가 설치된 것이다.
