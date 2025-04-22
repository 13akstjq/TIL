---
title: "Next.js 15 라우트 세그먼트 설정 방법과 활용팁"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:45
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Route Segment Config"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config"
isUpdated: false
---


# Route Segment Config

> 이 페이지에 소개된 옵션들은 `dynamicIO` 플래그가 켜져 있으면 비활성화되고, 앞으로는 없어질 예정이에요.

Route Segment 옵션은 Page, Layout, 또는 Route Handler의 동작 방식을 직접 제어하고 싶을 때 유용해요. 이 옵션들은 아래 변수들을 export 해서 설정할 수 있습니다.

| 옵션             | 타입                                                                                  | 기본값           |
|-----------------|-------------------------------------------------------------------------------------|-----------------|
| [experimental_ppr](#experimental_ppr) | `boolean`                                                                             |                 |
| [dynamic](#dynamic)                   | `'auto'` \| `'force-dynamic'` \| `'error'` \| `'force-static'`                        | `'auto'`        |
| [dynamicParams](#dynamicparams)      | `boolean`                                                                             | `true`          |
| [revalidate](#revalidate)             | `false` \| `0` \| `number`                                                            | `false`         |
| [fetchCache](#fetchcache)             | `'auto'` \| `'default-cache'` \| `'only-cache'` \| `'force-cache'` \| `'force-no-store'` \| `'default-no-store'` \| `'only-no-store'` | `'auto'`        |
| [runtime](#runtime)                   | `'nodejs'` \| `'edge'`                                                                | `'nodejs'`      |
| [preferredRegion](#preferredregion)  | `'auto'` \| `'global'` \| `'home'` \| `string` \| `string[]`                          | `'auto'`        |
| [maxDuration](#maxduration)           | `number`                                                                             | 배포 플랫폼에 따라 설정 |

---

### 한눈에 보기

- **experimental_ppr**: 실험적인 부분들에 대한 설정인데, 자세한 내용은 공식 문서를 참고해야 해요.
- **dynamic**: 페이지를 동적으로 처리할지, 정적으로 처리할지 결정할 수 있어요. 기본값은 `'auto'`라 알아서 최적의 방식을 선택합니다.
- **dynamicParams**: 동적 경로 파라미터 사용 여부를 설정해요. 기본적으로 `true`라 동적 파라미터를 허용합니다.
- **revalidate**: ISR(Incremental Static Regeneration)을 사용할 때 재빌드 주기를 설정합니다. `false`는 재빌드를 하지 않는다는 의미고, 숫자를 지정하면 해당 초마다 재빌드 돼요.
- **fetchCache**: 서버 측 데이터 fetching 시 어떤 캐싱 전략을 사용할지 정하는 옵션입니다.
- **runtime**: `nodejs` 또는 `edge` 중 실행 환경을 선택할 수 있어요. 기본은 `nodejs`.
- **preferredRegion**: 배포 리전을 지정할 수 있는 옵션입니다. 글로벌 서비스를 위한 `'global'`이나 특정 리전 이름, 혹은 배열로 여러 리전을 설정할 수 있죠.
- **maxDuration**: 함수 실행 최장 시간을 의미하는데, 보통 배포 환경에 따라 제한됩니다.

---

### 그리고 살짝 덧붙이자면...

이런 Route Segment 설정을 통해서 Next.js 같은 프레임워크에서 페이지나 API 경로의 동작 방식을 세밀하게 조정할 수 있어요. 예를 들어, `dynamic: 'force-dynamic'`으로 지정하면 요청마다 항상 새롭게 렌더링해서 가장 최신 데이터를 보여주고, `revalidate` 시간을 설정해두면 효율적인 캐싱을 하면서도 일정 주기로 콘텐츠가 갱신되게 할 수 있죠.

또한, `runtime`을 `edge`로 설정하면 전 세계 엣지 서버에서 빠르게 처리가 가능해서 속도 향상과 지연 감소에 도움 될 수 있습니다. 다만 엣지 런타임 환경 제약 조건이 있으니, 사용 시 주의하세요.

이렇게 구성해두면 사용자 경험을 더 좋게 만들어주고, 서버 리소스를 효과적으로 사용할 수 있다는 장점이 있습니다. 다음에는 각 옵션별 좀 더 구체적인 활용법도 소개해볼게요!

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

## 옵션

### experimental_ppr

Partial Prerendering, 줄여서 PPR을 레이아웃이나 페이지에 적용할 수 있는 옵션이에요. 이걸 활성화하면 페이지나 특정 레이아웃이 부분적으로 미리 렌더링 되어서, 사용자가 페이지를 더 빨리 볼 수 있도록 도와줘요.

```js
export const experimental_ppr = true
// true | false
```

참고로, 이 옵션은 아직 실험적인 기능이라 가끔 예상치 못한 동작이 있을 수도 있어요. 그래서 프로젝트에 바로 적용하기보다는 테스트 환경에서 먼저 써보고, 잘 맞는지 확인하는 걸 추천드려요!

추가로 Partial Prerendering은 페이지의 일부만 미리 만들어두고 나머지는 필요할 때 로드하는 방식이기 때문에, 데이터가 자주 변하는 부분이나 사용자 맞춤형 콘텐츠에서 유용할 수 있어요. 만약에 페이지가 크고 렌더링 시간이 길다면, PPR을 활용해서 초기 로딩 속도를 확실히 개선할 수 있답니다.

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

### dynamic

레이아웃이나 페이지의 동작 방식을 완전 정적(static) 또는 완전 동적(dynamic)으로 바꿀 수 있는 옵션이에요.

```js
export const dynamic = 'auto'
// 사용할 수 있는 값: 'auto' | 'force-dynamic' | 'error' | 'force-static'
```

여기서 각 값의 의미는 다음과 같아요:

- `'auto'`: 기본값이에요. 상황에 따라 동적 또는 정적으로 처리해요.
- `'force-dynamic'`: 무조건 동적으로 렌더링해요.
- `'force-static'`: 무조건 정적으로 렌더링해요.
- `'error'`: 동적 동작이 필요할 때 에러를 발생시켜요.

> 참고로 알면 좋은 점: Next.js의 최신 app 디렉토리 구조에서는 페이지 단위에서 모두 다 동적이거나 모두 다 정적인 getServerSideProps, getStaticProps 대신, fetch 요청 단위에서 좀 더 세밀하게 캐싱을 조절하는 방식을 선호해요. 즉, 이전의 방식보다 더 유연해졌죠. 그런데 기존의 페이지 단위 동작 모델로 쉽게 돌아가고 싶은 경우, 이 `dynamic` 옵션을 활용하면 간편하게 이전 모델을 유지할 수 있어서 마이그레이션할 때도 편리해요.

---

추가로 덧붙이자면, 실제로는 fetch 함수에서 `cache` 옵션을 주는 쪽이 더 권장됩니다. 예를 들어:

```js
const res = await fetch('/api/data', { cache: 'no-store' }) // 항상 최신 데이터를 받아오도록 강제
```

이런 식으로 개별 fetch 요청마다 캐싱 동작을 세부 조절할 수 있어요. `dynamic` 옵션은 주로 전체 페이지 단위의 기본 동작을 결정할 때 사용한다고 생각하면 됩니다. 

즉, Next.js의 최신 앱 구조는 “모든 것을 다 세부적으로 제어하는 시대”라고 이해하면 좋겠네요!

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

`dynamic` 옵션에는 총 네 가지가 있어요. 각 옵션이 어떤 차이를 만드는지 쉽게 정리해볼게요!

| 옵션           | 설명                                                                                                  | 추가 팁                                                                                          |
|---------------|-----------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| `auto` (기본값) | 가능한 한 많이 캐시하지만, 동적으로 동작하고 싶은 컴포넌트는 이를 선택할 수 있도록 허용합니다.                                    | 기본값 그대로 쓰면 딱히 설정하지 않아도 자동으로 적절히 작동해요.                                      |
| `force-dynamic` | 무조건 동적 렌더링을 합니다. 즉, 각 요청마다 매번 페이지가 새로 렌더링됨.                                       | 내부적으로 모든 fetch() 요청은 `'cache: no-store', next: { revalidate: 0 }` 설정이 되고, `fetchCache = force-no-store` 로 강제되죠. |
| `error`        | 정적 렌더링만 강제하고, 만약 Dynamic API나 캐시되지 않는 데이터를 쓰면 에러 발생!                                    | pages 디렉토리의 `getStaticProps()`와 비슷해요. fetch()는 `'cache: force-cache'`를 사용하고, `fetchCache = only-cache`, `dynamicParams = false`로 설정됩니다. 동적 파라미터가 필요하면 수동으로 `dynamicParams = true`로 바꿔야 해요.        |
| `force-static` | 정적 렌더링 강제 + cookies, headers(), useSearchParams() 호출 시 빈값만 반환하게 만듭니다.                          | 정말 정적으로만 작동해야 하는 상황에 쓸 수 있어요.                                                    |

---

### 참고! 
`getServerSideProps`, `getStaticProps`에서 `dynamic: 'force-dynamic'`이나 `dynamic: 'error'`로 마이그레이션하는 방법은 [업그레이드 가이드](https://nextjs.org/docs/pages/building-your-application/data-fetching/upgrading)에서 자세히 설명하고 있으니 참고하세요.

---

### dynamicParams란?
이건 `generateStaticParams`로 미리 생성하지 않은 동적 경로(`/posts/[id]` 같은)가 방문됐을 때 어떻게 처리할지 결정할 수 있는 옵션이에요. 예를 들면 동적 경로가 너무 많을 때 미리 다 생성할 수 없으니, 방문 시점에 동적으로 처리할지, 아니면 에러를 표시할지 선택할 수 있습니다.

---

이렇게 보면 `dynamic` 옵션을 상황과 필요에 맞게 조절하는 게 핵심이라는 걸 알 수 있죠. 예를 들어, 자주 바뀌는 데이터를 보여주는 페이지라면 `force-dynamic`이 유리하고, 가능하면 빌드 타임에 다 끝내고 싶으면 `error` 혹은 `force-static`을 쓰는 식이에요.

실제로 프로젝트에 적용할 때는 데이터 특성과 사용자 경험을 잘 고려해서 가장 적합한 설정을 찾아 보세요!

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

```js
export const dynamicParams = true // true | false,
```

- true (기본값): `generateStaticParams`에 포함되지 않은 동적 세그먼트는 요청 시점에 생성돼요.
- false: `generateStaticParams`에 없는 동적 세그먼트에 접근하면 404 페이지가 반환됩니다.

> 알아두면 좋은 점:  
이 옵션은 pages 디렉터리에서 사용하던 `getStaticPaths`의 `fallback: true | false | blocking` 옵션을 대체해요.  
만약 모든 경로를 처음부터 정적으로 렌더링하고 싶다면, `generateStaticParams`에서 빈 배열을 반환하거나 `export const dynamic = 'force-static'`을 사용해야 합니다.  
`dynamicParams = true`인 경우, 해당 세그먼트는 스트리밍 서버 렌더링(Streaming Server Rendering)을 사용해요.  
그리고 `dynamic = 'error'`나 `dynamic = 'force-static'`을 쓰면 `dynamicParams`의 기본값이 false로 바뀝니다.

---

### revalidate  

*여기서 부터는 `revalidate` 옵션에 대해 살펴볼게요.*  

Next.js의 `revalidate` 옵션은 ISR(Incremental Static Regeneration) 기능을 제어하는 데 사용돼요.  
간단히 말해서, 특정 페이지를 정적으로 렌더링한 뒤, 일정 시간이 지나면 백그라운드에서 다시 빌드해서 최신 데이터를 반영할 수 있게 해줍니다.

```js
export const revalidate = 10; // 10초마다 페이지 재생성
```

- 숫자 (초 단위): 페이지가 이 값만큼의 시간이 지나면 재생성됩니다.  
- `false` 또는 `0`: ISR 비활성화, 빌드 시 한 번만 생성됨.

재생성 시점에 방문한 사용자는 기존 페이지를 보고, 백그라운드에서 새 페이지가 만들어지니까 사용자 경험에 끊김이 적어요.

---

### 간단 요약

| 옵션               | 설명                                                |
|--------------------|---------------------------------------------------|
| `dynamicParams`     | 동적 세그먼트가 동적으로 생성되는지 여부 설정          |
| `revalidate`        | ISR 주기 설정, 몇 초마다 페이지를 다시 생성할지 결정     |

---

### 추가 팁!

- **`dynamicParams = true`와 ISR 함께 쓰기**  
   동적 경로가 많거나 데이터가 자주 바뀌는 경우에 유용해요. 첫 요청 때만 페이지를 생성하고, 나중에 ISR로 업데이트할 수 있거든요.

- **빈 배열 반환시 모든 경로 미리 생성**  
   미리 생성할 경로를 다 알 수 있으면 `generateStaticParams`에서 배열을 반환해 정적으로 빠른 페이지를 만들 수 있어요.  

- **`dynamic = 'force-static'` 사용**  
   앱 전체를 정적으로 만들고 싶을 땐 이 값을 써서 SSR이나 동적 params 없이 순수 정적 빌드만 하게 설정해보세요.

여러분 프로젝트에 맞게 이런 설정들을 적절히 활용하면, 효율적인 빌드와 빠른 퍼포먼스를 동시에 잡을 수 있답니다!  
추가로 Next.js 공식 문서도 자주 확인하면서 최신 기능을 놓치지 않는 게 중요해요~

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

레이아웃이나 페이지에 기본 revalidation(재검증) 시간을 설정할 수 있어요. 여기서 중요한 점은 이 설정이 개별 fetch 요청에서 설정한 revalidate 값을 덮어쓰지 않는다는 거예요.

```js
export const revalidate = false
// false | 0 | number
```

| 값 | 의미 |
|---|---|
| false (기본값) | 기본적으로 `force-cache` 옵션이 붙은 fetch 요청이나 Dynamic API 사용 이전에 발견된 요청들은 캐싱합니다. 사실상 재검증 기간이 무한대(`revalidate: Infinity`)인 것과 같아서, 리소스를 무한정 캐시하겠다는 의미예요. 그래도 개별 fetch 요청에서 `cache: no-store` 나 `revalidate: 0`을 써서 캐시를 회피하거나, 경로의 기본값보다 짧은 재검증 시간을 가진 양수로 설정해 재검증 빈도를 높이는 건 가능합니다. |
| 0 | 이 옵션을 설정하면 Dynamic API를 사용하지 않더라도 레이아웃이나 페이지가 항상 동적으로 렌더링됩니다. 이 경우 캐시 옵션을 명시하지 않은 fetch 요청은 기본적으로 `no-store`로 동작하지만, `force-cache` 옵션을 준 요청이나 양의 재검증 시간을 가진 요청은 그대로 유지됩니다. |
| number (초 단위) | 레이아웃이나 페이지의 기본 재검증 주기를 초 단위 숫자로 설정합니다. 예를 들어 `revalidate = 600`이면 10분마다 재검증합니다. |

---

### 참고할 점
- `revalidate` 값은 정적으로 분석 가능해야 해요. 예를 들어 `revalidate = 600`은 유효하지만, `revalidate = 60 * 10` 같이 계산식은 안 됩니다. 즉, 값을 변수나 계산식이 아닌 숫자 리터럴로 직접 입력해야 하는 점을 기억하세요.
- `runtime = 'edge'`를 사용할 때는 `revalidate` 값이 적용되지 않습니다.
- 개발 모드(Development)에서는 모든 페이지가 항상 on-demand로 렌더링되며 캐싱되지 않아요. 덕분에 코드를 바로바로 수정 확인할 수 있고, 재검증 시간을 기다릴 필요가 없답니다.

---

### 추가로 알아두면 좋은 팁!
`revalidate`를 적절히 사용하면 페이지의 최신성을 유지하면서도 캐싱을 통해 빠른 응답 속도를 얻을 수 있어요. 예를 들어 자주 변경되지 않는 블로그 글 같은 경우에는 긴 재검증 시간(`false` 또는 큰 숫자)을 줘서 트래픽 부담을 줄이고, 자주 갱신이 필요한 데이터엔 짧은 재검증 시간을 설정하는 식이죠.

기본 설정을 잘 활용하면 서버 부하를 줄이면서 사용자에게 더 빠른 웹 경험을 제공할 수 있으니, 상황에 맞는 값을 고민해보세요!

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

#### 재검증 주기 (Revalidation Frequency)

- 하나의 경로(route)에 속한 레이아웃(layout)과 페이지(page)들 각각이 재검증(revalidate) 시간을 갖는데, 이 중 가장 짧은 재검증 주기가 그 경로 전체의 재검증 주기가 됩니다. 즉, 자식 페이지들도 부모 레이아웃만큼 자주 재검증되도록 보장하는 거죠.
- 개별 fetch 요청(fetch requests)에서는 경로의 기본 재검증 주기보다 더 짧은 재검증 주기를 설정할 수 있어요. 이를 통해 특정 경로나 상황에 맞게 동적으로 재검증 빈도를 높일 수 있습니다.

예를 들어, 뉴스 기사 페이지는 자주 업데이트돼야 할 수도 있으니까 이 경우엔 재검증 주기를 더 짧게 설정할 수 있겠죠.

---

### fetchCache

`fetchCache`는 클라이언트에서 데이터를 호출할 때 결과를 캐시하는 방식을 다루는 설정이에요. 원격 API를 호출할 때마다 서버에 부담주지 않으면서도, 최신 데이터를 적절히 유지하기 위한 중요한 포인트랍니다. cache-control 헤더 같이 HTTP 표준 캐싱 정책과도 연동되니까, 이 부분을 적절히 활용하면 효율적인 데이터 요청과 성능 향상에 큰 도움이 됩니다.

---

### runtime

`runtime`은 코드를 실행하는 환경을 의미해요. Next.js 같은 프레임워크에서 `runtime`은 서버 사이드에서 동작하는지, 클라이언트 사이드에서 동작하는지에 따라 달라집니다. 서버에선 Node.js 환경에서 구동되고, 클라이언트에선 브라우저 환경이죠.

실제로 `runtime`이 중요해지는 이유는, 일부 기능들은 서버에서만, 또는 클라이언트에서만 동작해야 하기 때문입니다. 예를 들어, 민감한 API 키로 데이터를 불러오는 작업은 서버에서만 실행하도록 하고, 사용자 상호작용에 따른 UI 업데이트는 클라이언트에서 처리하는 식이죠.

---

이렇게 `revalidation frequency`부터 `fetchCache`, `runtime`까지 조합해서 쓰면, 데이터 신선도 유지와 성능 최적화를 모두 잡을 수 있어요. Next.js 같은 최신 프레임워크를 사용할 때 이 부분들을 잘 이해하고 활용하는 게 개발 생산성 향상에 큰 도움이 되니 참고하시길 바랍니다!

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

애플리케이션 렌더링에는 Node.js 런타임을 사용하는 걸 추천드리고, 미들웨어(Middleware)에는 Edge 런타임만 지원되니 이 점 참고해 주세요.

```js
export const runtime = 'nodejs'
// 'nodejs' | 'edge'
```

| 런타임 종류 | 설명                   |
| --------- | -------------------- |
| nodejs    | 기본값, 앱 렌더링용        |
| edge      | 미들웨어 전용, 빠른 응답에 적합 |

Node.js 런타임은 널리 사용되는 서버 환경으로, 풍부한 라이브러리와 안정성 덕분에 앱 렌더링에 최적입니다. 반면, Edge 런타임은 전세계적으로 분산된 서버에서 실행돼 지연시간을 최소화하기 때문에 미들웨어 같은 경량 작업에 딱이죠.

더 자세한 런타임 차이점과 사용법은 공식 문서를 참고하시면 도움이 될 거예요!

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

### preferredRegion

```js
export const preferredRegion = 'auto'
// 'auto' | 'global' | 'home' | ['iad1', 'sfo1']
```

`preferredRegion` 옵션은 배포하는 플랫폼에 따라 지원 여부와 사용 가능한 지역이 달라진다는 점, 꼭 기억하세요!

> 참고할 점  
만약 `preferredRegion`을 따로 지정하지 않으면, 가장 가까운 상위 레이아웃(parent layout)의 설정을 상속받습니다.  
그리고 최상위 레이아웃(root layout)은 기본값으로 모든 지역(all regions)을 대상으로 합니다.

---

이 기능은 여러 서버 지역(Region) 중 어디서 코드를 실행할지 선택할 때 아주 유용해요. 예를 들어, 사용자가 미국 동부에 많다면 'iad1' 같은 특정 리전을 지정해 그쪽에서 서비스하도록 설정할 수 있죠. 'auto'로 설정하면 Next.js가 가장 적합한 지역을 자동으로 선택해주니 편리합니다.

여기서 'global'은 전 세계 여러 지역에 걸쳐 배포하는 경우이고, 'home'은 특정 홈 지역을 지칭할 때 사용됩니다. 또한 배열 형태로 여러 리전을 직접 지정해 여러 곳에서 동시에 실행되도록 할 수도 있답니다.

이처럼 `preferredRegion`을 잘 활용하면 사용자 경험을 크게 향상시킬 수 있으니, 필요에 맞게 설정하는 걸 추천드립니다!

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

### maxDuration

Next.js에서는 기본적으로 서버 사이드 로직(페이지 렌더링이나 API 처리 등)의 실행 시간을 제한하지 않아요. 그런데 배포 플랫폼에서 Next.js 빌드 결과에 포함된 `maxDuration` 값을 활용해 실행 시간 제한을 걸 수 있답니다. 예를 들어, Vercel 같은 곳에서는 이 값을 참고해서 서버 로직이 너무 오래 걸리지 않도록 관리할 수 있죠.

> **참고**: `maxDuration` 설정은 Next.js 13.4.10 이상 버전에서만 사용할 수 있어요.

```js
export const maxDuration = 5
```

위처럼 `maxDuration`을 5로 설정하면, 해당 서버 로직이 5초 이상 실행되지 않도록 제한할 것을 제안하게 되는 거예요.

---

### 보너스 팁!

- **왜 실행 시간 제한이 중요할까?**  
  서버 쪽 로직이 무한루프에 빠지거나 너무 오래 걸리면, 서버 자원을 낭비하고 사용자 경험도 악화돼요. 특히 서버리스 환경에서는 실행 시간이 길어질수록 비용이 더 발생할 수 있어서, 이런 제한을 두는 게 효율적이랍니다.

- **`maxDuration`은 어디에 적용될까?**  
  서버 컴포넌트의 렌더링 로직이나 API Route 핸들러 등, 서버에서 실행되는 모든 로직에 적용할 수 있어요.

- **Next.js 버전 확인법**  
  프로젝트에서 현재 사용하는 Next.js 버전을 확인하려면 `package.json` 파일에서 `next` 항목을 보면 되니 참고하세요!

---

혹시 여러분의 프로젝트에서 실행 시간이 너무 긴 서버 코드를 발견했다면, `maxDuration` 설정과 함께 코드를 최적화하는 걸 추천드려요. 작은 설정과 최적화가 서버 환경 전반의 효율성을 크게 올려준답니다!

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

> 알아두면 좋은 점:  
만약 Server Actions를 사용할 경우, 페이지 단위에서 maxDuration을 설정해서 해당 페이지 내 모든 Server Actions의 기본 타임아웃 시간을 변경할 수 있어요.

### generateStaticParams

generateStaticParams 함수는 동적 라우트 세그먼트와 함께 사용할 수 있는데요, 이 함수를 이용하면 빌드 시점에 정적으로 생성할 라우트 세그먼트 파라미터 리스트를 정의할 수 있어요.  
즉, 요청할 때마다(dynamic하게) 생성하는 것이 아니라, 미리(static하게) 만들어 두는 거죠.

이렇게 하면 빌드 시 미리 생성된 페이지 덕분에 사용자 경험이 더 빨라지고 서버 부하도 줄일 수 있어요.

조금 더 자세한 내용은 공식 API 문서를 참고해보세요!

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

## 버전 히스토리

| Version         | 설명                                                                                                         |
|-----------------|------------------------------------------------------------------------------------------------------------|
| `v15.0.0-RC`    | `export const runtime = "experimental-edge"`가 deprecated 되었습니다. 이에 따른 코드 변경을 쉽게 해주는 [codemod](https://nextjs.org/docs/app/guides/upgrading/codemods#transform-app-router-route-segment-config-runtime-value-from-experimental-edge-to-edge)가 제공됩니다. |

여기서 codemod란, 코드 변환을 자동으로 해주는 도구를 의미해요. 기존에 `experimental-edge`로 설정해둔 부분을 새로운 방식인 `edge`로 한 번에 바꿔주니까, 직접 손으로 일일이 수정할 필요가 없어서 편하답니다. 혹시 Next.js로 프로젝트를 진행 중이라면 이 부분 꼭 참고하세요!