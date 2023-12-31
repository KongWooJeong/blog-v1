---
title: Codility Lesson 5 - PassingCars
date: 2023-07-17 14:00:11
category: Algorithm
draft: false
---

## 문제 설명

N개의 정수로 구성된 비어있지 않은 배열 A가 주어집니다. 배열 A의 연속된 요소는 도로 위의 연속된 자동차를 나타냅니다.

배열 A에는 0 및/또는 1만 포함됩니다:

- 0은 동쪽으로 이동하는 자동차를 나타냅니다,
- 1은 서쪽으로 이동하는 자동차를 나타냅니다.

목표는 지나가는 자동차를 세는 것입니다. 0 ≤ P < Q < N인 한 쌍의 자동차(P, Q)는 P가 동쪽으로 이동하고 Q가 서쪽으로 이동하는 경우 지나가고 있다고 말합니다.

예를 들어 다음과 같은 배열 A를 생각해 봅시다:

```
A[0] = 0
A[1] = 1
A[2] = 0
A[3] = 1
A[4] = 1
```

(0, 1), (0, 3), (0, 4), (2, 3), (2, 4)의 다섯 쌍의 지나가는 차가 있습니다.

함수를 작성합니다:

```javascript
function solution(A);
```

비어 있지 않은 정수 배열 A가 주어지면 지나가는 자동차 쌍의 수를 반환하는 함수입니다.

이 함수는 통과하는 자동차 쌍의 수가 1,000,000,000을 초과하면 -1을 반환해야 합니다.

예를 들어, 주어진

```
A[0] = 0
A[1] = 1
A[2] = 0
A[3] = 1
A[4] = 1
```

이면 함수는 위에서 설명한 대로 5를 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N은 [1..100,000] 범위 내의 정수입니다;
- 배열 A의 각 요소는 다음 값 중 하나를 가질 수 있는 정수입니다: 0, 1.

## 문제 접근

_이중 반복문으로 모든 경우를 확인하지 말고 동쪽으로 가는 차량 횟수를 변수에 저장하고 서쪽으로 가는 차량을 만난다면 동쪽으로 가는 차량 횟수만큼 더해주면 된다._

1. 반복문을 순회하면서 동쪽으로 가는 차량 횟수 저장

2. 서쪽으로 가는 차량을 만날시 동쪽으로 가는 차량만큼 더해준다.

   ```javascript
   if (A[i] === WEST) {
     passingCar += eastCar
   }
   ```

```javascript
function solution(A) {
  let passingCar = 0
  let eastCar = 0
  const EAST = 0
  const WEST = 1

  for (let i = 0; i < A.length; i++) {
    if (A[i] === EAST) {
      eastCar++
    } else if (A[i] === WEST) {
      passingCar += eastCar
    }

    if (passingCar > 1000000000) {
      return -1
    }
  }

  return passingCar
}
```
