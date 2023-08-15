---
title: async vs defer (script)
date: 2023-07-22 14:00:01
category: 야금야금
draft: false
---

**이 글은 스크립트 태그에 대하여 최적화하는 방법에 대하여 설명합니다.**

_async 및 defer는 모두 HTML의 스크립트 태그에 대한 속성으로, 이를 통해 외부 javascript를 로드하는 방법을 지정할 수 있습니다._

<br />

### async, defer 를 사용하는 이유?!

#### DOM 요소 접근 불가

```html
<head>
  <script>
    console.log(document.querySelector('h1'));
  </script>
</head>
<body>
  <h1>제목</h1>
</body>
```

위 코드에서 h1태그를 사용할 수 있기 전에 javascript를 실행하기 때문에 console.log 는 null 를 출력합니다. 따라서 querySelector 는 실제로 요소가 아직 존재하지 않기 때문에 요소에 접근할 수 없습니다.

스크립트 아래에 있는 DOM 요소에 접근할 수 없다. 따라서 DOM 요소에 핸들러를 추가하는 것과 같은 여러 행위가 불가능해진다.

<br />

#### 문서 차단

```html
<p>...스크립트 앞 콘텐츠...</p>

<script src="../"></script>

<!-- 스크립트 다운로드 및 실행이 끝나기 전까지 아래 내용이 보이지 않습니다. -->
<p>...스크립트 뒤 콘텐츠...</p>
```

페이지 위쪽에 용량이 큰 스크립트가 있는 경우 스크립트가 페이지 생성하는것을 막아버려서 페이지에 접속하는 사용자들은 스크립트를 다운받고 실행할 때까지 스크립트 아래쪽 페이지를 볼 수 없게 됩니다.

```html
<body>
  ... 스크립트 위 콘텐츠들 ...

  <script src="..."></script>
</body>
```

위 코드처럼 스크립트를 페이지 맨 아래 놓는 것이 하나의 방법이 될 수 있지만 완벽한 해결 방법은 아닙니다. 그 이유는 만약에 HTML 문서 자체가 아주 큰 경우에 브라우적 HTML 문서 전체를 다운로드 한 다음에 스크립트를 다운받게 하면 페이지가 정말 느려집니다.

네트워크 속도가 빠른 곳에서 페이지에 접속하고 있다면 이런 지연은 눈에 잘 띄지 않겠지만, 네트워크 환경이 느린곳에서는 그렇지 않습니다.

<br />

**_그래서 이러한 문제를 해결할 수 있는 스크립트 속성인 asycn, defer 가 있습니다._**

<br />

### 1. async 속성

async 스크립트는 백그라운드에서 다운로드됩니다. 따라서 HTML 페이지는 async 스크립트 다운이 완료되길 기다리지 않고 페이지 내 콘텐츠를 처리, 출력합니다. 그리고 async 스크립트 실행중에는 HTML 파싱이 멈춥니다.

async 는 외부 스크립트를 포함하는 데 사용할 때 스크립트 태그의 속성입니다. 사용하는 방법은 다음과 같습니다.

```javascript
<script src="jquery.js" async></script>
```

async는 스크립트가 다른 모든 리소스와 병렬로 로드되고 브라우저가 DOM을 빌드하고 동시에 스크립트를 로드할 수 있음을 의미합니다. async는 스크립트 로드가 더 이상 DOM을 차단하지 않도록 보장합니다.

스크립트가 로드를 완료하는 즉시 스크립트가 즉시 실행되어 브라우저가 무엇을 하든 차단합니다.

**스크립트가 언제 로드되고 언제 실행될지 명확하지 않습니다.**

따라서 스크립트가 DOM에 있는 무언가에 접근하려고 하지만 당시에는 존재하지 않을 수 있어 문제가 발생할 수 있습니다.

```html
<script src="library.js"></script>
<script src="app.js"></script>
```

위와 같이 async 속성없이 두 스크립트를 포함하면 library.js가 항상 먼저 실행되거나 사용할 수 있습니다. 브라우저는 위에서 아래로 이동하며 각각의 스크립트가 로드되고 실행될 때까지 일시 정지하기 때문입니다.

```html
<!-- library.js 가 app.js 보다 용량이 더 크다.  -->
<script src="library.js" async></script>
<script src="app.js" async></script>
```

이제 두 가지를 모두 async를 포함하면 중요한 점은 library.js 가 app.js보다 당연히 훨씬 더 크기 때문에 로드 시간이 길어진다는 것입니다. 그래서 두 스크립트가 병렬로 로드되지만 크기가 다르기 때문에 완료되는 시점이 달라집니다. 결론적으로는 로드시간이 더 짧은 app.js 스크립트가 더 일찍 로드되어 더 일찍 실행됩니다

<br/>

async 스크립트는 방문자 수 카운터나 광고 관련 스크립트처럼 각각 독립적인 역할을 하는 서드 파티 스크립트를 현재 개발 중인 스크립트에 통합하려 할 때 아주 유용합니다.

<br/>

### 2. defer 속성

브라우저는 `defer` 속성이 있는 스크립트를 '백그라운드’에서 다운로드 합니다. 따라서 지연 스크립트를 다운로드 하는 도중에도 HTML 파싱이 멈추지 않습니다. 그리고 `defer` 스크립트 실행은 페이지 구성이 끝날 때까지 _지연_ 됩니다.

defer는 스크립트를 로드할 때 브라우저를 차단하지 않습니다.

async를 사용하면 실행 중에 브라우저가 차단될 수 있다는 점이 결정적인 문제였는데, defer을 사용하면 그렇지 않습니다.

defer에 포함된 스크립트는 DOM이 준비되었을 때만 실행되므로 DOM 액세스가 보장되는 스크립트에 이상적입니다.

async와 defer의 공통점은 스크립트가 DOM 구성과 병렬로 로드된다는 점이며, 스크립트가 실행되는 시점만 두 가지 모두 다릅니다. 지연을 사용하면 DOM이 준비되었을 때만 실행되도록 보장됩니다.

```html
<!-- library.js 가 app.js 보다 용량이 더 크다.  -->
<script src="library.js" defer></script>
<script src="app.js" defer></script>
```

물론 library.js가 더 크기 때문에 로드하는 데 시간이 더 오래 걸리지만 library.js가 먼저 실행되고 그 다음에 app.js가 실행됩니다.

defer는 스크립트 태그의 순서로 지정한 실행 순서를 정확히 따르기 때문입니다. 그리고 defer의 경우 평소와 같이 DOM이 준비되었을 때만 실행됩니다.

### 참조

---

https://ko.javascript.info/script-async-defer

https://javascript.plainenglish.io/async-and-defer-the-complete-guide-to-loading-javascript-properly-ce6edce1e6b5
