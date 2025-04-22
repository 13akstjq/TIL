---
title: "Next.js 15 프로덕션 최적화 완벽 가이드"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:36
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to optimize your Next.js application for production"
link: "https://nextjs.org/docs/app/guides/production-checklist"
isUpdated: false
---


# Next.js 애플리케이션을 프로덕션에 최적화하는 방법

Next.js 애플리케이션을 프로덕션 환경에 배포하기 전에 사용자 경험, 성능, 그리고 보안을 위해 꼭 고려해야 할 최적화 방법과 좋은 패턴들이 있어요.

이 글에서는 애플리케이션을 만들 때, 배포 전에, 그리고 배포 후에 참고할 수 있는 실전 팁들과 Next.js가 자동으로 제공하는 최적화 기능들을 함께 소개할게요. 개발하면서 한 번쯤 꼭 확인해보면 좋아요!

## 자동 최적화 기능들

Next.js는 기본적으로 많은 부분을 자동으로 최적화해줘요. 그래서 우리가 따로 신경 쓰지 않아도 괜찮은 부분들이 있죠. 예를 들어:

| 최적화 요소            | 설명                                                         |
|---------------------|------------------------------------------------------------|
| 코드 분할(Code Splitting)    | 페이지 단위로 필요한 코드만 딱 불러와서 초기 로딩 속도를 빠르게 해줘요.              |
| 정적 사이트 생성(Static Generation) | 빌드 시점에 HTML 파일로 미리 생성해서 서버 부담을 줄이고, 빠른 응답을 가능하게 해줍니다. |
| 이미지 자동 최적화         | Next.js에서 제공하는 `<Image>` 컴포넌트로 이미지를 자동으로 최적화해서 로딩 속도를 개선합니다. |
| 트리 쉐이킹(Tree Shaking)      | 사용하지 않는 코드를 제거해서 번들 용량을 줄여줍니다.                                  |
| 프로덕션 빌드에서 압축       | JS와 CSS 파일을 압축하고 난독화해서 크기를 최소화합니다.                           |

이런 자동화 덕분에 개발자는 기본적인 부분에 신경 쓰지 않고도 빠른 앱을 만들 수 있지만, 더 세밀한 최적화가 가능하다는 것도 잊지 마세요.

다음에는 우리가 직접 적용할 수 있는 최적화 방법들을 살펴볼게요!

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

Next.js에서 기본으로 제공하는 최적화 기능들을 알려드릴게요! 별도의 설정 없이도 적용되니, 그냥 편하게 개발만 하시면 됩니다.

- **서버 컴포넌트(Server Components)**: Next.js는 기본적으로 서버 컴포넌트를 사용합니다. 서버 컴포넌트는 서버에서만 실행되고, 클라이언트에는 자바스크립트를 전혀 보내지 않아요. 덕분에 클라이언트 사이드 자바스크립트 용량이 줄어들죠. 그리고 필요에 따라 인터랙티브한 부분만 클라이언트 컴포넌트(Client Components)로 만들어 사용하면 됩니다.
- **코드 분할(Code-splitting)**: 서버 컴포넌트 덕분에 라우트 세그먼트별로 자동 코드 분할이 되어서, 필요한 부분만 내려받게 돼요. 여기에 더해서, 클라이언트 컴포넌트나 서드파티 라이브러리도 지연 로딩(lazy loading)하면 더 효율적입니다.
- **프리패칭(Prefetching)**: 사용자가 어떤 링크를 화면에서 볼 때, Next.js가 그 라우트를 미리 백그라운드에서 받아오는데요, 덕분에 새로운 페이지로의 이동이 거의 순식간에 이루어진답니다. 상황에 따라 프리패칭을 끌 수도 있어요.
- **정적 렌더링(Static Rendering)**: Next.js는 빌드 시점에 서버와 클라이언트 컴포넌트를 정적으로 렌더링해 결과를 캐싱하니까, 앱 성능이 좋아집니다. 필요하면 특정 경로에 대해 동적 렌더링(Dynamic Rendering)도 선택 가능합니다.
- **캐싱(Caching)**: 서버 데이터 요청, 정적 렌더 결과, 정적 자산 등 여러 부분을 캐싱해서 서버나 DB, 백엔드에 가는 네트워크 요청 수를 줄여줍니다. 적절하다면 캐싱을 끄는 것도 가능해요.

---

이 최적화 기본값들은 앱 성능을 향상시키고, 네트워크 요청 때 쓰는 데이터 양과 비용을 줄여주는 효과가 있어요. 

---

## 개발 중에는?

다음 답변에서 개발 환경에서의 최적화 적용이나 주의할 점을 자세히 알려드릴게요! 혹시 Next.js 개발 중에 느린 점이나 의문 있으면 언제든 질문 주세요.

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

애플리케이션을 만들다 보면 성능과 사용자 경험을 최적화하는 게 중요하죠. 그래서 다음 기능들을 사용하는 걸 추천해요!

### 라우팅과 렌더링

- **레이아웃(Layouts)**: 여러 페이지에서 공통 UI를 공유할 수 있어서, 페이지 이동 시 일부만 새로고침하는 부분 렌더링이 가능해요. 덕분에 사용자 경험이 훨씬 매끄럽답니다.
- **`Link` 컴포넌트**: 클라이언트 사이드 내비게이션할 때 꼭 써주세요. 미리 데이터를 가져오는(prefetch) 기능도 있어서 페이지 전환이 더 빨라져요.
- **에러 처리(Error Handling)**: 404나 그 외 예기치 못한 오류도 사용자에게 부드럽게 알리려면, 커스텀 에러 페이지를 만들어 놓으세요. 프로덕션 환경에서 꼭 필요하답니다.
- **컴포지션 패턴(Composition Patterns)**: 서버 컴포넌트와 클라이언트 컴포넌트를 어떻게 구성할지 정해진 패턴을 따르세요. 특히 `"use client"` 지시문을 어디에 붙이는지 신경 써야 클라이언트 번들 용량을 불필요하게 늘리지 않아요.
- **동적 API(Dynamic APIs)**: 쿠키나 `searchParams` 같은 동적 API를 쓰면 그 경로 전체가 동적 렌더링 모드로 전환돼요. 만약 루트 레이아웃에서 쓰면 앱 전체가 그렇게 되니 주의가 필요해요. 꼭 필요한 부분에만 사용하고, 적절하면 `Suspense` 경계로 감싸서 성능 저하를 막으세요.

> 참고: 현재 실험 중인 Partial Prerendering 기능을 사용하면, 라우트의 일부만 동적으로 처리하면서도 전체를 동적 렌더링으로 전환하지 않아도 돼요. 앞으로 더 편리해질 예정이니 기대해도 좋아요!

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

### 데이터 페칭과 캐싱에 대해 알아볼까요?

웹 앱 개발할 때 데이터 가져오는 방식과 캐싱 전략이 굉장히 중요하죠. 여기에 몇 가지 팁을 나눠드릴게요.

- **서버 컴포넌트(Server Components) 활용하기**  
  데이터는 서버에서 가져오는 게 성능상 유리해요. 서버 컴포넌트를 이용하면 클라이언트로 보내기 전에 데이터를 미리 받아서 렌더링할 수 있어요.

- **라우트 핸들러(Route Handlers) 사용법**  
  클라이언트 컴포넌트에서 백엔드 API에 접근할 때 라우트 핸들러를 사용하세요. 그런데 서버 컴포넌트에서 라우트 핸들러를 호출하는 건 피하는 게 좋아요. 왜냐하면 그러면 서버가 다시 서버에 요청하는 꼴이라 불필요한 추가 요청이 생기거든요.

- **스트리밍(Streaming)으로 사용자 경험 개선하기**  
  데이터가 완전히 로드될 때까지 기다리지 말고, 로딩 UI와 React Suspense를 활용해 UI를 점진적으로 보여주면 사용자 경험이 훨씬 좋아져요. 페이지 전체가 멈추지 않거든요.

- **병렬 데이터 페칭(Parallel Data Fetching) 활용하기**  
  여러 데이터를 순차적으로 가져오면 네트워크 병목이 생길 수 있어요. 가능하면 여러 데이터 요청을 동시에 보내서 속도를 앞당겨 보세요. 또, 미리 데이터를 불러오는 프리로딩(preloading)도 고려해볼 만해요.

- **데이터 캐싱(Cache)은 필수**  
  데이터 요청이 캐싱되고 있는지 꼭 확인하세요. fetch를 사용하지 않는 요청도 캐싱되도록 설정해주면 반복되는 요청을 줄일 수 있답니다.

- **정적 이미지 관리**  
  애플리케이션에 사용되는 이미지 같은 정적 자산은 `public` 디렉토리에 넣으면 자동으로 캐싱이 돼서 빠르게 로딩돼요.

---

### UI와 접근성에 대해

- **폼과 검증(Form & Validation) 처리하기**  
  사용자가 폼을 제출할 때 서버 액션(Server Actions)을 이용해 서버에서 검증하고 에러도 처리하세요. 클라이언트에서 모든 검증을 하는 것보다 안정적이고 보안에 더 좋아요.

---

이런 팁들을 활용하면 데이터 흐름을 효과적으로 관리하면서 사용자에게 쾌적한 경험을 줄 수 있어요. 서버 컴포넌트와 라우트 핸들러, React Suspense가 서로 어떻게 시너지를 내는지도 한번 실험해보시고요! 다음에 또 쉽게 이해할 만한 개발 지식으로 찾아올게요 :)

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

개발할 때 퍼포먼스와 보안을 신경 쓰는 건 정말 중요한데요, 이번에 제가 정리한 내용을 쉽게 설명해드릴게요.

### 퍼포먼스 최적화 팁

| 기능 | 설명 |
|---|---|
| Font Module | 폰트 모듈을 사용하면 폰트 파일을 다른 정적 자원들과 함께 자동으로 호스팅해서 외부 네트워크 요청을 줄이고, 레이아웃 이동(layout shift)도 감소시킬 수 있어요. |
| `Image` 컴포넌트 | 이미지 컴포넌트는 이미지를 자동으로 최적화해주고, 레이아웃 이동 문제를 막아주며, WebP 같은 현대적인 이미지 포맷으로 제공해줘요. 웹에서 이미지가 빠르게 뜨는 건 사용자 경험 향상에 아주 큰 도움이 되죠. |
| `Script` 컴포넌트 | 서드파티 스크립트를 최적화하는 데 유용해요. 자동으로 스크립트를 지연 실행(defer) 시켜서 메인 스레드를 막지 않도록 해주거든요. 덕분에 페이지가 더 부드럽고 빠르게 로딩됩니다! |
| ESLint | 내장된 `eslint-plugin-jsx-a11y` 플러그인을 활용하면 접근성(accessibility) 이슈를 미리 잡아낼 수 있어서, 누구나 쉽게 이용할 수 있는 웹을 만드는 데 도움이 됩니다. |

> 참고로, 레이아웃 시프트(layout shift)는 사용자 경험에 악영향을 주는 요소 중 하나라서 최대한 줄이는 게 좋아요.

---

### 보안 관련 팁

| 기능 | 설명 |
|---|---|
| Tainting | 민감한 데이터가 클라이언트 쪽으로 노출되지 않도록 데이터를 '오염(tainting)' 시키는 방법이에요. 특정 값이나 데이터 객체를 표시해서 관리하는 방식이죠. |
| Server Actions | 서버 액션을 호출할 때 사용자 권한이 꼭 확인되도록 하세요. 공식적으로 권장하는 보안 모범 사례를 정기적으로 리뷰하는 게 중요합니다. |
| 환경 변수 | `.env.*` 파일은 `.gitignore`에 반드시 추가해서 깃에 올라가지 않게 해야 해요. 공개해야 하는 변수만 `NEXT_PUBLIC_` 접두사를 붙여서 노출 범위를 명확히 관리하는 게 좋습니다. |
| Content Security Policy (CSP) | CSP는 교차 사이트 스크립팅(XSS), 클릭재킹(clickjacking), 코드 인젝션 같은 보안 위협으로부터 앱을 보호해주는 역할을 해요. 적절한 CSP 설정으로 좀 더 안전한 웹사이트를 만들 수 있습니다. |

---

여기서 한 가지 더! 보안에서 가장 기본적인 걸 깔끔하게 챙기는 게 중요해요. 예를 들어, 환경 변수 처리를 그냥 넘기지 말고, 꼭 `.gitignore`에 추가하는 습관은 실수로 중요한 정보가 유출되는 걸 예방할 수 있답니다. CSP도 조금 복잡해 보일 수 있는데, 보안 헤더 관련 툴이나 라이브러리를 활용하면 설정이 훨씬 수월해지니 참고하세요!

요약하자면, 폰트나 이미지, 스크립트 같은 프론트엔드 자원을 적절히 최적화하고, 보안에서 기본적인 데이터 관리와 권한, 정책 설정을 잘 챙기면 개발 퀄리티가 훨씬 올라간답니다. 앞으로 개발할 때 꼭 한번씩 체크리스트 삼아 활용해보세요!

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

### 메타데이터와 SEO

- **메타데이터 API**: 메타데이터 API를 활용하면 페이지 제목(title), 설명(description) 등 SEO에 중요한 정보를 손쉽게 추가할 수 있어요. 이렇게 하면 구글 같은 검색 엔진이 내 웹사이트를 더 잘 이해하고, 검색 결과에 더 잘 노출될 수 있답니다.
- **Open Graph(OG) 이미지**: 페이스북, 트위터 같은 소셜미디어에서 내가 만든 페이지를 공유할 때 멋진 썸네일 이미지가 나오도록 OG 이미지를 만들어두는 게 좋아요. 시각적으로도 훨씬 매력적이니, 공유가 늘어나는 효과도 있답니다.
- **사이트맵과 Robots 파일**: 사이트맵은 내 웹사이트에 어떤 페이지들이 있는지 검색 엔진에게 알려주는 역할을 해요. Robots.txt 파일은 검색 엔진이 어떤 페이지를 크롤링해도 되는지, 안 되는지를 지정해주는 친구입니다. 이 두 가지를 잘 설정하면 내 사이트가 검색 결과에 잘 노출되는 데 큰 도움이 돼요.

### 타입 안전성(Type Safety)

- **TypeScript와 TS 플러그인**: 자바스크립트는 유동적인 타입 때문에 때때로 예기치 못한 버그가 생기기 쉬운데요, TypeScript는 강력한 타입 시스템 덕분에 이런 문제를 미리 방지할 수 있도록 도와줘요. 게다가 VSCode 같은 개발 도구에서 타입 정보를 활용해서 자동완성도 되고, 에러도 빨리 발견할 수 있답니다. 그래서 요즘 웹 개발자라면 TypeScript를 꼭 배워두는 걸 추천해요!

---

개발하면서 SEO도 신경 쓰고, 코드 안정성도 챙기려면 위 내용들을 잘 활용해보세요. 작지만 이런 세심한 준비가 나중에 서비스 성공에 큰 차이를 만들어 줄 거예요!

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

## 배포 전에 꼭 확인해야 할 것들

배포 전에 `next build` 명령어로 애플리케이션을 로컬에서 미리 빌드해보세요. 이렇게 하면 빌드 도중에 발생할 수 있는 오류들을 미리 잡아낼 수 있거든요. 다음으로 `next start`를 실행해서 실제 배포 환경과 비슷한 상황에서 애플리케이션의 성능을 측정해보는 것도 좋은 방법이에요.

### Core Web Vitals

- **Lighthouse**
  - Lighthouse를 시크릿 모드(Incognito)로 실행해보세요. 이렇게 하면 실제 사용자들이 여러분의 사이트를 어떻게 경험할지 좀 더 정확하게 파악할 수 있어요. 그리고 개선이 필요한 부분도 쉽게 찾을 수 있죠.
  - 다만 Lighthouse는 시뮬레이션 테스트이기 때문에, 실제 사용자 데이터를 같이 보는 것이 중요해요. 예를 들어, Core Web Vitals 같은 필드 데이터를 참고해서 성능을 좀 더 정확히 평가해보는 걸 추천합니다.

> 추가 팁!
> 
> Lighthouse 외에도 Chrome DevTools Performance 탭을 활용해서 사이트의 렌더링 과정이나 렌더링 차단 요소가 있는지 직접 분석해보는 것도 좋아요. 그리고 Core Web Vitals는 페이지 로딩 속도, 반응성, 시각적 안정성 등 사용자 경험에 직접적인 영향을 미치는 지표들이라 배포 전후로 꾸준히 모니터링하는 습관을 들이면 사이트 품질 관리에 큰 도움이 됩니다!

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

- useReportWebVitals 훅: 이 훅을 사용하면 Core Web Vitals 데이터를 애널리틱스 도구로 전송할 수 있어요. 사이트 성능을 측정하고 개선할 때 유용하답니다.

### 번들 분석하기

@next/bundle-analyzer 플러그인을 활용해 자바스크립트 번들의 크기를 분석해보세요. 어떤 모듈이나 의존성이 큰지 파악해서 성능에 영향을 주는 부분을 쉽게 찾을 수 있어요.

또한, 새로운 의존성을 추가할 때 그 영향력을 알아보는 데 도움이 되는 도구들도 있어요. 이걸 잘 활용하면 불필요하게 무거운 라이브러리가 프로젝트에 들어오는 것을 막을 수 있죠. 대표적으로는 다음과 같은 툴들이 있으니 참고해 보세요:

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

- Import Cost  
- Package Phobia  
- Bundle Phobia  
- bundlejs

## 배포 후

어디에 애플리케이션을 배포하느냐에 따라, 성능을 모니터링하고 개선하는 데 도움이 되는 추가 도구나 통합 기능을 사용할 수 있어요.

예를 들어, Vercel에 배포했다면 다음과 같은 도구들을 써보길 추천해요:

- **Vercel Analytics**: 서버리스 환경에서 자동으로 트래픽과 성능 데이터를 수집해줘서, 별도의 설정 없이도 사용자 경험을 파악할 수 있어요.  
- **Edge Functions 로그**: 엣지 함수 사용 시 로그를 통해 실행 시간을 모니터링하고 최적화 포인트를 쉽게 찾을 수 있고요.  
- **Third-party 모니터링 도구 연동**: New Relic, Datadog 같은 도구들과 연동하면, 더 상세한 APM(애플리케이션 성능 관리)까지 할 수 있어요.  

물론 이런 도구들이 꼭 Vercel에만 국한된 건 아니지만, 배포 플랫폼별로 특화된 기능을 잘 활용하면 성능 최적화 작업이 훨씬 수월해진답니다.  

추가로, 배포 후에는 꼭 실시간 트래픽과 사용자 행동을 체크하면서 개선해나가는 걸 잊지 마세요! 이런 루틴이 결국 안정적이고 빠른 서비스 운영의 비결이에요.

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

- Analytics  
: 내장된 분석 대시보드를 통해 애플리케이션의 트래픽을 쉽게 파악할 수 있어요. 예를 들어, 고유 방문자 수, 페이지 조회 수 등 다양한 지표를 확인할 수 있답니다.

- Speed Insights  
: 실제 방문자 데이터를 바탕으로 한 성능 인사이트를 제공해요. 이를 통해 웹사이트가 실제 환경에서 얼마나 잘 작동하는지 실질적인 성과를 확인할 수 있죠.

- Logging  
: 런타임 및 활동 로그를 통해 문제를 디버깅하고 프로덕션 환경에서 애플리케이션을 모니터링할 수 있어요. 필요하다면 타사 도구 및 서비스 목록은 통합 페이지에서 찾아볼 수 있습니다.

> 알아두면 좋은 점:  
Vercel에서 프로덕션 배포 시 모범 사례를 종합적으로 이해하고 싶다면, [Vercel Production Checklist](https://vercel.com/docs/concepts/deployments/production-checklist)를 참고해 보세요. 웹사이트 성능 향상을 위한 다양한 전략과 자세한 내용이 담겨 있어서, 더 빠르고 안정적이며 안전한 애플리케이션을 만드는 데 큰 도움이 됩니다.

덧붙여, 이런 도구들을 잘 활용하면 단순히 코드만 잘 짜는 것을 넘어서, 실제 사용자 경험을 개선하고 문제 발생 시 빠르게 대응할 수 있는 능력이 생기니 꼭 챙겨보시길 추천해요!