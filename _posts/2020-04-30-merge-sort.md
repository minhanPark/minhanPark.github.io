---
title: "문제 51 : merge sort를 만들어보자"
excerpt: "자바스크립트로 merge sort를 만들어 보자."
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

## 문제 51 : merge sort를 만들어보자

```js
// 병합정렬은 다음과 같이 동작합니다.

// 1. 리스트의 길이가 0 또는 1이면 이미 정렬된 것으로 본다. 그렇지 않은 경우에는
// 2. 정렬되지 않은 리스트를 절반으로 잘라 비슷한 크기의 두 부분 리스트로 나눈다.
// 3. 각 부분 리스트를 재귀적으로 합병 정렬을 이용해 정렬한다.
// 4. 두 부분 리스트를 다시 하나의 정렬된 리스트로 병합한다.

// 다음 코드를 완성해라.

function mergeSort(arr){
  if (arr.length <= 1){
    return arr;
  }

  const mid = Math.floor(arr.length / 2);
  const left = arr.slice(0,mid);
  const right = arr.slice(mid);

  return merge(mergeSort(left), mergeSort(right));
}

function merge(left, right){
  let result = [];

  while (/*빈칸을 채워주세요*/ && /*빈칸을 채워주세요*/){
    if (/*빈칸을 채워주세요*/){
      result.push(left.shift());
    } else {
      result.push(right.shift());
    }
  }
  while (left.length) {
    /*빈칸을 채워주세요*/
  }
  while (right.length) {
    /*빈칸을 채워주세요*/
  }

  return result;
}

const array = prompt('배열을 입력하세요').split(' ').map(n => parseInt(n, 10));

console.log(mergeSort(array));
```

## 풀이 1

```js
function mergeSort(arr) {
  // mergeSort의 기저 조건은 들어오는 배열이 1과 같거나 작을때이다.
  if (arr.length <= 1) {
    return arr;
  }

  const mid = Math.floor(arr.length / 2);
  const left = arr.slice(0, mid);
  const right = arr.slice(mid);

  return merge(mergeSort(left), mergeSort(right));
}

function merge(left, right) {
  let result = [];

  while (left.length && right.length) {
    // 들어오는 두 배열 안에 계속 값이 있으면 실행한다.
    if (left[0] < right[0]) {
      result.push(left.shift());
      // shift 메소드는 맨 첫번째 값을 삭제하며 반환한다.
      // 왼쪽 배열의 첫 값과 오른쪽 배열의 첫 값을 비교해서
      // 왼쪽 배열이 작으면 왼쪽배열의 첫번째 값을 shift를 통해 result에 넣는다.
      // 반대일 경우 오른쪽의 첫번째 값을 result에 넣는다.
    } else {
      result.push(right.shift());
    }
  }
  while (left.length) {
    result.push(left.shift());
  }
  while (right.length) {
    result.push(right.shift());
  }

  return result;
}

const array = prompt("배열을 입력하세요")
  .split(" ")
  .map((n) => parseInt(n, 10));

console.log(mergeSort(array));
```

값이 순서대로 들어가지 않고 한 배열 부분만 다 들어가게 되었을 때 사용하는 부분이 밑에 while 반복문 부분이다.  
왼쪽 배열이 [1, 2], 오른쪽 배열이 [3, 4]라고 생각해보면 맨처음 while 반복문에서 왼쪽 배열 부분만 다 result로 들어갈 것이다.  
그리고 왼쪽 배열의 length가 0이 되기 때문에 처음의 while문은 종료될 것이다.  
그 후에 남은 오른쪽 배열 [3, 4]를 처리하는 부분이 밑에 while 반복문이다. 이미 정렬은 되어 있기 때문에  
순서대로 넣기만 하면 된다.
