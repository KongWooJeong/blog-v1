---
title: Codility Lesson 11 - CountNonDivisible
date: 2023-07-18 14:00:46
category: Algorithm
draft: false
---

## 문제 설명

N개의 정수로 구성된 배열 A가 주어집니다.

0 ≤ i < N인 각 숫자 A[i]에 대해 A[i]의 나눗셈이 아닌 배열의 원소 수를 세고 싶습니다. 이러한 원소를 제수가 아닌 원소라고 합니다.

예를 들어 정수 N = 5와 배열 A를 생각해 봅시다:

    A[0] = 3
    A[1] = 1
    A[2] = 2
    A[3] = 3
    A[4] = 6
다음 요소의 경우:

- A[0] = 3, 제수가 아닌 요소는 다음과 같습니다: 2, 6,

- A[1] = 1인 경우, 비제수는 다음과 같습니다: 3, 2, 3, 6,
- A[2] = 2, 비제수는 다음과 같습니다: 3, 3, 6,
- A[3] = 3, 비제수는 다음과 같습니다: 2, 6,
- A[4] = 6, 비소수가 없습니다.

함수를 작성합니다:

```javascript
funtion solution(A);
```

이 함수는 N개의 정수로 구성된 배열 A가 주어졌을 때 나눗셈이 아닌 정수의 양을 나타내는 정수의 시퀀스를 반환합니다.

결과 배열은 정수의 배열로 반환되어야 합니다.

예를 들어, 주어진

    A[0] = 3
    A[1] = 1
    A[2] = 2
    A[3] = 3
    A[4] = 6
함수는 위에서 설명한 대로 [2, 4, 3, 2, 0]을 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N은 [1..50,000] 범위 내의 정수입니다;
- 배열 A의 각 요소는 [1..2 * N] 범위 내의 정수입니다.

## 문제 접근

*빈 배열에 배열 A의 각 숫자가 나오는 횟수를 저장하고 배열 A 를 순회하면서 각 숫자의 약수를 구해 배열 A 전체 길이에서 빼준다.*

1. 배열의 A의 최대값까지 빈 배열을 만들어 0 으로 채운다.
2. 0 으로 채운 배열에 배열 A 나오는 숫자들의 횟수를 저장한다.
3. 배열 A 를 순회하면서 각 숫자의 약수를 구한다. 
4. A.length - 약수의 갯수 를 구한다. (배열의 총 길이에서 약수의 갯수를 뺴면 나머지 숫자들이 나누어지지 않는 숫자이다.)

```javascript
function solution(A) {
    const arr = new Array(Math.max(...A) + 1).fill(0);

    A.forEach((value) => {
        arr[value]++;
    });

    const result = [];

    for (let i = 0; i < A.length; i++) {
        const current = A[i];
        let count = 0;

        for (let j = 1; j <= Math.sqrt(current); j++) {
            if (current % j === 0) {
                count += arr[j];

                if (current / j !== j) {
                    count += arr[current / j];
                }
            }
        }

        result.push(A.length - count);
    }

    return result;
}
```
