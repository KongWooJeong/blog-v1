---
title: Codility Lesson 6 - NumberOfDiscIntersections
date: 2023-07-17 14:00:18
category: Algorithm
draft: false
---

## 문제 설명

평면에 N개의 원판을 그립니다. 원판의 번호는 0부터 N-1까지입니다. 원반의 반지름을 지정하는 음수가 아닌 정수 N개의 배열 A가 주어집니다. J번째 원반은 중심이 (J, 0)이고 반지름이 A[J]인 원반을 그립니다.

J ≠ K이고 J 번째 원판과 K 번째 원판에 공통점이 하나 이상 있는 경우 J 번째 원판과 K 번째 원판이 교차한다고 합니다(원판에 테두리가 있다고 가정).

아래 그림은 N = 6과 A에 대해 다음과 같이 그려진 원판을 보여줍니다:

```
A[0] = 1
A[1] = 5
A[2] = 2
A[3] = 1
A[4] = 4
A[5] = 0
```

교차하는 원반 쌍은 11개(정렬되지 않은) 쌍이 있습니다:

- 원반 1과 4는 교차하고, 두 원반은 다른 모든 원반과 교차합니다;
- 원반 2는 원반 0 및 3과도 교차합니다.

함수를 작성합니다:

```javascript
function solution(A);
```

위에서 설명한 대로 N개의 원반을 나타내는 배열 A가 주어지면 교차하는 원반의 (정렬되지 않은) 쌍의 수를 반환합니다. 교차하는 쌍의 수가 10,000,000개를 초과하면 함수는 -1을 반환해야 합니다.

위에 표시된 배열 A가 주어지면 위에서 설명한 대로 함수는 11을 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성하십시오:

- N은 [0..100,000] 범위 내의 정수입니다;
- 배열 A의 각 요소는 [0..2,147,483,647] 범위 내의 정수입니다.

## 문제 접근

_완전 탐색으로 풀어도되지만 성능측면에서 통과하지 못한다._

**그래서 아래와 같은 조건을 제시한다.**

- 원판이 시작되는 지점기준으로 오름차순으로 정렬한다.
- 기준이 되는 원판에 끝 지점이 타켓이 되는 원판에 시작 지점보다 작으면 교차하지 않는다.
  - 오름차순으로 정렬했기때문에 그 이후에 모든 원판은 교차하지 않는다는것을 의미한다.

---

1. 원판에 시작, 끝 지점을 배열을 순회하면 정보를 저장한다.
2. 해당 정보를 시작지점 기준으로 오름차순으로 정렬한다.
3. 반복문을 통해 타겟이 되는 원판 시작지점과 기준이 되는 원판 끝 지점을 비교하여
   - 교차 한다면 갯수를 늘려나간다.

```javascript
function solution(A) {
  let count = 0
  const arr = []

  A.forEach((value, index) => {
    const left = index - value
    const right = value + index

    arr.push([left, right])
  })

  arr.sort((a, b) => a[0] - b[0])

  for (let i = 0; i < A.length - 1; i++) {
    const [left, right] = arr[i]

    for (let j = i + 1; j < A.length; j++) {
      const [targetLeft, targetRight] = arr[j]

      if (targetLeft > right) {
        break
      }

      if (targetRight >= left) {
        count++
      }

      if (count > 10000000) {
        return -1
      }
    }
  }

  return count
}
```
