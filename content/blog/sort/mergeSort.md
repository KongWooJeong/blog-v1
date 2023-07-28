---
title: 병합 정렬
date: 2023-07-28 18:00:01
category: 정렬
draft: false
---

### 병합 정렬이란

병합 정렬은 더 자은 하위 배열로 나누고 각 하위 배열을 정렬한 다음 정렬된 하위 배열을 다시 병합하여 최종 정렬된 배열을 형성하는 정렬 알고리즘입니다. 즉, 분할 정복 방법을 통하여 구현됩니다.

병합 정렬하는 과정을 간단히 정리하면 다음과 같습니다.

- 분할: 입력 배열을 같은 크기의 2개의 부분 배열로 분할합니다.
- 정복: 부분 배열을 정렬한다. 부분 배열의 크기가 충분히 작지 않으면 다시 분할하여 같은 과정을 진행합니다.
- 결합: 정렬된 부분 배열들을 하나의 배열에 병합합니다.

병합 정렬은 배열을 더 이상 분할할 수 없을 때까지, 즉 배열에 요소가 하나만 남을 때까지 배열을 계속 반으로 분할하는 재귀 알고리즘입니다. (요소가 하나인 배열은 항상 정렬됨). 그런 다음 정렬된 하위 배열이 하나의 정렬된 배열로 병합됩니다.

<br/>

### 병합 정렬 동작 과정

배열 `[38, 27, 43, 10]`을 예를 들어 살펴 보겠습니다.

- 배열을 두 개의 동일한 절반으로 나눕니다.

  ```
  [38, 27] [43, 10]
  ```

- 더 이상 나눌 수 없는 단위까지 나누고, 해당 나눌 수 없는 단위(즉, 요소가 1개일때)는 항상 정렬된 상태입니다.

  ```
  [38] [27] [43] [10]
  ```

- 하위 배열을 정렬하고 병합합니다.

  ```
  [27, 38] [10, 43]
  ```

- 해당 병합 과정을 정렬된 배열이 만들어질 때까지 반복합니다.

  ```
  [10, 27, 38, 43]
  ```

<br/>

### 병합 정렬 구현 코드

```javascript



function merge(arr, left, mid, right) {
  const leftArrLength = mid - left + 1; // 완쪽 배열의 길이
  const rightArrLength = right - mid; // 오른쪽 배열의 길이
  
  // 왼쪽 배열의 길이 만큼 임시 배열 생성
  const leftArr = new Array(leftArrLength); 
  
  // 오른쪽 배열의 길이 만큼 임시 배열 생성
  const rightArr = new Array(rightArrLength);
 
  // 왼쪽 배열에 알맞은 요소를 추가
  for (let i = 0; i < leftArrLength; i++) {
    // left + i : 기준을 left 값(왼쪽 배열의 첫번째 index)을 가지고 알맞은 요소를 하나씩 추가한다.
    leftArr[i] = arr[left + i];
  }
  
  // 오른쪽 배열에 알맞은 요소를 추가
  for (let i = 0; i < rightArrLength; i++) {
    // mid + 1 + i : 기준을 mid + 1 값(오른쪽 배열의 첫번째 index)을 가지고 알맞은 요소를 하나씩 추가한다.
    rightArr[i] = arr[mid + 1 + i];
  }
  
  let i = 0;
  let j = 0;
  let k = left;
  
  // 왼쪽 배열과 오른쪽 배열의 각 첫번쨰 요소 부터 비교하여 정렬 후에 배열 arr에 추가한다. (결합)
  while (i < leftArr.length && j < rightArr.length) {
    if (leftArr[i] <= rightArr[j]) {
      arr[k] = leftArr[i];
      i++;
    } else {
      arr[k] = rightArr[j];
      j++;
    }
    
    k++;
  }
  
  // 왼쪽 배열의 남은 요소들을 배열 arr에 추가한다.
  while (i < leftArrLength) {
    arr[k] = leftArr[i];
    i++;
    k++;
  }
  
  // 오른쪽 배열의 남은 요소들을 배열 arr에 추가한다.
  while (j < rightArrLength) {
    arr[k] = rightArr[j];
    j++;
    k++;
  } 
}

// left은 하위 배열의 왼쪽 인덱스이고 right은 하위 배열의 오른쪽 인덱스입니다.
function mergeSort(arr, left, right) {
  if (left >= right) {
    return;
  }
  
  const mid = parseInt((left + right) / 2); // 중간 위치
  
  mergeSort(arr, left, mid); // 0 ~ 중간 위치까지 배열
  mergeSort(arr, mid + 1, right); // 중강 위치 + 1 ~ 마지막 요소까지 배열
  merge(arr, left, mid, right); // 분할한 배열을 정렬하여 결합하는 과정
}

const tempArr = [ 12, 11, 13, 5, 6, 7 ];

mergeSort(tempArr, 0, tempArr.length - 1);

console.log(tempArr); // [5, 6, 7, 11, 12, 13]
```

<br/>

### 시간 복잡도 & 공간 복잡도

#### 시간 복잡도

- 병합 정렬의 최악의 경우 시간 복잡도는 O(nlogn)입니다.
- 병합 정렬의 평균적인 경우의 시간 복잡도는 O(nlogn)입니다.
- 최선의 경우의 시간 복잡도는 O(nlogn)입니다.

#### 공간 복잡도

- 병합 정렬의 공간 복잡도는 O(N)입니다.

<br/>

### 병합 정렬의 특징

- 최악의 경우 O(nlogn)의 시간 복잡도로 인하여 대규모 데이터 집합을 정렬하는데 유용합니다.
- 외부 정렬을 사용합니다.
- 안정적인 정렬입니다. 즉, 입력 배열에서 동일한요소의 상대적인 순서를 유지합니다.
- 병합된 하위 배열을 저장하기 위해 추가 메모리가 필요합니다.
- 제자리 정렬이 아닙니다. 
- 작은 데이터 세트의 경우 성능이 느려질 수 있습니다. 

<br />

### 참조

---

https://gmlwjd9405.github.io/2018/05/08/algorithm-merge-sort.html

https://www.geeksforgeeks.org/merge-sort/

https://gyoogle.dev/blog/algorithm/Merge%20Sort.html
