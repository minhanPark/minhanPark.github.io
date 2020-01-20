---
title: "express로 만든 앱에 https 적용하기"
excerpt: "ec2로 만든 인스턴스에 express 앱을 배포했었습니다. 이제는 이 앱에 https를 적용시켜 보겠습니다."
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

ec2 인스턴스를 만들고, 거기에 mongodb와 nodejs를 설치하고, mongodb의 데이터를 옮기고,  
깃허브에서 코드를 클론해와서 express앱을 배포한 것들은 https를 적용시키기 위함이었습니다.  
이제 우리가 배포한 express 앱에 https로 접속이 될 수 있도록 만들어 보겠습니다.

## Route 53에 도메인 등록하기

cafe24나 후이즈나 무료 도메인을 주는 곳 등 다양하게 도메인을 구매할 수 있는 방법이 있습니다. 자신에게 맞게 도메인을 구입해주시면 됩니다.  
[후이즈에서 산 도메인 Route53에 등록하기](https://app.gitbook.com/@programmer-guides/s/hitchhiker/)  
해당 글은 후이즈에서 산 도메인을 Route53에 등록하는 방법이 나와 있습니다. 각자 산 곳에 맞게 진행한 후 다음 것들을 진행하시면 됩니다.

## 인증서 발급받기

aws의 서비스 중에 **보안, 자격 증명 및 규정 준수**에서 **Certificate Manager**로 접속해주세요.  
아직 아무 등록도 하지 않았다면 **인증서 프로비저닝**과 **사설 인증 기관** 중에 선택할 수 있는 페이지가 뜹니다.  
인증서 프로비저닝의 시작하기를 눌러주세요.  
![공인 인증서 요청하기](https://drive.google.com/uc?id=1kFuiKPQwiI4i2QFMh3HIYMxaKYle39uv)  
공인 인증서 요청을 선택하고 인증서 요청을 해주세요.  
![도메인 추가하기](https://drive.google.com/uc?id=11vHJUNdS60_IRlQlRUJztWsyqNBxpL2I)
Route53에 등록한 도메인을 추가해주시면 됩니다. 저의 경우 example.com의 형태 뿐만 아니라 www.example.com도 사용하고 있어서 와일드카드(\*) 인증서도 요청했습니다.  
![검증 방법 선택하기](https://drive.google.com/uc?id=1DDa8_gQ_KRRPCz2iZ4s7fHmtm3UspQzg)
도메인을 추가한 뒤에 검증 방법을 선택할 때는 DNS 검증을 선택하고 **다음**을 눌러주세요.  
태그는 생략하셔도 됩니다. **검토**를 눌러주세요.
![도메인 확인 페이지](https://drive.google.com/uc?id=12JCwJcttC__tHY7Ll7wTYkOYN_cQOWKq)  
마지막으로 확인창이 나옵니다. 도메인이나 검증 방법을 확인한 뒤 **확인 및 요청**을 눌러주세요.  
![Route53에 레코드 생성](https://drive.google.com/uc?id=1YT5K3cEuGGVnNx6w9ngvcGLyibLokDWQ)  
각 도메인을 클릭하면 **Route 53에서 레코드 생성**이라는 버튼이 있습니다. 버튼을 다 눌러주세요.  
그러면 aws에서 자동으로 Route53 호스팅 영역에 DNS 레코드를 만들어줍니다.  
레코드를 생성했다면 맨 밑에 **계속**을 눌러주세요.
시간이 좀 더 지나면 상태가 발급완료로 바뀝니다.(최대 30분)  
이제 다시 Route53으로 돌아와주세요.  
![CName 추가](https://drive.google.com/uc?id=1fDTrHjaUQ93-jCNCV9wKrzw-5h64Z7l2)  
해당 도메인으로 가시면 아까 추가된 레코드를 확인할 수 있습니다.

## 로드밸런스 생성하기

우선 ec2 보안그룹의 인바운드를 확인해주세요.  
![인바운드 확인하기](https://drive.google.com/uc?id=1BgCWuQ4XfZGzUMlkbpwsifWHdUbhspQC)  
**네트워크 및 보안**에서 **보안그룹**을 누르면 인바운드를 확인할 수 있습니다.  
인바운드에 https도 추가되어 있어야 합니다. 추가되어 있지 않다면 **편집**을 통해서 추가해주세요.  
이제 **로드밸런싱**에서 **로드밸런서**를 눌러주시고, **로드 밸런서 생성**을 해주세요.  
![로드밸런서 생성하기](https://drive.google.com/uc?id=1zKmAwSlejJWEMA8N-_7whA_LaE1GuOt4)  
여러 로드밸런서가 나오는데 맨 첫번째인 Application Load Balancer를 선택해주세요.  
![구성](https://drive.google.com/uc?id=1y-eNSJ4gt9354y9246tH_sfCAuWxY8x5)  
이름은 자신이 구별하기 쉽게 적어주시면 되고, 밑에 리스너에 **리스너 추가**를 통해서 HTTPS를 추가해주세요.  
![가용 영역 설정](https://drive.google.com/uc?id=1Djo2RvwcjTXPH8gYnbbOsBJvcbbYBDbH)  
그리고 밑 부분에 가용영역을 지정해주세요. 저는 3가지를 다 선택했습니다.  
가용영역까지 지정했다면 **다음:보안 설정 구성**을 눌러주세요.  
![인증서 선택](https://drive.google.com/uc?id=1n0akVgtVEBYl2SVGkHfXkg01dd6zkug0)  
ACM에서 인증서 선택(권장)을 선택했다면 아까 도메인 발급 받았던 인증서를 확인할 수 있습니다. 해당 인증서를 선택하고, 다음을 눌러주세요.  
![보안그룹 선택하기](https://drive.google.com/uc?id=1ZAAeQLRWmJbe4QBSeXJYPBir32r4Ymmk)  
이 섹션의 처음에 보안그룹의 인바운드를 확인했었습니다. 해당 보안그룹을 선택하시고, **다음**을 눌러주시면 됩니다.  
![라우팅 구성하기](https://drive.google.com/uc?id=1WVngLPYLGEpb4ddF_muBfDvN-bPpUfZK)  
이름을 적어주시고 **다음:대상등록**을 눌러주시면 됩니다.  
![대상 등록하기](https://drive.google.com/uc?id=1yF26VY0OZuv39LuC4XbThtgeHk9HXgLB)  
이제는 인스턴스에 등록해주시면 됩니다.  
밑에 제가 가지고 있는 인스턴스가 있고, 항목에 추가후에 **다음:검토**를 눌러주세요.  
검토단계에서는 제가 구성한 설정들을 확인할 수 있습니다. 다 맞다면 생성을 눌러주세요!

## Route53 레코드 설정 마무리하기

이제 Route53에서 마무리 설정을 해보겠습니다.  
![A 레코드 편집](https://drive.google.com/uc?id=1OaXyzhLUwHL6LcrbXxoDxarv5IiJMZKJ)  
A 레코드에 별칭을 선택하면 우리가 만든 로드밸런서가 보입니다. 누르면 값이 해당 로드밸런서로 바뀝니다.  
이제 시간이 좀 지나면 https로 접속이 가능해지는 것을확인할 수 있습니다.

## express 코드 처리 부분

저는 example.com로 접속 시 www.example.com로 리다이렉트 되도록 코드가 처리되어 있습니다.

```js
export const redirectWWW = (req, res, next) => {
  const host = req.get("host");
  if (host === "yourdomain.com") {
    res.redirect(301, `https://www.yourdomain.com${req.url}`);
  } else {
    next();
  }
};
```

로드밸런서 덕분에 "http://www.yourdomain.com"로 접속 시 "https://www.yourdomain.com"로 바로 보내지기 때문에 따로 코드부분에서 처리하지는 않았지만  
만약에 http나 https에 따라서 따로 처리를 하고 싶으시면 "req.headers['x-forwarded-proto']"를 이용하시면 됩니다.

```js
if (req.headers["x-forwarded-proto"] === "http") {
  //http일때 코드
} else {
  //그 외에 코드
}
```
