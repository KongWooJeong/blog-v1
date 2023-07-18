---
title: Codility Lesson 11 - CountSemiprimes
date: 2023-07-18 14:00:50
category: Algorithm
draft: false
---

## 문제 설명

소수는 정확히 두 개의 나눗셈이 있는 양의 정수 X입니다: 1과 X. 처음 몇 개의 소수는 2, 3, 5, 7, 11, 13입니다.

준소수는 두 개의 (반드시 구별되는 것은 아닌) 소수의 곱인 자연수입니다. 처음 몇 개의 준소수는 4, 6, 9, 10, 14, 15, 21, 22, 25, 26입니다.

비어 있지 않은 두 개의 배열 P와 Q가 주어지며, 각 배열은 M개의 정수로 구성됩니다. 이 배열은 지정된 범위 내의 소수 개수에 대한 쿼리를 나타냅니다.

쿼리 K는 1 ≤ P[K] ≤ Q[K] ≤ N 범위(P[K], Q[K]) 내의 소수 개수를 구해야 합니다.

예를 들어 정수 N = 26과 배열 P, Q를 고려합니다:

    p[0] = 1 q[0] = 26
    p[1] = 4 q[1] = 10
    p[2] = 16 q[2] = 20
이러한 각 범위 내의 반소수의 수는 다음과 같습니다:

- (1, 26)은 10입니다,

- (4, 10)은 4입니다,
- (16, 20)은 0입니다.

함수를 작성합니다:

```javascript
function solution(N, P, Q);
```

이 함수는 정수 N과 비어 있지 않은 두 개의 비어 있지 않은 배열 P와 Q가 주어지면 모든 쿼리에 대한 연속적인 답을 지정하는 M개의 요소로 구성된 배열을 반환합니다.

예를 들어 정수 N = 26과 배열 P, Q가 주어지면 다음과 같습니다:

    p[0] = 1 q[0] = 26
    p[1] = 4 q[1] = 10
    p[2] = 16 q[2] = 20
인 경우 함수는 위에서 설명한 대로 [10, 4, 0] 값을 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N은 [1..50,000] 범위 내의 정수입니다;
- M은 [1..30,000] 범위 내의 정수입니다;
- 배열 P와 Q의 각 요소는 [1..N] 범위 내의 정수입니다;
- P[i] ≤ Q[i].

## 문제 접근

*에라토스테네스의 체 의 방법으로 소수를 구한다.*

*소수, 준소수를 구하여 각 숫자의 준소수의 갯수를 동적계획법으로 하나씩 더한다. 마지막엔 범위 마지막 요소시점에서 총 준소수 개수 - 범위 시작 요소시점에서 총 준소수 개수 가 답이다.*

1. 소수, 준소수 빈배열을 만든다 (배열의 길이는 N + 1; 숫자는 1 부터 시작이기 때문에)
2. 에라토스테네스의 체의 방법으로 소수를 구한다. 
3. 구한 소수를 가지고 준소수를 구한다.
4. 해당 준소수 배열에서 각 숫자까지의 준소수의 총 갯수 동적계획법을 이용하여 구한다. 
5. 마지막엔 범위 마지막 요소시점에 준소수 총 개수 - 시작 요소시점에 준소수 총 개수 를 구한다.

```javascript
function solution(N, P, Q) {
    const primeArr = new Array(N + 1).fill(1);
    const semiPrimeArr = new Array(N + 1).fill(0);

    primeArr[0] = 0;
    primeArr[1] = 0;

    for (let i = 2; i <= Math.sqrt(N); i++) {
        if (primeArr[i]) {
            for (let j = i * i; j <= N; j += i) {
                primeArr[j] = 0;
            }
        }
    }

    const primeList = [];

    primeArr.forEach((value, index) => {
        if (value === 1) {
            primeList.push(index);
        }
    });

    for (let i = 0; i < primeList.length; i++) {
        for (let j = 0; j < primeList.length; j++) {
            const value = primeList[i] * primeList[j];

            if (value <= N) {
                semiPrimeArr[value] = 1;
            }
        }
    }

    for (let i = 2; i < semiPrimeArr.length; i++) {
        semiPrimeArr[i] += semiPrimeArr[i - 1];
    }

    const result = P.map((value, index) => {
        return semiPrimeArr[Q[index]] - semiPrimeArr[value - 1];
    });

    return result;
}
```
