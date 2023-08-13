---
title: 연결 리스트
date: 2023-07-21 14:00:01
category: 야금야금
draft: false
---

링크드 리스트는 데이터를 일직선으로 나영한 형태를 가지고 있습니다. 데이터 추가나 삭제는 쉽지만, 원하는 데이터에 접근하려면 시간이 많이 걸립니다.

### 배열 vs 링크드리스트

배열은 메모리 상에서 연속적인 공간을 할당받아 데이터를 저장한다. 배열에서는 데이터를 접근할때 인덱스(index)라는 데이터의 주소를 가리키는 식별자를 통해 빠르게 특정 데이터에 접근할 수 있다. 하지만 이러한 특징으로 인해 중간에 데이터를 삽입 또는 삭제하는 과정이 느리다.

반면에 링크드 리스트는 메모리 상에서 연속적인 공간에 데이터를 저장하지 않을 수 있다. 리스트의 각 데이터들은 다음 데이터에 위치를 알 수 있는 정보, 즉 포인터를 가지고 있다. 이러한 특징 때문에 배열과 다르게 데이터를 삽입 또는 삭제하는 과정이 빠르다. 하지만 특정 데이터에 접근을 할때 배열처럼 인덱스로 접근할 수 없기때문에 첫번째 데이터부터 선형 검색을 해야하기 때문에 배열보다 느리다.

<br />

### 링크드 리스트 구현

1. 노드 생성

```javascript
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}
```

링크드 리스트에서 각 데이터에 정보(값, 다음 데이터 위치)를 가지고 있는 노드를 구현합니다.

<br />

2. 링크드 리스트 생성자 함수

```javascript
class LinkedList {
  constructor() {
    this.head = null;
    this.size = 0;
  }
}
```

링크드 리스트를 생성하기 위해서 생성자 함수를 선언하고 그 함수 안에는 링크드 리스트에 첫번째 데이터를 담당하고 있는 "head" 프로퍼티를 선언합니다. 그리고 링크드 리스트에 노드의 갯수를 알기 위한 size 프로퍼티를 선언합니다.

<br />

_이후 과정부터는 class 를 제외하고 해당 구현하고자 하는 메소드만 표현하겠습니다._

<br />

3. add - 리스트 끝에 노드를 추가

```javascript
add(value) {
  // 노드 생성
	const node = New Node(value);

  if (this.head === null) {
    // 현재 head 가 비어있으면 head에 node를 할당한다.
    this.head = node;
  } else {
    // 현재 노드
  	let currentNode = this.head;

    // 해당 노드의 다음 값이 없을때까지 리스트를 탐색한다.
    while (currentNode.next) {
      currentNode = currentNode.next;
    }

    // 마지막 노드에 추가할 노드를 할당한다.
    currentNode.next = node;
  }

  // 리스트의 크기를 증가한다.
  this.size++;
}
```

목록 끝에 요소를 추가하는 경우 고려사항

- 리스트가 비어 있으면 요소를 추가하면 헤드가 됩니다.
- 리스트가 비어 있지 않으면 리스트 끝까지 반복하고 리스트 끝에 요소를 추가합니다.

<br />

4. insertAt(value, index) - 리스트에 지정된 index에 요소를 추가한다.

```javascript
insertAt(value, index) {
	if (index < 0 || index > this.size) {
    // index가 0보다 작거나 리스트 크기보다 클때
    console.log("유효한 인덱스를 입력하세요.");
  } else {
    const node = New Node(value);

    let currentNode = this.head;

    if (index === 0) {
      // 첫번째에 추가하기
      node.next = head.next;
      this.head = node;
    } else {
      let currentIndex = 0;
      let previousNode = null;

      // 리스트에서 삽입할 위치를 탐색
      while (currentIndex < index) {
        currentIndex++;
        previousNode = currentNode;
        currentNode = currentNode.next;
      }

      // 노드 추가
      node.next = currentNode; // 노드(추가할)에 다음 요소를 현재 노드로 설정
      previousNode.next = node; // 이전 노드에 다음 요소를 노드(추가할)로 설정
    }

    this.size++;
  }
}
```

리스트의 지정된 인덱스에 요소를 추가하기 위해 다음과 같은 조건을 고려 해야합니다.

- 인덱스가 0이면 리스트의 앞쪽에 요소를 추가하고 head로 만듭니다.
- 인덱스가 목록의 마지막 위치인 경우 리스트 끝에 요소를 추가합니다.
- 인덱스가 0 ~ this.size -1 사이인 경우 인덱스까지 반복하고 해당 인덱스에 요소를 추가합니다.

<br />

5. remove(value) - 목록에서 요소를 제거합니다. 제거된 요소를 반환하거나 요소를 찾을 수 없는 경우 -1을 반환합니다.

```javascript
remove(value) {
	let currentNode = this.head;
  let previousNode = null;

  // 리스트 순회
  while (currentNode !== null) {
    if (currentNode.value === value) {
      if (previousNode === null) {
        // 현재 노드가 head 일때
        // head 에 head 다음 요소 할당(즉, head 제거)
        this.head = currentNode.next;
      } else {
        // 현재 노드가 head가 아닐때
        // 이전 노드의 next 에 현재 노드의 next 할당(즉, 현재 노드 제거)
        previousNode.next = currentNode.next;
      }

      this.size--;
      return currentNode.value;
    }

    // 리스트를 순회하면서 이전노드, 현재노드 계속 변경
    previousNode = currentNode;
    currentNode = currentNode.next;
  }

  return -1;
}
```

<br/>

### 전체 코드

```javascript
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class LinkedList {
	constructor() {
    this.head = null;
    this.size = 0;
  }

  add(value) {
    // 노드 생성
    const node = New Node(value);

    if (this.head === null) {
      // 현재 head 가 비어있으면 head에 node를 할당한다.
      this.head = node;
    } else {
      // 현재 노드
      let currentNode = this.head;

      // 해당 노드의 다음 값이 없을때까지 리스트를 탐색한다.
      while (currentNode.next) {
        currentNode = currentNode.next;
      }

      // 마지막 노드에 추가할 노드를 할당한다.
      currentNode.next = node;
    }

    // 리스트의 크기를 증가한다.
    this.size++;
  }

  insertAt(value, index) {
    if (index < 0 || index > this.size) {
      // index가 0보다 작거나 리스트 크기보다 클때
      console.log("유효한 인덱스를 입력하세요.");
    } else {
      const node = New Node(value);

      let currentNode = this.head;

      if (index === 0) {
        // 첫번째에 추가하기
        node.next = head.next;
        this.head = node;
      } else {
        let currentIndex = 0;
        let previousNode = null;

        // 리스트에서 삽입할 위치를 탐색
        while (currentIndex < index) {
          currentIndex++;
          previousNode = currentNode;
          currentNode = currentNode.next;
        }

        // 노드 추가
        node.next = currentNode; // 노드(추가할)에 다음 요소를 현재 노드로 설정
        previousNode.next = node; // 이전 노드에 다음 요소를 노드(추가할)로 설정
      }

      this.size++;
    }
  }

  remove(value) {
    let currentNode = this.head;
    let previousNode = null;

    // 리스트 순회
    while (currentNode !== null) {
      if (currentNode.value === value) {
        if (previousNode === null) {
          // 현재 노드가 head 일때
          // head 에 head 다음 요소 할당(즉, head 제거)
          this.head = currentNode.next;
        } else {
          // 현재 노드가 head가 아닐때
          // 이전 노드의 next 에 현재 노드의 next 할당(즉, 현재 노드 제거)
          previousNode.next = currentNode.next;
        }

        this.size--;
        return currentNode.value;
      }

      // 리스트를 순회하면서 이전노드, 현재노드 계속 변경
      previousNode = currentNode;
      currentNode = currentNode.next;
    }

    return -1;
  }
}
```
