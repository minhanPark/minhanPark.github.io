---
title: "ec2의 인스턴스에 express로 만든 앱을 deploy하기"
excerpt: "express로 만든 앱을 ec2의 인스턴스에 배포해보도록 하겠습니다."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - aws
  - DevOps
  - nodejs
---

ec2의 인스턴스에 mongodb도 설치하고, 데이터도 옮겼다면 이제 nodejs를 다운로드 한 후에 express로 만든앱을 배포해보도록 하겠습니다.

## nodejs 설치하기

```
sudo yum install -y gcc-c++ make
curl -sL https://rpm.nodesource.com/setup_12.x | sudo -E bash -
```

해당 명령어를 입력한 후에 아래의 명령어도 입력해주세요.

```
sudo yum install -y nodejs
```

그러면 **현재 LTS 버전인 12.14.1 버전을 다운로드하고 설치**합니다.

![버전 확인하기](https://drive.google.com/uc?id=1KUlfcJu1bXf170nvDL8bCMlaydYU3cJY)  
**node -v**를 입력하면 node의 버전을 확인할 수 있고, **npm -v**를 입력하면 같이 다운로드된 npm의 버전을 확인할 수 있습니다.

## 공식문서에서 다운로드 하면 생기는 문제

[aws 공식문서 확인하기](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html)  
여기를 보면 nvm을 통해서 다운로드 하는 방법이 나와 있습니다.

nvm의 최신 버전을 확인하려면 아래 링크를 클릭하면 확인할 수 있습니다.  
[nvm github](https://github.com/nvm-sh/nvm/blob/master/README.md)

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash
```

aws의 공식 문서를 확인하면 위에 부분만 바뀐다면 똑같이 진행할 수 있습니다.  
처음엔 이 방법으로 진행했는데 sudo와 npm을 같이 사용할 수 없는 문제가 발생했습니다.  
**sudo: npm: command not found** 해당 에러 메시지가 떠서 sudo 없이 진행했는데,  
마지막에 express를 빌드하고 npm start 후에 접속하려고 해도 접속이 되지 않아서, 위에 nodejs 설치 방법으로 진행했습니다.  
혹시 해결 방법을 아시거나 [스택오버플로우 확인하기](https://stackoverflow.com/questions/4976658/on-ec2-sudo-node-command-not-found-but-node-without-sudo-is-ok)  
를 통해서 해결할 수 있는 분들은 공식문서 형태로 nvm을 다운로드하고 진행하셔도 될 것같습니다. 해결방법이 있다면  
댓글에 남겨주시길 부탁드립니다 :)

## 깃 다운로드 하기

```
sudo yum install git -y
```

해당 명령어를 입력하면 깃을 다운로드합니다. 잘 설치되었는지 확인하려면 **git --version**을 입력해주세요.

> -y 옵션은 모든 대답에 yes를 하겠다는 의미입니다.

![깃 버전확인](https://drive.google.com/uc?id=1ptrkcbaTWB12Nh9ZuVgiSk_K_Zy2VzlQ)

이제 git을 설치했으니 깃허브에서 프로젝트를 clone 해오시면 됩니다.

```
git clone ~.git
```

저장소를 클론해온 후 현재 디렉토리를 확인해보면 클론한 저장소가 추가되어 있는 것을 확인할 수 있습니다.  
**cd 저장소이름**을 통해서 클론한 저장소 안으로 들어가주세요.

## 노드 모듈들을 설치하고 배포하기

```
sudo npm i -g 전역모듈
npm i
```

등으로 모듈들을 다운로드하고, **sudo npm start**를 입력하시면 됩니다.

```js
  "scripts": {
    "dev:server": "nodemon --exec babel-node src/init.js --delay 2",
    "dev:assets": "gulp dev",
    "build:server": "babel src --out-dir build",
    "build:assets": "gulp build",
    "prebuild": "rm -rf build",
    "build": "npm run build:server && npm run build:assets",
    "start": "cross-env NODE_ENV=production PORT=80 pm2 start build/init.js -i 0"
  },
```

제 프로젝트의 스크립트는 현재 이렇게 생겼고, 저는 **npm run build**의 과정까지 거친 다음에 **sudo npm start**를 실행했습니다.

## 주의사항

저는 port를 80으로 지정했기 때문에 상관없습니다만 port를 다른 것으로 지정하셨다면 네트워킹에 접속하셔서 추가해주셔야 합니다.

![인바운드 규칙 추가하기](https://drive.google.com/uc?id=1k8TlsxbmiBXqJ3TLe81R1-U459PqNzwq)  
**사용자 지정 TCP** - **포트범위는 자기가 리스닝하는 포트** - **소스는 0.0.0.0/0** 를 입력해주시면 됩니다.

또 sudo를 붙이지 않고 npm start를 한다면 접속되지 않습니다.이유는 모릅니다. 아시는분은 댓글에 부탁드립니다 :)  
해당 부분까지 완료했다면 이제 ip주소를 통해서 접속할 수 있습니다.  
![IP 주소 확인하기](https://drive.google.com/uc?id=12cFZKspsXcc8mXHdpuQ99Hnt-f-BqGmf)  
ec의 대시보드에서 인스턴스를 클릭하고 밑에 설명을 보면 **IPv4 퍼블릭 IP**를 확인할 수 있습니다 해당 주소로 접속하실 수 있습니다. 만약에 포트가 80번이 아니라면
**127.0.0.1:포트번호** 와 같은 형태로 접속하셔야 합니다.
