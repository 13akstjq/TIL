---
title: "Nextjs 13에서 usePathname으로 현재 URL 경로 쉽게 가져오는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:51
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "usePathname"
link: "https://nextjs.org/docs/app/api-reference/functions/use-pathname"
isUpdated: false
---


# usePathname 훅 소개

`usePathname`은 Next.js에서 제공하는 훅으로, 현재 URL의 경로(pathname)를 읽어올 수 있는 **클라이언트 컴포넌트** 전용 훅이에요.

---

```js
'use client'

import { usePathname } from 'next/navigation'

export default function ExampleClientComponent() {
  const pathname = usePathname()
  return <p>Current pathname: {pathname}</p>
}
```

위 예제처럼 `usePathname`을 사용하면 현재 페이지의 경로를 쉽게 가져와서 화면에 출력할 수 있답니다.

---

### 왜 꼭 클라이언트 컴포넌트에서만 사용할까?

`usePathname` 훅은 **클라이언트 컴포넌트에서만** 사용해야 해요. 여기서 한 가지 오해하지 말아야 할 점은, 클라이언트 컴포넌트를 사용하는 것이 성능 저하(de-optimization)가 아니라는 거예요! 오히려 Next.js의 서버 컴포넌트 아키텍처 안에서 클라이언트 컴포넌트는 중요한 역할을 하죠.

즉, 서버 컴포넌트(Server Component)가 서버에서 렌더링하고, 클라이언트 컴포넌트(Client Component)는 브라우저에서 동적인 UI와 사용자 이벤트를 담당하는 역할 분담 덕분에 앱 성능을 최적화하는 구조랍니다.

---

### 참고로 알아두면 좋은 점

- `usePathname`으로 가져오는 경로는 쿼리 스트링이나 해시(#)를 포함하지 않고 순수한 경로만 반환해요. 예를 들어, `https://example.com/posts?id=123` 라면 `/posts`가 반환됩니다.
- 페이지 전환 시 URL이 변경되더라도 `usePathname`이 반환하는 값이 자동으로 업데이트되어 항상 최신 경로를 얻을 수 있어요.
- 만약 쿼리 스트링이나 해시까지 필요하다면 `useSearchParams` 같은 훅도 함께 활용해 보는 걸 추천해요.

이처럼 `usePathname`은 페이지 위치에 따라 UI를 다르게 표현해야 하는 상황에서 아주 유용하니, Next.js 프로젝트에서 꼭 숙지해 두시면 좋습니다!

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

예를 들어, Client Component에서 usePathname을 사용할 경우 초기 페이지 로드 시 HTML로 렌더링 됩니다. 이후에 다른 경로로 이동할 때 이 컴포넌트는 다시 받아올 필요가 없어요. 한 번 클라이언트 자바스크립트 번들로 다운로드되면, 현재 상태(state)에 맞게 다시 렌더링됩니다.

> 참고할 점:
서버 컴포넌트에서 현재 URL을 읽는 것은 지원하지 않아요. 이게 의도된 설계인데, 덕분에 페이지 간 이동 시 레이아웃 상태를 유지할 수 있어요.

### 호환성 모드:
- usePathname은 fallback 경로를 렌더링할 때나, Next.js가 자동으로 정적 최적화한 pages 디렉토리의 페이지에서는 null을 반환할 수 있어요.
- next.config.js에서 리라이트(rewrites)를 쓰거나, Middleware와 함께 usePathname을 사용할 때는 useState와 useEffect도 같이 써야 합니다. 그렇지 않으면 hydration mismatch 오류가 발생할 수 있거든요.
- 프로젝트 내에 app 디렉토리와 pages 디렉토리가 모두 존재하면, Next.js가 자동으로 타입을 업데이트해줍니다.

## 파라미터 사용법

```js
const pathname = usePathname()
```

---

### 조금 더 알려드리자면

- usePathname 훅은 클라이언트 측에서 현재 경로를 쉽게 얻어올 때 아주 유용해요. 예를 들어, 메뉴에서 현재 위치에 따라 스타일을 다르게 하거나, 페이지 이동 시에 조건부 렌더링을 하고 싶을 때 딱 좋죠.
- 서버 컴포넌트는 URL 정보에 의존하지 않고 렌더링되기 때문에 처음 로드가 더 빠르고, 레이아웃 재사용성이 높아지는 장점이 있어요.
- 하지만 서버 컴포넌트에서 현재 URL을 다뤄야 한다면, 쿼리 파라미터 등을 props로 넘겨주거나 클라이언트 컴포넌트를 활용하는 등 우회하는 방식을 사용해야 합니다.

이렇게 usePathname과 Server/Client 컴포넌트의 역할 차이를 이해하면 Next.js의 앱 라우팅과 렌더링 전략을 더 잘 활용할 수 있을 거예요!

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

usePathname은 파라미터를 받지 않는 훅(hook)입니다.

## 반환값

usePathname은 현재 URL의 경로(pathname)를 문자열로 반환해줘요. 예를 들어 볼게요:

| URL                | 반환값           |
|--------------------|------------------|
| `/`                | `'/'`            |
| `/dashboard`       | `'/dashboard'`   |
| `/dashboard?v=2`   | `'/dashboard'`   |
| `/blog/hello-world`| `'/blog/hello-world'` |

보시다시피 쿼리 스트링(`?v=2` 같은 부분)은 제외하고 경로 부분만 딱 가져온답니다. 이 기능 덕분에 현재 사용자가 어느 페이지에 있는지 쉽게 알 수 있고, 라우팅 처리할 때 아주 유용해요.

추가로, usePathname은 Next.js 같은 리액트 프레임워크에서 공식적으로 지원되는 훅이고, 클라이언트 컴포넌트에서만 사용 가능하다는 점 참고하세요!

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

## 예제

### 라우트 변경에 따라 무언가 하기

```js
'use client'

import { usePathname, useSearchParams } from 'next/navigation'
import { useEffect } from 'react'

function ExampleClientComponent() {
  const pathname = usePathname()
  const searchParams = useSearchParams()

  useEffect(() => {
    // 라우트(pathname)나 쿼리스트링(searchParams)이 바뀔 때마다 실행되는 코드 작성
    console.log('경로 또는 쿼리 파라미터가 변경되었습니다.')
  }, [pathname, searchParams])
}
```

| Version  | Changes               |
|----------|-----------------------|
| v13.0.0  | `usePathname`가 도입됨 |

---

이 예제는 Next.js의 `app` 디렉토리 기반 라우팅에서 클라이언트 컴포넌트가 라우트 경로나 쿼리 파라미터에 반응해 동작할 수 있게 해주는 방법입니다. `usePathname`과 `useSearchParams` 훅은 각각 현재 경로와 쿼리 스트링을 가져와서, `useEffect`의 의존성 배열에 넣어 경로나 쿼리가 바뀔 때마다 특정 작업을 실행할 수 있도록 도와줍니다.

🚀 참고로 `useSearchParams`는 반환하는 값이 `URLSearchParams` 객체와 유사한데, 내부적으로 Next.js가 리렌더링 최적화를 위해 커스텀한 부분이 있으니 참고하세요. 그리고 `useEffect`를 사용할 때는 클라이언트 컴포넌트임을 명시하는 `'use client'`가 꼭 필요합니다.

이런 방식은 사용자가 URL을 변경하면서도 데이터 페칭이나 UI 업데이트가 자동으로 반영되길 원할 때 유용하게 활용할 수 있으니까 꼭 기억해 두면 좋습니다!