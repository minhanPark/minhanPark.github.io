---
title: "라즈베리파이4에 파티션 확장"
excerpt: "라즈베리파이4에 파티션을 확장해보자"
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

## 파티션을 확장해야하는 이유

라즈베리파이를 구매하고, 내가 사용한 microsd 카드의 용량은 128gb 였다.  
하지만 명령어를 통해서 확인해보니 110gb 정도가 사용되지 않는 것이다.

```cmd
df -h
```

해당 명령어를 사용하면 어느 정도 용량을 사용하고 있는지 확인할 수 있다.
라즈베리파이는 microsd카드의 용량을 크게 하더라도, 기본적으로 다 사용하지 않는다.  
**다시 파티션을 잡아줘야 한다.**

내 상황은 **라즈베리파이4에 centos8을 사용**하고 있고, 아래의 글을 따라하며 해결했다.

> [디스크 파티션 확장](https://zelits.tistory.com/65)

나의 경우는 fdisk -l 명령어를 통해서 리스트가 3가지가 나왔고, 해당 글에서 2번째 파티션을 지우고 새로 잡아주는 것이 3번째로 했을뿐이다.

현재는 나머지 용량을 다 root에서 사용하고 있다.

![df](https://user-images.githubusercontent.com/29043491/117411816-f94fd280-af4e-11eb-8a68-086c3c22636f.PNG)
