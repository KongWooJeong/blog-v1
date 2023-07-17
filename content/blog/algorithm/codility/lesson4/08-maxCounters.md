---
title: Codility Lesson 4 - MaxCounters
date: 2023-07-16 14:00:09
category: Algorithm
draft: false
---

## 문제 설명

처음에는 0으로 설정된 N개의 카운터가 주어지며, 이 카운터에는 두 가지 가능한 연산이 있습니다:

- 증가(X) - 카운터 X가 1씩 증가합니다,
- 최대 카운터 - 모든 카운터가 모든 카운터의 최대값으로 설정됩니다.

비어 있지 않은 M개의 정수로 구성된 배열 A가 주어집니다. 이 배열은 연속 연산을 나타냅니다:

- A[K] = X, 즉 1 ≤ X ≤ N이면 연산 K는 증가(X)입니다,
- A[K] = N + 1이면 연산 K는 최대 카운터입니다.

예를 들어 정수 N = 5와 배열 A가 있다고 가정해 보겠습니다:

```
A[0] = 3
A[1] = 4
A[2] = 4
A[3] = 6
A[4] = 1
A[5] = 4
A[6] = 4
```

는 각 연속 연산 후 카운터의 값이 됩니다:

```
(0, 0, 1, 0, 0)
(0, 0, 1, 1, 0)
(0, 0, 1, 2, 0)
(2, 2, 2, 2, 2)
(3, 2, 2, 2, 2)
(3, 2, 2, 3, 2)
(3, 2, 2, 4, 2)
```

목표는 모든 연산 후 모든 카운터의 값을 계산하는 것입니다.

함수를 작성합니다:

```javascript
function solution(N, A);
```

정수 N과 M개의 정수로 구성된 비어 있지 않은 배열 A가 주어지면 카운터의 값을 나타내는 정수 시퀀스를 반환하는 함수입니다.

결과 배열은 정수의 배열로 반환되어야 합니다.

예를 들어, 주어진

```
A[0] = 3
A[1] = 4
A[2] = 4
A[3] = 6
A[4] = 1
A[5] = 4
A[6] = 4
```

함수는 위에서 설명한 대로 [3, 2, 2, 4, 2]를 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N과 M은 [1..100,000] 범위 내의 정수입니다;
- 배열 A의 각 요소는 [1..N + 1] 범위 내의 정수입니다.

## 문제 접근

_최대 카운터(maxCount)를 설정하는 방법을 모든 배열을 최대카운터를 한번에 설정(모든 배열을 순회하면서 설정)하게되면 성능측면에서 떨어지므로 다음 카운트를 증가할때 해당 인덱스만 최대카운터로 설정_

1. 반환되어야 할 배열을 생성하여 모두 0 으로 채운다.
2. 최대카운터가 발생하면 별도의 변수로 설정한다.
3. 최대카운터가 아닐 경우엔
   1. 해당 index의 값보다 최대카운터가 크다면 result[index] 값을 최대 카운터로 초기화한다.
   2. 해당 index의 값을 증가 시킨다.
4. 마지막 모든 배열을 모두 순회하고 최대카운터를 설정하지 못한 경우(증가를 하지 못한 index 값)에 maxCount 를 설정해줍니다.

```javascript
function solution(N, A) {
  let maxCount = 0
  let max = 0
  const result = new Array(N).fill(0)

  for (let i = 0; i < A.length; i++) {
    const index = A[i] - 1

    if (A[i] >= N + 1) {
      maxCount = max
    } else {
      if (maxCount > result[index]) {
        result[index] = maxCount
      }

      result[index]++

      if (result[index] > max) {
        max = result[index]
      }
    }
  }

  return result.map(value => {
    return value < maxCount ? maxCount : value
  })
}
```
