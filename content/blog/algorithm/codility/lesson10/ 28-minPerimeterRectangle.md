---
title: Codility Lesson 10 - MinPerimeterRectangle
date: 2023-07-17 14:00:44
category: Algorithm
draft: false
---

## 문제 설명

어떤 직사각형의 면적을 나타내는 정수 N이 주어집니다.

변의 길이가 A와 B인 직사각형의 넓이는 A * B이고 둘레는 2 * (A + B)입니다.

이 사각형의 변은 정수여야 하며, 면적이 N과 같은 직사각형의 최소 둘레를 구하는 것이 목표입니다.

예를 들어 정수 N = 30이 주어지면 면적 30의 직사각형이 있습니다:

- (1, 30), 둘레 62,

- (2, 15), 둘레가 34,
- (3, 10), 둘레가 26,
- (5, 6), 둘레가 22입니다.

함수를 작성합니다:

```javascript
function solution(N);
```

이 함수는 정수 N이 주어지면 면적이 N과 정확히 같은 직사각형의 최소 둘레를 반환합니다.

예를 들어 정수 N = 30이 주어지면 위에서 설명한 대로 함수는 22를 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성하십시오:

- N은 [1..1,000,000,000] 범위 내의 정수입니다.

## 문제 접근

*둘레는 면적에 약수이다, 즉 면적의 약수 두개의 더한값이 가장 작은 값을 구하면 된다.*

1. 면적의 제곱근까지 순회한다.
2. 순회하면서 각 약수 두개가 발견되면 두개를 더한다.
3. 더한 값이 현재 최솟값이랑 비교하면서 작은값을 저장해 나간다.

```javascript
function solution(N) {
    let result = Number.MAX_SAFE_INTEGER;

    for (let i = 1; i <= Math.sqrt(N); i++) {
        if (N % i === 0) {
            const b = N / i;

            result = Math.min(result, 2 * (i + b));
        }
    }

    return result;
}
```
