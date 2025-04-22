---
title: "Next.js 15에서 Edge Runtime 사용하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:41
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Edge Runtime"
link: "https://nextjs.org/docs/app/api-reference/edge"
isUpdated: false
---


# Next.js Edge Runtime API 소개

Next.js의 Edge Runtime은 Middleware 용으로 사용되며, 다양한 네트워크 관련 API를 지원합니다. 이걸 잘 활용하면 훨씬 가볍고 빠른 엣지 환경에서 코드를 실행할 수 있어요!

아래는 Edge Runtime에서 지원하는 주요 네트워크 API들입니다.

| API | 설명 |
| --- | --- |
| [Blob](https://developer.mozilla.org/docs/Web/API/Blob) | 바이너리 데이터 덩어리를 나타내는 객체입니다. 파일이나 네트워크 데이터를 다룰 때 유용해요. |
| [fetch](https://developer.mozilla.org/docs/Web/API/Fetch_API) | 네트워크 리소스를 비동기로 요청하는 함수입니다. API 호출이나 데이터 가져올 때 많이 쓰죠. |
| [FetchEvent](https://developer.mozilla.org/docs/Web/API/FetchEvent) | fetch 요청 이벤트를 나타냅니다. 미들웨어에서 요청을 가로채거나 처리할 때 사용됩니다. |
| [File](https://developer.mozilla.org/docs/Web/API/File) | 사용자가 업로드한 파일 정보를 나타내는 객체입니다. |
| [FormData](https://developer.mozilla.org/docs/Web/API/FormData) | 폼 데이터를 쉽게 다루기 위한 객체입니다. 파일 업로드와 함께 많이 사용돼요. |
| [Headers](https://developer.mozilla.org/docs/Web/API/Headers) | HTTP 요청 및 응답의 헤더를 다루는 객체입니다. 요청을 커스터마이징할 때 필수! |
| [Request](https://developer.mozilla.org/docs/Web/API/Request) | HTTP 요청 자체를 나타내는 객체로, fetch 호출 시 사용합니다. |
| [Response](https://developer.mozilla.org/docs/Web/API/Response) | HTTP 응답을 나타내는 객체입니다. 미들웨어에서 응답을 조작하고 싶을 때 씁니다. |
| [URLSearchParams](https://developer.mozilla.org/docs/Web/API/URLSearchParams) | URL 쿼리 스트링 파라미터를 손쉽게 읽고 조작할 수 있습니다. |
| [WebSocket](https://developer.mozilla.org/docs/Web/API/WebSocket) | 실시간 양방향 통신이 가능한 웹소켓 연결을 나타냅니다. |

---

**추가 팁!**  
Edge Runtime 환경은 서버리스 함수처럼 동작하면서도 훨씬 더 빠른 응답성을 제공합니다. 대신 브라우저 환경이 아닌 만큼, 기존 Node.js API 전체를 사용하는 것보다 지원하는 API가 제한되어 있다는 점 꼭 기억하세요.

이 중 `fetch` API는 기본 제공되니 서버사이드에서 외부 API 호출할 때 별도의 라이브러리 없이 그냥 사용 가능하답니다! 그 외에도 HTTP 요청과 응답을 직접 만지면서 미들웨어를 만들어보면 Next.js의 내부 동작 원리에 대해 훨씬 쉽게 이해할 수 있습니다.

한번 써보시고, 엣지 컴퓨팅의 빠른 속도를 체험해보시길 추천드려요!

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 인코딩 API들

| API | 설명 |
| --- | --- |
| [atob](https://developer.mozilla.org/en-US/docs/Web/API/atob) | base-64로 인코딩된 문자열을 디코딩해줍니다. |
| [btoa](https://developer.mozilla.org/en-US/docs/Web/API/btoa) | 문자열을 base-64로 인코딩해줍니다. |
| [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder) | Uint8Array 형태의 바이트 데이터를 문자열로 디코딩합니다. |
| [TextDecoderStream](https://developer.mozilla.org/docs/Web/API/TextDecoderStream) | 스트림을 순차적으로 디코딩할 수 있는 체이닝 가능한 디코더입니다. |
| [TextEncoder](https://developer.mozilla.org/docs/Web/API/TextEncoder) | 문자열을 Uint8Array 형태의 바이트 배열로 인코딩합니다. |
| [TextEncoderStream](https://developer.mozilla.org/docs/Web/API/TextEncoderStream) | 스트림 데이터를 순차적으로 인코딩할 수 있는 체이닝 가능한 인코더입니다. |

---

### 조금 더 이야기해볼게요!

- **atob/btoa**는 간단하게 텍스트와 base64 변환을 할 때 유용해요. 다만 유니코드 문자열을 제대로 처리하지 못하는 단점이 있어요. 유니코드 문자열 인코딩이 필요하다면 `TextEncoder`/`TextDecoder`를 추천합니다.
- `TextEncoder`와 `TextDecoder`는 UTF-8 인코딩을 기본으로 하고 있어서, 국제 문자도 안전하게 다룰 수 있답니다!
- 그리고 스트림 관련 인코더/디코더인 `TextEncoderStream`과 `TextDecoderStream`은 대용량 데이터를 실시간 처리하거나 네트워크 통신 시에 아주 편리해요.

---

## 스트림 API들

| API | 설명 |
| --- | --- |
| [ReadableStream](https://developer.mozilla.org/docs/Web/API/ReadableStream) | 읽을 수 있는 스트림을 나타냅니다. |
| [ReadableStreamBYOBReader](https://developer.mozilla.org/docs/Web/API/ReadableStreamBYOBReader) | ReadableStream의 데이터를 직접 읽는 리더입니다. (BYOB: Bring Your Own Buffer) |
| [ReadableStreamDefaultReader](https://developer.mozilla.org/docs/Web/API/ReadableStreamDefaultReader) | ReadableStream의 기본적인 리더로, 자동 버퍼 관리를 지원합니다. |
| [TransformStream](https://developer.mozilla.org/docs/Web/API/TransformStream) | 입력된 데이터를 즉시 변환하여 출력하는 변환용 스트림입니다. |
| [WritableStream](https://developer.mozilla.org/docs/Web/API/WritableStream) | 쓸 수 있는 스트림을 나타냅니다. |
| [WritableStreamDefaultWriter](https://developer.mozilla.org/docs/Web/API/WritableStreamDefaultWriter) | WritableStream에 데이터를 쓸 수 있게 해주는 기본 작성자입니다. |

---

### 스트림 API는 왜 꼭 알아야 할까?

웹에서는 단순히 한 번에 모든 데이터를 처리하는 것보다, **실시간으로 데이터를 조금씩 주고받거나 변환하는 작업**이 훨씬 많아졌어요. 특히 네트워크 스트리밍, 미디어 처리, 대용량 파일 처리 등에서 매우 중요하죠.

- `ReadableStream`은 '읽을 수 있는' 데이터 흐름,  
- `WritableStream`은 '쓸 수 있는' 데이터 흐름,  
- `TransformStream`은 '읽기와 쓰기를 동시에 하면서 중간에 가공하는' 역할을 합니다.

예를 들어, 사용자의 음성 데이터를 받아서 텍스트로 변환하고, 다시 텍스트를 서버로 전송하는 복잡한 작업도 이 스트림 API를 이용해 효율적으로 처리할 수 있어요!

---

이처럼 자바스크립트의 인코딩 및 스트림 API는 데이터 처리에 있어 아주 강력한 도구입니다. 직접 써보면서 익히면, 어떠한 데이터도 자유자재로 다룰 수 있게 될 거예요! 필요하면 간단한 예제도 공유할게요~

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

## Crypto APIs

| API | Description |
| ---- | ----------- |
| [`crypto`](https://developer.mozilla.org/docs/Web/API/Window/crypto) | 플랫폼의 암호화 기능에 접근할 수 있게 해줘요. |
| [`CryptoKey`](https://developer.mozilla.org/docs/Web/API/CryptoKey) | 암호화 키를 나타내는 객체에요. |
| [`SubtleCrypto`](https://developer.mozilla.org/docs/Web/API/SubtleCrypto) | 해싱, 서명, 암호화, 복호화 같은 일반적인 암호화 작업을 할 수 있게 해줘요. |

> 암호화 관련 API는 보안에 민감한 작업을 할 때 꼭 알아둬야 해요. 예를 들어, 비밀번호를 처리하거나 데이터를 안전하게 전송할 때 활용할 수 있답니다.

---

## Web Standard APIs

| API | Description |
| ---- | ----------- |
| [`AbortController`](https://developer.mozilla.org/docs/Web/API/AbortController) | 여러 개의 DOM 요청을 원할 때 중단할 수 있게 도와줘요. 예를 들어, API 호출을 취소할 때 유용하죠. |
| [`Array`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array) | 값들의 배열을 표현해요. 자바스크립트의 기본이죠! |
| [`ArrayBuffer`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) | 고정 길이의 원시 바이너리 데이터를 저장하는 버퍼예요. |
| [`Atomics`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Atomics) | 멀티스레딩 환경에서 안전한 원자적 연산을 제공하는 정적 메서드들이에요. |
| [`BigInt`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/BigInt) | 아주 큰 정수를 표현할 때 쓰이는 타입으로, 숫자 한계가 걱정 없어요. |
| [`BigInt64Array`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/BigInt64Array) | 64비트 부호있는 정수들의 타입드 배열이에요. |
| [`BigUint64Array`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/BigUint64Array) | 64비트 부호없는 정수들의 타입드 배열이에요. |
| [`Boolean`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean) | 참(`true`) 또는 거짓(`false`)을 나타내는 논리 타입이에요. |
| [`clearInterval`](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/clearInterval) | `setInterval()`로 예약되어 있는 반복 작업을 취소해요. |
| [`clearTimeout`](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/clearTimeout) | `setTimeout()`으로 예약된 작업을 취소해요. |
| [`console`](https://developer.mozilla.org/docs/Web/API/Console) | 브라우저 디버깅 콘솔에 접근할 수 있게 해요. 디버깅 시 자주 사용하죠. |
| [`DataView`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/DataView) | `ArrayBuffer`의 다양한 데이터 뷰를 제공해요. |
| [`Date`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Date) | 플랫폼 독립적인 시간 정보를 표현해요. 시간 관련 작업에 필수! |
| [`decodeURI`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/decodeURI) | `encodeURI`로 인코딩된 URI를 디코딩해요. |
| [`decodeURIComponent`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/decodeURIComponent) | `encodeURIComponent`로 인코딩된 URI 컴포넌트를 디코딩해요. |
| [`DOMException`](https://developer.mozilla.org/docs/Web/API/DOMException) | DOM 관련 에러를 나타내는 객체예요. |
| [`encodeURI`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/encodeURI) | URI 내 일부 문자를 이스케이프 시퀀스로 변환해요. |
| [`encodeURIComponent`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent) | URI 컴포넌트의 특수 문자를 이스케이프해요. |
| [`Error`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Error) | 실행 중 발생한 일반적인 에러를 나타내요. |
| [`EvalError`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/EvalError) | `eval()` 함수와 관련된 에러를 나타내요. 근데 요즘은 거의 안 써요. |
| [`Float32Array`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Float32Array) | 32비트 부동소수점 숫자들의 타입드 배열이에요. |
| [`Float64Array`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Float64Array) | 64비트 부동소수점 숫자들의 타입드 배열이에요. |
| [`Function`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Function) | 자바스크립트 함수 자체를 나타내는 객체에요. |
| [`Infinity`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Infinity) | 수학적 무한대를 나타내는 값이에요. |
| [`Int8Array`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Int8Array) | 8비트 부호있는 정수 배열이에요. |
| [`Int16Array`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Int16Array) | 16비트 부호있는 정수 배열이에요. |
| [`Int32Array`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Int32Array) | 32비트 부호있는 정수 배열이에요. |
| [`Intl`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Intl) | 국제화(internationalization)와 로컬라이제이션 기능을 제공해요. |
| [`isFinite`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/isFinite) | 숫자가 유한한 값인지 검사해요. |
| [`isNaN`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/isNaN) | 값이 `NaN`인지 검사해요. |
| [`JSON`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON) | 자바스크립트 값과 JSON 포맷 간 변환 기능을 제공해요. |
| [`Map`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Map) | 키-값 쌍을 저장하는 컬렉션으로, 키의 중복을 허용하지 않아요. |
| [`Math`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Math) | 수학 관련 함수와 상수를 제공해요. |
| [`Number`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number) | 숫자형 데이터를 나타내는 객체에요. |
| [`Object`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object) | 자바스크립트 객체의 기본이 되는 객체에요. |
| [`parseFloat`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/parseFloat) | 문자열을 부동소수점 숫자로 파싱해요. |
| [`parseInt`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/parseInt) | 문자열을 지정한 진수(radix)의 정수로 파싱해요. |
| [`Promise`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) | 비동기 작업의 완료 또는 실패와 그 결과를 나타내요. |
| [`Proxy`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Proxy) | 기본 객체 동작을 가로채고 커스텀 동작을 가능하게 해주는 객체에요. |
| [`queueMicrotask`](https://developer.mozilla.org/docs/Web/API/queueMicrotask) | 마이크로태스크를 큐에 넣어 실행하도록 예약해요. |
| [`RangeError`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/RangeError) | 값이 허용된 범위 밖일 때 발생하는 에러예요. |
| [`ReferenceError`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError) | 존재하지 않는 변수에 접근할 때 발생해요. |
| [`Reflect`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Reflect) | 메타프로그래밍(메서드 가로채기 등)을

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

## Next.js에서 알아두면 좋은 폴리필(Polyfills)

- **AsyncLocalStorage**  
  Next.js 환경에서 AsyncLocalStorage를 사용할 때, 기본적으로 브라우저에는 지원되지 않으니까 서버 사이드에서만 동작하도록 신경 써야 해요. 만약 브라우저에서도 비슷한 기능이 필요하면, 직접 폴리필을 적용하거나 대체 라이브러리를 찾아보는 게 좋아요.

> 참고로 AsyncLocalStorage는 비동기 컨텍스트를 관리하는 데 유용한데, 특히 요청별 데이터를 격리할 때 자주 쓰입니다. Next.js로 SSR 작업할 때 이런 기능을 활용하면 상태 관리가 좀 더 깔끔해지니까, 필요하면 꼭 공부해보길 추천해요!

---

## 환경 변수(Environment Variables) 다루기

Next.js에서는 환경 변수를 다룰 때 `process.env`를 통해 값을 가져올 수 있어요. `next dev`나 `next build` 할 때도 똑같이 접근할 수 있어서 편리하답니다.

예를 들어, `.env.local` 파일에 다음과 같이 환경 변수를 정의했다고 치면,

```js
NEXT_PUBLIC_API_URL=https://api.example.com
SECRET_API_KEY=abcdef123456
```

- 클라이언트에서 접근하고 싶으면 반드시 `NEXT_PUBLIC_`로 시작하는 변수여야 해요.  
- 서버 사이드 코드(예: API 라우트, getServerSideProps)에서는 `process.env.SECRET_API_KEY`처럼 바로 사용 가능하고요.

```javascript
// 클라이언트 코드 예시 (React 컴포넌트 내부)
const apiUrl = process.env.NEXT_PUBLIC_API_URL;

// 서버 코드 예시 (API 라우트)
export default function handler(req, res) {
  const secret = process.env.SECRET_API_KEY;
  res.end(`Your secret key is ${secret}`);
}
```

> 주의할 점! 환경 변수의 값은 빌드 시점에 정적으로 주입되기 때문에, 변경사항이 생기면 반드시 `next dev` 혹은 `next build`를 다시 실행해줘야 합니다.

---

Next.js에서 환경 변수를 활용하는 방법은 기본적이지만, 프로젝트 설정과 배포 환경에서 아주 중요한 역할을 해요. 진짜 중요한 정보(비밀번호, API 키 등)는 클라이언트에 노출되지 않도록 환경 변수 이름에 `NEXT_PUBLIC_`을 붙이지 말고 서버 사이드에서만 다뤄야 한다는 것도 꼭 기억하세요!

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 지원되지 않는 API들

Edge Runtime을 사용할 때는 몇 가지 제약사항이 있어요. 

- Native Node.js API는 지원되지 않아요. 예를 들어, 파일 시스템을 읽거나 쓰는 작업은 불가능해요.
- node_modules는 사용 가능하지만, 꼭 ES Modules 형식이어야 하고, Native Node.js API를 사용하지 않는 패키지여야 해요.
- 직접 `require`를 호출하는 것도 안 돼요. 대신 ES Modules 방식을 사용해야 합니다.

그리고 아래의 JavaScript 언어 기능들은 비활성화되어 있어서 사용할 수 없어요: 

| 제한된 기능 목록 |
|------------------|
| (여기에 구체적인 기능 목록을 적어주세요, 원본 내용에 따라 추가) |

---

### 조금 더 알아보기!

Edge Runtime은 서버리스 환경 같은 경량화된 실행 환경에서 동작하도록 설계되었기 때문에, 기존 Node.js 환경에서 가능했던 기능들이 제한되는 경우가 많아요. 특히 파일 시스템 접근, 네이티브 바인딩 등은 보안 문제나 실행 환경 특성상 막혀있죠.

그래서 여러분이 외부 라이브러리를 사용할 때에는 꼭 ES Module 형식을 지원하는지, 그리고 네이티브 API를 호출하지는 않는지 꼼꼼히 체크하는 게 중요해요. 만약 기존에 `require`로 불러오던 라이브러리가 있다면, `import` 구문으로 대체해야 해요.

이러한 제한을 감안하고 코드를 작성하면, Edge Runtime에서 훨씬 더 안정적으로 실행할 수 있어요!

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

이번 글에서는 JavaScript에서 동적 코드 평가(dynamic code evaluation) 관련 API들과, Next.js에서 특정 파일에서 동적 코드를 허용하는 방법에 대해 알아볼게요.

### 자바스크립트에서 동적 코드 평가 API들

| API | 설명 |
| --- | --- |
| [`eval`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/eval) | 문자열로 표현된 JavaScript 코드를 실행해요. 정말 강력하지만, 보안과 성능 면에서 조심해야 해요. |
| [`new Function(evalString)`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Function) | 새 함수를 생성하는데, 함수 본문을 문자열로 받아요. eval과 비슷하지만, 실행 컨텍스트가 다르답니다. |
| [`WebAssembly.compile`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/WebAssembly/compile) | WebAssembly 모듈을 버퍼 형태의 소스에서 컴파일해요. 빠른 성능이 필요한 작업에 유용하죠. |
| [`WebAssembly.instantiate`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/WebAssembly/instantiate) | WebAssembly 모듈을 컴파일하고 바로 인스턴스화해요. |

이 API들은 주로 동적으로 코드를 생성하거나 실행할 때 사용하지만, 이렇게 동적으로 코드를 평가하는 부분은 트리쉐이킹(Tree-shaking) 과정에서 걸러지지 않는 경우가 있어서 문제가 될 수 있어요. 트리쉐이킹은 불필요한 코드를 제거해주는 최적화 기술인데, 동적 코드는 정적으로 분석하기 어려워서 걸러지지 않는 거죠.

---

### Next.js 미들웨어에서 동적 코드 평가 허용하기

가끔 프로젝트에 포함된 코드 중에 동적 코드 평가가 사용되는데, 그 부분이 실제로는 런타임 시에 호출되지 않아서 제거하기 힘들 때가 있어요. 이럴 때는 Next.js의 미들웨어 설정에서 특정 파일만 동적 코드를 허용하도록 설정해주는 방법이 있어요.

```js
export const config = {
  unstable_allowDynamic: [
    // 단일 파일 허용 예시
    '/lib/utilities.js',
    // 3rd party 모듈 내 특정 폴더 전체 허용 (glob 패턴)
    '**/node_modules/function-bind/**',
  ],
}
```

- `unstable_allowDynamic` 옵션은 glob 패턴을 사용해서 특정 파일이나 폴더 내 동적 코드 평가를 무시하게 해줘요.
- glob 패턴은 애플리케이션 루트 폴더를 기준으로 작성해주시면 됩니다.
- 이렇게 지정된 파일들은 동적 코드 평가가 포함되어 있더라도 Next.js가 문제 삼지 않고 넘어가게 돼요.

---

### 마치며

동적 코드 평가 자체는 매우 유용한 기능이지만, 보안, 성능, 그리고 번들 최적화 측면에서 복잡한 문제를 일으킬 수 있답니다. 그러니까 특별한 필요가 없다면 사용을 최소화하는 게 좋아요.

만약 외부 라이브러리를 쓸 때 동적 코드 평가가 포함되어 있어서 어쩔 수 없이 사용해야 한다면, 위 `unstable_allowDynamic` 옵션을 활용해 Next.js 빌드 시스템을 적절히 조정해보세요.

참고로, WebAssembly는 요즘 성능이 중요한 웹 애플리케이션에서 자주 쓰이니 한번 공부해보시는 것도 추천드려요. 다음에 WebAssembly에 대해서도 자세히 다뤄볼게요!

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

경고! 만약 이 문장들이 Microsoft Edge 브라우저에서 실행된다면, 에러가 발생하면서 프로그램이 중단될 수 있어요.

특히 Edge는 다른 브라우저에 비해 지원하는 자바스크립트 기능이나 API 호환성이 다를 때가 많아서 이런 문제들이 종종 발생합니다. 그래서 크로스 브라우저 호환성을 신경 쓸 때는 항상 Edge 같은 브라우저에서도 테스트를 하는 게 중요해요.

만약 Edge에서 문제가 발생한다면, 다음과 같은 방법들을 시도해볼 수 있어요:

- **폴리필(Polyfill) 사용**: Edge에서 지원하지 않는 기능을 다른 코드로 보완할 수 있어요.
- **조건문으로 브라우저 판별**: 실행 전에 현재 브라우저가 Edge인지 확인하고, 별도의 처리를 해줄 수 있어요.
- **최신 버전 업데이트 권장**: Edge도 점점 개선되고 있으니 최신 버전에서는 문제 없이 동작할 수도 있어요.

항상 코드 작성할 때는 주요 브라우저 모두에서 정상 동작하는지 확인하는 습관을 들이는 게 좋아요!