---
title: Codility Lesson 7 - Brackets
date: 2023-07-17 14:00:20
category: Algorithm
draft: false
---

## 문제 설명

다음 조건 중 하나라도 해당하면 N개의 문자로 구성된 문자열 S가 올바르게 중첩된 것으로 간주됩니다:

- S가 비어 있습니다;
- S의 형식이 "(U)" 또는 "[U]" 또는 "{U}"인 경우, 여기서 U는 올바르게 중첩된 문자열입니다;
- S의 형식이 "VW"이고 V와 W가 올바르게 중첩된 문자열인 경우.

예를 들어 문자열 "{ [ ( ( ) ( ) ] }"는 올바르게 중첩되지만 "( [ ) ( ) ]"는 중첩되지 않습니다.

함수를 작성합니다:

```javascript
function solution(S);
```

이 함수는 N개의 문자로 구성된 문자열 S가 주어졌을 때 S가 올바르게 중첩된 경우 1을 반환하고 그렇지 않은 경우 0을 반환합니다.

예를 들어 S = "{ [ ( ( ) ( ) ] }"이면 위에서 설명한 대로 함수는 1을 반환해야 하고, S = "( [ ) ( ) ]"이면 함수는 0을 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N은 [0..200,000] 범위 내의 정수입니다;
- 문자열 S는 '(', '{', '[', ']', '}' 및/또는 ')'와 같은 문자로만 구성됩니다.

## 문제 접근

*스택 자료구조를 이용하여 열린 괄호이면 push 하고 닫힌괄호일때 마지막 아이템 괄호가 같은 종류에 열린괄호 이면 pop 한다.*

1. 괄호 정보를 객체로 선언
2. 빈 배열 선언
3. 문자열을 순회하면서 괄호들을 비교하여 stack에 push pop 과정을 거친다.
4. 배열을 모두 순회하고도 반환되지 않는다면 stack 요소가 남아있는지를 확인하여 알맞은 값을 반환한다.

```javascript
function solution(S) {
	const stack = [];
  const bracketInfo = {
      "(": ")",
      "[": "]",
      "{": "}",
  }

  for (let i = 0; i < S.length; i++) {
      const char = S[i];

      if (bracketInfo.hasOwnProperty(char)) {
          stack.push(char);
          continue;
      }

      const lastChar = stack[stack.length - 1];

      if (bracketInfo[lastChar] === char) {
          stack.pop();
      } else {
          return 0;
      }
  }

  const isEmpty = stack.length === 0;

  return isEmpty ? 1 : 0;
}
```
