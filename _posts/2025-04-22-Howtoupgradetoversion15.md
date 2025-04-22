---
title: "Nextjs 15로 안전하게 업그레이드하는 방법 정리"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:45
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to upgrade to version 15"
link: "https://nextjs.org/docs/app/guides/upgrading/version-15"
isUpdated: false
---


# 버전 15로 업그레이드하는 방법

## 14 버전에서 15 버전으로 업그레이드하기

Next.js 15 버전으로 업데이트하려면, 업그레이드용 codemod(codemod는 코드 자동 변경 도구)를 사용하면 편리해요. 터미널에서 아래 명령어를 입력하면 자동으로 코드에 필요한 변경사항을 반영해줍니다.

```bash
npx @next/codemod@canary upgrade latest
```

이 명령어는 Next.js 최신 버전(15)으로 맞춰서 코드와 설정을 업데이트해주기 때문에, 직접 수동으로 하나하나 고치는 번거로움을 많이 줄여준답니다.

---

### 추가 팁

- 항상 업그레이드 전에 현재 프로젝트를 백업하거나 Git에 커밋해 두세요. 혹시 모를 문제 발생 시 빠르게 되돌아갈 수 있어요.
- 업그레이드 후에는 실제 프로젝트가 잘 돌아가는지 꼭 테스트해보세요. 버전 변경에 따라 작동 방식이 조금씩 바뀔 수 있으니까요.
- Next.js의 공식 릴리즈 노트를 참고하면 새 버전에서 바뀐 점과 주의할 사항들을 한눈에 파악할 수 있어서 유용해요.

업그레이드가 생각보다 간단하지만, 미리 대비만 잘 하면 훨씬 수월하게 최신 기능도 누리고 안정성도 챙길 수 있답니다!

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

수동으로 설치하는 걸 선호한다면, 꼭 최신 버전의 Next와 React를 설치하는지 확인하세요:

```bash
npm i next@latest react@latest react-dom@latest eslint-config-next@latest
```

> 참고할 점:
설치 중에 peer dependencies 경고가 뜬다면, react와 react-dom을 권장 버전으로 업데이트해야 할 수도 있어요. 아니면 `--force` 혹은 `--legacy-peer-deps` 옵션을 사용해 경고를 무시할 수도 있습니다. 이 문제는 Next.js 15와 React 19가 안정화되면 자연스럽게 해결될 예정이에요.

## React 19

React 19 버전에서는 성능 향상과 더불어 새롭게 도입된 기능들이 많이 있습니다. 예를 들어, 더 나은 서버 사이드 렌더링(SSR) 지원과 경량화된 빌드, 그리고 React 에코시스템 전반에 걸친 호환성 강화 등이 포함되어 있죠.

참고로, React와 Next.js는 항상 같이 업데이트하는 게 가장 좋아요. 왜냐하면 Next.js가 React의 기능을 기반으로 동작하기 때문에 버전 불일치 시 예상치 못한 버그가 발생할 수도 있거든요.

앞으로 Next.js 15와 React 19가 공식 안정 버전이 나오면, 더 쉽게 최신 기능을 누릴 수 있으니 그때까지는 위 방법대로 최신 버전으로 업데이트 해두는 걸 추천합니다!

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

안녕하세요 여러분! 오늘은 React 19 버전에서 바뀐 점들을 간단하게 정리해보려고 해요. 특히 기존에 사용하던 훅들이 어떻게 바뀌었는지, 그리고 비동기 API 관련 중요한 변경사항들을 다뤄볼게요.

---

### 주요 변경점 요약

| 내용 | 설명 |
|---|---|
| React 최소 버전 | 이제 React와 React-DOM 최소 지원 버전이 19가 되었습니다. 기존 버전 사용자분들은 이번 기회에 업그레이드 해주세요! |
| useFormState → useActionState | `useFormState` 훅은 React 19에서도 사용할 수 있지만 곧 사라질 예정이에요. 대신 `useActionState`를 쓰는 걸 권장합니다. `useActionState`는 대기(pending) 상태를 직접 읽을 수도 있고, 더 많은 프로퍼티를 제공합니다. 좀 더 자세한 내용은 공식 문서를 참고하세요. |
| useFormStatus 업데이트 | `useFormStatus` 훅도 업데이트되면서 `data`, `method`, `action` 같은 추가 키가 생겼어요. React 19 버전이 아니면 `pending` 키만 사용할 수 있다는 점 꼭 기억하세요. |
| TypeScript 사용자 주의 | TypeScript를 쓰는 분들은 `@types/react`와 `@types/react-dom` 역시 최신 버전으로 함께 업그레이드 해주세요. 타입 안정성을 위해 꼭 필요합니다! |

---

### 비동기 요청 API 관련 주요 변경사항 (Breaking Change)

예전에는 런타임 정보를 기반으로 동작하는 동기 방식의 Dynamic API들이 있었는데요, 이제 이걸 전부 비동기 방식으로 변경했어요. 이 변화로 인해 비동기 요청을 처리하는 로직을 조금 다르게 써야 할 수도 있답니다.

---

### 추가 팁!

- React 19로 넘어가면서 form 관련 훅들이 많이 바뀌었는데, 특히 서버 액션과 관련된 `useActionState` 같은 훅을 사용하는 게 앞으로 리액트 생태계에서 더 권장됩니다.
- 새로운 API들이 추가되면서 동작 방식이 세밀해졌으니, 직접 애플리케이션에 적용해 보면서 어떤 점이 달라졌는지 꼭 확인해보세요.
- 공식 React 19 업그레이드 가이드도 꼭 한 번 읽어보시길 추천합니다. 그 안에 더 자세한 마이그레이션 팁과 변경점들이 잘 정리되어 있으니까요!

---

더 궁금한 점이나 실사용하면서 겪는 문제 있으면 댓글로 알려주세요. 다음에도 유용한 개발 소식으로 찾아올게요! 😊

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

- cookies  
- headers  
- draftMode  
- params in layout.js, page.js, route.js, default.js, opengraph-image, twitter-image, icon, and apple-icon.  
- searchParams in page.js  

마이그레이션을 조금이라도 쉽게 하기 위해서, 자동으로 변환해주는 codemod 도구가 준비되어 있어요. 그리고 새로운 API들은 잠시 동안 동기(synchronous) 방식으로도 접근이 가능해서 천천히 전환할 수 있답니다.

### cookies

#### 권장하는 비동기 사용법 (Async Usage)

Next.js 같은 최신 프레임워크에서는 cookie를 다룰 때도 비동기 방식을 추천하고 있어요. 예를 들어, `cookies()` 함수를 호출할 때 바로 데이터를 받을 수 있도록 `await`를 사용하는 겁니다. 이렇게 하면 네트워크 요청이나 복잡한 작업에 더 유연하게 대응할 수 있죠.

```js
import { cookies } from 'next/headers';

export async function GET() {
  const cookieStore = cookies();
  const token = cookieStore.get('token')?.value;

  // 비동기 작업 가능!
  // 예: 토큰 검증, 다른 API 호출 등
  if (!token) {
    return new Response('Unauthorized', { status: 401 });
  }

  return new Response('Hello, authorized user!');
}
```

이렇게 async/await 패턴으로 작성하면, 요청 중간에 필요한 정보를 쉽게 읽고 처리할 수 있어요.  

---

참고로, 만약 기존처럼 동기적으로 쓰고 싶다면 임시로 `cookies().get(...)` 같은 API를 이용할 수 있지만, 장기적으로는 꼭 비동기로 전환하는 게 추후 유지보수나 퍼포먼스 측면에서 좋아요.  

필요하다면, 내가 직접 작성해 본 codemod 스크립트도 공유해줄 수 있으니, 댓글로 알려주세요!

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

Next.js 13.4부터 `cookies()` 함수가 비동기 함수로 바뀌었어요. 그래서 예전처럼 동기적으로 `const cookieStore = cookies()` 이렇게 쓰면 안 되고, `await`를 붙여서 `const cookieStore = await cookies()` 이렇게 사용해야 합니다.

```js
import { cookies } from 'next/headers'

// 이전 방식
const cookieStore = cookies()
const token = cookieStore.get('token')

// 변경된 방식
const cookieStore = await cookies()
const token = cookieStore.get('token')
```

하지만 아직 완전히 비동기 방식에 적응하지 못한 상황에서는, 아래처럼 타입 캐스팅을 해서 임시로 동기적으로 사용하는 방법도 있어요. 다만 이 방법은 개발 환경에서 경고 메시지가 뜰 수 있으니 참고하세요.

```js
import { cookies, type UnsafeUnwrappedCookies } from 'next/headers'

// 임시 동기 처리 방법
const cookieStore = cookies() as unknown as UnsafeUnwrappedCookies
const token = cookieStore.get('token')
```

---

### headers 도 비슷해요!

`headers()`도 내부적으로 비동기 방식으로 변경될 수 있으니, 비슷하게 `await`를 붙여주는 게 좋습니다. 

---

### 참고로!

- `cookies()`를 쓸 때는 꼭 서버 컴포넌트(server component)나 API Route 등 서버 환경에서만 사용해야 해요. 클라이언트 컴포넌트에서는 동작하지 않으니 주의하세요.
- Next.js가 계속 발전하면서 내부 API도 조금씩 바뀌니까, 항상 공식 문서를 확인하고 업데이트 내역을 살펴보는 습관을 들여주세요.

이렇게 하면 쿠키를 읽을 때 생기는 비동기 문제를 깔끔하게 해결할 수 있답니다! :)

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

### 추천하는 비동기 사용법 (Async Usage)

```js
import { headers } from 'next/headers'

// 예전 방식
const headersList = headers()
const userAgent = headersList.get('user-agent')

// 요즘 추천 방식 (await 사용)
const headersList = await headers()
const userAgent = headersList.get('user-agent')
```

Next.js에서 `headers()`를 사용할 때 이제는 비동기 함수처럼 `await`를 붙여서 호출하는 게 권장됩니다. 이유는 `headers()`가 내부적으로 비동기 작업을 할 수 있기 때문이에요. 이렇게 하면 코드가 더 안정적으로 동작하죠.

---

### 잠깐! 동기 방식 사용법 (Temporary Synchronous Usage)

```js
import { headers, type UnsafeUnwrappedHeaders } from 'next/headers'

// 예전 방식
const headersList = headers()
const userAgent = headersList.get('user-agent')

// 동기 방식 그대로 쓰고 싶다면 (개발 중 경고 떠요)
const headersList = headers() as unknown as UnsafeUnwrappedHeaders
const userAgent = headersList.get('user-agent')
```

만약 코드를 당장 고치기 어려운 상황이라면 이렇게 타입을 강제로 변환해서 동기 방식으로 쓸 수는 있어요. 다만 개발 모드에서는 경고가 뜨니 나중에 꼭 비동기 방식으로 바꾸시는 걸 권장합니다.

---

### 추가 팁!

- `headers()`를 비동기 호출로 바꾸려면, 호출하는 함수도 `async` 함수여야 합니다. 예를 들어 서버 컴포넌트나 API 라우트 함수에서 많이 쓰게 돼요.
- 클라이언트 컴포넌트에서는 `next/headers`를 바로 사용할 수 없고, 서버에서 전달해줘야 한다는 점 꼭 기억하세요.
- `headers()` 대신 `request.headers`를 직접 다룰 수도 있지만, Next.js에서는 제공하는 `headers()`가 버전업에 맞춰 최적화되어 있답니다.

오늘 내용 참고해서 Next.js 최신 버전에 맞는 헤더 처리 스타일로 코딩해보세요! 개발할 때 헤더 정보를 안전하게 다루는 건 보안과도 직결되는 중요한 부분입니다. 😊

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

### draftMode

#### 비동기 방식 사용 추천

```js
import { draftMode } from 'next/headers'

// 예전 방식 (동기 방식)
const { isEnabled } = draftMode()

// 최신 권장 방식 (비동기 방식)
const { isEnabled } = await draftMode()
```

Next.js에서 `draftMode`를 사용할 때, 요즘은 `await`를 붙여서 비동기적으로 사용하는 걸 추천해요. 왜냐하면 최신 Next.js 환경에서는 내부적으로 비동기 처리를 더 잘 지원하기 때문이죠. 덕분에 코드가 안정적이고 예측 가능하게 동작하게 됩니다.

#### 임시로 동기 방식 사용하기

만약 당장 async/await를 적용하기 어렵거나, 함수가 동기 컨텍스트 안에 있어서 비동기 코드를 쓸 수 없는 상황이라면 임시로 동기적으로 사용할 수 있지만, 가능한 빨리 비동기 방식으로 전환하는 게 좋아요. 동기적으로 쓸 경우 예상치 못한 문제가 생길 수 있거든요.

---

추가 팁: `draftMode`는 Next.js의 미리보기 모드(preview mode)와 비슷한 기능으로, 콘텐츠를 변경하거나 임시 상태를 확인하는 데 유용해요. 미리보기 모드를 쓴 적이 있다면 이해하기 쉬울 거예요!

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

이번에 Next.js를 사용하면서 바뀐 부분들을 직접 공부해 보고 정리해봤어요. 혹시 Next.js에서 draftMode나 params 관련해서 헷갈렸던 부분이 있다면, 이 글이 도움이 될 거예요.

---

## draftMode 사용법 변경

예전에는 이렇게 했죠?

```ts
const { isEnabled } = draftMode()
```

그런데 이제는 이렇게 바뀌었어요.

```ts
const { isEnabled } = draftMode() as unknown as UnsafeUnwrappedDraftMode
```

왜 이런 변경이 있을까요? 사실 개발환경(dev)에선 경고를 띄우기 위한 의도된 타입 캐스팅이에요. 타입스크립트가 더 엄격해지면서 아래와 같은 경고를 피하려고 이중 캐스팅(as unknown as ...)을 해주는 꼼수가 필요해졌죠.

> 간단히 말하면, 공식 API가 안정화되기 전이라 타입 안전성 경고를 잡기 위해서 이렇게 사용한다는 점 참고하세요!

---

## params & searchParams - 비동기 Layout에서 바뀐 점

예전에는 generateMetadata나 Layout에서 params가 바로 객체 형태였어요.

| 구분         | 예전 코드                                | 변경된 코드                                |
|--------------|----------------------------------------|------------------------------------------|
| params 타입  | `{ slug: string }`                      | `Promise<{ slug: string }>`              |
| 호출 시점    | `const { slug } = params`               | `const { slug } = await params`          |
| 함수         | `function generateMetadata()`           | `async function generateMetadata()`      |
| Layout 함수  | `function Layout()`                      | `async function Layout()`                 |

직접 비교해보면,

### 예전 코드

```ts
type Params = { slug: string }

export function generateMetadata({ params }: { params: Params }) {
  const { slug } = params
}

export default function Layout({
  children,
  params,
}: {
  children: React.ReactNode
  params: Params
}) {
  const { slug } = params
  return <>{children}</>
}
```

### 변경된 코드

```ts
type Params = Promise<{ slug: string }>

export async function generateMetadata({ params }: { params: Params }) {
  const { slug } = await params
}

export default async function Layout({
  children,
  params,
}: {
  children: React.ReactNode
  params: Params
}) {
  const { slug } = await params
  return <>{children}</>
}
```

---

### 왜 params가 Promise로 바뀌었을까?

Next.js 13+의 동적 라우팅에서는 더 좋은 성능과 유연성을 위해, `params`가 비동기적으로 처리되어야 할 경우가 많아요. 예를 들어 API 호출이나 데이터베이스 조회로 라우팅 정보를 가져올 때, `params`는 즉시 사용할 수 있는 값이 아니라 비동기 Promise가 될 수 있어요.

그래서 이를 감안해서 `await`로 `params`를 받아온다는 점! 이 부분을 잊지 마세요.

---

## 정리하자면

| 변경 포인트         | 예전 방식                       | 변경된 방식                             |
|---------------------|--------------------------------|---------------------------------------|
| `draftMode` 타입    | `const { isEnabled } = draftMode()` | `const { isEnabled } = draftMode() as unknown as UnsafeUnwrappedDraftMode` |
| `params` 타입       | `{ slug: string }`              | `Promise<{ slug: string }>`            |
| 함수 선언          | 동기 함수                      | `async` 함수로 변경                    |
| params 사용법       | 바로 사용 (`const { slug } = params`) | `await` 처리해서 사용 (`const { slug } = await params`) |

---

## 추가 팁 - 실무에서 주의할 점

- `await params`로 받는 부분에서 실수로 `await`를 빼먹지 않도록 조심하세요. 그러면 타입 오류가 생기거나 런타임 에러가 발생할 수 있어요.
- `draftMode()`는 아직 공식 문서에도 "experimental" 표시가 있을 수 있어요. 실제 프로덕션에서 쓸 땐 신중 체크하시고, 시간이 지나면 정식 API가 나오면서 타입 경고도 사라질 가능성이 높아요.
- Next.js의 새로운 앱 디렉토리 구조 + 점점 강화되는 타입스크립트 환경에 대응하려면 이런 변경 사항을 꼭 미리 알아두는 게 좋아요.

---

궁금한 점 있으면 댓글 달아주세요! Next.js 최신 기능, 더 재밌고 편리하게 같이 공부해봐요~ 😄✨

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

이번에 React 컴포넌트에서 동기와 비동기 데이터를 다루는 방식을 조금 더 최신 방법으로 바꾸는 예제를 소개할게요. 쉬운 예시와 함께 변화한 코드를 설명하니까, 천천히 읽으면서 적용해보세요!

---

### 1. Synchronous Layout (동기 레이아웃)

기존엔 `params`가 그냥 객체였는데, 이걸 `Promise`를 감싸서 비동기 데이터처럼 쓸 수 있도록 바꿨어요.

```tsx
// Before (예전 방식)
type Params = { slug: string }

export default function Layout({
  children,
  params,
}: {
  children: React.ReactNode
  params: Params
}) {
  const { slug } = params
}
```

```tsx
// After (최신 방식)
import { use } from 'react'

type Params = Promise<{ slug: string }>

export default function Layout(props: {
  children: React.ReactNode
  params: Params
}) {
  const params = use(props.params) // 여기서 비동기 데이터를 바로 처리해버림
  const slug = params.slug
}
```

> 여기서 핵심은 바로 React의 `use` 훅을 활용해서 비동기 Promise 상태를 동기처럼 쓴다는 점이에요. `use`는 React 18에서 실험적으로 도입된 기능으로, Next.js 13같은 최신 프레임워크에서 지원하죠. 이렇게 하면 컴포넌트 내부가 훨씬 깔끔해지고, 비동기 데이터 처리가 자연스러워져요.

---

### 2. Asynchronous Page (비동기 페이지)

페이지 컴포넌트와 `generateMetadata`에서 비동기 params, searchParams를 다루는 예시입니다.

```tsx
// Before (예전 방식)
type Params = { slug: string }
type SearchParams = { [key: string]: string | string[] | undefined }

export function generateMetadata({
  params,
  searchParams,
}: {
  params: Params
  searchParams: SearchParams
}) {
  const { slug } = params
  const { query } = searchParams
}

export default async function Page({
  params,
  searchParams,
}: {
  params: Params
  searchParams: SearchParams
}) {
  const { slug } = params
  const { query } = searchParams
}
```

```tsx
// After (최신 방식)
type Params = Promise<{ slug: string }>
type SearchParams = Promise<{ [key: string]: string | string[] | undefined }>

export async function generateMetadata(props: {
  params: Params
  searchParams: SearchParams
}) {
  const params = await props.params
  const searchParams = await props.searchParams
  const slug = params.slug
  const query = searchParams.query
}

export default async function Page(props: {
  params: Params
  searchParams: SearchParams
}) {
  const params = await props.params
  const searchParams = await props.searchParams
  const slug = params.slug
  const query = searchParams.query
}
```

> 여기서는 `params`와 `searchParams`가 Promise로 감싸져서 넘어옵니다. 그래서 컴포넌트나 함수 내에서 `await`으로 값을 받아와 처리하는 거죠. 이렇게 하면 URL 파라미터나 쿼리 스트링이 비동기적으로 처리될 때도 문제없이 데이터를 다룰 수 있어요.

---

### 마무리 - 왜 이렇게 바꾸는 걸까?

- **React 18, Next.js 13** 등 최신 도구들이 비동기를 더 자연스럽게 다루도록 발전하고 있기 때문이에요.
- `use` 훅으로 비동기 데이터를 처리하면 렌더링 로직이 엄청 깔끔해지고 버그도 줄어듭니다.
- Promise를 직접 받고 `await` 하면서 컴포넌트가 더 유연하게 변해요.

---

### 간단 요약

| 이전 방식                       | 최신 방식                          |
|------------------------------|---------------------------------|
| `params`는 동기 객체              | `params`는 Promise로 감싸진 비동기 객체  |
| 데이터 바로 사용                  | `use` 훅 (Layout) / `await` (Page) 이용 |
| 함수 파라미터에 바로 받음           | Promise 감싸진 객체를 `await` 또는 `use`로 처리 |

---

이제 이런 패턴을 알았으니, Next.js 프로젝트나 React 앱에서 URL 파라미터 등을 더 깔끔하고 안전하게 처리해보세요! 필요하면 `use` 훅이 지원되는지, 환경이 맞는지 먼저 확인하는 것도 잊지 마시고요. 궁금한 점 있으면 또 알려드릴게요~!

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

### 동기식 페이지 (Synchronous Page) 코드 전환하기

안녕하세요! 오늘은 Next.js에서 동기식 페이지를 만드는 방법을 조금 더 최신 React 문법에 맞게 바꾸는 방법을 이야기해볼게요. 코드를 조금만 바꾸면 더 깔끔해지고, Promise를 직접 받아서 사용할 수 있게 되는 점이 포인트입니다.

---

#### 기존 코드 Before → 최신 코드 After

##### Before (기존 방식)

```tsx
// 기존에는 params와 searchParams가 객체로 바로 들어왔어요
type Params = { slug: string }
type SearchParams = { [key: string]: string | string[] | undefined }

export default function Page({
  params,
  searchParams,
}: {
  params: Params
  searchParams: SearchParams
}) {
  const { slug } = params
  const { query } = searchParams
}
```

##### After (업데이트된 방식)

```tsx
import { use } from 'react'

// 이제 params와 searchParams가 Promise로 들어옵니다
type Params = Promise<{ slug: string }>
type SearchParams = Promise<{ [key: string]: string | string[] | undefined }>

export default function Page(props: {
  params: Params
  searchParams: SearchParams
}) {
  // react의 use 훅을 써서 Promise를 바로 resolve합니다
  const params = use(props.params)
  const searchParams = use(props.searchParams)
  const slug = params.slug
  const query = searchParams.query
}
```

---

#### 심플 버전도 이렇게 바꿔요!

```tsx
// Before
export default function Page({ params, searchParams }) {
  const { slug } = params
  const { query } = searchParams
}

// After
import { use } from "react"

export default function Page(props) {
  const params = use(props.params)
  const searchParams = use(props.searchParams)
  const slug = params.slug
  const query = searchParams.query
}
```

---

### 여기서 잠깐!

- `use` 훅은 React 18부터 도입된 실험적 API 중 하나로, Promise를 받아 바로 결과를 얻을 수 있어요.
- Next.js의 새로운 데이터 페칭 방식과 잘 어울리는데, async/await를 쓰지 않고도 동기식처럼 코드를 작성할 수 있게 해 줍니다.
- 물론 아직 안정화 단계이니 실제 프로젝트에 적용할 때는 주의가 필요해요. (React 릴리즈 노트를 꼭 확인해보세요!)

---

#### 마무리

이처럼 Next.js에서 page 컴포넌트에 들어오는 params와 searchParams 타입이 Promise로 변경되었고, React의 `use` 훅을 사용해 데이터를 간편하게 사용할 수 있게 되었습니다. 개발자 경험이 점점 좋아지는 것 같아 기대됩니다!

다음 글에서는 Route Handlers 관련 내용도 다뤄볼 예정이니까 기대해 주세요~

---

궁금한 점 있으면 언제든 댓글 주세요! 😊

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

이번에 Next.js에서 `GET` 함수의 파라미터 `params` 타입과 처리 방식에 변화가 생겼어요. 기존에는 `params`가 동기적으로 바로 사용할 수 있는 객체였는데, 이제는 `params`가 `Promise`로 감싸져 비동기 처리를 해야 합니다. 간단히 말하면, `params`를 바로 쓰면 안 되고, `await`으로 비동기 값을 받아와야 한다는 거죠.

아래 예시 코드를 보며 차이점을 살펴볼게요.

```ts
// Before
type Params = { slug: string }
 
export async function GET(request: Request, segmentData: { params: Params }) {
  const params = segmentData.params        // 동기 처리
  const slug = params.slug
}
 
// After
type Params = Promise<{ slug: string }>
 
export async function GET(request: Request, segmentData: { params: Params }) {
  const params = await segmentData.params   // 비동기 처리 필수
  const slug = params.slug
}
```

이처럼 `params` 타입이 `Promise`로 변경되면서, `await` 없이는 `slug`를 바로 꺼낼 수 없어요. 이것은 Next.js가 내부 라우팅 파라미터를 더 유연하게 처리하기 위한 변경 사항으로 보입니다.

또한 타입이 명시되어 있지 않은 코드에서도 마찬가지에요.

```ts
// Before
export async function GET(request, segmentData) {
  const params = segmentData.params    // 동기 처리
  const slug = params.slug
}
 
// After
export async function GET(request, segmentData) {
  const params = await segmentData.params   // 비동기 처리 필수
  const slug = params.slug
}
```

이 부분은 간단하지만, 만약 기존 코드를 그대로 쓴다면 `slug`를 읽으려 할 때 `params`가 `Promise`니까 `undefined`가 나오거나 에러가 날 수 있어요. 따라서 반드시 `await` 키워드를 붙여주셔야 합니다.

---

## 런타임 설정 관련 깨알 변화! - Breaking change

`runtime` 설정을 하실 때, 이전에는 `experimental-edge`와 `edge` 두 가지 옵션을 모두 쓸 수 있었어요. 그런데 이 둘은 사실 같은 걸 의미해요. 그래서 앞으로는 `experimental-edge`를 사용하면 에러가 납니다! 깔끔하게 `edge`로만 쓸 수 있도록 정리한 거죠.

만약 예전에 `experimental-edge`로 설정해둔 게 있다면, 아래처럼 바꾸시면 됩니다.

| Before           | After   |
|------------------|---------|
| `experimental-edge` | `edge`   |

귀찮으시다면 자동으로 바꿔주는 코드모드(codemod)도 제공하고 있으니 참고하세요.

---

이 변경 사항들은 Next.js가 더 견고하고 명확한 API를 제공하려는 노력의 일환입니다. 여러분의 API 핸들러 코드를 수정할 때는 이번 `params` 비동기 처리 부분과 `runtime` 설정을 꼭 점검해 주세요. 특히 `await` 처리 누락은 의외로 찾기 힘든 버그가 될 수 있으니 조심하시고요!

추가로, `params`가 이제 `Promise`라는 점은 Next.js가 내부에서 라우트 파람 해석을 비동기로 처리하거나, 더 복잡한 데이터 준비과정이 추가됐기 때문인데요. 앞으로 이런 패턴이 더 확산될 가능성이 높으니, 비동기 처리를 기본으로 생각하는 습관을 들이면 좋겠어요.

필요하다면 `GET` 외에 다른 HTTP 메서드 핸들러(`POST`, `PUT` 등)도 동일하게 바뀌었을 가능성이 있기에 한번 점검해 보시길 권합니다.

오늘 알려드린 내용, 작게는 한 줄 바꾸는 거지만, 다음 프로젝트에서 여러분 코드를 한층 안정적으로 만들어 줄 거예요! 잘 적용해보시길 바랍니다~

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

## fetch 요청 캐싱 방식이 바뀌었어요!

이번에 fetch 요청이 기본적으로 더 이상 자동으로 캐싱되지 않는다는 점, 알고 계셨나요? 이전에는 그냥 fetch 호출하면 브라우저가 알아서 캐싱해줬는데, 이제는 기본값이 캐싱 안 함으로 바뀌었답니다.

그래서 만약 특정 fetch 요청을 꼭 캐싱하고 싶다면, fetch 함수에 옵션으로 `cache: 'force-cache'`를 명시해줘야 해요. 예를 들어 아래처럼요:

```js
export default async function RootLayout() {
  // 기본 fetch는 캐시 안 함
  const a = await fetch('https://...')

  // force-cache 옵션을 주면 캐시 사용!
  const b = await fetch('https://...', { cache: 'force-cache' })
  
  // ...
}
```

### 좀 더 알아두면 좋은 점
- `cache` 옵션은 브라우저의 기본 캐시 정책보다 fetch 요청마다 세밀하게 제어할 수 있게 해줍니다.
- `force-cache` 외에도 다양한 옵션이 있는데, 예를 들어 `no-cache`는 항상 네트워크에서 새로운 데이터를 가져오고 캐시는 무시하는 기능이에요.
- 캐시를 적절히 활용하면 네트워크 비용과 로딩 속도를 크게 줄일 수 있으니까, 상황에 맞게 잘 조절해보세요!

요즘은 SSR(서버 사이드 렌더링)이나 SSG(정적 사이트 생성)를 자주 사용하는데, fetch 캐싱 정책이 이런 환경과도 잘 맞게 바뀌었다고 보시면 됩니다. 꼭 최신 정책에 맞게 코드를 작성해보세요!

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

fetch 요청을 페이지나 레이아웃 단위에서 한 번에 캐싱 처리하고 싶다면, `export const fetchCache = 'default-cache'` 설정을 사용하면 됩니다. 이 옵션을 추가하면, 해당 레이아웃이나 페이지 내의 모든 fetch 요청은 별도로 캐시 옵션을 지정하지 않는 이상 기본적으로 캐싱됩니다.

```js
// 이게 루트 레이아웃이라면, 앱 내 fetch 요청 중
// 따로 캐시 옵션을 안 줬으면 전부 캐싱됩니다.
export const fetchCache = 'default-cache'

export default async function RootLayout() {
  const a = await fetch('https://...') // 캐시됨
  const b = await fetch('https://...', { cache: 'no-store' }) // 캐시 안됨

  // ...
}
```

여기서 중요한 점은, 개별 fetch 요청 쪽에서 `cache` 옵션을 직접 지정하면 그 설정이 우선 적용된다는 거예요. 따라서 전체적인 기본 캐싱 전략은 위 설정으로 하고, 특정 요청만 예외를 두고 싶은 경우 각 fetch에 `cache: 'no-store'` 같은 옵션을 직접 지정할 수 있죠.

---

### Route Handlers (라우트 핸들러)에서의 캐시 전략

다음으로, Route Handlers에서 GET 메서드는 기본적으로는 캐싱되지 않아요. 그렇기 때문에 GET 요청을 캐싱하고 싶을 때는 라우트 핸들러 파일에 `export const dynamic = 'force-static'` 같은 설정을 추가해줘야 합니다.

즉, `dynamic` 설정을 통해 해당 라우트의 리소스가 정적(static)으로 처리되어 캐시될 수 있도록 강제하는 거예요.

---

이 내용들을 잘 활용하면, 앱 내 네트워크 요청들을 효과적으로 관리할 수 있습니다. 특히 데이터가 자주 변경되지 않는 경우 캐시를 적극 활용하는 게 로딩 속도를 확실히 높이고 사용자 경험도 개선하니까요!

추가로, Next.js에서 fetch의 `cache` 옵션으로는 'no-store' 외에도 'default', 'reload', 'force-cache' 등이 있는데, 각 옵션의 차이를 익혀두면 상황에 맞는 캐싱 정책을 세우기 좋습니다. 예를 들어:

| 옵션        | 설명                                         |
|-------------|---------------------------------------------|
| default     | 기본값, 브라우저/런타임의 기본 캐싱 정책 사용     |
| no-store    | 캐시하지 않고 항상 네트워크에서 새로 가져오기     |
| reload      | 캐시 무시하고 항상 네트워크 요청하기             |
| force-cache | 캐시된 데이터가 있으면 무조건 가져오기             |

이런 차이점도 참고하세요!

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
export const dynamic = 'force-static';

export async function GET() {}
```

## 클라이언트 사이드 라우터 캐시

Next.js 같은 프레임워크에서 `Link`나 `useRouter`를 통해 페이지를 이동할 때, 클라이언트 사이드 라우터 캐시에서 기존 페이지 세그먼트를 재사용하지 않아요. 대신 브라우저의 뒤로 가기, 앞으로 가기 버튼을 누를 때, 그리고 공통 레이아웃(shared layouts)을 사용할 때는 세그먼트를 재사용합니다.

만약 페이지 세그먼트의 캐싱을 활성화하고 싶다면 `staleTimes` 설정 옵션을 활용할 수 있는데요, 이 옵션을 통해 특정 시간 동안 페이지 데이터를 캐시에 저장해 두고 재사용할 수 있어요.

### 좀 더 쉽게 설명하자면!

- **기본 동작:** `Link`나 `useRouter`를 이용한 페이지 이동 시, 매번 새로 데이터를 가져오고 새 페이지를 렌더링합니다.
- **하지만:** 뒤로 가기 / 앞으로 가기 를 할 땐 캐시된 데이터를 씁니다.
- **해결 방법:** `staleTimes`를 설정해 캐시된 데이터를 일정 시간 동안 유지할 수 있게 해요.

이렇게 하면 네비게이션 시 유저 경험이 좀 더 부드러워지고, 불필요한 데이터 요청도 줄일 수 있습니다. 만약 대형 앱에서 사용자 인터렉션 빈도가 높다면 캐싱 매커니즘을 활용하는 것을 강력히 추천드려요!

아래 표는 대표적인 `staleTimes` 설정 예시입니다:

| 설정 값         | 설명                                |
|----------------|-----------------------------------|
| `staleTimes: 0` | 캐시 사용 안 함 (기본값)             |
| `staleTimes: 60000` | 1분간 캐시 유지                       |
| `staleTimes: 300000` | 5분간 캐시 유지                       |

캐싱 시간은 앱의 특성이나 사용자 흐름에 따라 적절히 조절하면 됩니다.

---

필요하다면 다음에 `staleTimes`를 실제 코드에 적용하는 예시도 공유할게요! 언제든 질문 주세요. :)

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

안녕하세요! 오늘은 Next.js에서 최근에 업데이트된 부분 중 하나를 같이 살펴볼게요.

먼저, 여러분이 보신 것처럼 `next.config.js`에 아래와 같은 설정이 들어갈 수 있습니다.

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    staleTimes: {
      dynamic: 30,
      static: 180,
    },
  },
}

module.exports = nextConfig
```

### 이 설정, 뭐 하는 거냐?

`staleTimes` 설정은 Next.js에서 페이지를 캐싱하는 시간과 관련된 실험적 기능입니다. 여기서 'dynamic'은 동적 데이터가 있는 페이지에 대한 캐시 시간(초 단위), 'static'은 정적인 페이지에 대한 캐시 시간을 의미해요.

- `dynamic: 30` → 동적 페이지는 30초 동안 캐시를 유지
- `static: 180` → 정적 페이지는 180초 동안 캐시 유지

이렇게 하면 방문자가 페이지를 빠르게 로드할 수 있게 되면서도, 너무 오래된 데이터가 보여지는 것을 어느 정도 방지할 수 있죠.

하지만 중요한 점! 레이아웃(layouts)이나 로딩 상태(loading states)는 여전히 네비게이션할 때 캐시되고 재사용됩니다. 즉, 페이지 전체가 아닌 일부 컴포넌트의 상태 재사용은 유효하다는 거예요. 이 부분은 개발자가 미리 염두에 두면 사용자 경험을 개선하는 데 도움이 됩니다.

---

### next/font가 사라졌다고?

예전 Next.js에서는 `@next/font`라는 패키지를 통해서 폰트를 쉽게 관리할 수 있었죠. 그런데 이제는 `next/font`가 빌트인 기능으로 통합되면서 별도의 패키지를 설치할 필요가 없게 됐어요.

즉, 옛날 방식으로 `@next/font`에서 import 했다면, 이제는 그냥 `next/font`로 바꾸면 됩니다. 게다가, 이 작업을 안전하고 자동으로 해주는 **codemod** 도구가 제공되고 있으니, 큰 걱정 없이 업그레이드할 수 있습니다.

---

### 개발자로서 알아두면 좋은 팁!

- 이런 실험적 기능들은 실제 프로덕션에 적용하기 전에 꼭 테스트를 해보세요. 자동 캐싱 때문에 데이터가 오래된 상태로 보여질 수 있으니, 데이터 신선도를 유지하는 전략을 같이 고민하는 게 중요합니다.
- Next.js가 업데이트될 때마다 공식 문서, 마이그레이션 가이드, 그리고 codemod 도구를 적극 활용하면, 시간과 노력을 절약할 수 있어요.

Next.js가 점점 더 사용하기 편리해지고 있어서 앞으로가 더욱 기대됩니다! 다음에도 새롭고 쓸만한 기능이나 팁 있으면 같이 나눠봐요~ 😄

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

이번에 Next.js에서 바뀐 import 방식과 설정 옵션에 대해 알아볼게요. 코드를 보면서 쉽게 따라올 수 있도록 차근차근 설명해드릴게요.

---

### 1. 폰트 import 경로 변경

```js
// Before
import { Inter } from '@next/font/google'

// After
import { Inter } from 'next/font/google'
```

예전에는 `@next/font/google` 경로에서 구글 폰트를 가져왔는데, 이제는 `next/font/google` 경로로 바뀌었어요. 굳이 `@next/`가 붙지 않아서 더 깔끔해졌죠.

> 참고로, 이 변경으로 인해 폰트 관련 코드에서 오류가 발생한다면 경로를 꼭 확인해보세요!

---

### 2. 설정 옵션 이름 변경: `bundlePagesExternals` → `bundlePagesRouterDependencies`

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  // Before
  experimental: {
    bundlePagesExternals: true,
  },

  // After
  bundlePagesRouterDependencies: true,
}

module.exports = nextConfig
```

기존엔 experimental(실험 기능) 중 하나로 쓰이던 `bundlePagesExternals`가 정식으로 자리 잡으면서 이름도 `bundlePagesRouterDependencies`로 바뀌었어요. 이제 `experimental` 안에 넣지 않고 바로 최상위 옵션으로 작성하면 됩니다.

> 이 옵션은 페이지 라우터에서 의존성들을 묶어서 번들링하는 기능인데요. 페이지별로 필요한 모듈들을 효율적으로 관리할 때 도움되니, 대규모 프로젝트에서는 성능 최적화에 유용합니다.

---

### 요약

| 변경 전                       | 변경 후                         | 설명                                    |
|------------------------------|--------------------------------|----------------------------------------|
| `import { Inter } from '@next/font/google'` | `import { Inter } from 'next/font/google'` | 구글 폰트 import 경로 변경                  |
| `experimental.bundlePagesExternals`         | `bundlePagesRouterDependencies`           | 실험적 기능에서 정식 옵션으로 변경 및 위치 이동 |

---

Next.js는 꾸준히 발전하면서 더 깔끔하고 성능 좋은 설정 방법들을 제시해주니까 자주 공식 문서를 살펴보면서 최신 변화를 체크하는 게 좋아요. 필요하면 새 설정도 실험해보면서 최적화해 보시길 추천드립니다! 😊

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

## serverExternalPackages

Next.js에서 `experimental.serverComponentsExternalPackages`가 안정화되어 이제 `serverExternalPackages`로 이름이 변경되었어요.

예전엔 `experimental` 안에 설정했지만, 이제는 그냥 바로 `serverExternalPackages`로 설정하면 됩니다.

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  // 예전 방식
  experimental: {
    serverComponentsExternalPackages: ['package-name'],
  },
 
  // 최신 방식
  serverExternalPackages: ['package-name'],
}

module.exports = nextConfig
```

이 설정은 서버 컴포넌트에서 외부 패키지를 사용할 때 필요한 건데, 특히 기본적으로 서버 컴포넌트에 포함되지 않는 서드파티 패키지를 명시할 때 유용해요. 이렇게 하면 빌드 과정에서 오류를 방지하고, 필요한 패키지를 서버에서 올바르게 로드할 수 있습니다.

추가로, `serverExternalPackages`를 잘 활용하면 서버 컴포넌트의 성능 최적화에 도움이 될 수 있어요. 왜냐하면, 서버쪽에서 필요한 패키지를 명확히 분리해서 로드하니까 클라이언트 번들 사이즈가 줄어들고 불필요한 코드가 포함되는 걸 막을 수 있거든요.

## Speed Insights

Next.js 같은 프레임워크를 쓰면서 성능에 대해 고민하는 건 정말 중요하죠! 구글의 Speed Insights 같은 도구를 통해 내 웹사이트가 얼마나 빠른지, 어느 부분에서 병목 현상이 일어나는지 확인해보는 걸 추천합니다.

Speed Insights는 페이지 로딩 시간, 사용자 경험, 접근성, SEO 등 다양한 지표를 점수로 보여주는데, 결과에 따라 개선할 수 있는 부분을 구체적으로 안내해줘요.

저도 개발하면서 Speed Insights를 꾸준히 체크하면서 아래 부분들에 주로 신경써요:

- 이미지 최적화 (이미지 사이즈 축소, next/image 컴포넌트 활용)
- 코드 스플리팅 및 동적 import
- 불필요한 자바스크립트 제거
- 서버 쪽 렌더링 활용과 클라이언트에서의 첫 로딩 속도 최적화

Next.js 최신 버전을 활용하는 것 자체가 이미 많은 최적화가 적용되어 있지만, `serverExternalPackages` 같이 서버 컴포넌트 관련 설정을 꼼꼼히 맞추는 것도 결국 Speed Insights 점수를 끌어올리는 데 도움이 됩니다.

웹사이트 속도는 사용자 경험에도 큰 영향을 주니, 한번에 완벽하려고 하기보다 꾸준하게 모니터링하고 개선하는 습관을 들이는 게 가장 좋아요!

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

Next.js 15부터는 Speed Insights를 자동으로 연동해주는 기능이 제거되었어요.

그래서 Speed Insights를 계속 사용하고 싶다면, Vercel에서 제공하는 Speed Insights Quickstart 가이드를 참고해서 직접 설정해줘야 합니다.

---

## NextRequest의 Geolocation 변경점

이전 버전에서는 NextRequest 객체에서 `geo`와 `ip` 같은 지리 정보 관련 속성을 직접 제공했는데요, Next.js 15부터는 이 값들이 호스팅 제공자(예: Vercel)에서 직접 제공되기 때문에 해당 속성들이 제거되었어요.

만약 프로젝트에서 이 부분을 사용하고 있다면, 자동으로 변경해주는 codemod(코드 변환 도구)가 있으니 이를 활용하면 편리하게 마이그레이션 할 수 있습니다.

---

### 덧붙여서

- Speed Insights를 자동으로 처리해주던 기능이 빠진 만큼, 수동 설정 과정에서 분석된 데이터를 직접 확인하고 조작할 수 있다는 장점도 있어요.
- hosting provider가 제공하는 geo 정보는 좀 더 정확하거나 세밀한 경우가 많으니 이를 활용해보는 것도 추천합니다.
- Codemod는 CLI 기반 도구로, 프로젝트 루트에서 실행하면 자동으로 코드 패턴을 찾아서 새 구조에 맞게 바꿔줍니다. 사용법은 보통 공식 문서를 참고하면 되고, 큰 규모 프로젝트에서도 안정적으로 변경 가능해요.

더 궁금한 점 있으면 알려주세요!

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

만약 Vercel을 사용하고 있다면, @vercel/functions 패키지에서 제공하는 geolocation과 ipAddress 함수를 활용할 수도 있어요. 이렇게 하면 클라이언트의 위치 정보나 IP 주소를 쉽게 가져올 수 있답니다.

예를 들어, geolocation 함수는 요청 정보를 기반으로 위치 데이터를 반환해줘서, 미들웨어에서 바로 도시 이름(city) 같은 걸 뽑아 쓸 수 있어요:

```js
import { geolocation } from '@vercel/functions'
import type { NextRequest } from 'next/server'
 
export function middleware(request: NextRequest) {
  const { city } = geolocation(request)
 
  // 여기서 city 정보를 활용할 수 있습니다.
}
```

또한 ipAddress 함수는 요청에서 IP 주소만 딱 반환하기 때문에, IP 기반 처리나 인증 등에 쓸 때 편리해요:

```js
import { ipAddress } from '@vercel/functions'
import type { NextRequest } from 'next/server'
 
export function middleware(request: NextRequest) {
  const ip = ipAddress(request)
 
  // ip를 활용하는 로직 작성
}
```

참고로, 이 함수들을 쓰면 직접 클라이언트 IP를 헤더에서 뽑거나, 외부 API를 호출해서 위치를 확인하는 등의 번거로운 작업을 줄일 수 있어서 개발 속도가 훨씬 빨라질 거예요. 다만, Vercel 플랫폼에서만 동작하니 로컬이나 다른 환경에서는 호환성에 유의해 주세요!