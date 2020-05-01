---
title: "문제 53 : 괄호 문자열"
excerpt: "주어진 괄호 문자열이 바른 문자열인지 판단해보자."
toc: true
toc_sticky: true
toc_label: "목차"
# header:
#   teaser: /assets/images/profile.png

categories:
  - today-i-learned
tags:
  - Javascript
  - Algorithm
---

## 문제 53 : 괄호 문자열

```js
// 괄호 문자열 "{", "}", "[", "]", "(", ")"을 입력으로 받았을 때
// 괄호의 모양이 바르게 구성된 것을 바른 문자열이라고 하자.
// 입력으로 주어진 괄호 문자열이 바르면 YES 아니면 NO를 출력하라.
```

## 풀이 1

```js
const openingBrackets = ["[", "{", "("];
const bracketMatch = { "[": "]", "{": "}", "(": ")" };
const brackets = [];
let result = true;

const bracketStr = prompt("괄호문자열을 입력하세요.");

for (let i = 0; i < bracketStr.length; i++) {
  if (openingBrackets.includes(bracketStr[i])) {
    brackets.push(bracketStr[i]);
  } else {
    result = bracketMatch[brackets.pop()] === bracketStr[i];
    console.log("bracketStr[i] is", bracketStr[i], "result is", result);

    if (result === false) {
      break;
    }
  }
}
console.log(`${result ? "YES" : "NO"}`);
```

원래 문제에서는 "(", ")"만 다루었던것 같지만 모든 괄호에 대해서 제대로 닫혔는지 확인하는 풀이다.  
여는 괄호를 만나면 brackets 배열에 push 한다. 닫는 괄호는 만나면 brackets 배열을 pop해서  
나오는 괄호가 닫는 괄호와 쌍인지를 확인하고, result 값을 반환해주면 된다.
