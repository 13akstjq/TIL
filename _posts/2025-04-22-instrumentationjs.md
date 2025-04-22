---
title: "instrumentation.js로 웹사이트 성능 측정 도구 만들기"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:38
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "instrumentation.js"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/instrumentation"
isUpdated: false
---


# instrumentation.js

instrumentation.js|ts 파일은 애플리케이션에 관찰 가능성(Observability) 도구를 통합하는 용도로 사용돼요. 이를 통해 애플리케이션의 성능과 동작을 추적할 수 있고, 실제 서비스 중에 발생하는 문제를 더 효과적으로 디버깅할 수 있답니다.

사용법도 간단해요! 프로젝트 루트 디렉토리에 파일을 두거나, 만약 src 폴더를 사용한다면 그 안에 넣으면 되죠.

## 파일에서 제공하는 주요 기능 (Exports)

(여기에 실제로 어떤 내용이 export 되는지 명시되어야 하는데요, 이어서 있으면 같이 정리해보겠습니다.)

---

### 조금 더 알아두면 좋은 점!

- 관찰 가능성 도구에는 여러 가지가 있는데, 예를 들어 로그 수집, 메트릭 수집, 트레이싱 등이 있어요. instrumentation.js를 통해 이런 도구들을 한 곳에 모아 효율적으로 관리할 수 있어요.
- 실제 운영 환경에서 문제가 발생했을 때, 미리 설정한 instrumentation 덕분에 원인을 빠르게 파악할 수 있으니 개발, 운영 모두에 큰 도움이 된답니다.
- TypeScript를 사용한다면 .ts 확장자를 사용해 타입 안정성을 더할 수도 있어요.

다음에는 instrumentation.js 안에 어떤 코드가 들어가는지, 구체적인 예시를 하나씩 살펴보도록 할게요!

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

### register (선택 사항)

Next.js 서버가 새로 시작될 때 한 번 호출되는 `register` 함수를 파일에서 export할 수 있어요. 이 함수는 async 함수여도 되고, 주로 초기 설정이나 라이브러리 등록 같은 작업에 사용되죠.

예를 들어, 아래처럼 OpenTelemetry를 연동하는 작업을 할 수 있어요:

```js
import { registerOTel } from '@vercel/otel'
 
export function register() {
  registerOTel('next-app')
}
```

위 코드에서 `registerOTel` 함수는 'next-app'이라는 이름으로 OpenTelemetry를 설정해주는 역할을 해요. 이 부분은 프로젝트에서 필요한 모니터링이나 트레이싱을 할 때 유용하답니다.

### onRequestError (선택 사항)

`onRequestError` 함수도 선택 사항인데요, 이 함수는 HTTP 요청 중 에러가 발생했을 때 실행되도록 만들어져 있어요. 만약 API 라우트나 서버 함수에서 에러 로그를 따로 남기거나 오류를 처리하고 싶을 때 이 함수를 정의해서 사용할 수 있답니다.

---

참고로, `register` 함수 같은 초기화 함수는 앱이 구동될 때 한 번만 실행되기 때문에 무거운 초기 작업이나 설정을 이곳에 모아두면 깔끔하게 관리할 수 있어요. Next.js를 활용할 때, 이런 선택적 함수들로 서버 사이드의 세밀한 로직을 다루는 팁을 꼭 알아두시면 좋아요!

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

Next.js에서 onRequestError라는 함수를 선택적으로 내보내서 서버 에러를 커스텀한 관측(Observability) 도구로 추적할 수 있어요.

- 만약 onRequestError 안에서 비동기 작업을 한다면 꼭 `await` 해줘야 해요. 이 함수는 Next.js 서버가 에러를 포착할 때 실행되거든요.
- 여기서 주의할 점! 에러 인스턴스가 실제로 던져진 원본 에러가 아닐 수도 있어요. 특히 Server Components 렌더링 도중 React가 처리한 에러라면 그럴 수 있는데, 이럴 때는 에러 객체의 `digest` 속성을 이용해서 실제 에러 타입을 확인할 수 있답니다.

아래 코드를 보면 실제로 onRequestError에서 에러 정보를 외부 서버로 POST 요청을 보내서 보고하는 간단한 예시를 보여주고 있어요:

```js
import { type Instrumentation } from 'next'
 
export const onRequestError: Instrumentation.onRequestError = async (
  err,
  request,
  context
) => {
  await fetch('https://.../report-error', {
    method: 'POST',
    body: JSON.stringify({
      message: err.message,
      request,
      context,
    }),
    headers: {
      'Content-Type': 'application/json',
    },
  })
}
```

### onRequestError 함수 파라미터

| 파라미터명  | 설명                                                   |
|-------------|--------------------------------------------------------|
| `err`       | 발생한 에러 객체입니다. React가 가공할 수도 있어요.            |
| `request`   | 현재 요청 정보가 담긴 객체입니다.                             |
| `context`   | 요청에 관련된 추가 컨텍스트 정보가 들어있습니다.                   |

이걸 활용하면 서버 에러를 커스텀 로그 서버나 Sentry, Datadog 같은 외부 모니터링 서비스에 손쉽게 연결할 수 있어요. 특히 여러 비동기 작업을 할 때 `await`를 빠뜨리지 않도록 주의해주세요! 안 그러면 에러 추적이 중간에 끊길 수 있거든요.

그리고 `digest` 프로퍼티를 써서 에러 타입을 확인하는 팁! React에서 처리된 에러는 내부적으론 변형될 수 있기 때문에, 원래 에러를 정확히 파악하려면 이걸 참고하면 좋아요.

이 기능을 잘 쓰면 서버 안정성 모니터링에 훨씬 도움 돼서, 문제 발생 시 빠르게 대응 가능하니 꼭 한번 적용해보시길 추천드립니다!

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

이번에는 `onRequestError` 함수에 대해 알아볼게요. 이 함수는 에러가 발생했을 때 호출되며, 세 가지 매개변수를 받아요: `error`, `request`, 그리고 `context`입니다.

```ts
export function onRequestError(
  error: { digest: string } & Error,
  request: {
    path: string; // 리소스 경로, 예: /blog?name=foo
    method: string; // 요청 메서드, 예: GET, POST 등
    headers: { [key: string]: string };
  },
  context: {
    routerKind: 'Pages Router' | 'App Router'; // 사용하는 라우터 종류
    routePath: string; // 라우트 파일 경로, 예: /app/blog/[dynamic]
    routeType: 'render' | 'route' | 'action' | 'middleware'; // 에러 발생 컨텍스트
    renderSource:
      | 'react-server-components'
      | 'react-server-components-payload'
      | 'server-rendering'; // 렌더링 소스
    revalidateReason: 'on-demand' | 'stale' | undefined; // 재검증 사유, undefined 는 일반 요청 의미
    renderType: 'dynamic' | 'dynamic-resume'; // 'dynamic-resume'은 PPR 사용 시
  }
): void | Promise<void>;
```

| 파라미터 | 설명                        |
|----------|-----------------------------|
| `error`  | 발생한 실제 에러, 고유 ID인 `digest` 포함 |
| `request`| 에러와 관련된 요청 정보      |
| `context`| 에러가 발생한 환경 정보를 담고 있음 |

### 파라미터 자세히 살펴보기

- `error`: 항상 Error 타입이며, 여기에 `digest`라는 고유 ID가 있어 에러를 추적하기 좋습니다. 예를 들어, 같은 에러가 여러 번 발생할 경우 이 ID로 쉽게 분류할 수 있죠.
- `request`: 실제 요청 정보입니다. 어떤 경로(`path`)로, 어떤 메서드(`method`)가 쓰였고, 헤더에는 어떤 정보가 담겨있는지 확인할 수 있어요.
- `context`: 어느 라우터에서 발생했는지(`routerKind`), 어떤 라우트 파일에서 발생했는지(`routePath`), 그리고 에러가 일어난 시점(`routeType`)을 알려줍니다. 렌더링 소스(`renderSource`)나 재검증 사유(`revalidateReason`)도 담겨있어, 상황별 대응에 유용해요.

### 추가 팁: 왜 이렇게 복잡한 정보가 필요할까?

에러 핸들링은 단순히 에러 메시지를 로그에 남기는 걸 넘어서, 상황에 맞게 대처하거나 사용자에게 정확한 안내를 해주는 데 중요해요. 예를 들어 `revalidateReason`이 `'stale'`이면 캐시된 데이터를 다시 불러오는 중 문제가 생긴 거고, 이를 활용하면 재시도 로직이나 사용자 안내 메시지를 다르게 할 수 있겠죠.

최근 Next.js 같은 프레임워크가 여러 라우터 시스템과 다양한 렌더링 방식을 지원하면서, 이런 세분화된 정보가 꼭 필요해졌답니다.

---

다음에 이어서 `### Specifying the runtime` 부분도 한 번 살펴볼게요! 혹시 여기까지 궁금한 점 있으면 댓글로 물어봐 주세요 :)

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

instrumentation.js 파일은 Node.js와 Edge 런타임 둘 다에서 동작하는데요, 만약 특정 런타임을 대상으로 하고 싶다면 process.env.NEXT_RUNTIME 값을 활용하면 됩니다.

```js
export function register() {
  if (process.env.NEXT_RUNTIME === 'edge') {
    return require('./register.edge')
  } else {
    return require('./register.node')
  }
}

export function onRequestError() {
  if (process.env.NEXT_RUNTIME === 'edge') {
    return require('./on-request-error.edge')
  } else {
    return require('./on-request-error.node')
  }
}
```

여기서 중요한 점은 process.env.NEXT_RUNTIME이 'edge'인지 아닌지에 따라서 각각 다른 모듈을 require해서 사용하는 방식이에요. 이렇게 하면 동일한 코드베이스에서 런타임 환경에 맞게 동작 방식을 유연하게 바꿀 수 있어서 정말 편리합니다.

---

## 버전 히스토리

| Version   | Changes                                    |
|-----------|--------------------------------------------|
| v15.0.0   | `onRequestError` 추가, `instrumentation` 안정화 |
| v14.0.4   | `instrumentation`에 Turbopack 지원 추가        |
| v13.2.0   | `instrumentation` 실험적 기능으로 처음 소개      |

버전 히스토리를 보면 알 수 있듯, instrumentation 기능이 처음에는 실험적으로 도입됐다가 이후 안정화 되고, Turbopack 같은 최신 툴도 지원이 추가되면서 점점 완성도 있게 발전해온 걸 확인할 수 있어요.

---

개발할 때 runtime 환경 구분이 필요한 경우에는 이렇게 환경변수를 통해 동적으로 모듈을 분기하는 패턴을 자주 쓰게 되는데, Next.js 같은 프레임워크에서 공식적으로 지원하니까 훨씬 편하게 적용할 수 있는 것 같더라고요. 혹시 특정 런타임에서만 동작하는 코드를 작성하려고 하면 참고해보시면 좋을 것 같아요!