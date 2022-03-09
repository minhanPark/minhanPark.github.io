---
title: "공유기 관리자 페이지 접속하기"
excerpt: "iptime 등 공유기 관리자 페이지 접속해보기"
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - etc
---

## 사설 ip 확인하기

사설 ip의 1번이 공유기 이므로 우선 사설 ip의 확인이 필요하다.

```bash
# 맥 또는 리눅스
ifconfig

# 윈도우
ipconfig
```

위와 자신의 pc나 노트북에서 위와 같은 명령어를 쳐보자.
그러면 아래와 같은 정보들을 볼 수 있다.  
![config 확인](https://user-images.githubusercontent.com/29043491/157448630-6734fc06-1533-40e2-95a6-2ee0f301b738.png)  
그리고 **_en0: 부분에서 inet 192.168~ 라고 ip가 적힌 부분을 주목_**하자.  
해당 부분이 나의 사설 ip단이 된다.  
여기서 만약 192.168.30.120이 적혀 있었다고 하면 나의 사설 ip단은 192.168.30 이 되는 것이고, 해당 부분의 1번에 접속하면 된다.(ex: 192.168.30.1)

![관리자 페이지](https://user-images.githubusercontent.com/29043491/157450039-98ac5436-219b-44fe-ae88-4fdc42257e2e.png)  
나의 경우엔 sk 브로드밴드라서 이러한 페이지고 iptime이라면 또 다른 로그인 창이 보일 것이다.

> iptime일 경우 id는 admin이고 초기 비밀번호는 0000이거나 맨처음 비밀번호를 설정했다면 해당 비밀번호 이다.

나의 경우엔 아이디는 admin이고 매뉴얼에 들어가보면 비밀번호를 알 수 있다.

![비밀번호 확인](https://user-images.githubusercontent.com/29043491/157452430-21dab1fd-1947-4116-8b75-983c8b6de53e.png)

하단부 라벨을 보면 유선 MAC 부분에 숫자와 알파벳(대문자)이 적힌 부분이 있는데 해당 부분을 통해서 내 공유기의 비밀번호를 알 수 있다.(sk 브로드밴드인 경우에만 해당)

> 하여튼 **자기 사설 ip단의 1번에 접속** 후 **비밀번호를 공유기에 맞게 넣으면 접속가능**함
