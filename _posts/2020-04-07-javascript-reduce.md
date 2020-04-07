---
title: "reduce 메소드 활용해보기"
excerpt: "reduce 메소드를 활용해서 평균값을 구해보자."
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

## 문제 18

```js
// 공백으로 구분하여 세 과목의 점수가 주어지면 전체평균 점수를 구하는 프로그램을 작성하세요.
// 단, 소수점 자리는 모두 버립니다.

// 입출력
// 입력 : 20 30 40
// 출력 : 30
```

## 자바스크립트의 reduce

**reduce는 배열의 각 요소에 대해 주어진 리듀서 함수를 실행하고, 하나의 결과값을 반환합니다.**라고 설명되어 있는 reduce 메소드를 살펴보자.

```js
const array = [1, 2, 3, 4];
const sum = array.reduce((acc, cur) => acc + cur);
// sum은 10
```

모든 값을 더했다. 왜 모든 것을 더한 값이 나온 지 순서대로 알아보자.  
reduce는 콜백에서 4개의 인자를 가질 수 있다. **"누산기, 현재 값, 현재 인덱스, 원본배열"**이다.  
위의 코드를 설명하자면 기본 값이 주어지지 않았을 경우엔 누산기는 첫번째 인덱스의 값이 할당된다.  
즉 acc는 1이다. 그리고 현재 값은 2가 들어간다. 콜백함수에 둘을 합친다. 리턴 값은 3이 된다.  
나온 값을 acc에 할당한다. 다시 현재 값이 3이 된다. 콜백함수에서 둘을 합친다. 리턴값이 6이 된다.  
이 순서를 반복하면서 누산기에 값이 하나로 합쳐지고, 하나의 값을 리턴하는 것이다.

```js
arr.reduce((누산기, 현재값, 현재 인덱스, 원본배열) => {...코드 작성...}, 초기값);
```

reduce 메소드에 줄 수 있는 매개변수들을 모두 적어봤다. 콜백 말고도 초기값을 줄 수 있는데, 이럴경우 누산기는 초기값을 가지고 시작하고,  
현재값은 배열의 첫번째 숫자가 된다.

```js
const array = [1, 2, 3, 4];
const sum = array.reduce((acc, cur) => acc + cur, 10);
// 초기값이 10이기 때문에 sum은 20이 된다.
```

## 풀이 1

```js
const score = prompt("점수를 입력하시오");
const splitedScore = score.split(" ");

const sum = splitedScore.reduce((arr, cur) => Number(arr) + Number(cur));

console.log(Math.floor(sum / splitedScore.length));
```

prompt로 입력받는 값은 다 문자가 된다. 그래서 계산하는 값을 Number()를 사용해서 숫자로 바꿔주었다.  
평균값만 계산하기엔 활용도가 큰 함수이지만 공부할겸 사용해보았다.
