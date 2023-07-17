---
title: Codility Lesson 7 - StoneWall
date: 2023-07-17 14:00:23
category: Algorithm
draft: false
---

## 문제 설명

돌담을 만들려고 합니다. 벽은 직선이고 길이가 N미터여야 하며 두께는 일정해야 하지만 장소에 따라 높이가 달라야 합니다. 벽의 높이는 N개의 양의 정수로 구성된 배열 H로 지정됩니다. H[I]는 I에서 왼쪽 끝의 오른쪽으로 I+1미터까지 벽의 높이입니다. 특히 H[0]은 벽의 왼쪽 끝의 높이이고 H[N-1]은 벽의 오른쪽 끝의 높이입니다.

벽은 직육면체 돌 블록으로 만들어져야 합니다(즉, 이러한 블록의 모든 면이 직사각형입니다). 여러분의 임무는 벽을 만드는 데 필요한 최소 블록 수를 계산하는 것입니다.

함수를 작성합니다:

```javascript
function solution(H);
```

벽의 높이를 지정하는 N개의 양의 정수 배열 H가 주어지면 벽을 만드는 데 필요한 최소 블록 수를 반환하는 함수입니다.

예를 들어, N = 9개의 정수를 포함하는 배열 H가 주어집니다:

```
  H[0] = 8 H[1] = 8 H[2] = 5
  H[3] = 7 H[4] = 9 H[5] = 8
  H[6] = 7 H[7] = 4 H[8] = 8
```

함수는 7을 반환해야 합니다. 그림은 7개의 블록의 가능한 배열 중 하나를 보여줍니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N은 [1..100,000] 범위 내의 정수이다;
- 배열 H의 각 요소는 [1..1,000,000,000] 범위 내의 정수입니다.

## 문제 접근

*현재 보고 있는 벽 높이가 스택의 맨 위에 있는 벽 높이보다 높으면 새로운 벽을 쌓아야 한다는 점*

*현재 보고 있는 벽 높이가 스택의 맨 위에 있는 벽 높이보다 낮을 때까지 스택에서 벽을 제거*

```javascript
function solution(H) {
    const stack = [];
    let count = 0;

    for (let i = 0; i < H.length; i++) {
        const currentBlock = H[i];

        while (stack.length > 0 && stack[stack.length -1] > currentBlock) {
            stack.pop();
        }

        if (stack.length === 0 || stack[stack.length -1] < currentBlock) {
            stack.push(H[i]);
            count++;
        }
    }

    return count;
}
```
