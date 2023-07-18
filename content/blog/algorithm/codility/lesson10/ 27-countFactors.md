---
title: Codility Lesson 10 - CountFactors
date: 2023-07-17 14:00:42
category: Algorithm
draft: false
---

## 문제 설명

N = D * M이 되는 정수 M이 존재하는 경우 양의 정수 D는 양의 정수 N의 인수입니다.

예를 들어, M = 4는 위의 조건(24 = 6 * 4)을 만족하므로 6은 24의 인수입니다.

함수를 작성합니다:

```javascript
function solution(N);
```

양수 N이 주어지면 그 요소의 개수를 반환하는 함수를 작성합니다.

예를 들어 N = 24가 주어지면 24에는 1, 2, 3, 4, 6, 8, 12, 24 등 8개의 요인이 있으므로 함수는 8을 반환해야 합니다. 24의 다른 요인은 없습니다.

다음 가정에 대한 효율적인 알고리즘을 작성하십시오:

N은 [1..2,147,483,647] 범위 내의 정수입니다.

## 문제 접근

*숫자의 약수는 해당 숫자의 제곱근까지 순회하면서 나머지가 0인 숫자의 갯수 * 2를 하면된다.*

1. 숫자의 제곱근까지 순회하면서 나머지 0인 경우 갯수를 구한다.
2. 순회 완료 후에 갯수에 2 를 곱한다.
3. 그리고 제곱근이 완전히 정수 일수도 있으니 정수이면 갯수에 1을 더한다.

```javascript
function solution(N) {
    let count = 0;

    for (let i = 1; i < Math.sqrt(N); i++) {
        if (N % i === 0) {
            count++;
        }
    }

    count = count * 2;

    if (Number.isInteger(Math.sqrt(N))) {
        count++;
    }

    return count;
}
```
