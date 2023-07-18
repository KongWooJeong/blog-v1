---
title: Codility Lesson 14 - MinMaxDivision
date: 2023-07-18 14:00:54
category: Algorithm
draft: false
---

## 문제 설명

정수 K, M과 N개의 정수로 구성된 비어 있지 않은 배열 A가 주어집니다. 배열의 모든 요소는 M보다 크지 않습니다.

이 배열을 연속된 요소로 구성된 K 블록으로 나누어야 합니다. 블록의 크기는 0에서 N 사이의 정수이며, 배열의 모든 요소는 어떤 블록에 속해야 합니다.

X에서 Y까지의 블록의 합은 A[X] + A[X + 1] + ... + A[Y]와 같습니다. + A[Y]입니다. 빈 블록의 합은 0입니다.

큰 합은 모든 블록의 최대 합입니다.

예를 들어 정수 K = 3, M = 5와 배열 A가 주어집니다:

```
  A[0] = 2
  A[1] = 1
  A[2] = 5
  A[3] = 1
  A[4] = 2
  A[5] = 2
  A[6] = 2
```

배열은 예를 들어 다음과 같은 블록으로 나눌 수 있습니다:

- [2, 1, 5, 1, 2, 2, 2], [], [], 15의 큰 합으로;
- 2], [1, 5, 1, 2], [2, 2], [2, 2]의 큰 합계가 9입니다;
- [2, 1, 5], [], [1, 2, 2, 2], 큰 합계가 8인 경우;
- 2, 1], [5, 1], [2, 2, 2], [2, 2, 2]의 큰 합이 6입니다.

목표는 큰 합을 최소화하는 것입니다. 위의 예에서 6은 최소 큰 합계입니다.

함수를 작성합니다:

```javascript
function solution(K, M, A);
```

정수 K, M과 비어 있지 않은 N개의 정수로 구성된 배열 A가 주어졌을 때 최소의 큰 합을 반환하는 함수입니다.

예를 들어 K = 3, M = 5, 배열 A가 주어지면 다음과 같습니다:

```
	A[0] = 2
  A[1] = 1
  A[2] = 5
  A[3] = 1
  A[4] = 2
  A[5] = 2
  A[6] = 2
```

이면 위에서 설명한 대로 함수는 6을 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N과 K는 [1..100,000] 범위 내의 정수입니다;
- M은 [0..10,000] 범위 내의 정수입니다;
- 배열 A의 각 요소는 [0..M] 범위 내의 정수입니다.

## 문제 접근

*중간값을 이용하여 이분 탐색을 진행한다.*

*중간값을 기준으로 최종값에 범위를 줄여나가면서 비교한다.*

```javascript
function solution(K, M, A) {
    let minValue = Math.max(...A);
    let maxValue = A.reduce((acc, value) => acc + value);

    if (K === 1) {
        return minValue;
    }

    if (K >= A.length) {
        return maxValue;
    }

    while (minValue <= maxValue) {
        let mid = Math.floor((maxValue + minValue) / 2);

        let sum = 0;
        let count = 0;

				// 블록수가 K 보다 작으면 값이 너무 크다는 의미
				// 블록수가 K 보다 크면 기준 값이 너무 작다는 의미
        let isValid = true;

        for (let i = 0; i < A.length; i++) {
            if (sum + A[i] > mid) {
                sum = A[i];
                count++;
            } else {
                sum += A[i];
            }

            if (count >= K) {
                isValid = false;
                break;
            }
        }

        if (isValid) {
            maxValue = mid - 1;
        } else {
            minValue = mid + 1;
        }
    }
    
    return minValue;
}
```
