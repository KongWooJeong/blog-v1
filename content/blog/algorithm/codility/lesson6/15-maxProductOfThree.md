---
title: Codility Lesson 6 - MaxProductOfThree
date: 2023-07-17 14:00:16
category: Algorithm
draft: false
---

## 문제 설명

N개의 정수로 구성된 비어 있지 않은 배열 A가 주어집니다. 삼중항(P, Q, R)의 곱은 A[P] _ A[Q] _ A[R](0 ≤ P < Q < R < N)과 같습니다.

예를 들어, 배열 A는 다음과 같습니다:

```
  A[0] = -3
  A[1] = 1
  A[2] = 2
  A[3] = -2
  A[4] = 5
  A[5] = 6
```

에는 다음과 같은 예제 삼중 항이 포함되어 있습니다:

```
(0, 1, 2), 곱은 -3 * 1 * 2 = -6입니다.
(1, 2, 4), 곱은 1 * 2 * 5 = 10입니다.
(2, 4, 5), 곱은 2 * 5 * 6 = 60입니다.
```

목표는 모든 삼항식의 최대 곱을 찾는 것입니다.

함수를 작성합니다:

```
function solution(A);
```

비어 있지 않은 배열 A가 주어지면 모든 삼항식의 최대 곱의 값을 반환하는 함수를 작성합니다.

예를 들어, 배열 A가 주어졌을 때

```
  A[0] = -3
  A[1] = 1
  A[2] = 2
  A[3] = -2
  A[4] = 5
  A[5] = 6
```

인 경우 함수는 삼중항(2, 4, 5)의 곱이 최대이므로 60을 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성하십시오:

- N은 [3..100,000] 범위 내의 정수입니다;
- 배열 A의 각 요소는 [-1,000..1,000] 범위 내의 정수입니다.

## 문제 접근

_배열 A를 내림차순으로 정렬후 최대곱의 값을 구한다._

1. 배열을 복사하여 내림차순으로 정렬한다.
2. index (0 , 1 , 2) 번째 값을 곱한 값을 구한다.
3. index (A.length - 1, A.length - 2, 0) 번째 값을 곱한 값을 구한다.
   - 왜?! 음수 \* 음수 = 양수 가 될 수 있으므로 마지막 두개의 값을 이용하여 양수를 만들고 첫번째 값을 곱한다.
4. 두개 결과를 비교하여 큰 값을 반환한다.

```javascript
function solution(A) {
  const sortedA = A.slice().sort((a, b) => b - a)
  const result = sortedA.slice(0, 3).reduce((acc, value) => acc * value)

  const result2 = sortedA[A.length - 1] * sortedA[A.length - 2] * sortedA[0]

  return Math.max(result, result2)
}
```
