---
title: Codility Lesson 7 - Nesting
date: 2023-07-17 14:00:22
category: Algorithm
draft: false
---

## 문제 설명

N개의 문자로 구성된 문자열 S가 다음과 같은 경우 올바르게 중첩된 문자열이라고 합니다:

S가 비어 있는 경우;

- S의 형식이 "(U)"인 경우, 여기서 U는 올바르게 중첩된 문자열입니다;
- S의 형식이 "VW"이고 V와 W가 올바르게 중첩된 문자열인 경우.

예를 들어 문자열 "( ( ( ) ( ( ) ) ( ) )"는 올바르게 중첩되지만 문자열 "( ) )"는 중첩되지 않습니다.

함수를 작성합니다:

```javascript
function solution(S);
```

이 함수는 N개의 문자로 구성된 문자열 S가 주어졌을 때 문자열 S가 올바르게 중첩되면 1을 반환하고 그렇지 않으면 0을 반환합니다.

예를 들어 S = "( ( ( ) ( ( ) ) ( ( ) )"이 주어지면 위에서 설명한 대로 함수는 1을 반환해야 하고, S = "( ) )"이 주어지면 함수는 0을 반환해야 합니다.

다음 가정에 대한 효율적인 알고리즘을 작성합니다:

- N은 [0..1,000,000] 범위 내의 정수입니다;
- 문자열 S는 '(' 및/또는 ')' 문자로만 구성됩니다.

## 문제 접근

*스택구조를 이용하여 접근*

1. 스택 생성
2. "(" 일때는 push
3. ")" 일때는 스택에 마지막 값이 "(" 이면 pop 
4. 마지막에는 스택이 비었을경우, 안비었을경우에 따라 알맞은 값 반환

```javascript
function solution(S) {
    const stack = [];

    for (let i = 0; i < S.length; i++) {
        const char = S[i];

        if (char === "(") {
            stack.push(char);
            continue;
        }

        const lastChar = stack[stack.length - 1];
        
        if (char === ")" && lastChar === "(") {
            stack.pop();
        } else {
            return 0;
        }
    }

    const isEmpty = stack.length === 0;

    return isEmpty ? 1 : 0;
}
```
