---
title: Codility Lesson 5 - CountDiv
date: 2023-07-17 14:00:12
category: Algorithm
draft: false
---

## 문제 설명

함수를 작성합니다:

```javascript
function solution(A, B, K);
```

세 개의 정수 A, B, K가 주어졌을 때 [A..B] 범위 내에서 K로 나눌 수 있는 정수의 개수를 반환하는 함수입니다:

```
{ i : A ≤ i ≤ B, i mod K = 0 }.
```

예를 들어

```
A = 6, B = 11, K = 2인 경우
```

[6..11] 범위 내에 2로 나눌 수 있는 숫자는 6, 8, 10 등 세 개이므로 함수는 3을 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성하십시오:

- A와 B는 [0..2,000,000,000] 범위 내의 정수입니다;
- K는 [1...2,000,000,000] 범위 내의 정수입니다;
- A ≤ B.

## 문제 접근

_배열의 모든 값을 순회하지말고 A를 K로 나눈 몫과 B를 K로 나눈 몫 사이에 존재하는 갯수를 확인하면 된다._

1. A, B 를 K 로 나눈 몫 을 구한다. (소수점 버림)
2. A가 K로 정확이 나누어 떨어진다면 해당 갯수에 A 를 포함 그렇지 않다면 A 를 포함하지 않는다.

```javascript
function solution(A, B, K) {
  const start = Math.floor(A / K)
  const end = Math.floor(B / K)

  return A % K === 0 ? end - start + 1 : end - start
}
```
