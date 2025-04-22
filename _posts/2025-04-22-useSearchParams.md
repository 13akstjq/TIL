---
title: "Next.js 15에서 useSearchParams로 현재 URL 쿼리 쉽게 가져오는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:54
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "useSearchParams"
link: "https://nextjs.org/docs/app/api-reference/functions/use-search-params"
isUpdated: false
---


# useSearchParams 훅 간단 정리

오늘은 Next.js에서 클라이언트 컴포넌트에서 URL의 쿼리 파라미터를 쉽게 읽을 수 있는 `useSearchParams` 훅에 대해 이야기해볼게요.

`useSearchParams`는 현재 페이지 URL의 쿼리 스트링을 읽을 수 있게 도와주는 훅입니다. 마치 `URLSearchParams` 객체처럼 동작하지만, 읽기 전용이라는 점이 특징이에요. 그래서 쿼리 파라미터를 **조회**할 때 딱 편하답니다.

---

## 사용법 예시

```jsx
'use client'

import { useSearchParams } from 'next/navigation'

export default function SearchBar() {
  const searchParams = useSearchParams()
  
  // 쿼리 스트링에서 'search'라는 값 가져오기
  const search = searchParams.get('search')
  
  // 예를 들어 URL이 /dashboard?search=my-project 라면,
  // search 변수는 'my-project'가 됩니다.
  return <>Search: {search}</>
}
```

---

## 추가 팁!

- **읽기 전용이라 값 변경은 불가!**  
  이 훅으로 쿼리 파라미터 값을 변경하고 싶으면 `useRouter`의 `push`나 `replace` 메서드를 활용해야 해요.

- **쿼리 파라미터가 여러 개일 때도 OK!**  
  예를 들어 `?search=apple&page=2` 인 경우, `searchParams.get('page')`로 페이지 번호도 바로 꺼낼 수 있답니다.

- **초기값과 기본값 처리**  
  쿼리 파라미터가 없으면 `get` 메서드가 `null`을 반환해요. 따라서 기본값이 필요하면 `const search = searchParams.get('search') ?? '기본값'` 처럼 널 병합 연산자를 활용하세요.

---

이렇게 `useSearchParams`는 클라이언트 컴포넌트 내에서 URL 쿼리 정보를 쉽게 확인할 수 있게 도와줘서, 필터링, 검색 기능 구현할 때 아주 유용하답니다.  
한번 직접 프로젝트에 써보면 훨씬 더 이해가 쉬울 거예요!

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

## Parameters

```js
const searchParams = useSearchParams()
```

useSearchParams는 파라미터를 받지 않아요.

## Returns

useSearchParams 훅을 호출하면 URL의 쿼리 파라미터를 다룰 수 있는 `URLSearchParams` 객체와 이를 업데이트할 수 있는 함수를 반환해요.

간단히 말해서, 이 훅을 사용하면 리액트 컴포넌트 내에서 현재 URL의 쿼리스트링을 쉽게 읽고 수정할 수 있답니다.

예를 들어, 현재 URL이 `?page=2&sort=desc`라면, `searchParams.get('page')`를 통해 `2`라는 값을 가져올 수 있고, `setSearchParams` 함수를 통해 쿼리스트링을 업데이트할 수 있어요.

이 때문에 리액트 라우팅에서 상태를 URL에 반영하거나, URL을 통해 상태를 공유할 때 매우 유용하답니다!

필요하면 더 자세히 어떻게 사용하는지도 알려줄게요!

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

useSearchParams는 URL의 쿼리 스트링을 읽을 수 있는 유틸리티 메서드들을 포함하는 URLSearchParams 인터페이스의 읽기 전용 버전을 반환해줘요.

주요 메서드를 한번 살펴볼게요:

| 메서드 | 설명 | 예시 및 결과 |
|--------|------|-------------|
| `URLSearchParams.get()` | 특정 검색 파라미터와 연관된 첫 번째 값을 반환해요. | `/dashboard?a=1` → `"1"`<br>`/dashboard?a=` → `""` (빈 문자열)<br>`/dashboard?b=3` → `null`<br>`/dashboard?a=1&a=2` → `"1"` (모든 값을 받으려면 `getAll()` 사용) |
| `URLSearchParams.has()` | 특정 파라미터가 존재하는지 불리언 값을 반환해요. | `/dashboard?a=1` → `true`<br>`/dashboard?b=3` → `false` |

이 외에도 `getAll()`, `keys()`, `values()`, `entries()`, `forEach()`, `toString()` 같은 읽기 전용 메서드들이 있어요. 각각의 메서드를 활용하면 쿼리 파라미터를 다양하게 조회하고 조작하는 데 편리합니다.

> 알아두면 좋아요!
> 
> - `useSearchParams`는 클라이언트 컴포넌트에서만 쓸 수 있는 훅으로, 서버 컴포넌트에서는 지원되지 않아요. 이는 부분 렌더링 시 값이 오래되어 버리는 문제를 방지하기 위함이죠.
> - 만약 프로젝트에 `/pages` 디렉터리를 사용 중이라면, `useSearchParams`는 `ReadonlyURLSearchParams | null` 타입을 반환해요. 이는 페이지를 사전 렌더링할 때 검색 파라미터를 알 수 없는 경우가 있기 때문인데요, 이 부분은 `getServerSideProps`를 사용하지 않는 페이지의 호환성을 위해서예요.

참고로, 쿼리 파라미터를 다룰 때는 여러 개의 같은 이름 파라미터가 존재할 수도 있는데, `get()`은 첫 번째 값만 반환하기 때문에 모든 값을 다루려면 `getAll()`을 꼭 기억해두세요! 그리고 쿼리 스트링을 직접 파싱하거나 조작하는 것보다 이 훅을 이용하면 편하고 안전합니다.

다음에는 `useSearchParams`를 실제로 어떻게 활용하는지 간단한 예제와 함께 살펴볼게요!

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

### Static Rendering(정적 렌더링)

라우트(route)가 정적으로 렌더링될 때, `useSearchParams`를 호출하면 해당 클라이언트 컴포넌트(Client Component) 트리가 가장 가까운 `Suspense` 경계까지 클라이언트 사이드 렌더링으로 처리됩니다.

즉, 경로의 일부는 정적으로 미리 렌더링 하면서, `useSearchParams`를 사용하는 동적 부분만 클라이언트에서 렌더링할 수 있다는 얘기죠. 이 덕분에 초기 로드 속도를 유지하면서도 URL 쿼리 파라미터 같은 동적인 부분을 처리할 수 있게 돼요.

그래서 `useSearchParams`를 쓰는 클라이언트 컴포넌트는 꼭 `Suspense` 경계로 감싸는 걸 추천합니다. 이렇게 하면 그 위쪽에 위치한 클라이언트 컴포넌트들은 정적으로 렌더링되고, 초기 HTML과 함께 전달될 수 있어서 SEO나 퍼포먼스에 도움을 주죠.  

간단한 예시를 들어보자면,  

```jsx
import { Suspense } from 'react';
import ClientComponent from './ClientComponent';

function Page() {
  return (
    <Suspense fallback={<Loading />}>
      <ClientComponent />
    </Suspense>
  );
}
```

이렇게 하면 `ClientComponent`가 `useSearchParams`를 사용해도, `Page` 상단에 있는 컴포넌트들은 정적 렌더링 됩니다.

> 참고로, React의 `Suspense`는 비동기 컴포넌트 로딩이나 데이터를 기다릴 때 UI를 깔끔하게 처리할 수 있게 도와줘서 사용자 경험이 훨씬 좋아져요.

요약하면, 정적 렌더링과 동적 쿼리 파라미터를 함께 사용하고 싶다면:

- `useSearchParams` 사용하는 컴포넌트는 클라이언트 컴포넌트여야 한다.
- 그리고 해당 컴포넌트를 `Suspense` 경계로 감싸야 한다.

이게 현재 Next.js나 React 기반 프로젝트에서 가장 권장하는 방식입니다.

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

이번에는 Next.js에서 동적 렌더링(Dynamic Rendering)과 Suspense를 활용하는 예제를 소개할게요. 이걸 통해 클라이언트 측에서 URL 파라미터를 받아서 동적으로 화면에 보여주는 방법과, 로딩 대기 상태를 관리하는 방법을 함께 배워볼 수 있습니다.

---

### 예제 코드 설명

```js
'use client'

import { useSearchParams } from 'next/navigation'

export default function SearchBar() {
  const searchParams = useSearchParams()

  const search = searchParams.get('search')

  // 정적 렌더링 사용 시 서버에서는 출력되지 않아요
  console.log(search)

  return <>Search: {search}</>
}
```

일단 `SearchBar` 컴포넌트에선 `'use client'` 선언을 통해 클라이언트 전용 컴포넌트임을 알려주고 있어요. 그러면서 `useSearchParams` 훅을 이용해 URL 쿼리 파라미터에서 `search` 값을 받아오죠. 여기서 중요한 점은 이 코드 내부의 `console.log(search)`는 클라이언트에서만 찍힌다는 것! 즉, 정적인 서버 렌더링 시에는 실행되지 않아서 서버 로그엔 표시되지 않습니다.

---

```js
import { Suspense } from 'react'
import SearchBar from './search-bar'

// Suspense의 fallback으로 보여질 컴포넌트
function SearchBarFallback() {
  return <>placeholder</>
}

export default function Page() {
  return (
    <>
      <nav>
        <Suspense fallback={<SearchBarFallback />}>
          <SearchBar />
        </Suspense>
      </nav>
      <h1>Dashboard</h1>
    </>
  )
}
```

두 번째 코드에서는 React의 `Suspense` 컴포넌트를 사용하고 있어요. Suspense는 비동기 작업을 기다리는 동안 사용자에게 보여줄 대체 UI (`fallback` 속성으로 지정된 컴포넌트)를 만들어주는데요.

- `SearchBarFallback`은 초기 로딩 시 `"placeholder"` 라는 간단한 텍스트를 보여줍니다.
- `SearchBar`가 클라이언트에서 필요한 데이터를 받아오면, `"placeholder"`가 실제 검색어를 보여주는 `SearchBar` 컴포넌트로 자연스럽게 교체되죠.

---

| 주요 개념          | 설명                                             |
|-----------------|----------------------------------------------|
| `'use client'` 선언  | 컴포넌트를 클라이언트 전용으로 만들어서 클라이언트 사이드 훅 사용 가능 |
| `useSearchParams` | URL 쿼리 파라미터를 읽는 훅                            |
| `Suspense`        | 비동기 컴포넌트 로딩 시 대체 UI 표시                      |
| `fallback`        | Suspense가 로딩 중일 때 보여줄 컴포넌트 혹은 UI              |

---

### 좀 더 알아보기

- **왜 Suspense를 사용할까?**  
  서버에서 정적으로 페이지를 렌더링할 때는 아직 클라이언트 데이터가 준비되어 있지 않습니다. 이때 Suspense는 *로딩 상태*를 관리해주어 사용자 경험(UX)을 더 좋게 만들어줍니다. 페이지가 순간적으로 비어 있거나 깜빡거리지 않고, 부드럽게 동작하도록 도와주는 거죠.

- **useSearchParams 활용 팁**  
  이 훅은 클라이언트에서만 동작하기 때문에 서버측 렌더링에서는 사용할 수 없어요. 그래서 `'use client'` 선언이 반드시 필요합니다. 만약 페이지에서 쿼리 파라미터를 서버에서도 이용하고 싶다면 `getServerSideProps`나 Next.js의 서버 측 데이터 패칭 방법을 활용해야 합니다.

- **정적 렌더링과 클라이언트 렌더링의 차이**  
  저 코드에서 `console.log(search)`는 클라이언트에서만 보이고 서버 콘솔에는 찍히지 않는다고 했죠? 이 부분도 큰 의미가 있습니다. 서버에서는 빌드 시점에 동작해 정적 HTML을 만들어내고, 클라이언트에서 hydrate되면서 실제 유저가 보는 인터랙티브 상태로 바뀌는 거예요.

---

### 마무리

이번 글에서는 Next.js에서 동적 URL 파라미터를 클라이언트에서 받아서 화면에 반영하는 방법과, React Suspense를 활용해 로딩 UI를 매끄럽게 처리하는 방법을 정리해봤어요. 간단하지만 앞으로 다양한 인터랙티브 UI를 구현할 때 매우 유용할 거예요!

궁금한 점 있으면 댓글 달아 주세요~ 같이 배워나가요! 😊

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

동적으로 렌더링되는 라우트에서는 `useSearchParams`가 클라이언트 컴포넌트의 초기 서버 렌더링 시에도 사용 가능하다는 점, 알고 계셨나요?

예를 들어, 아래와 같은 컴포넌트가 있다고 해봅시다.

```jsx
'use client'
 
import { useSearchParams } from 'next/navigation'
 
export default function SearchBar() {
  const searchParams = useSearchParams()
  const search = searchParams.get('search')
 
  // 이 값은 초기 서버 렌더링 시에도 로그에 찍히고,
  // 이후 클라이언트 내비게이션 시에도 찍힙니다.
  console.log(search)
 
  return <>Search: {search}</>
}
```

여기서 `useSearchParams`는 URL에 포함된 쿼리 파라미터를 쉽게 가져올 수 있게 도와줍니다. `searchParams.get('search')`를 통해 `?search=키워드` 같은 쿼리 값을 추출할 수 있죠.

그리고 이 `SearchBar`를 포함하는 페이지는 아래처럼 구성할 수 있어요:

```jsx
import SearchBar from './search-bar'
 
// 페이지가 항상 동적으로 렌더링되게 강제합니다.
export const dynamic = 'force-dynamic'
 
export default function Page() {
  return (
    <>
      <nav>
        <SearchBar />
      </nav>
      <h1>Dashboard</h1>
    </>
  )
}
```

여기서 `export const dynamic = 'force-dynamic'`는 이 페이지가 항상 서버에서 동적으로 렌더링되도록 Next.js에 알려주는 역할을 합니다. 덕분에 쿼리 파라미터 값이 실시간으로 반영되어, `SearchBar` 컴포넌트도 그에 맞게 동작합니다.

---

### 참고로 알아두면 좋은 점

- **서버컴포넌트 vs 클라이언트컴포넌트**: `useSearchParams`는 클라이언트 컴포넌트에서만 사용할 수 있습니다. 서버 컴포넌트에서 사용하면 오류가 발생해요.
- **검색어 반영하기**: 만약 검색어를 URL에 반영하고 싶다면, `useRouter`의 `push`나 `replace` 메서드도 활용할 수 있습니다.
- **초기 렌더링과 클라이언트 내비게이션**: 이 예제처럼 쿼리 파라미터가 서버 렌더링 시점에도 유효해 로그를 찍을 수 있다면, 사용자 경험이 더 부드럽고 빠릅니다.

이런 방식을 적용하면 Next.js 앱에서 URL 쿼리 파라미터를 다루는 작업이 한결 간편해지고, 서버와 클라이언트를 아우르는 일관된 데이터 접근이 가능해져요!

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

> 알아두면 좋은 팁: 동적 라우트 세그먼트 설정 옵션을 `force-dynamic`으로 지정하면 동적 렌더링을 강제로 수행할 수 있어요.

### 서버 컴포넌트

#### 페이지에서 (Pages)

서버 컴포넌트인 페이지에서 검색 파라미터(search params)를 사용하려면, `searchParams`라는 prop을 활용하면 됩니다.

---

여기서 잠깐!  
Next.js 같은 프레임워크를 쓸 때, 페이지 컴포넌트에 `searchParams`가 기본적으로 들어와서 URL 쿼리 스트링을 쉽게 다룰 수 있어요. 예를 들어, `?q=nextjs` 같은 검색어를 받아서 필터나 검색 기능에 바로 사용할 수 있죠.

또, `force-dynamic` 옵션을 활용하면 빌드 시점에 정적으로 미리 렌더링하는 게 아니라, 매 요청마다 서버에서 데이터를 가져와서 최신 상태를 유지할 수 있습니다. 사용자마다 다르게 보여줘야 하거나, 자주 바뀌는 콘텐츠를 동적으로 처리할 때 아주 유용해요!

필요하다면, 다음처럼 설정해보세요:

```js
export const dynamic = 'force-dynamic';
```

이걸 쓰면 해당 페이지가 항상 서버에서 최신 데이터를 받아서 렌더링됩니다.

이렇게 하면 사용자 경험도 더 좋아지고, SEO에도 유리할 수 있어요!

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

#### 레이아웃(Layouts)

페이지와는 달리, 레이아웃(Layouts, 서버 컴포넌트)은 `searchParams`라는 prop을 받지 않아요. 그 이유는, 공유 레이아웃은 내비게이션 중에 재렌더링되지 않아서, 만약 `searchParams`를 받는다면 탐색 간에 오래된(즉, 구버전의) 검색 파라미터가 남아있을 수 있기 때문이에요. 이 부분에 대해서는 좀 더 자세한 설명도 있으니 참고하면 좋아요.

대신에, 페이지 컴포넌트에서는 `searchParams` prop을 받아서 사용하거나, 클라이언트 컴포넌트에서 `useSearchParams` 훅을 사용해서 최신 검색 파라미터를 받아오는 것을 권장해요. 클라이언트 컴포넌트는 브라우저에서 최신 상태로 다시 렌더링되기 때문에 항상 최신의 `searchParams` 값을 가져올 수 있답니다.

---

## 예제

(예제 코드를 첨부하면 더 이해하기 쉬울 것 같아요! 마크다운 테이블 등으로 정리해서 보여주면 좋고, 필요하면 제가 도와드릴게요.)

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

### searchParams 업데이트하는 방법

Next.js에서 `useRouter`나 `<Link>`를 이용해 새로운 `searchParams`를 쉽게 설정할 수 있어요. 이렇게 네비게이션이 이루어지면 해당 페이지(`page.js`)에서 업데이트된 `searchParams`를 props로 바로 받을 수 있답니다.

아래 예제 코드를 한번 볼게요! 이 코드는 클라이언트 컴포넌트에서 현재 URL의 쿼리스트링을 가져오고, 특정 파라미터를 업데이트해서 새로운 쿼리스트링을 만든 다음 라우터로 이동하거나 링크를 만들어 줍니다.

```jsx
'use client'

import { useRouter, usePathname, useSearchParams } from 'next/navigation';
import { useCallback } from 'react';
import Link from 'next/link';

export default function ExampleClientComponent() {
  const router = useRouter();
  const pathname = usePathname();
  const searchParams = useSearchParams();

  // 현재 쿼리스트링에 새 key/value를 합쳐서 새로운 쿼리문자열 생성
  const createQueryString = useCallback(
    (name, value) => {
      const params = new URLSearchParams(searchParams.toString());
      params.set(name, value);

      return params.toString();
    },
    [searchParams]
  );

  return (
    <>
      <p>Sort By</p>

      {/* useRouter를 써서 버튼 클릭시 이동하기 */}
      <button
        onClick={() => {
          // 현재 경로에 ?sort=asc 쿼리가 붙음
          router.push(pathname + '?' + createQueryString('sort', 'asc'));
        }}
      >
        ASC
      </button>

      {/* Link를 이용해 바로 이동 링크 만들기 */}
      <Link href={pathname + '?' + createQueryString('sort', 'desc')}>
        DESC
      </Link>
    </>
  );
}
```

---

#### 조금만 더 설명을 보태자면!

- `useSearchParams()`는 현재 URL의 쿼리 파라미터를 `URLSearchParams` 형태로 반환해 줘요. 그래서 손쉽게 `.set()`, `.get()`, `.has()` 같은 메서드를 쓸 수 있죠.
- `useCallback`으로 함수 메모이제이션을 해서 불필요한 재생성을 막아 성능 최적화를 도와줍니다.
- `router.push()`는 자바스크립트 코드에서 프로그래밍적으로 경로 변경할 때 사용해요.
- `<Link>`는 마크업에서 하이퍼링크를 만들 때 활용하고요.
- 이렇게 쿼리스트링을 관리하면 페이지 상태(예: 필터, 정렬 등)를 URL에 명확히 표현할 수 있어 SEO에도 이롭답니다.

---

### 정리

| 메서드        | 역할                          | 언제 쓰면 좋을까?                   |
|---------------|-------------------------------|-----------------------------------|
| `useRouter()` | 프로그래밍적으로 네비게이션하기 | 버튼 클릭 시 동적인 경로 변경 필요할 때 |
| `<Link>`      | 하이퍼링크를 JSX에서 만들기    | 정적인 링크나 사용자 클릭 이동 시      |
| `useSearchParams()` | URL 쿼리 정보 읽고 조작하기       | 현재 쿼리 상태를 알고 싶을 때           |

정리해 보니 쾌적한 라우팅과 상태 관리를 위해 정말 자주 쓰게 될 기능들이죠? 여러분도 꼭 코드에 적용해서 깔끔한 페이지 전환 경험 만들어 보세요!

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

| Version   | Changes                      |
|-----------|------------------------------|
| `v13.0.0` | `useSearchParams`가 추가됨  |

위 표는 React Router 라이브러리의 변화 중 하나인 `v13.0.0` 버전에서 `useSearchParams`라는 훅이 새로 도입됐다는 내용이에요. 

`useSearchParams`는 URL의 쿼리 파라미터를 쉽게 읽고 수정할 수 있게 도와주는 훅이에요. 예를 들어, 사용자가 검색어를 URL 쿼리에 남겨두고 싶을 때, 직접 복잡한 쿼리 스트링을 다룰 필요 없이 React처럼 상태 관리하듯이 다룰 수 있답니다.

직접 사용해보면 다음과 같이 쓸 수 있어요:

```jsx
import { useSearchParams } from 'react-router-dom';

function SearchPage() {
  const [searchParams, setSearchParams] = useSearchParams();
  const query = searchParams.get('q') || '';

  const handleInputChange = (e) => {
    setSearchParams({ q: e.target.value });
  };

  return (
    <input type="text" value={query} onChange={handleInputChange} />
  );
}
```

여기서 `q`는 쿼리 파라미터 키이고, 사용자가 입력할 때마다 URL이 자동으로 업데이트돼서 뒤로가기도 편해지고, 검색 결과 공유도 쉬워져요.

이 기능 덕분에 URL과 상태를 동기화하는 작업이 훨씬 편리해졌으니 React Router를 사용한다면 꼭 알아두시길 추천해요!