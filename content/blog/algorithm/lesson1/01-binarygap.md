---
title: Codility Lesson 1 - BinaryGap
date: 2023-07-16
category: Algorithm
draft: false
---

## 문제 설명

양수 N 내의 이진 갭은 N의 이진 표현에서 양쪽 끝이 1로 둘러싸인 연속된 0의 최대 시퀀스입니다.

예를 들어, 숫자 9는 이진 표현 1001을 가지며 길이 2의 이진 간격을 포함합니다. 숫자 529는 이진 표현 1000010001을 가지며 길이 4와 길이 3의 두 이진 간격을 포함합니다. 숫자 20은 이진 표현 10100을 가지며 길이 1의 이진 간격을 하나 포함합니다. 숫자 15는 이진 표현 1111을 가지며 이진 간격을 갖지 않습니다. 숫자 32는 이진 표현 100000을 가지며 이진 간격이 없습니다.

Write a function:

```
class Solution { public int solution(int N); }
```

함수는 양의 정수 N이 주어지면 가장 긴 이진 간격의 길이를 반환합니다. N에 이진 간격이 포함되지 않으면 함수는 0을 반환해야 합니다.

예를 들어, N이 1041이면 N은 이진 표현 10000010001을 가지므로 가장 긴 이진 간격은 길이 5이므로 함수는 5를 반환해야 합니다. N이 32이면 N은 이진 표현 '100000'을 가지므로 이진 간격이 없으므로 함수는 0을 반환해야 합니다.

Write an **efficient** algorithm for the following assumptions:

- N is an integer within the range [1..2,147,483,647].

## 문제 접근

1. 숫자를 2진수로 변환
2. 2진수 문자열을 반복문을 진행하면서 처음 "1" 이 나올 경우 하고 그 다음 "1"인 나올 경우 사이에 수를 센다.
3. "1" 과 "1" 사이에 갯수 와 현재 가장 큰 갯수를 비교해가면 업데이트 한다.

```javascript
function solution(N) {
  const binaryNumber = N.toString(2)
  let isFirst = false
  let startCountIndex = 0
  let maxCount = 0

  for (let i = 0; i < binaryNumber.length; i++) {
    if (isFirst && binaryNumber[i] === '1') {
      maxCount = Math.max(maxCount, i - startCountIndex - 1)
      isFirst = false
    }

    if (binaryNumber[i] === '1') {
      startCountIndex = i
      isFirst = true
    }
  }

  return maxCount
}
```
