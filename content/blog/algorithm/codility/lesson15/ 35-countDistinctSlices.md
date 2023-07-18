---
title: Codility Lesson 15 - CountDistinctSlices
date: 2023-07-18 14:02:00
category: Algorithm
draft: false
---

## 문제 설명

정수 M과 음수가 아닌 정수 N으로 구성된 비어 있지 않은 배열 A가 주어집니다. 배열 A의 모든 정수는 M보다 작거나 같습니다.

0 ≤ P ≤ Q < N인 한 쌍의 정수(P, Q)를 배열 A의 슬라이스라고 하며, 이 슬라이스는 A[P], A[P + 1], ..., A[Q] 요소로 구성됩니다. 고유 슬라이스는 고유 번호로만 구성된 슬라이스입니다. 즉, 슬라이스에서 개별 번호가 두 번 이상 발생하지 않습니다.

예를 들어 정수 M = 6과 배열 A를 생각해 봅시다:

    A[0] = 3
    A[1] = 4
    A[2] = 5
    A[3] = 5
    A[4] = 2
(0, 0), (0, 1), (0, 2), (1, 1), (1, 2), (2, 2), (3, 3), (3, 4) 및 (4, 4) 등 정확히 9개의 슬라이스가 있습니다.

목표는 고유한 조각의 수를 계산하는 것입니다.

함수를 작성합니다:

```javascript
function solution(M, A);
```

정수 M과 N개의 정수로 구성된 비어 있지 않은 배열 A가 주어지면 고유한 조각의 개수를 반환하는 함수입니다.

고유 조각의 수가 1,000,000,000보다 크면 함수는 1,000,000,000을 반환해야 합니다.

예를 들어 정수 M = 6과 배열 A가 주어지면 다음과 같습니다:

    A[0] = 3
    A[1] = 4
    A[2] = 5
    A[3] = 5
    A[4] = 2
이면 위에서 설명한 대로 함수는 9를 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N은 [1..100,000] 범위 내의 정수입니다;
- M은 [0..100,000] 범위 내의 정수입니다;
- 배열 A의 각 요소는 [0..M] 범위 내의 정수입니다.

## 문제 접근

*투 포인터를 이용하여 배열에서 슬라이스의 갯수는 "end - front + 1" 이다*

1. 투 포인트 이용
2. M 길이의 빈 배열 생성 
3. 중복 숫자를 나오는 index 고정 (end)
4. start 는 제외하고 그다음부턴 중복 숫자를 나오는 위치를 알고 있다.
5. 다음에 start 가 해당 중복숫자이면 false 로 초기화 하기 때문에 

```javascript
function solution(M, A) {
    const check = new Array(M + 1).fill(false);
    let end = 0;
    let result = 0;

    for (let start = 0; start < A.length; start++) {
        while (end < A.length && !check[A[end]]) {
            result += end - start + 1;
            check[A[end]] = true;
            end++;
        }

        check[A[start]] = false;
    }

    return Math.min(result, 1000000000);
}
```
