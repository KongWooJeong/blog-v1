---
title: Codility Lesson 3 - FrogJmp
date: 2023-07-16
category: Algorithm
draft: false
---

## 문제 설명

작은 개구리가 길 반대편으로 가고 싶어 합니다. 개구리는 현재 X 위치에 있으며 Y보다 크거나 같은 위치로 이동하려고 합니다. 작은 개구리는 항상 고정된 거리인 D를 점프합니다.

작은 개구리가 목표에 도달하기 위해 점프해야 하는 최소한의 횟수를 세십시오.

함수를 작성합니다:

```javascript
function solution(X, Y, D);
```

세 개의 정수 X, Y, D가 주어졌을 때 X 위치에서 Y보다 큰 위치로 이동하는 최소 점프 횟수를 반환하는 함수를 작성합니다.

예를 들어, 주어진

```
X = 10
Y = 85
D = 30
```

이면 개구리의 위치는 다음과 같으므로 함수는 3을 반환해야 합니다:

- 첫 번째 점프 후, 위치 10 + 30 = 40에서
- 두 번째 점프 후, 위치 10 + 30 + 30 + 30 = 70
- 세 번째 점프 후, 위치 10 + 30 + 30 + 30 = 100에 위치합니다.

다음 가정에 대한 효율적인 알고리즘을 작성하십시오

- X, Y, D는 [1..1,000,000,000] 범위 내의 정수입니다;
- X ≤ Y.

## 문제 접근

_각 배열을 순회하면서 값이 몇번 나오는지 횟수 정보를 저장하고, 해당 횟수가 홀수인것을 반환한다._

1. 배열을 순회 하면서 각 값이 나오는 횟수정보를 저장한다.
2. 해당 배열에서 반복되는 숫자를 제거하기 위해 Set 자료구조를 이용한다.
3. 반복되는 숫자를 제거하는 배열을 순회하면서 각 숫자의 횟수가 홀수 인것을 찾아서 반환한다.

```javascript
function solution(A) {
  const info = {}

  A.forEach(value => {
    if (info[value] === undefined) {
      info[value] = 1
    } else {
      info[value]++
    }
  })

  const arr = Array.from(new Set(A))

  for (let i = 0; i < arr.length; i++) {
    const key = arr[i]

    if (info[key] % 2 === 1) {
      return Number(key)
    }
  }
}
```
