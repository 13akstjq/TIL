---
title: "Next.js 15에서 useRouter로 현재 URL의 query 파라미터 쉽게 가져오는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:53
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "useRouter"
link: "https://nextjs.org/docs/app/api-reference/functions/use-router"
isUpdated: false
---


# useRouter

`useRouter` 훅은 클라이언트 컴포넌트에서 프로그래밍 방식으로 라우트를 변경할 수 있게 해줍니다.

> 추천하는 방법: 특별한 이유가 없다면 `Link` 컴포넌트를 사용하는 것을 권장해요. `useRouter`는 버튼 클릭 같은 이벤트 핸들러 내에서 라우팅을 처리해야 할 때 유용합니다.

```js
'use client'

import { useRouter } from 'next/navigation'

export default function Page() {
  const router = useRouter()

  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  )
}
```

### 추가 팁!

- `router.push()`는 특정 경로로 이동할 때 사용해요.
- `router.replace()`도 있는데, 이건 현재 기록을 새 기록으로 대체해서 뒤로 가기 시 이전 페이지로 못 돌아가게 할 때 쓰입니다.
- `replace` 기능이 필요할 땐 `router.replace('/path')`를 사용해 봐요.
- `useRouter`는 클라이언트 컴포넌트 전용 훅이라는 것도 기억하세요. 서버 컴포넌트에서는 사용할 수 없습니다.

간단하게 버튼 클릭만으로 페이지 이동이 가능해서, 사용자 액션에 따른 동적인 라우팅 구현할 때 매우 편리하답니다!

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

## useRouter() 훅 쉽게 정리하기!

Next.js에서 클라이언트 라우팅을 다룰 때 꼭 알아야 하는 `useRouter()` 훅! 이 훅을 사용하면 페이지 전환, 새로고침, 뒤로가기 등 라우터 관련 동작들을 손쉽게 할 수 있답니다.

아래 표를 보면서 주요 메서드들을 쉽게 정리해봤어요.

| 메서드 | 설명 |
|-------|-------|
| `router.push(href: string, scroll: boolean)` | 클라이언트 라우터 이동! 브라우저 히스토리에 새 주소를 추가해요. (페이지 이동 느낌) |
| `router.replace(href: string, scroll: boolean)` | 페이지 이동 같은데, 히스토리에는 새로 추가하지 않아요. (현재 페이지 교체) |
| `router.refresh()` | 현재 페이지를 새로고침! 서버에 다시 요청해서 데이터 갱신, React 서버 컴포넌트 다시 렌더링! 클라이언트 상태(예: `useState`)나 스크롤 위치는 그대로 유지해줘요. |
| `router.prefetch(href: string)` | 주소를 미리 로딩해서 페이지 이동을 더 빠르게 하도록 준비해요. |
| `router.back()` | 브라우저 history에서 이전 페이지로 이동 (뒤로가기) |
| `router.forward()` | 브라우저 history에서 다음 페이지로 이동 (앞으로가기) |

---

### 주의할 점! 🚨

`router.push` 나 `router.replace`에 신뢰할 수 없거나 필터링되지 않은 URL을 넣으면 XSS(크로스 사이트 스크립팅) 공격에 노출될 위험이 있어요. 예를 들어, `javascript:` URL을 넣으면 페이지 내에서 실행돼서 보안 문제를 일으킬 수 있죠. 그러니까 항상 URL을 검증하거나 정제해서 넣는 습관을 가지는 게 좋아요.

그리고 Next.js의 `<Link>` 컴포넌트는 사용자가 화면에 보는 순간 해당 경로를 자동으로 prefetch하기 때문에 보통 직접 `router.prefetch`를 호출할 일은 많지 않아요.

`router.refresh()`를 호출해도 fetch 요청이 캐시되어 있다면 똑같은 결과가 나올 수 있고, 쿠키나 헤더 같은 동적인 요소 때문에 서버 응답이 달라질 수도 있어요. 그래서 상황에 맞게 적절히 사용해야 합니다.

---

### next/router에서 useRouter로 마이그레이션하기

기존에 `next/router`에서 `useRouter`를 썼다면, 앱 라우팅 구조가 바뀌면서 `next/navigation` 패키지의 `useRouter()`를 써야 하는 경우가 있어요. 가장 큰 차이점은 서버 컴포넌트 지원과 라우팅 전략인데, 새로운 `useRouter`는 서버와 클라이언트 상태를 자연스럽게 연결해주고, 좀 더 React 18 기준에 맞춘 동작을 제공해 줍니다.

마이그레이션할 때는 중요한 메서드들의 파라미터가 조금 바뀔 수 있으니 Next.js 공식 문서를 잘 참고하세요!

---

### 개인적인 경험 한마디

요즘 Next.js에서 페이지 전환할 때 `router.push()` 와 `router.replace()`를 적절히 구분해서 사용하면 UX가 확실히 좋아요. 예를 들어, 검색 페이지에서 쿼리만 바꾸는 경우에는 replace로 히스토리를 쌓지 않고, 새 페이지로 이동할 땐 push를 쓰면 뒤로가기 시 사용자가 혼란스럽지 않죠.

또, `router.refresh()`는 상태 유지가 필요한 상황에서 데이터만 싹 다시 불러오고 싶을 때 정말 유용해요. React Query나 SWR 같은 데이터 페칭 라이브러리와 함께 쓰면 더욱 시너지!

오늘은 Next.js `useRouter()` 메서드들을 정리해봤는데, 실제 프로젝트에서 꼭 써보면서 경험 쌓아보세요~! 궁금한 점 있으면 댓글로 알려주세요. 함께 공부해봅시다! 😊

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

Next.js의 App Router를 사용하면서 알아두면 좋은 네비게이션 관련 변경사항들을 정리해봤어요. 기존에 많이 쓰던 `useRouter` 훅이나 `pathname`, `query` 같은 것들이 달라졌으니 참고하시면 좋아요!

---

### 주요 변경점

- `useRouter` 훅은 이제 **`next/router`**가 아니라 **`next/navigation`**에서 가져와야 해요.
- `pathname` 문자열이 없어지고, 대신 **`usePathname()`** 훅을 사용해서 현재 경로를 가져와요.
- `query` 객체도 없애고 **`useSearchParams()`** 훅으로 쿼리 파라미터를 다루게 되었어요.
- `router.events` 이벤트 리스너는 다른 방식으로 대체되었으니 아래에서 확인해 주세요.

---

추가로, 이전에는 `router.events`를 통해 라우팅 이벤트를 듣고 상태를 업데이트하는 경우가 많았는데, App Router에서는 이런 이벤트 관리도 좀 더 간단하고 직관적으로 바뀌었답니다.

더 자세한 내용과 전체 마이그레이션 가이드는 공식 문서를 참고하는 걸 추천드려요!

---

## 예시

### 라우터 이벤트 다루기 예제

| 이전 방식 (Old - `next/router`)        | 새로운 방식 (New - `next/navigation`)                |
|---------------------------------------|-----------------------------------------------------|
| `import { useRouter } from 'next/router';` | `import { useRouter } from 'next/navigation';`         |
| `const router = useRouter();`          | `const router = useRouter();`                         |
| `const path = router.pathname;`        | `import { usePathname } from 'next/navigation';`<br>`const path = usePathname();`       |
| `const query = router.query;`           | `import { useSearchParams } from 'next/navigation';`<br>`const searchParams = useSearchParams();`<br>`const myParam = searchParams.get('myParam');`      |
| `router.events.on('routeChangeStart', callback);` | 새로운 라우터 이벤트 방식 적용 (아래 공식 문서 참고) |

---

이전보다 더 함수형, 훅 중심으로 API가 개편된 느낌이죠? 뭔가 더 모던해졌고, React와도 자연스럽게 어울려서 쓰기 편해졌어요.

다음 프로젝트에서 Next.js App Router 쓸 때 참고해보시고, 궁금한 거 있으면 댓글이나 DM 주세요!

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

페이지가 변경되는 걸 감지하고 싶을 때, Next.js의 Client Component 훅인 usePathname과 useSearchParams를 조합해서 사용할 수 있어요. 예를 들어, 이렇게 하면 현재 URL이 바뀔 때마다 콘솔에 찍히게 할 수 있죠.

```js
'use client'

import { useEffect } from 'react'
import { usePathname, useSearchParams } from 'next/navigation'

export function NavigationEvents() {
  const pathname = usePathname()
  const searchParams = useSearchParams()

  useEffect(() => {
    const url = `${pathname}?${searchParams}`
    console.log(url)
    // 현재 URL을 활용하고 싶다면 여기서 작업 가능
    // ...
  }, [pathname, searchParams])

  return '...'
}
```

여기서 `usePathname()`은 현재 페이지의 경로(`/about`, `/product/1` 등)를 가져오고, `useSearchParams()`는 쿼리 스트링(`?id=123&ref=google` 같은 부분)을 가져와요. 두 값을 조합해서 완전한 URL을 만들 수 있어요.

그리고 이렇게 만든 `NavigationEvents` 컴포넌트를 레이아웃(layout)이나 다른 컴포넌트에 넣어서 페이지 전환 시 동작을 감지하게 할 수 있습니다. 예를 들어, 아래처럼 레이아웃 컴포넌트에 넣어주면 돼요:

```js
import { Suspense } from 'react'
import { NavigationEvents } from './NavigationEvents' // 경로는 실제 위치에 맞게

export default function Layout({ children }) {
  return (
    <html lang="en">
      <body>
        <NavigationEvents />
        {children}
      </body>
    </html>
  )
}
```

이 방법의 장점은, Next.js 13의 App Router에서 라우트가 변경될 때마다 자동으로 현재 경로와 쿼리 파라미터를 받아서 효과적으로 반응할 수 있다는 점입니다. 예를 들어, 페이지 전환 시마다 Google Analytics 같은 외부 서비스에 페이지뷰를 기록하거나, 특정 조건에 따라 UI를 변경하는 로직을 넣을 때 유용해요.

추가로, `useSearchParams()`가 반환하는 객체는 실제로 `URLSearchParams` 인스턴스라서, `.get('paramName')` 같은 메서드도 쓸 수 있어요. 예를 들어 특정 쿼리파라미터 값만 가져오고 싶다면 이렇게 하면 되죠:

```js
const category = searchParams.get('category')
console.log('현재 카테고리:', category)
```

요약하자면, 페이지 이동을 감지하고 싶을 때 Next.js의 `usePathname`과 `useSearchParams` 훅을 조합해서 쉽게 현재 경로와 쿼리를 감시할 수 있습니다. 이걸 커스텀 훅이나 컴포넌트로 만들어서 재사용하면 훨씬 편해지겠죠? 실제로 이런 기능을 구현해 보면서 원하는 로직을 붙여보세요!

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

> 참고할 점: `NavigationEvents` 컴포넌트가 Suspense 경계 안에 감싸져 있는데요, 이는 `useSearchParams()` 훅이 정적 렌더링 중에 가장 가까운 Suspense 경계까지 클라이언트 사이드 렌더링을 발생시키기 때문이에요. 자세한 내용을 확인해보세요.

### 스크롤 탑 이동 비활성화하기

Next.js에서는 기본적으로 페이지 간 이동할 때 스크롤이 맨 위로 자동 이동합니다. 근데 이게 항상 필요하지는 않죠? 예를 들어, 사용자가 페이지를 이동해도 그 위치를 유지하고 싶을 때가 있어요.

이럴 땐 `router.push()`나 `router.replace()`를 호출할 때 옵션으로 `{ scroll: false }`를 넘기면 스크롤 이동을 막을 수 있어요.

아래 예제를 참고하세요!

```jsx
'use client'

import { useRouter } from 'next/navigation'

export default function Page() {
  const router = useRouter()

  return (
    <button
      type="button"
      onClick={() => router.push('/dashboard', { scroll: false })}
    >
      Dashboard
    </button>
  )
}
```

버튼을 누르면 `/dashboard` 페이지로 이동하지만, 스크롤 위치는 그대로 유지됩니다. 이런 기능은 특히 긴 페이지를 자주 오가는 SPA에서 유용하게 쓸 수 있어요.

---

추가 팁!  
`router.push()`와 `router.replace()`는 둘 다 페이지 이동을 하지만, `push`는 히스토리에 새 엔트리를 추가하고, `replace`는 현재 엔트리를 대체합니다. 스크롤 유지 옵션은 두 메서드에서 똑같이 사용할 수 있으니 상황에 맞게 선택하세요!

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

| 버전      | 변경사항                                  |
|-----------|-----------------------------------------|
| `v13.0.0` | `next/navigation`에서 `useRouter`가 새로 도입됨 |

---

여기서 주목할 점은 Next.js가 13버전부터 라우팅 시스템에 변화를 줬다는 거예요. 기존에 `next/router`에서 제공하던 `useRouter` 훅이 이제는 `next/navigation`이라는 새로운 모듈로 이동했답니다. 

이렇게 바뀐 이유는 Next.js 팀이 서버 컴포넌트와 클라이언트 컴포넌트를 더 명확히 구분하고, 라우팅 관련 기능을 한층 더 최적화하기 위해서예요. 만약 여러분이 Next.js 13 버전으로 업그레이드하거나 새 프로젝트를 시작한다면, `useRouter`를 불러올 때 꼭 `next/navigation` 모듈에서 가져오는지 확인해보세요.

그리고 또 하나! 기존에 `next/router`를 사용하던 프로젝트를 마이그레이션 할 때는, API 차이가 있을 수 있으니 공식 문서나 마이그레이션 가이드를 참고하는 게 좋아요. 라이브러리나 플러그인이 아직 새 API를 완벽히 지원하지 않을 수도 있으니까요.

더불어 Next.js 13 버전에서는 앱 디렉토리(app directory)라는 새로운 폴더 구조가 등장했고, 이와 함께 라우터도 완전히 다시 설계되었으니 관심 있는 분들은 꼭 한 번 살펴보세요. React 서버 컴포넌트와 결합해서 정말 편리한 개발 경험을 제공합니다!