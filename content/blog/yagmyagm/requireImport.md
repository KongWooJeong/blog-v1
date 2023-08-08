---
title: require VS import
date: 2023-07-30 13:00:00
category: 야금야금
draft: false
---

여러분은 자바스크립트를 사용하고 있다면 `require`, `import` 구문을 한번쯤은 사용해 보았을겁니다. 이번글에서 이 구문을 대하여 한번 살펴보겠습니다.

<br/>

### CommonJS

CommonJS를 사용하면 파일 간에 모듈(코드)을 쉽게 정의하고 공유할 수 있습니다. CommonJS는 서버에서 실행되도록 설계되었으며 `module.exports`를 사용하여 코드를 내보내고 `require()`를 사용하여 코드를 가져옵니다.

- 동기식이기 때문에 필요한 모듈이 완정히 로드될 때까지 require() 함수가 스크립트 실행을 차단합니다.
- 'global leaky bucket' 이라는 패턴을 사용하는데, 이러한 패턴은 모듈에서 선언된 변수가 전역 범위에 추가되어 다른 모든 모듈에서 접근할 수 있도록합니다.
- 주로 Node 프로젝트에서 사용됩니다.

#### 사용법

내보내기

```javascript
const greeting = (name) => console.log(`Hello! ${name}`);

module.exports = greeting;

// 다른 방법으로 내보내기
exports.greeting = () => {};
```

가져오기

```javascript
const greeting = require(./foo.js);

const welcome = (place) => console.log(`${greeting(Pratik)}. Welcome ${place}`);
```

여러개 내보내기

```javascript
const greeting = (name)=> console.log(`Hello! ${name}`);
const greeting_VIP = () => {};

module.exports = {
  greeting,
  greeting_VIP
};
```

여러개 가져오기

```javascript
const {greeting, greeting_VIP} = require('./foo.js');

greeting('Pratik');
greeting_VIP();
```

<br/>

### ES Modules

브라우저에서 사용하도록 설계되었습니다. 브라우저와 서버에서 모두 사용할 수 있습니다. 그리고 모듈간에 공유하려면 변수를 명시적으로 내보내고 가져와야 하는 'explicit exports' 라는 패턴을 사용하여 모듈에서 선언된 변수가 전역 범위 추가되어 다른 모든 모듈에서 접근할 수 있는 CommonJS 모둘에서 사용되는 'global leaky bucket' 패턴을 방지하는 데 도움이 됩니다.

- 다른 함수 내부에서 사용할 수 없습니다. 외부에 있어야 합니다.

#### 사용법

내보내기

```javascript
const greeting = (name)=> console.log(`Hello! ${name}`);
const greeting_VIP = (name)=> console.log(`Hello! ${name}. You are VIP`);

export default greeting;
```

가져오기

```javascript
import greeting from './foo.js';

import {greeting, greeting_VIP} from './foo.js';
```

HTML 스크립트 태그를 사용하여 ES module를 가져오기 (`type=module` 필수)

```html
<body> 
<script src='./foo.js' type='module'> </script>
</body>
```

내보내기

```javascript
export const greeting = (name)=> console.log(`Hello! ${name}`);
export const greeting_VIP = (name)=> console.log(`Hello! ${name}. You are VIP`);
```

<br/>

---

**지금까지는 각 모듈에 대한 특징과 간단하게 사용하는 방법에 대하여 알아보았습니다. 이제부터는 어떠한 차이점이 있는지 대하여 살펴보겠습니다.**

---

<br/>

### Node.js 에서 require 과 import 차이

- `import ... from ...` 을 사용하는 경우 모듈 경로는 문자열이야 하며 `require`를 사용하는 경우 모듈 경로는 동적일 수 있습니다.

```javascript
const name = "module2";
const obj = require(`./${name}`);
```

​	아래처럼 사용하다면 `SyntaxError: Unexpected template string` 오류가 발생합니다.

```javascript
const name = "module2";
import { func } from `./${name}`;

import { func2 } from ("./partial/path/" + name);
```

- 

 asdf

```javascript
console.log("require module1");

const obj = require("./module2");
console.log(`module2 = ${obj.module2}`);
```

```
require module1
require module2
module2 = require module2
```

 Pdf

```javascript
console.log("require module1");

import module2 from "./module2.js";
console.log(`module2 = ${module2}`);
```

```
require module2
require module1
module2 = require module2
```

<br/>

### 정리

Node.js 프로젝트에서 ES module을 사용하려면 ES module을 지원하는 Node.js 버전을 사용 중인지 확인해야 합니다. Node.js 14이상에서는 기본적으로 ES module을 지원하므로 사용할 수 있습니다.

ES module을 지원하지 않는 이전 버전의 Node.js를 사용한다면 바벨과 같은 도구를 사용하여 해당 코드를 트랜스파일링하면 사용할 수 있습니다.

<br />

### 참조 

---

https://medium.com/tldr-notes/import-vs-require-in-javascript-commonjs-vs-es-modules-b43806c5735c

https://pencilflip.medium.com/the-three-differences-between-require-and-import-in-node-js-f54f78f5936f

