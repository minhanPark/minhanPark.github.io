---
title: "nodejs 및 Next.js에서 간편하게 메일 기능 넣기"
excerpt: "nodemailer를 통해서 node app에서 간편하게 메일기능을 구현해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - nodejs
  - Next.js
---

## 노드메일러 설치 및 세팅하기

노드메일러를 설치해보자.

```cmd
npm install nodemailer
```

해당 명령어를 이용하면 간편하게 다운로드 할 수 있다.

노드메일러에 설정을 지메일로 해주면 가장 간단하게 메일보내는 기능을 구현할 수 있다.  
(**_백엔드에서 설정해줘야하기 때문에 React에서는 설정할 수 없으나 Next.js에서는 가능하다._**)

```js
const nodemailer = require("nodemailer");

const transporter = nodemailer.createTransport({
  service: "gmail",
  auth: {
    user: "id@gmail.com",
    pass: "password",
  },
});
```

위와 같이 설정해주면 지메일을 활용해서 보낼 수 있다.

## 보내기 전에 구글에서 보안 수준이 낮은 앱의 액세스를 허용하자.

![구글 헤더](https://user-images.githubusercontent.com/29043491/115137192-99f15780-a05f-11eb-8c67-0c3d3548c168.PNG)  
우선 구글에 로그인한 후에 헤더의 우측상단을 보자.  
그러면 자신의 닉네임을 볼 수 있다.  
해당 부분을 클릭하면 **Google 계정 관리**버튼이 있는데 해당 버튼을 누르자.

![보안](https://user-images.githubusercontent.com/29043491/115137194-9e1d7500-a05f-11eb-9c4b-5fbaea2d5e0f.PNG)  
이제 좌측에 보면 메뉴가 보이는데, 해당 메뉴에서 **보안**을 클릭하자.

![액세스](https://user-images.githubusercontent.com/29043491/115137193-9bbb1b00-a05f-11eb-8421-1b4834e356f2.PNG)  
스크롤을 내리다 보면 보안 수준이 낮은 앱의 액세스가 보인다.  
해당 부분을 사용함 으로 바꿔야한다.

## 메일 보내기

위에서 설정한 transporter의 sendMail 메소드를 활용해서 메일을 보낼 수 있다.

```js
const mailOptions = {
  from: "보내는 메일",
  to: "받는 메일",
  subject: "메일의 제목",
  html: `<p>메일의 내용(html 템플릿 형식으로 작성)</p>`,
  text: "템플릿 정도가 아니고 단순히 텍스트 보낼때는 해당 값으로 보내도 됨",
};

transporter.sendMail(mailOptions, (err, data) => {
  if (err) {
    console.error(err);
    res.status(500).json({ status: "fail" });
  } else {
    res.status(200).json({ status: "success" });
  }
});
```

위와 같이 메일 옵션을 활용해서 보내면 된다.

## 파일 첨부하기

노드메일러는 파일첨부도 아주 간편하다.  
위와 같이 메일옵션에 attachments를 추가하고 상황에 맞게 추가해주면 된다.

```js
const mailOptions = {
    ...
    attachments: [
        {
             path: '/path/to/file.txt'
        }
    ]
}
```

위와 같이 정의하면 해당 위치의 파일이 첩부된다.

```js
const mailOptions = {
    ...
    attachments: [
        {   // utf-8 string as an attachment
            filename: 'text1.txt',
            content: 'hello world!'
        },
        {   // binary buffer as an attachment
            filename: 'text2.txt',
            content: new Buffer('hello world!','utf-8')
        },
        {   // file on disk as an attachment
            filename: 'text3.txt',
            path: '/path/to/file.txt' // stream this file
        },
        {   // filename and content type is derived from path
            path: '/path/to/file.txt'
        },
        {   // stream as an attachment
            filename: 'text4.txt',
            content: fs.createReadStream('file.txt')
        },
        {   // define custom content type for the attachment
            filename: 'text.bin',
            content: 'hello world!',
            contentType: 'text/plain'
        },
        {   // use URL as an attachment
            filename: 'license.txt',
            path: 'https://raw.github.com/nodemailer/nodemailer/master/LICENSE'
        },
        {   // encoded string as an attachment
            filename: 'text1.txt',
            content: 'aGVsbG8gd29ybGQh',
            encoding: 'base64'
        },
        {   // data uri as an attachment
            path: 'data:text/plain;base64,aGVsbG8gd29ybGQ='
        },
        {
            // use pregenerated MIME node
            raw: 'Content-Type: text/plain\r\n' +
                 'Content-Disposition: attachment;\r\n' +
                 '\r\n' +
                 'Hello world!'
        }
    ]
}
```

파일첨부에 활용할 수 있는 옵션들은 위와 같다.  
상황에 맞게 사용하면 된다.
