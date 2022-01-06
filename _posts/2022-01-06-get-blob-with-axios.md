---
title: "Blob에 대해서 이해하고 엑셀 파일 갖고 오기"
excerpt: "Blob에 대해서 알아보고 axios를 통해서 엑셀 파일을 갖고 오기"
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - javascript
---

## Blob이란 무엇인가?

Blob은 Binary Large Object의 줄임말로 글자 그대로 대용량의 바이너리를 다룰 수 있는 객체라고 할 수 있다.  
프론트엔드에서 데이터를 가지고 엑셀 파일을 만들어줘야 했을 땐 SheetJS를 통해서 했었는데, 이번엔 백엔드에서 엑셀 파일을 만들어서 프론트에서 받기만 하는 형태로 바뀌었고, 이 때 Blob 객체를 활용한다.  
[기존에 이용했던 SheetJS를 npm에서 확인하기](https://www.npmjs.com/package/xlsx)

## Blob 활용하기

우선 Blob 객체를 만들어보자.

```js
const newBlob = new Blob(array, options);
```

new 생성자를 통해서 Blob 객체를 만들어 주고, **array와 options**를 전달할 수 있다.  
array에는 데이터를 전달하는데 전달할 수 있는 데이터 종류에는 ArrayBuffer, ArrayBufferView, Blob, DOMstring 등이 있고 option에는 받는 데이터의 타입 등을 정의할 수 있다.

```js
// response.data에는 axios를 통해서 받은 blob 데이터가 들어가 있음
// application/vnd.openxmlformats-officedocument.spreadsheetml.sheet은 xlsx 데이터 타입
const blob = new Blob([response.data], {
  type: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet",
});
```

위의 경우는 내가 사용한 xlsx 데이터를 Blob으로 받았을 때의 형태이다. 만약에 타입이 pdf라면 "application/pdf"등으로 바꿔주면 된다.

제대로 된 blob 객체를 만들었다면 객체 안에서 size나 type 속성을 확인할 수 있다._(만약 타입을 알 수 없는 경우에는 빈 문자열이 할당된다.)_

이제 만들어진 Blob을 어떻게 활용할 수 있을까? 바로 다운로드가 가능한 url을 만들어서 dom에 연결해주면 된다.

```js
const url = window.URL.createObjectURL(newBlob);
console.log(url); // blob:xxxxxx-xxxxxxx-xxxx-xxx
```

URL.createObjectURL 메소드를 이용하면 Blob 객체를 가상의 url로 만들 수 있고, 이 url을 가지고 a 태그에 붙여주면 다운로드 가능하다.  
만들어지는 url은 주석으로 적어 두었듯이 **blob:xxxxxx** 형태가 된다.

해당 문자열은 Blob을 식별하기 위한 유일한 식별자가 되고, 브라우저가 메모리나 디스크에 저장한 Blob을 가리킨다. 또한 동일 출처상의 문서에서만 유효하고, 영구적이지 않기 때문에 저장해두어도 재활용하기엔 불가능하다는 것이 중요하다.

한번 다운로드 했다면 필요없어진 url을 해제해야한다. 해제하지 않으면 기존 url을 유효하다고 판단하고 자바스크립트 엔진에서 [GC](https://developer.mozilla.org/ko/docs/Web/JavaScript/Memory_Management)되지 않기 때문이다.

```js
window.URL.revokeObjectURL(blobUrl);
```

해당 메소드를 통해서 url을 해제할 수 있다.

```js
async downloadExcel(){
    const response = await getExcelFile()
    const blob = new Blob([response.data], {type: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"});
    const link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download = '파일이름.xlsx';
    link.click();
	URL.revokeObjectURL(link);
}
```

Vue에서 사용한다고 만든 예시이다.

axios를 활용할 때 중요한 점은 responseType에 'blob'을 추가해줘야 한다는 것이다.

```js
// getExcelFile 코드 풀어보기
const getExcelFile = () =>
  instance.get("/example/url", {
    responseType: "blob",
  });
```

기본적으로 responseType에는 Arraybuffer, blob, document, json, text 등이 들어가고 기본값은 text로 되어있다.  
blob타입을 받을 때 responseType을 blob으로 받지 않으면 깨진 형태로 받게 된다.
