---
title: Typescript - any vs unknown
date: 2023-07-22 19:00:01
category: 야금야금
draft: false
---

`any` 타입의 변수에는 무엇이든 지정할 수 있습니다.

```typescript
let myVar: any = 0;
myVar = '1';
myVar = false;
```

타입스크립트 가이드에서는 `any` 를 사용하면 타입 제한이 사라지기 때문에 사용을 권장하지 않습니다.

`unknown` 타입의 변수에는 무엇이든 지정할 수 있습니다.

```typescript
let myVar: unknown = 0;
myVar = '1';
myVar = false;
```

**그러면 any 와 unknown의 차이는 무엇일까요?**

<br/>

### any vs unknown

예를들어 하나의 함수를 작성하겠습니다.

```typescript
function invokeAnything(callback: any) {
  callback();
}

invokeAnything(1);
```

callback 매개변수는 어떤 유형이든 가능하므로 `callback()` 문은 type error 를 발생시키지 않습니다.

그러나 해당 코드를 실행하게 되면 런타임 오류가 발생합니다.

`TypeError: callback is not a function`

`1`은 숫자이므로 함수로 호출할 수 없기 떄문입니다.

`unknown` 타입 변수는 `any`와 마찬가지로 모든 값을 허용합니다. 그러나 `unknown` 변수를 사용하력 할때 타입스크립튼 유형 검사를 시행합니다.

이전과 비슷하나 `unknown` 타입을 코드를 예시를 들어보겠습니다.

```typescript
function invokeAnything(callback: unknown) {
  callback(); // Type error: 'callback' is of type 'unknown'
}

invokeAnything(1);
```

`callback` 매개변수의 타입을 알 수 없기 때문에 타입 오류가 발생합니다.

위 코드를 정상적으로 실행하기 위해서는 `unknown` 타임의 변수를 사용하기 전에 타입 검사를 수행 하면 됩니다.

```typescript
function invokeAnything(callback: unknown) {
  if (typeof callback === 'function') {
    callback();
  }
}

invokeAnything(1);
```

`typeof callback === 'function'` 구문을 추가하면 `unknown` 타입이 `Function` 타입으로 좁혀졌으므로 안전하에 `callback()` 을 호출할 수 있습니다. 그래서 타입 에러도 없고 런타임 에러도 없습니다.

any 와 unknown 의 차이점은 다음과 같습니다.

- `unknown` 타입에는 무엇이든 할당할 수 있지만 `unknown` 타입에서 작업하려면 타입 검사를 수행해야 합니다.
- `any` 타입에는 무엇이든 할당할 수 있으며 모든 타입에 대해 어떠한 작업이든지 수행할 수 있습니다.

<br />

### 참조

---

https://dmitripavlutin.com/typescript-unknown-vs-any/
