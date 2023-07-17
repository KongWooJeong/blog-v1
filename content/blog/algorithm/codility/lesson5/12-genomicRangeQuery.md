---
title: Codility Lesson 5 - GenomicRangeQuery
date: 2023-07-17 14:00:13
category: Algorithm
draft: false
---

## 문제 설명

DNA 염기서열은 염기서열의 연속되는 뉴클레오타이드의 유형에 해당하는 문자 A, C, G, T로 구성된 문자열로 나타낼 수 있습니다. 각 뉴클레오타이드에는 정수인 임팩트 팩터가 있습니다. A, C, G, T 유형의 뉴클레오타이드는 각각 1, 2, 3, 4의 영향 계수를 가집니다. 이 형식의 몇 가지 질문에 답하게 될 것입니다: 주어진 DNA 서열의 특정 부분에 포함된 뉴클레오타이드의 최소 영향 계수는 얼마인가요?

DNA 서열은 N개의 문자로 구성된 비어 있지 않은 문자열 S = S[0]S[1]...S[N-1]로 주어집니다. 비어 있지 않은 배열 P와 Q에 각각 M개의 정수로 구성된 M개의 쿼리가 주어집니다. K 번째 쿼리(0 ≤ K < M)는 위치 P[K]와 Q[K](포함) 사이의 DNA 서열에 포함된 뉴클레오타이드의 최소 영향 계수를 구해야 합니다.

예를 들어 문자열 S = CAGCCTA와 배열 P, Q가 있다고 가정해 보겠습니다:

```
p[0] = 2 q[0] = 4
p[1] = 5 q[1] = 5
p[2] = 0 q[2] = 6
```

이러한 M = 3 쿼리에 대한 답은 다음과 같습니다:

2번과 4번 위치 사이의 DNA 부분에는 영향 계수가 각각 3과 2인 뉴클레오티드 G와 C(두 개)가 포함되어 있으므로 답은 2입니다.
5번과 5번 사이의 부분에는 영향 계수가 4인 단일 뉴클레오티드 T가 포함되어 있으므로 답은 4입니다.
위치 0과 6 사이의 부분(전체 문자열)에는 모든 뉴클레오타이드, 특히 영향 계수가 1인 뉴클레오타이드 A가 포함되어 있으므로 답은 1입니다.

함수를 작성합니다:

```javascript
function solution(S, P, Q);
```

이 함수는 N개의 문자로 구성된 비어 있지 않은 문자열 S와 M개의 정수로 구성된 두 개의 비어 있지 않은 배열 P와 Q가 주어질 때 모든 쿼리에 대한 연속적인 답을 지정하는 M개의 정수로 구성된 배열을 반환합니다.

결과 배열은 정수의 배열로 반환되어야 합니다.

예를 들어 문자열 S = CAGCCTA와 배열 P, Q가 주어지면 다음과 같이 됩니다:

```
p[0] = 2 q[0] = 4
p[1] = 5 q[1] = 5
p[2] = 0 q[2] = 6
```

인 경우 함수는 위에서 설명한 대로 [2, 4, 1] 값을 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N은 [1..100,000] 범위 내의 정수입니다;
- M은 [1...50,000] 범위 내의 정수입니다;
- 배열 P와 Q의 각 요소는 [0..N - 1] 범위 내의 정수입니다;
- P[K] ≤ Q[K], 여기서 0 ≤ K < M;
- 문자열 S는 대문자 영어 문자 A, C, G, T로만 구성됩니다.

## 문제 접근

_DNA 문자열인 S를 특정 범위로 문자열로 잘라서 각 A, C, G, T DNA 를 찾아 result 배열에 하나씩 담는다._

1. P, Q 배열 길이만큼 순회한다.
2. 각 index 값으로 S 문자열을 자른다.
3. 자른 문자열에서 A, C, G, T 를 찾아 각 알맞은 숫자를 담는ㄷ.

```javascript
function solution(S, P, Q) {
  let temp = ''
  const result = []

  for (var i = 0; i < P.length; i++) {
    temp = S.slice(P[i], Q[i] + 1)

    if (dna.indexOf('A') !== -1) {
      result.push(1)
    } else if (dna.indexOf('C') !== -1) {
      result.push(2)
    } else if (dna.indexOf('G') !== -1) {
      result.push(3)
    } else {
      result.push(4)
    }
  }

  return result
}
```

_또 다른 방법은 있다, 그것은 미리 모든 각 S 문자열에서 A, C, G, T 가 몇개씩 나오는지 미리 합을 계산하는 방법이다._

```javascript
function solution(S, P, Q) {
  const len = S.length
  const prefixSums = {
    A: Array(len + 1).fill(0),
    C: Array(len + 1).fill(0),
    G: Array(len + 1).fill(0),
  }

  for (let i = 0; i < len; i++) {
    prefixSums.A[i + 1] = prefixSums.A[i] + (S[i] === 'A' ? 1 : 0)
    prefixSums.C[i + 1] = prefixSums.C[i] + (S[i] === 'C' ? 1 : 0)
    prefixSums.G[i + 1] = prefixSums.G[i] + (S[i] === 'G' ? 1 : 0)
  }

  const result = []

  for (let j = 0; j < P.length; j++) {
    const from = P[j]
    const to = Q[j] + 1
    if (prefixSums.A[to] - prefixSums.A[from] > 0) result.push(1)
    else if (prefixSums.C[to] - prefixSums.C[from] > 0) result.push(2)
    else if (prefixSums.G[to] - prefixSums.G[from] > 0) result.push(3)
    else result.push(4)
  }

  return result
}
```
