---
title: "Next.js 15로 싱글 페이지 애플리케이션(SPA) 쉽게 만드는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:38
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to build single-page applications with Next.js"
link: "https://nextjs.org/docs/app/guides/single-page-applications"
isUpdated: false
---


# Next.js로 싱글 페이지 애플리케이션(SPA) 만들기

Next.js는 싱글 페이지 애플리케이션(SPA) 구축을 완벽하게 지원해요.

빠른 라우트 전환을 위한 사전 페칭(prefetching), 클라이언트 쪽 데이터 가져오기, 브라우저 API 활용하기, 타사 클라이언트 라이브러리 연동, 정적 라우트 생성 등 다양한 기능이 다 포함되어 있죠.

만약 이미 SPA를 가지고 있다면, Next.js로 마이그레이션할 때 코드에 큰 변화를 줄 필요 없이 진행할 수 있어요. 그리고 필요에 따라 서버 기능을 점진적으로 추가해 나갈 수 있다는 점도 큰 장점입니다.

추가로, Next.js는 기본적으로 페이지 기반 라우팅 시스템을 제공해서, 라우트 관리를 훨씬 간편하게 할 수 있어요. React를 잘 다뤄왔다면 Next.js로 넘어오는 과정도 아주 자연스럽게 느껴질 거예요.

SPA를 만들 때 자주 고민하는 SEO 문제도 Next.js가 서버 사이드 렌더링(SSR)이나 정적 사이트 생성(SSG)을 지원하기 때문에 훨씬 수월하게 해결할 수 있답니다. 그래서 성능과 UX, SEO 모두 신경 써야 하는 프로젝트에 정말 적합하죠!

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

## SPA(싱글 페이지 애플리케이션)이란?

사실 SPA의 정의는 사람마다 조금씩 달라요. 여기서는 “엄격한 SPA” 기준으로 설명해볼게요.

- **클라이언트 사이드 렌더링(CSR)**: 앱이 한 개의 HTML 파일(보통 index.html)로 제공돼요. 다른 페이지로 이동하거나, 화면 전환, 데이터 요청 등 모든 것이 브라우저 안에서 자바스크립트가 처리해요.
- **전체 페이지 새로고침 없음**: 새로운 페이지를 서버에서 받아오는 대신, 현재 페이지의 DOM(문서 구조)를 자바스크립트가 바꿔주고, 필요한 데이터만 따로 받아와서 화면에 보여줘요.

즉, SPA는 한 번 페이지가 로드되면, 이후에는 전체 페이지 리로드 없이 화면이 바뀌는 웹 앱이라고 보면 돼요.

---

### 그런데 왜 엄격한 SPA는 어려울까?

SPA는 로딩 초반에 많은 자바스크립트를 한꺼번에 내려받아야 해서, 페이지가 제대로 인터랙티브해지기까지 시간이 걸릴 수 있어요. 그리고 클라이언트에서 데이터를 요청하는 순서, 즉 데이터 ‘워터폴’ 문제도 발생할 수 있는데, 이게 길어지면 사용자 경험이 좋지 않을 수 있죠.

이런 문제들을 개선하고 싶다면 Next.js 같은 프레임워크를 써보는 걸 추천해요. Next.js는 서버 사이드 렌더링(SSR)과 CSR을 적절히 섞어서 빠르고 효율적인 웹 앱 개발을 도와주거든요.

---

참고로, 요즘은 SSR, CSR, 그리고 정적 사이트 생성(SSG)까지 적절히 활용하며 사용자 경험과 SEO를 모두 챙기려는 시도가 많아지고 있어요. SPA라고 무조건 자바스크립트만 쓰는 게 아니라는 점도 꼭 기억하세요!

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

## 왜 Next.js를 사용해서 SPA를 만들까?

Next.js는 여러분이 작성한 JavaScript 코드를 자동으로 쪼개서(bundle splitting) 각각의 라우트마다 필요한 코드만 불러올 수 있게 해줘요. 덕분에 클라이언트 쪽에서는 쓸데없는 자바스크립트 코드를 안 불러와서 번들 크기도 줄고, 페이지 로딩 속도도 훨씬 빨라집니다.

또, Next.js의 `next/link` 컴포넌트는 라우트를 미리 불러오는(prefetch) 기능을 갖고 있어서, 딱딱 끊기지 않는 부드러운 SPA의 빠른 페이지 전환을 경험할 수 있어요. 그뿐만 아니라 URL이 앱의 상태를 계속 담고 있어서, 공유하거나 북마크하기도 편해지죠.

흥미로운 점은, Next.js는 처음부터 정적인 사이트로 시작할 수도 있고, 아무 서버 기능 없이 클라이언트 사이드에서만 렌더링되는 순수 SPA로도 사용할 수 있다는 거예요. 그런데 프로젝트가 점점 커지고 기능이 복잡해지면, 필요한 만큼만 서버 기능(예를 들어 React Server Components나 Server Actions 같은)을 점진적으로 도입할 수도 있어서 유연하답니다.

---

### 한눈에 보는 Next.js의 SPA 장점

| 장점                   | 설명                                                                                 |
|----------------------|------------------------------------------------------------------------------------|
| 자동 코드 분할           | 각 페이지별로 필요한 자바스크립트만 로딩해서 번들 크기를 줄이고, 로딩 속도를 높임                     |
| 라우트 프리패칭          | `next/link`가 미리 라우트를 불러와서 페이지 전환이 빠르고 부드러움                                   |
| URL 기반 상태 유지       | SPA처럼 빠르면서도 URL에 상태가 포함되어 공유와 북마크가 쉬움                                         |
| 유연한 서버 렌더링 지원    | 초기엔 정적 사이트나 클라이언트 렌더링 SPA로 시작 가능, 필요하면 점진적으로 서버 기능 추가 가능                |

---

간단히 말해, Next.js는 SPA의 좋은 점과 SSR(server-side rendering)의 장점을 절묘하게 섞어놨어요. 그래서 좀 더 규모가 큰 프로젝트에서 SPA를 관리하기에 훨씬 편하답니다. 

이런 장점 덕분에 요즘 많은 개발자들이 Next.js를 선택해 SPAs를 만들고 있어요! 혹시 Next.js로 SPA를 고민하고 있다면, 꼭 한번 써보라고 추천드리고 싶네요.

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

## 예시

이번에는 SPA(싱글 페이지 애플리케이션)를 만들 때 자주 사용되는 패턴들을 살펴보고, Next.js가 이 문제들을 어떻게 해결하는지 함께 알아보도록 할게요.

### React의 use 훅을 Context Provider 내에서 사용하기

저희는 데이터를 가져올 때 보통 상위 컴포넌트(또는 레이아웃)에서 데이터를 불러오고, 그 결과로 Promise를 반환한 뒤, 하위 클라이언트 컴포넌트에서 React의 use 훅으로 그 값을 해제하는 방식을 추천해요.

이 방법이 좋은 이유는, 데이터를 한 곳에서 통합 관리할 수 있고 컴포넌트 간에 쉽게 공유가 가능해진다는 점이에요. 또한 Next.js가 제공하는 서버 컴포넌트와 클라이언트 컴포넌트의 장점을 살릴 수 있어서 성능 최적화에도 도움이 된답니다.

여기서 중요한 점은, 서버에서 데이터를 미리 불러오고 그 상태를 클라이언트에서 재사용할 수 있게 하는 흐름 덕분에 불필요한 중복 요청을 줄일 수 있고, 사용자 경험이 자연스럽게 개선된다는 것! 다음에 실제 예제도 함께 볼게요.

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

Next.js는 서버에서 데이터를 미리 가져오는 작업을 빨리 시작할 수 있어요. 예를 들어, 이걸 루트 레이아웃(root layout)에서 한다고 생각해보면, 애플리케이션의 시작점에서 서버가 곧바로 클라이언트에게 응답을 스트리밍할 수 있다는 뜻이죠.

여기서 “데이터 fetching을 루트 레이아웃으로 끌어올린다(hoisting)”는 말은, Next.js가 앱 내 다른 어떤 컴포넌트보다 먼저 서버에서 데이터를 요청하게 만든다는 의미예요. 덕분에 클라이언트에서 서버로 여러 번 왕복하는 ‘워터폴 문제’를 없애고, 네트워크 호출도 줄여주죠. 또, 서버가 보통 데이터베이스와 가까운 위치에 있기에 성능 향상에도 큰 도움이 됩니다.

예를 들어, 다음과 같이 루트 레이아웃에서 getUser() 함수를 호출하지만, await 하지 않고 바로 Promise를 넘겨줄 수 있어요.

```jsx
import { UserProvider } from './user-provider'
import { getUser } from './user' // 서버 사이드 함수

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  let userPromise = getUser() // 여기서 await 하지 않아요

  return (
    <html lang="en">
      <body>
        <UserProvider userPromise={userPromise}>{children}</UserProvider>
      </body>
    </html>
  )
}
```

이렇게 하면 UserProvider 같은 하위 컴포넌트에서 Promise를 필요할 때까지 기다렸다가 데이터를 가져올 수 있죠. 이 패턴 덕분에 서버는 상황에 맞게 데이터를 빠르고 효율적으로 준비할 수 있어요.

> 참고로, Next.js 13부터 도입된 이 ‘룻트 레이아웃에서 데이터 fetching 미리 시작’ 방식은 React의 Suspense와도 잘 맞아서, 사용자 경험 개선에 큰 도움이 됩니다. 실제로 서버와 클라이언트의 역할을 명확히 분리하고, 렌더링 대기 프로세스도 자연스럽게 최적화해준답니다.

혹시 데이터 fetching 타이밍이나 React Suspense에 대해 더 궁금하면 알려주세요!

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

리액트에서 Promise를 클라이언트 컴포넌트에 prop으로 넘겨주는 패턴이 있어요. 특히, React context provider와 함께 사용하면 더 편리해지는데요. 이렇게 하면 커스텀 훅을 통해 클라이언트 컴포넌트 어디서든 쉽게 Promise에 접근할 수 있거든요.

예를 들어, User 정보를 담고 있는 Promise를 React context provider에 전달하는 코드를 한번 볼게요:

```jsx
'use client';

import { createContext, useContext, ReactNode } from 'react';

type User = any;
type UserContextType = {
  userPromise: Promise<User | null>;
};

const UserContext = createContext<UserContextType | null>(null);

export function useUser(): UserContextType {
  const context = useContext(UserContext);
  if (!context) {
    throw new Error('useUser는 UserProvider 안에서만 사용해야 합니다.');
  }
  return context;
}

export function UserProvider({
  children,
  userPromise
}: {
  children: ReactNode;
  userPromise: Promise<User | null>;
}) {
  return (
    <UserContext.Provider value={{ userPromise }}>
      {children}
    </UserContext.Provider>
  );
}
```

조금 더 설명을 덧붙이자면, UserProvider 컴포넌트는 userPromise를 받아서 context에 세팅해주고, useUser라는 커스텀 훅으로 이 Promise를 쉽게 사용할 수 있도록 도와줘요.

마지막으로 클라이언트 컴포넌트 내에서 useUser 훅을 호출한 다음 Promise를 풀어내는 방식은 이렇습니다:

```jsx
'use client';

import { useEffect, useState } from 'react';
import { useUser } from './UserProvider';

export default function UserProfile() {
  const { userPromise } = useUser();
  const [user, setUser] = useState(null);

  useEffect(() => {
    userPromise.then(setUser);
  }, [userPromise]);

  if (!user) return <div>로딩 중...</div>;

  return <div>안녕하세요, {user.name}님!</div>;
}
```

여기서 핵심은, context를 통해 Promise를 전달받고 클라이언트 컴포넌트가 그 Promise를 풀어서 사용자 데이터를 받아온다는 점이에요. 이렇게 하면 데이터 fetching과 상태 관리가 깔끔하게 분리되면서 유지보수도 수월해집니다.

추가 팁을 드리자면, 이 패턴은 서버에서 데이터를 미리 준비해서 클라이언트로 넘길 때도 유용해요. Next.js 같은 프레임워크에서 서버 컴포넌트가 데이터를 fetch하고 Promise를 클라이언트 컴포넌트로 넘기는 구조에 잘 어울립니다. 그러면 클라이언트에서는 loading 상태 관리나 데이터 fetching 코드를 간단히 처리할 수 있죠.

요약하자면:

| 장점 | 설명 |
| --- | --- |
| Context + Promise | 여러 컴포넌트에서 같은 Promise 접근 가능 |
| 커스텀 훅 활용 | 사용하기 편하고, 오류 관리도 쉬움 |
| 클라이언트 상태 관리 | Promise 풀어서 상태로 관리 가능 |
| 서버 - 클라이언트 연동 | 서버에서 데이터 준비 → 클라이언트에서 깔끔하게 소비 |

이 패턴을 프로젝트에 적용해보시면 데이터 흐름이 훨씬 명확해지는 걸 느끼실 거예요!

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

여러분, 리액트에서 비동기 데이터를 다룰 때 `use` 훅을 사용해보셨나요? 예를 들어, 위 코드처럼 `userPromise`를 `use(userPromise)`로 바로 받아서 사용할 수 있는데요, 이렇게 하면 컴포넌트가 데이터를 받을 때까지 ‘서스펜스(suspense)’ 상태가 됩니다.

이게 무슨 말이냐면, 리액트가 아직 데이터가 준비되지 않았을 때는 화면 일부를 잠시 렌더링하지 않고 기다렸다가, 데이터가 준비되면 그 부분만 다시 렌더링하는 ‘부분 하이드레이션(partial hydration)’을 할 수 있다는 거예요. 덕분에 페이지가 완전히 로드되기 전이라도 서버에서 미리 스트리밍한 HTML을 받아서 빠르게 화면을 보여주고, 나중에 자바스크립트가 로드되면 필요한 부분만 채워 넣는 거죠.

---

### SWR과 함께하는 SPA 데이터 패칭

그리고 한편으론, SPA 환경에서 데이터를 불러오는 작업이 자주 필요하죠? 여기서 SWR이라는 React 라이브러리가 정말 많이 쓰입니다. SWR은 ‘stale-while-revalidate’의 약자로, 데이터를 캐시해두고 백그라운드에서 재검증하는 방식을 채택해 사용자 경험을 극대화해요.

예를 들어, 여러분이 어떤 API를 호출해 데이터를 받으면, SWR은 기존 데이터를 먼저 보여주면서 동시에 최신 데이터를 받아 업데이트를 시도합니다. 이렇게 하면 화면이 번쩍이는 걸 막고, 항상 최신 데이터도 유지할 수 있죠. React의 최신 기능인 ‘서스펜스’와도 잘 어울려서, 점점 더 자연스럽게 비동기 데이터를 처리할 수 있게 되었습니다.

---

간단 정리하자면:

| 기능              | 설명                                                      |
|-------------------|---------------------------------------------------------|
| `use` 훅           | Promise를 바로 받아서 처리, 컴포넌트 서스펜스 상태 관리          |
| 부분 하이드레이션   | 서버에서 미리 렌더링한 HTML을 보여주고, 필요한 부분만 자바스크립트로 채우기 |
| SWR               | 데이터 캐싱 & 백그라운드 갱신으로 빠르고 최신 데이터 제공             |

앞으로 리액트에서 비동기와 데이터 페칭을 어떻게 효과적으로 다룰지 고민할 때, 이렇게 `use` 훅과 SWR 같은 도구들을 잘 활용하면 한결 수월해질 거예요! 궁금한 점 있으면 언제든 댓글로 알려주세요. :)

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

SWR 2.3.0과 React 19+를 사용하면, 기존에 클라이언트에서 SWR로 데이터를 가져오던 코드를 점진적으로 서버 기능과 함께 사용해볼 수 있어요. 쉽게 말해, 위에서 말한 use() 패턴을 추상화한 거라고 보면 됩니다. 즉, 데이터 패칭을 클라이언트 쪽에서 하기도 하고 서버 쪽에서 하기도 하거나, 둘 다 섞어서 사용할 수 있다는 거죠!

예를 들어,

- 클라이언트 전용: `useSWR(key, fetcher)`  
- 서버 전용: `useSWR(key)` + RSC(React Server Components)에서 제공하는 데이터  
- 혼합 사용: `useSWR(key, fetcher)` + RSC 데이터  

이렇게 다양하게 활용할 수 있습니다.

실제 예제를 보면, 애플리케이션의 최상위 컴포넌트를 `SWRConfig` 컴포넌트로 감싸고 `fallback` 값을 넣어 서버에서 미리 데이터를 준비해두게 할 수 있어요.

```jsx
import { SWRConfig } from 'swr'
import { getUser } from './user' // 서버 사이드 함수 예시

export default function RootLayout({ children }) {
  return (
    <SWRConfig
      value={{
        fallback: {
          // 여기서 getUser() 결과를 바로 넘기는데,
          // getUser()를 await 하지 않고 Promise 상태로 넘겨요.
          // 그래야 실제로 이 데이터를 쓰는 컴포넌트가 렌더링될 때 suspend 하면서 데이터를 기다림
          '/api/user': getUser(),
        },
      }}
    >
      {children}
    </SWRConfig>
  )
}
```

여기서 중요한 포인트는 `getUser()`를 호출할 때 `await`를 하지 않는다는 점인데요. 이렇게 하면 컴포넌트에서 `useSWR('/api/user')`를 호출할 때, 서버에서 이미 준비된 Promise를 그대로 사용하게 되고, 해당 데이터가 준비될 때까지 React가 자동으로 대기(suspend)를 해줘요. 이게 React 19의 서버 컴포넌트와 SWR이 연동되는 굉장히 직관적이고 효율적인 방식입니다.

---

### SWR 2.3.0 & React 19+ 사용하는 팁!

- 이 방식을 쓰면 서버 사이드 데이터 페칭과 클라이언트 사이드 리페칭을 자연스럽게 혼합 가능  
- 클라이언트 캐시를 미리 서버에서 채우고 싶을 때 유용  
- 서버 컴포넌트와 클라이언트 컴포넌트 혼합 환경에서 데이터 일관성 유지가 쉬워짐  
- 성능 최적화에 큰 도움이 되며, 전체 페이지 로딩 UX 개선 가능!

만약 SWR을 쓰고 있고 앞으로 React 19+ 서버 컴포넌트를 시도해보고 싶다면, 이번 버전부터 좀 더 매끄럽게 데이터 관리를 할 수 있으니 꼭 한번 시도해보세요!

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

자, 이번에는 서버 컴포넌트와 클라이언트 컴포넌트 간에 데이터를 주고받을 때 SWR을 어떻게 깔끔하게 쓸 수 있는지 이야기해볼게요.

---

### 서버 컴포넌트에서 getUser() 함수 사용하기

서버 컴포넌트에서는 `getUser()` 같은 함수를 이용해서 **쿠키, 헤더, DB 접근** 등을 안전하게 할 수 있어요. 이 말은 별도의 API 라우트를 만들 필요가 없다는 뜻이죠!

즉, 서버에서 이미 사용자 데이터를 가져와서 그걸 클라이언트 컴포넌트에 바로 전달해줄 수 있다는 거예요.

---

### 클라이언트 컴포넌트에서 SWR로 데이터 재사용하기

클라이언트 컴포넌트는 `SWRConfig` 하위에서 `useSWR()`을 호출할 때 이전에 서버에서 미리 받아온 사용자 데이터를 **fallback**으로 사용할 수 있어요. 이때 SWR를 쓰는 코드는 이전과 똑같이 쓸 수 있답니다.

```js
'use client'

import useSWR from 'swr'

export function Profile() {
  const fetcher = (url) => fetch(url).then((res) => res.json())
  // 기존에 쓰던 SWR 코드 그대로!
  const { data, error } = useSWR('/api/user', fetcher)

  return (
    <div>
      {error && <p>사용자 데이터를 불러오지 못했어요.</p>}
      {!data ? <p>로딩중...</p> : <p>안녕하세요, {data.name}님!</p>}
    </div>
  )
}
```

---

### 미리 렌더링 된 fallback 데이터의 장점

- 서버에서 데이터를 미리 받아서 **초기 HTML에 포함**시키기 때문에 화면이 빠르게 뜨고, SEO에도 유리해요.
- 클라이언트에서 `useSWR`가 바로 이 fallback 데이터를 읽어서 재사용하고,
- SWR의 **폴링, 재검증, 캐싱 기능**은 클라이언트 측에서 계속 잘 동작해 SPA 같은 인터랙티브한 경험을 제공해줘요.

---

### 번거로운 조건문은 이제 그만!

이전에는 데이터를 가져오는 상태에 따라 `data === undefined`인지 체크하고 로딩 UI를 따로 처리해야 했지만, Next.js가 이제는 이런 fallback 데이터를 자동으로 관리해줘서 그런 조건문을 지워도 돼요.

- 데이터가 로드되는 동안에는 가장 가까운 `Suspense` 경계가 알아서 작동해서 로딩 상태를 처리해주고,
- 개발자는 UI 로직에만 집중할 수 있죠.

---

### 요약 정리

| 항목                         | 설명                                                            |
|----------------------------|---------------------------------------------------------------|
| 서버 컴포넌트의 getUser() | 서버에서 안전하게 사용자 데이터를 가져와 바로 컴포넌트에 사용할 수 있음       |
| 클라이언트 컴포넌트의 SWR | 서버에서 받은 데이터를 fallback으로 사용해 동일한 키로 데이터 재활용 가능      |
| 초기 fallback 데이터         | 서버에서 미리 받아와 HTML에 포함 → 클라이언트가 즉시 활용, SEO & UX 모두 좋아짐 |
| 개발 편의성                  | 데이터 undefined 체크 등 복잡한 로딩 처리 코드가 필요없음                    |

---

### 마지막 팁!

몇 가지 참고하면 좋은 점들!

- `SWRConfig`로 글로벌 설정을 해줄 때 `fallback` 데이터를 넘겨주면, 그 아래의 모든 컴포넌트에서 쉽게 같은 데이터를 쓸 수 있어요.
- `Suspense`를 꽤 적극적으로 활용하면 로딩 상태 관리가 훨씬 깔끔해져요.
- SWR은 기본적으로 클라이언트 전용이니, 서버에서 데이터를 미리 받아서 넘겨주는 형태가 매우 중요한 패턴이에요.

---

이렇게 서버와 클라이언트를 오가며 데이터를 매끄럽게 공유하면, 코드도 단순해지고 사용자에게 훨씬 부드러운 경험을 줄 수 있답니다. 다음 프로젝트에 꼭 적용해 보세요!

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

아래 표는 SWR, RSC, 그리고 RSC + SWR 각각의 특징을 비교한 내용입니다. 체크 표시(✅)는 해당 기능을 지원한다는 의미이고, 엑스 표시(❌)는 지원하지 않음을 나타냅니다.

| 기능                | SWR       | RSC       | RSC + SWR  |
|---------------------|-----------|-----------|------------|
| SSR 데이터           | ❌        | ✅        | ✅         |
| SSR 중 스트리밍       | ❌        | ✅        | ✅         |
| 요청 중복 제거        | ✅        | ✅        | ✅         |
| 클라이언트 사이드 기능 | ✅        | ❌        | ✅         |

- **SSR 데이터**: 서버 사이드 렌더링 시 미리 데이터를 가져와서 렌더링하는 기능입니다. RSC(리액트 서버 컴포넌트)는 SSR을 완벽히 지원하지만 SWR은 지원하지 않아요.
- **SSR 중 스트리밍**: 서버 렌더링을 하면서 데이터를 조금씩 점진적으로 보내주는 기능으로, 사용자 경험을 향상시킵니다. RSC는 이를 지원하지만 SWR은 지원하지 않습니다.
- **요청 중복 제거**: 동일한 요청을 중복해서 보내지 않고 하나로 통합해 서버 부담과 네트워크 낭비를 줄여줍니다. 세 방식 모두 지원합니다.
- **클라이언트 사이드 기능**: 클라이언트 상태 관리, 캐싱, 리페칭 등 클라이언트 기반 작업을 의미합니다. SWR과 RSC+SWR에서는 가능하지만 RSC 단독으로는 지원 못 해요.

---

### React Query와 Next.js를 활용한 SPA 구현

React Query는 클라이언트와 서버 양쪽에서 모두 사용할 수 있어서 Next.js와 함께 엄청 유용해요. 덕분에 전통적인 싱글 페이지 애플리케이션(SPA)뿐 아니라 Next.js의 서버 기능도 활용하면서 데이터 요청과 상태 관리를 효율적으로 할 수 있습니다.

예를 들어, 서버 측에서 프리페칭(pre-fetching)을 하거나 캐시된 데이터를 활용하고, 클라이언트에서는 동적인 데이터 갱신이나 리페칭(refetching) 기능을 부드럽게 처리할 수 있죠.

React Query 공식 문서에서 더 자세한 내용과 예제를 확인해 보세요!  
(https://react-query.tanstack.com/)

---

> **추가 팁!**  
> Next.js 13과 같은 최신 버전에서는 React Server Components(RSC)와 클라이언트 상태 관리 라이브러리들을 함께 조합하는 방식이 굉장히 각광받고 있어요. 상황에 맞게 SSR을 적극 활용하면서도 필요할 때는 클라이언트 단에서 쾌적한 UX를 제공할 수 있죠.  
> 이 표를 참고해서 여러분의 프로젝트에 맞는 최적의 데이터 페칭 전략을 찾아보시길 추천드립니다!

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

### 브라우저에서만 컴포넌트 렌더링하기

Next.js에서 클라이언트 컴포넌트는 기본적으로 빌드 시점에 미리 렌더링(prerendering)됩니다. 하지만 어떤 컴포넌트를 **오직 브라우저 환경에서만** 렌더링하고 싶다면, 즉 서버에서는 렌더링을 하지 않고 클라이언트에서만 불러오고 싶을 때가 있죠. 이런 경우 `next/dynamic`을 활용하면 됩니다.

```js
import dynamic from 'next/dynamic'

const ClientOnlyComponent = dynamic(() => import('./component'), {
  ssr: false, // 서버사이드 렌더링 비활성화
})
```

여기서 `ssr: false` 옵션이 핵심인데요, 이 옵션을 주면 해당 컴포넌트는 서버에서 렌더링하지 않고 클라이언트(브라우저)에서만 렌더링됩니다.

---

#### 왜 이런 방식이 필요할까요?

예를 들어, 여러분이 `window`나 `document` 같은 브라우저 전용 API를 사용하는 서드파티 라이브러리를 쓸 때 문제가 됩니다. 서버는 이런 API가 없으니까 에러가 나죠. 그래서 클라이언트에서만 로딩하도록 강제하는 겁니다.

또 다른 방법으로는 `useEffect` 훅 안에서 이런 브라우저 API가 있는지 체크하고, 없으면 `null`이나 로딩 컴포넌트를 반환해 미리 렌더링 상태를 조절하는 방식도 있습니다.

```js
import { useEffect, useState } from 'react'

function BrowserOnlyComponent() {
  const [isClient, setIsClient] = useState(false)

  useEffect(() => {
    setIsClient(true)
  }, [])

  if (!isClient) {
    return null // 서버나 렌더링 초기 상태에서는 아무것도 보여주지 않음
  }

  return <YourComponent />
}
```

이렇게 하면 서버에서는 렌더링하지 않고, 클라이언트 환경이 됐을 때 컴포넌트가 나타나게 됩니다.

---

### 정리하자면

- `next/dynamic`을 쓰면 SSR(Server Side Rendering)을 끄고 클라이언트에서만 컴포넌트를 불러올 수 있어요.
- 브라우저 전용 API를 쓰는 라이브러리나 컴포넌트에 유용하죠.
- 아니면 `useEffect`로 브라우저 여부를 체크해서 조건부 렌더링하는 방법도 있습니다.

이 팁은 Next.js와 같은 SSR 프레임워크에서 개발할 때 자주 쓰게 되니 잘 기억해두세요! 😉

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

### 클라이언트에서의 Shallow Routing(얕은 라우팅) 이해하기

만약 기존에 Create React App이나 Vite처럼 전통적인 SPA(싱글 페이지 애플리케이션)를 쓰다가 Next.js로 옮겨 오는 중이라면, 아마 URL 상태만 바꾸는 얕은 라우팅(shallow routing)을 사용한 코드가 있을 거예요. 이 방법은 Next.js의 기본 파일 시스템 기반 라우팅을 쓰지 않고도, 애플리케이션 내에서 뷰 전환을 직접 컨트롤할 때 꽤 유용합니다.

Next.js에서는 브라우저의 페이지를 새로 고침하지 않고도 주소(URL) 상태를 업데이트할 수 있게, 네이티브 `window.history.pushState`와 `window.history.replaceState` 메서드를 사용할 수 있도록 지원해 주는데요.  
이 두 메서드는 브라우저의 히스토리 스택을 수정해 주지만, 페이지 전환은 없고 URL만 바뀌고 싶을 때 아주 딱 맞아요.

흥미로운 점은, Next.js 라우터가 이 `pushState`와 `replaceState` 호출을 잘 인식해서, 내부적으로 `usePathname`이나 `useSearchParams` 같은 훅으로 URL 상태를 동기화 할 수 있다는 겁니다. 그래서 여러분이 직접 `window.history`를 건드려도 Next.js 라우터 상태와 호환되니 걱정할 필요가 없어요.

---

#### 참고로 얕은 라우팅이란?
- **일반 라우팅:** URL이 바뀌면 Next.js가 서버 혹은 클라이언트에서 완전한 페이지 변화를 처리해서 렌더링을 다시 해요.
- **얕은 라우팅:** URL은 바뀌지만, 페이지 컴포넌트는 바뀌지 않고 특정 상태만 바꾸거나 부분적으로 업데이트할 때 쓰는 방식이에요.

이 방법은 특히 검색, 필터 등 화면 내 상태 상태변경을 URL과 연동해서 반영할 때 유용하니 알아두면 좋습니다!

---

### 간단한 예시 코드

```jsx
import { useRouter } from 'next/router';

export default function ShallowRoutingExample() {
  const router = useRouter();

  const handleClick = () => {
    // URL 쿼리만 바꾸고 페이지는 다시 로드하지 않고 싶을 때
    router.push('/mypage?tab=profile', undefined, { shallow: true });
  };

  return <button onClick={handleClick}>Change Tab</button>;
}
```

- `shallow: true` 옵션을 주면 Next.js가 페이지 컴포넌트를 다시 렌더링하지 않고 URL만 바뀝니다.
- 내부적으로 이 방법은 `window.history.pushState`를 사용하고 있어요.

---

### 덧붙여 알려주고 싶은 팁

- **`window.history` 메서드 직접 쓰기:**  
  직접 `window.history.pushState`를 호출해서 URL을 바꿔도 되지만, Next.js의 라우터 함수를 쓰는 게 유지보수와 리액티브 상태 관리에 더 유리해요.

- **SEO 같은 측면**에서 URL이 바뀌는 것은 좋은데, 얕은 라우팅은 페이지가 완전히 새로고침되지 않으니 서버 기반 데이터 변경이 필요하다면 주의해야 합니다.

- Next.js 13 이상부터는 App Directory와 같은 최신 기능들이 나오면서 라우팅 방식에 변화가 생겼는데, 얕은 라우팅 개념은 여전히 유용하니 필요할 때 잘 활용해보세요!

---

요약하면, Next.js에서 얕은 라우팅은 URL은 변경하면서 페이지를 리로드하지 않고 빠르게 상태 변화를 반영하는 좋은 방법입니다. 기존 SPA에서 익숙했던 히스토리 API를 활용하면서도 Next.js 라우터와 잘 연동되는 덕에, 여러분의 앱에 맞춰 유연하게 써먹으실 수 있어요!

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

여러분, Next.js에서 클라이언트 컴포넌트 내에서 URL 쿼리 스트링을 다루는 간단한 방법을 공유해드릴게요. 위 예제 코드는 `useSearchParams` 훅을 이용해 현재 URL의 쿼리 파라미터를 조회하고, 버튼 클릭으로 정렬 상태(오름차순/내림차순)를 바꾸면서 URL을 업데이트하는 방법이에요.

```jsx
'use client'

import { useSearchParams } from 'next/navigation'

export default function SortProducts() {
  const searchParams = useSearchParams()

  function updateSorting(sortOrder: string) {
    const urlSearchParams = new URLSearchParams(searchParams.toString())
    urlSearchParams.set('sort', sortOrder)
    window.history.pushState(null, '', `?${urlSearchParams.toString()}`)
  }

  return (
    <>
      <button onClick={() => updateSorting('asc')}>Sort Ascending</button>
      <button onClick={() => updateSorting('desc')}>Sort Descending</button>
    </>
  )
}
```

자, 여기서 핵심은 `useSearchParams`로 현재 URL 파라미터를 쉽게 가져오고, `window.history.pushState`를 이용해 페이지를 리로드하지 않고 주소창만 바꿔주는 점이에요. 이렇게 하면 페이지 상태는 유지하면서 URL 파라미터만 변경할 수 있죠. 사용자 경험이 훨씬 부드러워져요.

---

### Next.js 라우팅과 네비게이션 이해하기

Next.js에서는 `useSearchParams`, `useRouter` 같은 훅을 통해 클라이언트 사이드에서 라우팅 정보에 접근하거나 조작하는 게 매우 편리해요. `useSearchParams`는 URL 쿼리값을 읽고 쓸 수 있도록 도와주고, `useRouter`는 `push`, `replace`, `back` 같은 네비게이션 메서드를 써서 라우팅을 제어할 수 있게 해주거든요.

---

### 클라이언트 컴포넌트에서 Server Actions 사용하기

Next.js 13부터 도입된 Server Actions는 클라이언트 컴포넌트에서도 점진적으로 도입 가능해요. 예전에는 API 라우트를 호출하며 로딩 상태 관리, 에러 핸들링 같은 따로 처리해야 할 일이 많았는데 Server Actions로 간단해졌답니다.

React의 `useActionState` 같은 훅을 함께 쓰면, 로딩 중인지, 에러가 났는지 상태를 자동으로 알려줘서 UI 처리가 한결 쉬워져요. 그래서 클라이언트 컴포넌트 내에서도 서버 데이터를 바로 호출하고, 결과를 바로 반영하는 강력한 패턴으로 자리 잡고 있죠.

---

이런 기능들을 잘 활용하면 복잡한 상태 관리 없이도 매끄럽고 직관적인 사용자 인터페이스를 만들 수 있는데요, 특히 정렬, 필터링 같은 URL 기반 필터 기능 구축에 딱 맞습니다. 혹시 Next.js의 최신 라우팅이나 Server Actions에 대해 궁금한 점 있으면 또 공유할게요!

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

이번에는 처음으로 Server Action을 만들어보는 예시를 들어볼게요!

```js
'use server'
 
export async function create() {}
```

위처럼 `create`라는 비동기 함수를 `server` 모드에서 정의할 수 있어요. 여기서 중요한 점은, 이 Server Action은 API 엔드포인트를 따로 만들 필요 없이 클라이언트 쪽에서 바로 호출할 수 있다는 거예요.

클라이언트 쪽 컴포넌트에서 이렇게 사용할 수 있죠:

```js
'use client'
 
import { create } from './actions'
 
export function Button() {
  return <button onClick={() => create()}>Create</button>
}
```

버튼 클릭 시 `create()`라는 서버 함수가 호출돼요. 마치 일반 자바스크립트 함수를 호출하는 것처럼 간편하죠.

---

### 여기서 조금 더 알아두면 좋은 팁!

- Server Actions 덕분에 클라이언트와 서버 사이의 데이터 통신이 훨씬 간단해졌어요.
- 전통적인 REST API처럼 별도의 API 라우트 파일을 만들거나 fetch 요청을 직접 작성하지 않아도 돼서 빠르게 개발할 수 있답니다.
- 물론, Server Action 내부에서 데이터베이스 작업, 인증 검증 등 서버 작업을 마음껏 할 수 있어요.
- 그러나 이 함수는 클라이언트에서 직접 실행되는 게 아니라 서버에서 실행되며, 클라이언트에 노출되는 코드는 아니라는 점 기억하세요.

이처럼 Server Actions를 활용하면 React 컴포넌트 내에서 서버 작업을 매우 쉽게 할 수 있으니, Next.js나 React Server Components를 사용할 때 꼭 활용해보세요!

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

서버 액션(Server Actions)으로 데이터 변경하기에 대해 좀 더 알아보자!

## 정적 내보내기 (선택 사항)

Next.js는 완전한 정적 사이트를 생성하는 것도 지원해요. 이 방법은 일반적인 SPA(싱글 페이지 애플리케이션)보다 몇 가지 장점이 있답니다!

- **자동 코드 분할**: 보통 SPA라면 하나의 index.html에 모든 코드가 모여서 클라이언트가 로딩하는 데 시간이 걸리는데, Next.js는 각 라우트마다 별도의 HTML 파일을 생성해줘요. 그래서 방문자가 페이지에 더 빨리 접근할 수 있죠.
- **향상된 사용자 경험**: 모든 라우트에서 최소한의 스켈레톤 화면만 보여주는 대신, 각 라우트마다 완전히 렌더링된 페이지를 먼저 보내요. 클라이언트 사이드에서 이동할 때는 여전히 SPA처럼 부드럽고 즉각적인 전환을 할 수 있어서 사용자가 자연스럽게 느껴집니다.

즉, Next.js의 정적 내보내기는 SEO 최적화도 되고, 초기 로딩 속도와 사용자 경험을 동시에 잡을 수 있는 훌륭한 기능이에요.

추가로, 서버 액션을 활용하면 데이터 변경이 서버 측에서 직접 이루어지므로 보안이나 성능 면에서도 이점이 있어요. 클라이언트에서 불필요하게 데이터를 전달하거나 검증하는 단계가 줄어들거든요. Next.js를 사용한다면 이런 서버 액션과 정적 내보내기를 적절히 조합해서 깔끔하고 빠른 웹사이트를 만들어보세요!

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

Next.js에서 정적 내보내기(Static Export)를 활성화하려면 설정 파일을 아래처럼 업데이트 해주면 돼요:

```js
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  output: 'export',
}

export default nextConfig
```

이렇게 설정하고 나서 `next build` 명령을 실행하면, Next.js가 `out` 폴더를 만들어서 HTML, CSS, JS 같은 정적 자산들을 그 안에 담아줍니다. 덕분에 별도의 서버 없이도 완전히 정적인 웹사이트를 배포할 수 있죠.

---

> 참고: 정적 내보내기를 사용할 경우, Next.js의 서버 사이드 기능들은 지원되지 않습니다. 예를 들어, API 라우트나 getServerSideProps 같은 기능은 쓸 수 없어요.  
> 만약 서버 기능이 필요하면, 정적 내보내기 대신 ISR(Incremental Static Regeneration)이나 SSR(Server Side Rendering)을 고민해보는 게 좋아요.

---

추가로, 정적 내보내기는 간단한 블로그나 포트폴리오 사이트, 문서 페이지 등에 아주 적합해요. 만약 사용자 맞춤형 데이터나 실시간 정보가 적게 필요하고, 최대한 빠른 로딩이 목표라면 꼭 고려해보세요!

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

## 기존 프로젝트를 Next.js로 점진적으로 마이그레이션하기

Next.js로 완전히 갈아타기 부담스럽다고? 걱정 마세요! Next.js는 기존 프로젝트를 단계별로 천천히 옮길 수 있게 가이드가 잘 마련되어 있어요.

- Create React App에서 마이그레이션하기
- Vite에서 마이그레이션하기

이미 SPA를 만들고 Pages Router를 사용 중이라면, App Router를 점진적으로 적용하는 방법도 배울 수 있답니다.

---

사실 큰 프로젝트를 한꺼번에 옮기려면 막막할 수 있잖아요. Next.js는 이런 점을 고려해서 '조각조각' 나눠서 적용할 수 있도록 설계되었거든요. 덕분에 기존 코드도 유지하면서 점차 Next.js의 장점들을 누려볼 수 있어요.

그리고 App Router는 Next.js 최신 기능 중 하나인데, 기존 Pages Router 프로젝트에 조금씩 섞어 쓰다가 완전히 갈아타는 것도 가능해요. 그래서 기존에 만들어 놓은 SPA에 큰 변화 없이도 최신 패턴을 활용할 수 있다는 점, 참고하세요!