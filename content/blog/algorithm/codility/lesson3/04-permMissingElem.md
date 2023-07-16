---
title: Codility Lesson 3 - PermMissingElem
date: 2023-07-16 14:00:05
category: Algorithm
draft: false
---

## 문제 설명

서로 다른 N개의 정수로 구성된 배열 A가 주어집니다. 배열에는 [1...(N + 1)] 범위의 정수가 포함되며, 이는 정확히 하나의 요소가 누락되었음을 의미합니다.

여러분의 목표는 그 누락된 요소를 찾는 것입니다.

함수를 작성합니다:

```javascript
function solution(A);
```

배열 A가 주어지면 누락된 요소의 값을 반환하는 함수를 작성합니다.

예를 들어 배열 A가 주어졌을 때

```
A[0] = 2
A[1] = 3
A[2] = 1
A[3] = 5
```

인 경우 함수는 누락된 요소이므로 4를 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N은 [0..100,000] 범위 내의 정수입니다;
- A의 요소는 모두 고유하다;
- 배열 A의 각 원소는 [1...(N + 1)] 범위 내의 정수입니다.

## 문제 접근

_(배열 A 길이 + 1 을 한 연속된 수의 합) - (배열 A 에 연속된 수의 합) 이 배열 A의 누락된 수이다._

1. 배열 A 의 수의 합 구하기.
   - 배열 A 에 연속된 수의 합
2. 배열 A의 길이에서 더하기 1 을 한 배열에서 (index + 1) 이 숫자를 담당하므로 총 합을 구한다.
   - 배열 A 길이 + 1 을 한 연속된 수의 합
3. (배열 A의 길이 + 1 을 한 연속된 수의 합) - (배열 A 에 연속된 수의 합) 을 구한다.

```javascript
function solution(A) {
  if (A.length === 0) {
    return 1
  }

  const sumOfA = A.reduce((acc, value) => acc + value)
  const totalSum = new Array(A.length + 1)
    .fill(1)
    .reduce((acc, _, index) => acc + index + 1, 0)

  return totalSum - sumOfA
}
```
