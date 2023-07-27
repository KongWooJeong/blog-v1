---
title: Programmers - 스킬 트리
date: 2023-07-27 13:20:10
category: Algorithm
draft: false
---

## 문제 설명

선행 스킬이란 어떤 스킬을 배우기 전에 먼저 배워야 하는 스킬을 뜻합니다.

예를 들어 선행 스킬 순서가 `스파크 → 라이트닝 볼트 → 썬더`일때, 썬더를 배우려면 먼저 라이트닝 볼트를 배워야 하고, 라이트닝 볼트를 배우려면 먼저 스파크를 배워야 합니다.

위 순서에 없는 다른 스킬(힐링 등)은 순서에 상관없이 배울 수 있습니다. 따라서 `스파크 → 힐링 → 라이트닝 볼트 → 썬더`와 같은 스킬트리는 가능하지만, `썬더 → 스파크`나 `라이트닝 볼트 → 스파크 → 힐링 → 썬더`와 같은 스킬트리는 불가능합니다.

선행 스킬 순서 skill과 유저들이 만든 스킬트리[1](https://programmers.co.kr/skill_checks/502888?challenge_id=2508#fn1)를 담은 배열 skill_trees가 매개변수로 주어질 때, 가능한 스킬트리 개수를 return 하는 solution 함수를 작성해주세요.

##### 제한 조건

- 스킬은 알파벳 대문자로 표기하며, 모든 문자열은 알파벳 대문자로만 이루어져 있습니다.
- 스킬 순서와 스킬트리는 문자열로 표기합니다.
  - 예를 들어, `C → B → D` 라면 "CBD"로 표기합니다
- 선행 스킬 순서 skill의 길이는 1 이상 26 이하이며, 스킬은 중복해 주어지지 않습니다.
- skill_trees는 길이 1 이상 20 이하인 배열입니다.
- skill_trees의 원소는 스킬을 나타내는 문자열입니다.
  - skill_trees의 원소는 길이가 2 이상 26 이하인 문자열이며, 스킬이 중복해 주어지지 않습니다.

##### 입출력 예

| skill   | skill_trees                         | return |
| ------- | ----------------------------------- | ------ |
| `"CBD"` | `["BACDE", "CBADF", "AECB", "BDA"]` | 2      |

##### 입출력 예 설명

- "BACDE": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트립니다.
- "CBADF": 가능한 스킬트리입니다.
- "AECB": 가능한 스킬트리입니다.
- "BDA": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트리입니다.

## 문제 접근

*skill_trees의 각 스킬에서 필요한 스킬만 남기고 필요없는 스킬을 제외시킨 배열을 만들고, 각 스킬과 스킬트리에 순서를 하나씩 확인하고 모두 스킬트리 진행했다면 count 값을 증가시킵니다.*

1. 스킬트리에 각 스킬들에서 필요없는 스킬을 제외하고 필요한 스킬만 뽑아서 새로운 배열을 생성합니다.
2. 해당 필요한 스킬이 스킬트리와 순서와 맞는지 비교하여 맞으면 count 값을 증가 시킵니다.

```javascript

function solution(skill, skill_trees) {
    let count = 0;

    const skillSet = new Set(skill);

    const skillTrees = skill_trees.map((skill) => {
        let str = "";

        for (let i = 0; i < skill.length; i++) {
            if (skillSet.has(skill[i])) {
                str += skill[i];
            }
        }

        return str;
    });

    skillTrees.forEach((value) => {
       for (let i = 0; i < value.length; i++) {
           if (value[i] !== skill[i]) {
               return;
           } 
       } 

       count++;
    });

    return count;
}

```
