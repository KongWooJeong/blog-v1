---
title: Codility Lesson 3 - TapeEquilibrium
date: 2023-07-16 14:00:06
category: Algorithm
draft: false
---

## 문제 설명

N개의 정수로 구성된 비어 있지 않은 배열 A가 주어집니다. 배열 A는 테이프에 있는 숫자를 나타냅니다.

0 < P < N이 되는 정수 P는 이 테이프를 비어 있지 않은 두 부분으로 나눕니다.

- A[0], A[1], ..., A[P - 1] 및 A[P], A[P + 1], ..., A[N - 1].

두 부분의 차이는 다음의 값입니다:

- |(a[0] + a[1] + ... + a[p - 1]) - (a[p] + a[p + 1] + ... + a[n - 1])| 입니다.

즉, 첫 번째 부분의 합과 두 번째 부분의 합 사이의 절대적인 차이입니다.

예를 들어 다음과 같은 배열 A를 생각해 봅시다:

```
A[0] = 3
A[1] = 1
A[2] = 2
A[3] = 4
A[4] = 3
```

이 테이프를 네 곳으로 나눌 수 있습니다:

```
P = 1, 차이 = |3 - 10| = 7
P = 2, 차이 = |4 - 9| = 5
P = 3, 차이 = |6 - 7| = 1
P = 4, 차이 = |10 - 3| = 7
```

함수를 작성합니다:

```javascript
function solution(A);
```

비어 있지 않은 N개의 정수 배열 A가 주어졌을 때 얻을 수 있는 최소 차이를 반환하는 함수입니다.

예를 들어, 주어진

```
A[0] = 3
A[1] = 1
A[2] = 2
A[3] = 4
A[4] = 3
```

이면 위에서 설명한 대로 함수는 1을 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N은 [2..100,000] 범위 내의 정수입니다;
- 배열 A의 각 요소는 [-1,000..1,000] 범위 내의 정수입니다.

## 문제 접근

_아래 두 구역으로 나누어서 생각해야 한다._

- left : A[0]
- right : A[1] + A[2] ... A[N - 1]

_동적계획법을 이용하여 배열을 index 1 부터 **순회**하면서 **left 에 A[index]를 더하고** **right 에 A[index] 를 빼주면**된다._

1. A[0] 과 A[1] + A[2] .. + A[N - 1] 두 구역의 값을 구한다.
   - left : A[0]
   - right : A[1] + A[2] ... A[N - 1]
2. 배열을 순회하면서 아래 와 같은 로직을 수행한다.
   - left : A[index] 더하기
   - right: A[index] 빼기
3. 각 배열을 순회하면서 **|left - right|** 값을 비교하여 가장 작은 값을 result 변수에 저장한다.
4. result 변수를 반환한다.

```javascript
function solution(A) {
  let left = A[0]
  let right = A.slice(1).reduce((acc, current) => acc + current)
  let result = Number.MAX_SAFE_INTEGER

  for (let i = 1; i < A.length; i++) {
    result = Math.min(result, Math.abs(left - right))

    left += A[i]
    right -= A[i]
  }

  return result
}
```
