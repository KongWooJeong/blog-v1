---
title: Codility Lesson 8 - Dominator
date: 2023-07-17 14:00:30
category: Algorithm
draft: false
---

## 문제 설명

N개의 정수로 구성된 배열 A가 주어집니다. 배열 A의 지배자는 A의 요소 중 절반 이상에서 발생하는 값입니다.

예를 들어 배열 A가 다음과 같다고 가정해 보겠습니다.

```
 a[0] = 3 a[1] = 4 a[2] = 3
 a[3] = 2 a[4] = 3 a[5] = -1
 a[6] = 3 a[7] = 3
```

A의 8개의 요소 중 5개(즉, 인덱스가 0, 2, 4, 6, 7인 요소)에서 발생하고 5가 8의 절반 이상이기 때문에 A의 도미네이터는 3입니다.

함수 작성

```javascript
function solution(A);
```

이 함수는 N개의 정수로 구성된 배열 A가 주어졌을 때, A의 지배자가 있는 배열 A의 모든 요소의 인덱스를 반환합니다. 배열 A에 지배자가 없는 경우 함수는 -1을 반환해야 합니다.

예를 들어 다음과 같은 배열 A가 주어지면

```
 a[0] = 3 a[1] = 4 a[2] = 3
 a[3] = 2 a[4] = 3 a[5] = -1
 a[6] = 3 a[7] = 3
```

함수는 위에서 설명한 대로 0, 2, 4, 6 또는 7을 반환할 수 있습니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N은 [0..100,000] 범위 내의 정수입니다;
- 배열 A의 각 요소는 [-2,147,483,648..2,147,483,647] 범위 내의 정수입니다.

## 문제 접근

*배열의 값의 노출 횟수를 저장하고 배열의 길이 절반 이상 노출되면 지배자이다.*

1. 배열의 값들에 노출 횟수 저장
2. 각 노출 횟수가 배열 길이의 절반이상인지 확인

```javascript
function solution(A) {
    const info = {};
    let result = -1;

    A.forEach((value, index) => {
        if (info.hasOwnProperty(value)) {
            info[value][0]++;
        } else {
            info[value] = [1, index];
        }
    });


    for (const [count, index] of Object.values(info)) {
        if (count > A.length / 2) {
            result = index;
        }
    }

    return result;
}
```
