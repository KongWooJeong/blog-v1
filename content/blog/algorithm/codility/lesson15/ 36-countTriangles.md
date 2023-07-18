---
title: Codility Lesson 15 - CountTriangles
date: 2023-07-18 14:03:00
category: Algorithm
draft: false
---

## 문제 설명

N개의 정수로 구성된 배열 A가 주어집니다. 삼중항(P, Q, R)은 변의 길이가 A[P], A[Q], A[R]인 삼각형을 만들 수 있다면 삼각형입니다. 즉, 삼중항(P, Q, R)은 0 ≤ P < Q < R < N이면 삼각형입니다:

```
A[P] + A[Q] > A[R],
a[q] + a[r] > a[p],
a[r] + a[p] > a[q].
```

예를 들어 다음과 같은 배열 A를 생각해 봅시다:

```
  a[0] = 10 a[1] = 2 a[2] = 5
  a[3] = 1 a[4] = 8 a[5] = 12
```

이 배열의 요소로 구성할 수 있는 삼각형 삼중항은 (0, 2, 4), (0, 2, 5), (0, 4, 5), (2, 4, 5) 등 네 가지가 있습니다.

함수를 작성합니다:

```javascript
function solution(A);
```

이 함수는 N개의 정수로 구성된 배열 A가 주어지면 이 배열에 있는 삼각형 삼중 항의 개수를 반환합니다.

예를 들어 배열 A가 주어지면

```
  a[0] = 10 a[1] = 2 a[2] = 5
  a[3] = 1 a[4] = 8 a[5] = 12
```

이면 위에서 설명한 대로 함수는 4를 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N은 [0..1,000] 범위 내의 정수입니다;
- 배열 A의 각 요소는 [1..1,000,000,000] 범위 내의 정수입니다.

## 문제 접근

*삼각형 조건을 배열 A를 오름 차순으로 정렬하면 하나의 조건만 고려하면된다.*

```javascript
function solution(A) {
    const arr = A.slice().sort((a, b) => a - b);
    let result = 0;

    for (let p = 0; p < arr.length - 2; p++) {
        let r = p + 2;

        for (let q = p + 1; q < arr.length - 1; q++) {
            while(arr[p] + arr[q] > arr[r]) {
                r++;
            }

            result += r - q - 1;
        }
    }

    return result;
}
```
