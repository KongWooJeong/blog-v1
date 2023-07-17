---
title: Codility Lesson 8 - EquiLeader
date: 2023-07-17 14:00:32
category: Algorithm
draft: false
---

## 문제 설명

N개의 정수로 구성된 비어 있지 않은 배열 A가 주어집니다.

이 배열의 리더는 A의 요소 중 절반 이상에서 발생하는 값입니다.

이퀘이 리더는 0 ≤ S < N - 1이고 두 시퀀스 A[0], A[1], ..., A[S]와 A[S + 1], A[S + 2], ..., A[N - 1]이 같은 값의 리더를 갖는 인덱스 S입니다.

예를 들어 다음과 같은 배열 A가 주어집니다:

    A[0] = 4
    A[1] = 3
    A[2] = 4
    A[3] = 4
    A[4] = 4
    A[5] = 2
두 개의 이퀴 리더를 찾을 수 있습니다:

- 0, 시퀀스이기 때문입니다: (4) 및 (3, 4, 4, 4, 4, 2)는 값이 4인 동일한 리더를 갖습니다.
- 2, 왜냐하면 시퀀스: (4, 3, 4) 및 (4, 4, 2)는 값이 4인 동일한 리더를 갖기 때문입니다.

목표는 등호 리더의 수를 세는 것입니다.

함수를 작성합니다:

```javascript
function solution(A);
```

이 함수는 비어 있지 않은 정수 배열 A가 주어졌을 때, N개의 정수로 구성된 등차수열의 개수를 반환합니다.

예를 들어, 주어진

    A[0] = 4
    A[1] = 3
    A[2] = 4
    A[3] = 4
    A[4] = 4
    A[5] = 2
이면 위에서 설명한 대로 함수는 2를 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N은 [1..100,000] 범위 내의 정수입니다;
- 배열 A의 각 요소는 [-1,000,000,000..1,000,000,000] 범위 내의 정수입니다.

## 문제 접근

*배열을 두부분으로 나누어 생각한다, 그리고 동적계획법을 이용한다.*

예를들어

```
left = []
right = [4, 3 ... 2]
```

**처음에 두 변수에 하나에는 빈 배열 또 다른 하나에는 배열의 모든 요소**

1. 배열을 순회하면서
2. left 에는 요소를 추가하여 리더 요소 확인
3. right 에는 요소를 삭제하여 리더 요소 확인
4. 두개 리더 요소가 같을때를 찾는다.

```javascript
function solution(A) {
   let result = 0;

   const right = {};
   const left = {};
   let leftLength = 0;
   let rightLength = A.length;
   let leftLeader = 0;
   let leftLeaderCount = 0;

   A.forEach((value) => {
       if (right.hasOwnProperty(value)) {
           right[value]++;
       } else {
           right[value] = 1;
       }
   });

   A.forEach((value) => {
       if (left.hasOwnProperty(value)) {
           left[value]++;
       } else {
           left[value] = 1;
       }

       right[value]--;

       leftLength++;
       rightLength--;

       if (left[value] > leftLeaderCount) {
           leftLeader = value;
           leftLeaderCount = left[value];
       }

       if (
           right[leftLeader] > (rightLength / 2) 
           && leftLeaderCount > (leftLength / 2)
       ) {
           result++;
       }
   });

   return result;
}
```
