---
title: Codility Lesson 9 - MaxDoubleSliceSum
date: 2023-07-17 14:00:40
category: Algorithm
draft: false
---

## 문제 설명

N개의 정수로 구성된 비어 있지 않은 배열 A가 주어집니다.

0 ≤ X < Y < Z < N이 되는 삼중항(X, Y, Z)을 이중 슬라이스라고 합니다.

이중 슬라이스(X, Y, Z)의 합은 A[X + 1] + A[X + 2] + ... + A[Y - 1] + A[Y + 1] + A[Y + 2] + ... + A[Z - 1]입니다.

예를 들어, 배열 A는 다음과 같습니다:

    A[0] = 3
    A[1] = 2
    A[2] = 6
    A[3] = -1
    A[4] = 4
    A[5] = 5
    A[6] = -1
    A[7] = 2

에는 다음 예제 이중 슬라이스가 포함되어 있습니다:

- 이중 슬라이스 (0, 3, 6), 합계는 2 + 6 + 4 + 5 = 17입니다,

- 이중 슬라이스 (0, 3, 7), 합계는 2 + 6 + 4 + 5 - 1 = 16입니다,
- 이중 슬라이스 (3, 4, 5), 합계는 0입니다.

목표는 모든 이중 슬라이스의 최대 합을 찾는 것입니다.

함수를 작성합니다:

```javascript
function solution(A);
```

이 함수는 N개의 정수로 구성된 비어 있지 않은 배열 A가 주어졌을 때 모든 이중 슬라이스의 최대 합을 반환합니다.

예를 들어, 주어진

    A[0] = 3
    A[1] = 2
    A[2] = 6
    A[3] = -1
    A[4] = 4
    A[5] = 5
    A[6] = -1
    A[7] = 2

이면 배열 A의 이중 슬라이스 중 17보다 큰 합이 없기 때문에 함수는 17을 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성하십시오:

- N은 [3..100,000] 범위 내의 정수입니다;
- 배열 A의 각 요소는 [-10,000..10,000] 범위 내의 정수입니다.

## 문제 접근

_Y 를 기준으로 왼쪽 부분에 대한 부분 값, 오른쪽 부분에 대한 부분 값_

예를들면,

```
1. Y = 2: [2] [6 - 1 ... ] 각 부분의 최댓값
2. Y = 3: [2 + 6] [-1 + 4 ... ] 각 부분의 최댓값
.....
```

이렇게 나누어질 수 있다.

1. 왼쪽 부분 배열, 오른쪽 부분 배열 생성
2. 왼쪽은 index = 1 부터 left 배열에 가장 큰 값을 저장해 나간다.
3. 오른쪽은 배열의 끝 - 2 부터 right 배열에 가장 큰값을 저장해 나간다.
4. 마지막에는 왼쪽, 오른쪽 배열 두개를 순회하면서 가장 큰 값이 나오는 경우를 찾는다.

```javascript
function solution(A) {
  if (A.length <= 3) {
    return 0
  }

  const leftArr = new Array(A.length).fill(0)
  const rightArr = new Array(A.length).fill(0)
  let result = 0

  for (let i = 1; i < A.length - 1; i++) {
    leftArr[i] = Math.max(0, leftArr[i - 1] + A[i])
  }

  for (let i = A.length - 2; i > 0; i--) {
    rightArr[i] = Math.max(0, rightArr[i + 1] + A[i])
  }

  for (let i = 1; i < A.length - 1; i++) {
    result = Math.max(result, leftArr[i - 1] + rightArr[i + 1])
  }

  return result
}
```
