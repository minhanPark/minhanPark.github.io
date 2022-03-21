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
  - docker
  - docker-compose
---

## 도커 설치하기

공식 문서를 활용하면 도커는 편하게 설치할 수 있다.  
[도커 설치 공식 문서](https://docs.docker.com/engine/install/ubuntu/)

```bash
 sudo apt-get update

 sudo apt-get install ca-certificates curl gnupg lsb-release
```

apt-get 패키지를 업데이트 하고 위의 패키지들을 설치한다.

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

도커의 공식 GPG 키를 추가한다.

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

stable 버전의 레포지토리를 위의 명령어로 가지고 온다.

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

다시 패키지를 업데이트 하고 도커 관련 패키지들을 설치한다.

```bash
# 유저 root로 변경 후
docker run hello-world
```

명령어를 통해서 도커가 hello-world 컨테이너를 제대로 실행시키는 지 확인하면 된다.

## docker-compose v2 설치하기

현재는 도커 컴포즈 v2가 릴리즈 되어있다.  
라즈베리파이에 우분투 서버 이미지를 다운로드 했다면 기존에 도커 컴포즈 v1은 실행되지 않는다. _맞는 버전이 없기 때문에_

하나씩 따라하며 v2를 설치해보자.

```bash
uname -a
```

만약 라즈베리파이4b에 우분투 서버 이미지를 쓰고 있다면 해당 명령어를 쳤을 때 linux/aarch64를 볼 수 있을 것이다.

[최신 버전 확인](https://github.com/docker/compose/releases)  
현재는 2.3.3 버전이 최신 버전이고, docker-compose-linux-aarch64를 다운받으면 된다.

```bash
# 만약 해당 디렉토리가 없다면 만들도록 하자 /usr/local/lib/docker/cli-plugins
curl -SL https://github.com/docker/compose/releases/download/v2.3.3/docker-compose-linux-aarch64 -o /usr/local/lib/docker/cli-plugins/docker-compose
```

해당 명령어로 레포지토리를 다운로드 하자.

```bash
chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
```

해당 명령어로 바이너리 파일에 권한을 주면된다.

```bash
docker compose version
```

해당 명령어로 우리가 설치한 2.3.3이 나오면 제대로 설치된 것이다.
