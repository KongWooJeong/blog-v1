---
title: 알고 쓰자 - async, await
date: 2023-08-12 15:00:00
category: 야금야금
draft: false
---

이번 글에서는 우리가 자주 사용하는 async, await 관련해서 살펴보겠습니다. async, await에 대한 사용하는 방법에 대한 부분은 다루지 않고 어떻게 구현되어있는지에 대하여 알아보겠습니다.

<br/>

### async-await

async-await은 **ECMAScript 2017**에서 표준으로 정의되었습니다. 비동기 프로그래밍을 동기 방식처럼 직관적으로 표현할 수 있고, callback 지옥 문제를 해결해 줍니다.

비동기 로직을 쉽게 작성하기 위해, 즉 비동기가 들어간 비즈니스 로직은 중첩될수록 그 복잡도가 기하 급수적으로 늘어나며, 이를 간결하게 하여 유지보수성 향상에 기여할 수 있습니다.

그러나 한가지 문제점은 **ECMAScript 2017**를 지원하지 않는 하위 버전에 브라우저에서는 사용할 수 없다는 단점이 있습니다. 그래서 Babel과 같은 트랜스파일러를 이용하여 하위 버전 브라우저에서 이해 할 수 있는 **ECMAScript 2016**문법으로 변환을 하면 사용할 수 있습니다.

그러면 여기서 한가지 알 수 있는것은 **ECMAScript 2016**로 변환한다는 의미는 **ECMAScript 2016**로 구현이 가능하다는 것입니다. async-await 구문이 어떻게 **ECMAScript 2016** 문법으로 표현되는지 알아 보겠습니다.

<br/>

### async-await을 바벨로 변환

async-await 구문을 바벨을 이용하여 변환하게 된다면 아래와 같이 `promise` 와 `generator`로 구현되어 있는것을 확인 할 수 있습니다.

변환할 수 있는 사이트는 [바벨로 변환]을 참조 해주시면 됩니다.

```javascript
**ECMAScript 2017**
async function sample() {
	await fetch("api");
}

**ECMAScript 2016**
function asyncGeneratorStep(gen, resolve, reject, _next, _throw, key, arg) {
  try {
    var info = gen[key](arg);
    var value = info.value;
  } catch (error) {
    reject(error);
    return;
  }
  if (info.done) {
    resolve(value);
  } else {
    Promise.resolve(value).then(_next, _throw);
  }
}

function _asyncToGenerator(fn) {
  return function () {
    var self = this,
      args = arguments;
    return new Promise(function (resolve, reject) {
      var gen = fn.apply(self, args);
      function _next(value) {
        asyncGeneratorStep(gen, resolve, reject, _next, _throw, "next", value);
      }
      function _throw(err) {
        asyncGeneratorStep(gen, resolve, reject, _next, _throw, "throw", err);
      }
      _next(undefined);
    });
  };
}

function sample() {
  return _sample.apply(this, arguments);
}

function _sample() {
  _sample = _asyncToGenerator(function* () {
    yield fetch("api");
  });
  return _sample.apply(this, arguments);
}

```

<br/>

### async-await 과 promise, generator

바벨로 변환된 코드에 대하여 살펴보겠습니다.

```javascript
**변환 전**
async function sample() {
	await fetch("api");
}

**변환 후**
function _sample() {
  _sample = _asyncToGenerator(function* () {
    yield fetch("api");
  });
}
```

변환된 코드를 간략히 하였습니다. 위 코드를 봤을때 무언가 차이점이 보이시나요? 제가 봤을때는 `await` 이 `yield` 로 바뀌고 `async` 가 `function* () {}` (제너레이터)로 바뀐것 같이 보이는데요. 일단 지금은 `_asyncToGenerator` 함수에 대해서는 잠시 보류하면 결국 async-await 은 제너레이터를 사용하여 구현되었다고 볼 수 있겠네요.

```javascript
function* gen() {
  yield 1;
}

const generator = gen();

const one = gen.next();

console.log(one.value); // 1 출력
```

여기서 제너레이터에 대하여 간단히 설명하자면 제너레이터 함수를 실행하고 반환되어진 객체에 next() 메소드를 실행을 하였을때 가장 가까운 yield 문을 만날때까지 실행이 되고 해당 값이 반환 됩니다.

따라서 비동기 로직이 종료되었을 때마다 적절하게 next() 메서드를 호출해주기만 하면, async-await이 구현되어 지는것입니다.

그러면 이번에는 `_asyncToGenerator` 살펴보겠습니다.

```javascript
function _asyncToGenerator(fn) {
  return function() {
    var self = this,
      args = arguments;
    return new Promise(function(resolve, reject) {
      var gen = fn.apply(self, args);
      function _next(value) {
        asyncGeneratorStep(gen, resolve, reject, _next, _throw, 'next', value);
      }
      function _throw(err) {
        asyncGeneratorStep(gen, resolve, reject, _next, _throw, 'throw', err);
      }
      _next(undefined);
    });
  };
}
```

`_asyncToGenerator` 함수는 함수를 인자로 받고 함수를 반환하고 있는것 같습니다. 반환하는 함수에 대하여 자세히 살펴보겠습니다.

여기서 인자로 전달 받은 함수는 다음과 같습니다.

```javascript
function* () {
  yield fetch("api");;
}
```

반환하는 함수는 다음과 같습니다.

```javascript
function () {
  return new Promise(function (resolve, reject) {

    ## 제너레이터 함수를 실행하여 반환되어진 제너레이터 객체
    var gen = fn.apply(self, args);

    function _next(value) {
      asyncGeneratorStep(gen, resolve, reject, _next, _throw, "next", value);
    }
    function _throw(err) {
      asyncGeneratorStep(gen, resolve, reject, _next, _throw, "throw", err);
    }

    _next(undefined);
  });
};
```

위 코드와 같이 반한되어진 함수는 `promise`를 반환하고 있네요. `promise`의 콜백 함수를 보면 `_next` , `_throw` 메서드가 선언되어 있는것으로 보입니다. 두 함수 내부에는 `asyncGeneratorStep` 함수를 호출하고 있습니다.

`asyncGeneratorStep` 함수는 제너레이터 객체, resolve, reject 등등 다양한 인자를 전달하여 호출하고 있습니다.

여기까지 봤을때는 아직도 세부 구현이 어떻게 돌아가고 있는지 잘 파악이 되지 않습니다. 그래서 조금 더 깊이 들어보겠습니다.

이번에는 `asyncGeneratorStep` 함수에 대하여 살펴보겠습니다.

```javascript
## gen: 제너레이터 함수를 실행하여 반환되어진 제너레이터 객체
function asyncGeneratorStep(gen, resolve, reject, _next, _throw, key, arg) {
  try {

    ## gen.next(arg) or gen.throw(arg)
    var info = gen[key](arg);

    ## { value: yield 구문에 있는 어떠한 로직이 실행되고 나서 결과값, done: 모든 yield 구문이 실행 여부 }
    var value = info.value;
  } catch (error) {
    reject(error);
    return;
  }

  if (info.done) {
    ## 제너레이터의 모든 yield 구문이 처리가 되었을때
    resolve(value);
  } else {
    Promise.resolve(value).then(_next, _throw);
  }
}
```

전달 받은 gen(제너레이터 객체)에 key는 "next" or "throw"이므로 `gen[key]()`는 `gen[next]() (gen.next())` `gen[throw] (gen.throw())`가 됩니다. 즉, 제너레이터의 next 함수, throw 함수를 실행하는 부분입니다. 그리고 실행할때는 인자로 넘어온 `arg` 값이 `next` 또는 `throw`가 호출될때 인자로 넘겨집니다.

```javascript
Promise.resolve(value).then(_next, _throw);
```

`Promise.resolve(value)`는 결괏값이 `value`인 이행 상태 프라미스를 생성합니다. 그래서 프로미스가 이행된 값이 `then` 메소드의 첫번째 메소드인 `_next(프로미스 결과 값)`으로 호출됩니다. 그리고 프로미스가 "rejected" 상태이면 즉, 오류가 발생하면 두번째 메소드인 `_throw(에러)`가 실행됩니다.

그러면 만약에서 여기서 `_next(프로미스 결과 값)`가 호출되면 다시 `asyncGeneratorStep`함수가 실행되고 이번엔 해당 함수가 실행되면서 `arg`로 인자로 프로미스가 이행된 결과 값을 전달하고 다시 `gen.next(프로미스 결과 값)` 이 실행됩니다. 그리고 제너레이터 `next` 함수에 인자로 넘어간 값은 `yield` 구문의 반환값으로 할당 됩니다.

```javascript
function* generator() {
  ## next 함수 호출시 인자로 넘어간 값이 result에 할당 됩니다.
  const result = yield fetch("api");
}

<샘플 예시>
const gen = generator();

gen.next();
gen.next("결과");

result 변수에는 "결과"가 할당됩니다.
```

**즉, 여기서 한번 정리를 하자면 asyncGeneratorStep 함수는 재귀함수를 통해 next() 메서드를 대신 호출하여 yield 구문의 결과 값이 나올때 next("결과") 호출할때 인자로 결과값을 넣어 yield 구문의 반환값을 할당하여 async-await 을 구현할 수 있습니다.**

그리고 마지막으로는 해당 제너레이터 객체의 속성중 `done` 속성이 true 이면 해당 재귀함수는 멈춥니다. 즉, 제너레이터 함수의 모든 yield 구문을 처리하면 재귀함수는 멈춥니다.

<br/>

### async-await을 간단하게 구현해보기

앞서서 설명드린 바벨로 변환되어진 코드는 조금 복잡하니 해당 부분을 간단하게 한번 구현해보도록 하겠습니다.

```javascript
function api(time) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(time);
    }, time);
  });
}

function _asyncToGenerator(genFunc) {
  return function() {
    const gen = genFunc();

    function next(value) {
      const genInfo = gen.next(value);

      if (!genInfo.done) {
        Promise.resolve(genInfo.value).then(next);
      }
    }

    next();
  };
}

function* sample() {
  const result = yield api(1000);

  console.log(result);
}

const asyncAwaitSample = _asyncToGenerator(sample);

asyncAwaitSample();
```

<br/>

---

https://medium.com/@la.place/async-await%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EA%B5%AC%ED%98%84%ED%95%98%EB%8A%94%EA%B0%80-fa08a3157647

[바벨로 변환]: https://babeljs.io/repl#?browsers=chrome%2050&build=&builtIns=false&corejs=3.21&spec=false&loose=false&code_lz=IYZwngdgxgBAZgV2gFwJYHsIxMAtgBwBsBTACgEoYBvAKAEhgB3YVZGYfVCgbhoF8gA&debug=false&forceAllTransforms=false&modules=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=env&prettier=true&targets=&version=7.22.10&externalPlugins=&assumptions=%7B%7D '바벨로 변환'
