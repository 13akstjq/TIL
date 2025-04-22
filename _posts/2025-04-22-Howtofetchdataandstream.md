---
title: "Next.js 15에서 데이터 fetch하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:13
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to fetch data and stream"
link: "https://nextjs.org/docs/app/getting-started/fetching-data"
isUpdated: false
---

# 데이터 가져오기와 스트리밍하는 방법

이번 글에서는 Next.js 같은 환경에서 서버 컴포넌트와 클라이언트 컴포넌트에서 데이터를 가져오는 방법을 알려드릴게요. 그리고 데이터를 기반으로 콘텐츠를 스트리밍하는 방법도 같이 살펴볼 거예요.

## 데이터 가져오기

### 서버 컴포넌트에서 데이터 가져오기

서버 컴포넌트(Server Components)는 서버에서 실행되기 때문에 데이터를 바로 fetch해서 렌더링할 수 있어요. 클라이언트에게는 이미 준비된 결과만 내려주기 때문에 성능도 좋고 보안상 이점도 있죠.

예를 들어, 이렇게 사용할 수 있어요:

```jsx
// ServerComponent.jsx
async function ServerComponent() {
  const res = await fetch("https://api.example.com/data");
  const data = await res.json();

  return (
    <div>
      <h1>서버에서 가져온 데이터</h1>
      <p>{data.message}</p>
    </div>
  );
}

export default ServerComponent;
```

서버 컴포넌트는 기본적으로 비동기 함수(async function)로 만들 수 있어서, 데이터를 기다렸다가 렌더링하는 것에 전혀 무리가 없답니다.

---

추가 팁!

- fetch를 쓸 때는 캐싱과 관련된 옵션을 꼭 확인하세요. 예를 들어 Next.js에서는 `fetch('url', { next: { revalidate: 60 }})` 처럼 60초마다 다시 가져오게 설정할 수 있어요.
- 만약 API 호출이 오래 걸리는 경우, React 18 스트리밍 기능과 함께 쓰면 사용자에게 더 빠른 피드백을 줄 수 있어요. 이 부분도 곧 설명할게요!

다음으로, 클라이언트 컴포넌트에서 데이터 가져오는 법과 스트리밍하는 예시를 이어서 다뤄볼게요.

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

서버 컴포넌트(Server Components)에서 데이터를 가져올 때는 주로 이렇게 두 가지 방법을 사용해요:

- fetch API 활용하기
- ORM이나 데이터베이스 직접 호출하기

#### fetch API 사용법

fetch API를 쓰려면 컴포넌트를 비동기 함수(async function)로 만들고, fetch 호출 앞에 await를 붙여서 데이터를 받아오면 돼요. 예시를 살펴볼게요:

```jsx
// 비동기 함수로 만든 서버 컴포넌트 예시
export default async function UserProfile() {
  const res = await fetch("https://api.example.com/user");

  // 서버 컴포넌트에서 fetch할 때는 기본적으로 캐싱이 되는데,
  // 필요에 따라 cache 옵션을 조절할 수 있어요.
  // 예: await fetch(url, { cache: 'no-store' }) // 매번 새로 불러오기

  if (!res.ok) {
    // 에러 처리도 서버 컴포넌트 내에서 할 수 있답니다.
    throw new Error("사용자 정보를 가져오는데 실패했습니다.");
  }

  const user = await res.json();

  return (
    <div>
      <h1>{user.name}님 안녕하세요!</h1>
      <p>이메일: {user.email}</p>
    </div>
  );
}
```

> 여기서 중요한 팁! Server Components는 클라이언트와 달리 fetch API를 사용할 때 네트워크 요청이 서버에서 처리되기 때문에 API 키 같은 민감한 정보도 안전하게 다룰 수 있어요.

다음 번에는 ORM이나 데이터베이스 직접 호출 방법도 같이 정리해볼게요! 컴포넌트에서 데이터 가져오는 게 이제 좀 감 잡히셨죠?

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

이번에는 React의 Server Component에서 데이터 가져오는 방법에 대해 살펴보려고 해요. 특히 fetch API를 사용하거나 ORM을 통해 데이터베이스에서 직접 데이터를 불러오는 예제를 함께 보여드릴게요.

먼저, `fetch`를 사용해서 외부 API에서 블로그 글 목록을 가져오는 아주 기본적인 예제입니다.

```jsx
export default async function Page() {
  const data = await fetch("https://api.vercel.app/blog");
  const posts = await data.json();

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

서버 컴포넌트는 서버에서 렌더링되기 때문에 async 함수로 만들고 `await`로 데이터를 받아올 수 있는 게 큰 장점이에요. 이렇게 하면 클라이언트에서 별도로 데이터를 가져오는 로직을 짤 필요가 없고, 초기 렌더링 시점에 바로 데이터를 사용할 수 있죠.

---

그리고 조금 더 고급 예제로, ORM이나 직접 데이터베이스 클라이언트를 사용해서 데이터베이스 쿼리를 하는 방법도 있는데요. 예를 들어, Prisma나 Drizzle ORM 같이 JavaScript/TypeScript에서 많이 사용하는 ORM을 쓸 때도 동일한 패턴입니다.

```jsx
import { db, posts } from "@/lib/db";

export default async function Page() {
  const allPosts = await db.select().from(posts);

  return (
    <ul>
      {allPosts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

이 경우 `db.select().from(posts)` 같은 쿼리문을 `await` 해서 서버에서 바로 데이터베이스를 조회하고, 그 결과를 UI로 바로 뿌려줄 수 있어요.

---

### 여기서 몇 가지 더 알려드릴 팁!

- **서버 컴포넌트와 클라이언트 컴포넌트의 역할 분리!**  
  서버 컴포넌트는 데이터를 불러오고, 클라이언트 컴포넌트는 사용자 상호작용(UI 이벤트 등)을 담당하는 식으로 분리하면 앱 구조가 더 깔끔해져요.

- **fetch 캐싱 전략**  
  `fetch` 호출할 때 `cache: 'no-store'` 옵션을 넣으면 항상 최신 데이터를 받아올 수 있고, 반대로 캐싱 옵션을 활용하면 퍼포먼스를 높일 수도 있어요.

- **환경변수 사용하기**  
  데이터베이스 연결이나 API 호출 시 민감한 정보(API 키 등)는 .env 파일에 숨기는 게 안전해요.

---

이번 글에서는 React 서버 컴포넌트 내에서 외부 API, ORM으로 데이터 가져오는 기본적인 방법을 정리해보았는데요. 실제 프로젝트에서 서버 사이드 렌더링을 하거나 Next.js와 같은 프레임워크를 사용할 때 꼭 알아두면 유용한 패턴이에요. 편하게 데이터를 다루고 싶을 때 async/await와 서버 컴포넌트를 적극 활용해보세요!

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

### 클라이언트 컴포넌트에서 데이터 가져오기

클라이언트 컴포넌트에서 데이터를 가져오는 방법은 크게 두 가지가 있어요.

- React의 `use` 훅 사용하기
- SWR, React Query 같은 커뮤니티 라이브러리 활용하기

#### 1. React `use` 훅 사용하기

React 18부터 도입된 `use` 훅을 사용하면 데이터 패칭을 조금 더 간결하게 할 수 있어요. 이 훅은 비동기 함수에서 데이터를 받아와서 컴포넌트가 렌더링되기 전에 데이터 로딩을 처리해주죠.

예를 들면 이렇게 사용할 수 있어요:

```jsx
"use client";

import { use } from "react";

async function fetchUser() {
  const res = await fetch("/api/user");
  if (!res.ok) throw new Error("Failed to fetch user");
  return res.json();
}

export default function UserComponent() {
  const user = use(fetchUser());

  return <div>Hello, {user.name}!</div>;
}
```

`use` 훅은 서버 컴포넌트에서는 바로 사용할 수 없고 클라이언트 컴포넌트에서만 지원되니 참고하세요!

#### 2. SWR, React Query 같은 라이브러리 이용하기

`use` 훅이 꽤 편리하지만, 상태 관리, 캐싱, 리페치 같은 점에서 기능이 부족할 때는 SWR이나 React Query가 많이 쓰여요.

이들 라이브러리는 다음과 같은 이점이 있어요:

| 장점                  | 설명                                                         |
| --------------------- | ------------------------------------------------------------ |
| 자동 캐싱             | 이전에 가져온 데이터를 캐시해서 빠르게 보여줘요.             |
| 배경 리페치           | 데이터를 최신 상태로 유지하기 위해 자동으로 재요청해요.      |
| 로딩과 에러 상태 처리 | 로딩 중인 상태나 에러를 쉽게 관리할 수 있어요.               |
| 폴링과 리프레시       | 정기적으로 데이터를 갱신하거나 수동으로 새로고침이 가능해요. |

예를 들어 SWR을 쓰면 이렇게 간단하게 데이터를 가져올 수 있죠:

```jsx
"use client";

import useSWR from "swr";

const fetcher = (url) => fetch(url).then((res) => res.json());

export default function UserComponent() {
  const { data, error } = useSWR("/api/user", fetcher);

  if (error) return <div>에러 발생!</div>;
  if (!data) return <div>로딩 중...</div>;

  return <div>Hello, {data.name}!</div>;
}
```

React Query 역시 비슷한 방식으로 동작하고, 복잡한 데이터 동기화 문제를 쉽게 해결할 수 있어서 대규모 프로젝트에 많이 사용돼요.

---

데이터를 어떻게 패칭할지는 프로젝트 규모와 요구사항에 따라 선택하면 좋아요. 간단한 경우에는 React `use` 훅만으로도 충분하지만, 상태 관리가 복잡하거나 다양한 기능을 원한다면 SWR이나 React Query를 한 번 써보시길 추천해요!

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

React의 새로운 use hook을 활용해서 서버에서 클라이언트로 데이터를 스트리밍하는 방법을 소개할게요. 이걸 쓰면 데이터를 미리 기다리지 않고도 렌더링을 시작할 수 있어서 사용자 경험이 더 좋아져요.

먼저, 서버 컴포넌트에서 데이터를 가져오고, 그 Promise를 클라이언트 컴포넌트에 props로 넘겨줍니다:

```jsx
import Posts from "@/app/ui/posts";
import { Suspense } from "react";

export default function Page() {
  // 데이터 fetching 함수 호출, 여기서 await는 하지 않아요
  const posts = getPosts();

  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Posts posts={posts} />
    </Suspense>
  );
}
```

여기서 중요한 점! getPosts() 의 Promise를 바로 넘겨주는 거예요. 아직 데이터가 준비되지 않았어도, Suspense로 감싸고 있기 때문에 로딩 UI가 보였다가 데이터가 준비되면 실제 내용이 렌더링됩니다.

그 다음, 클라이언트 컴포넌트에선 `use` hook을 이용해서 Promise를 읽을 수 있어요:

```jsx
"use client";
import { use } from "react";

export default function Posts({ posts }: { posts: Promise<{ id: string, title: string }[]> }) {
  // Promise가 끝날 때까지 기다렸다가 결과를 가져옵니다
  const allPosts = use(posts);

  return (
    <ul>
      {allPosts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

이 방법의 장점은 서버 컴포넌트에서 데이터를 바로 넘겨주기 때문에 클라이언트에서 fetch 코드를 따로 작성할 필요가 없고, Suspense와 함께 쓰면 자연스럽게 로딩 상태도 처리할 수 있다는 점이에요.

> 참고로 `use` hook은 React 18에서 도입된 기능이고 Next.js 13 같은 최신 프레임워크와 아주 잘 맞아요. 만약 여러분이 Next.js 같은 환경에서 작업한다면 적극 활용해보세요!

요약하자면:

| 단계                   | 설명                                            |
| ---------------------- | ----------------------------------------------- |
| 1. 서버 컴포넌트       | 데이터를 fetching하고 Promise를 props로 넘기기  |
| 2. 클라이언트 컴포넌트 | `use` hook으로 Promise 처리해서 데이터 사용하기 |
| 3. Suspense 적용       | 로딩 UI 관리해서 UX 향상                        |

이렇게 하면 fetch로 데이터를 직접 기다리지 않고도 부드러운 렌더링과 데이터 스트리밍 효과를 낼 수 있답니다. 한번 써보면 React의 새로운 가능성에 감탄하게 될 거예요!

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

위 예제에서 `Posts /` 컴포넌트를 `Suspense` 경계로 감싸줘야 합니다. 이렇게 하면 데이터 로딩 중일 때 fallback UI가 보여지면서 Promise가 해결될 때까지 기다릴 수 있어요. 스트리밍에 대해 더 알고 싶다면 실제 React 공식 문서나 관련 자료를 참고해보세요.

---

### 커뮤니티 라이브러리 활용하기

React에서 클라이언트 컴포넌트 내 데이터를 가져올 때는 SWR이나 React Query 같은 커뮤니티 라이브러리를 많이 사용합니다. 이 라이브러리들은 캐싱, 스트리밍, 갱신 정책 등 다양한 기능을 제공해서 데이터 패칭을 훨씬 편리하게 만들어줘요.

예를 들어 SWR을 사용한 코드는 다음과 같습니다:

```js
"use client";
import useSWR from "swr";

const fetcher = (url) => fetch(url).then((r) => r.json());

export default function BlogPage() {
  const { data, error, isLoading } = useSWR("https://api.vercel.app/blog", fetcher);

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <ul>
      {data.map((post: { id: string, title: string }) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

- 여기서 `useSWR`은 데이터를 쉽게 가져오고 자동으로 캐싱해주며, 필요하면 재요청도 해줍니다.
- `isLoading`, `error` 상태도 관리해줘서 로딩중이나 에러가 났을 때 사용자에게 알기 쉽게 UI를 보여줄 수 있어요.
- `Suspense`를 직접 쓰는 대신 SWR이 내부적으로 비슷한 원리(loading 상태 관리)를 처리해준다고 생각하면 편합니다.

---

이 밖에도 React Query는 더 많은 기능과 옵션을 제공하니, 프로젝트 성격에 맞게 골라 써보시면 좋겠죠? 데이터를 클라이언트에서 관리할 때 편리한 도구들이 많으니 적극 활용해보세요!

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

## 스트리밍 (Streaming)

> 주의: 아래 내용은 Next.js 15 카나리 버전에서 도입된 dynamicIO 설정 옵션이 활성화되어 있다고 가정한 설명입니다.

서버 컴포넌트(Server Components)에서 async/await를 사용할 때, Next.js는 자동으로 동적 렌더링(dynamic rendering)을 선택하게 돼요. 즉, 매번 사용자 요청이 들어올 때마다 서버에서 데이터를 가져오고 렌더링을 한다는 뜻이죠. 여기서 문제가 되는 점은, 만약 데이터 요청이 느리다면 그 경로 전체가 렌더링 되지 않고 기다리게 된다는 거예요.

이런 상황에서 사용자 경험과 초기 로딩 속도를 개선하기 위해 스트리밍(Streaming)을 사용할 수 있습니다. 스트리밍은 페이지의 HTML 내용을 작고 여러 개의 청크(chunk)로 나누고, 그 청크들을 서버에서 클라이언트로 점진적으로 보내는 방식을 말합니다. 이렇게 하면 사용자에게는 빠르게 일부 콘텐츠가 먼저 보이고, 나머지도 순차적으로 로딩되어 부드러운 경험을 제공할 수 있죠.

---

### 추가 팁: 스트리밍 적용 시 주의할 점

- **콘텐츠 순서 보장**  
  스트리밍은 HTML을 나누어 보내지만, 클라이언트는 리소스를 받는 순서대로 렌더링해요. 그래서 서버에서 청크를 보내는 순서를 적절히 관리하는 게 중요합니다.

- **상태(state) 관리**  
  서버에서 각 청크가 독립적으로 렌더링되므로, 상태 관리에 신경 써야 해요. React의 서버 컴포넌트 특성상 상태를 클라이언트와 분리해서 다뤄야 할 때도 있습니다.

- **SEO 영향 고려**  
  스트리밍은 SEO에도 긍정적입니다. 초기 콘텐츠를 빠르게 보여주기 때문에 검색엔진 크롤러가 콘텐츠를 더 잘 인식할 수 있어요.

---

스트리밍 덕분에 사용자들은 느린 데이터 로딩으로 인한 기다림이 줄어들고, 동시에 서버 자원을 효율적으로 사용할 수 있게 됩니다. Next.js의 스트리밍 기능, 꼭 활용해보세요!

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

<img src="/TIL/assets/img/2025-04-22-Howtofetchdataandstream_0.png" />

애플리케이션에서 스트리밍(streaming)을 구현하는 방법은 크게 두 가지가 있어요:

- `loading.js` 파일을 활용하는 방법
- React의 `Suspense` 컴포넌트를 사용하는 방법

### loading.js를 활용한 방법

loading.js 파일을 이용하면 컴포넌트가 로딩 중일 때 보여줄 UI를 간단히 처리할 수 있는데요, 이것만 잘 활용해도 로딩 상태 관리가 훨씬 깔끔해집니다. React 18 이후로 서버 컴포넌트를 지원하면서 이 방식이 점점 많이 쓰이고 있죠.

다음에 React `Suspense` 컴포넌트를 활용하는 방법도 한번 살펴볼게요.

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

여러분, Next.js에서 페이지 데이터를 가져올 때 로딩 상태를 어떻게 더 잘 보여줄 수 있을까요? 바로 `loading.js` 파일을 활용하는 방법입니다!

예를 들어, `app/blog/page.js` 파일과 같은 폴더(`app/blog`)에 `loading.js` 파일을 만들어주면, 페이지 데이터를 가져오는 동안 사용자가 기다리는 모습을 좀 더 깔끔하게 보여줄 수 있어요.

```js
export default function Loading() {
  // 여기서 로딩 화면 UI를 정의해주면 됩니다
  return <div>Loading...</div>;
}
```

이렇게 하면 사용자가 페이지로 이동할 때, 화면은 즉시 레이아웃과 이 로딩 UI를 보여주고, 데이터가 모두 준비되면 자동으로 실제 콘텐츠로 바뀌어요.

참고로, 이런 방식이 좋은 이유는 “빈 화면”이나 갑작스러운 깜빡임 없이 자연스럽게 사용자 경험이 이어진다는 점이에요. 실제로 프로젝트에 적용할 때는 단순 텍스트 대신 스피너 애니메이션이나 뼈대 UI(Skeleton UI)를 넣어주면 더 효과적이랍니다.

아래는 예시 이미지인데, 이런 식으로 로딩 중인 상태를 좀 더 친절하게 보여줄 수 있어요.

| ![로딩 화면 예시](https://yourdomain.com/assets/img/2025-04-22-Howtofetchdataandstream_1.png) |
| --------------------------------------------------------------------------------------------- |

나중에 `loading.js`에서 다양한 로딩 상태 구현을 고민해보면서, 사용자 경험을 한층 업그레이드 해보세요!

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

<img src="/TIL/assets/img/2025-04-22-Howtofetchdataandstream_2.png" />

뒤에서 보면, `loading.js`는 `layout.js` 내부에 중첩되어 자동으로 `page.js` 파일과 그 아래 자식들을 `Suspense` 경계로 감싸게 돼요.

<img src="/TIL/assets/img/2025-04-22-Howtofetchdataandstream_3.png" />

이 방식은 라우트 세그먼트(레이아웃과 페이지)에 잘 맞지만, 더 세밀한 스트리밍이 필요하다면 직접 `Suspense`를 활용하는 게 좋아요.

---

여기서 잠깐! `Suspense`를 사용하면 리액트에서 비동기 데이터를 로딩하는 동안 그 자리를 대체하는 로딩 UI를 보여줄 수 있다는 점이 정말 편해요. 특히 Next.js 13처럼 서버 컴포넌트가 도입된 환경에서는 `loading.js` 같은 파일로 페이지 단위 로딩 처리를 자동으로 해주니까 개발이 훨씬 수월해지죠.

하지만, 한 단계 더 들어가서 페이지 일부만 따로 로딩하고 싶을 때(예를 들어, 큰 리스트 아이템 중 일부만 먼저 보여주고, 나머지는 천천히 로딩하고 싶을 때) `Suspense`를 직접 사용하는 것이 필요해집니다.

즉,

- `loading.js` + `layout.js` = 페이지 단위 자동 로딩 UI 처리
- `Suspense` = 하위 컴포넌트 단위 세밀한 로딩 UI 제어

이 두 가지를 적절하게 조합해 사용하는 게 개발 생산성을 높이는 좋은 팁이에요!

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

### `Suspense`를 활용한 부분적 스트리밍

`Suspense`는 페이지의 특정 부분만 스트리밍할 수 있게 해줘서, 사용자 경험을 더 부드럽고 빠르게 만들어주는데요. 예를 들어, 전체 페이지 중에서 `Suspense` 경계 밖에 있는 내용은 바로 보여주고, 경계 안에 있는 블로그 목록 같은 무거운 컴포넌트만 나중에 스트리밍해서 로드할 수 있어요.

아래 예제를 보면, 제목과 인사말은 즉시 렌더링되고, 블로그 목록은 `Suspense` 안에서 비동기로 불러와집니다. 로딩 중일 때는 `BlogListSkeleton` 컴포넌트가 대신 보여지겠죠?

```jsx
import { Suspense } from "react";
import BlogList from "@/components/BlogList";
import BlogListSkeleton from "@/components/BlogListSkeleton";

export default function BlogPage() {
  return (
    <div>
      {/* 이 부분은 즉시 클라이언트에게 전송됩니다 */}
      <header>
        <h1>Welcome to the Blog</h1>
        <p>Read the latest posts below.</p>
      </header>
      <main>
        {/* Suspense 경계 안의 컴포넌트는 스트리밍 되어 로드됩니다 */}
        <Suspense fallback={<BlogListSkeleton />}>
          <BlogList />
        </Suspense>
      </main>
    </div>
  );
}
```

### 의미 있는 로딩 상태 만들기

`Suspense`를 사용하면 단순히 "로딩 중" 텍스트만 보여주는 대신, 실제 콘텐츠 형태를 모방한 스켈레톤 UI를 보여줄 수 있는데요. 이는 사용자에게 페이지가 정말로 반응하고 있다는 느낌을 잘 줍니다. 예를 들어 블로그 목록이라면 개별 목록 아이템이 빈 카드처럼 보이는 스켈레톤 화면을 만드는 거죠.

이런 UX 테크닉을 적용하면, 페이지가 로딩될 때 사용자가 느끼는 대기시간이 훨씬 짧고 부드러워 보여 실제 체감 성능이 높아집니다.

더불어, `Suspense`를 적절히 활용하면 데이터가 준비되는 순서에 따라 UI를 부분별로 제어할 수 있어서, 복잡한 페이지도 단계적으로 로드할 수 있다는 점!

요약하자면,

- 초기 콘텐츠는 지체 없이 보여주기
- 데이터를 불러오는 부분은 `Suspense`와 fallback UI로 감싸기
- 스켈레톤 UI 등 의미 있는 로딩 화면 제공하기

이렇게만 해도 사용자 경험은 대폭 개선될 거예요! 😊

필요하다면 Suspense의 fallback 컴포넌트를 직접 커스터마이징해서 브랜드 느낌이나 스타일을 살리는 것도 추천드려요.

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

즉시 로딩 상태란, 사용자가 페이지를 이동한 직후 바로 보여주는 대체 UI를 의미해요. 이게 왜 중요하냐면, 사용자가 앱이 반응하고 있다는 걸 바로 알 수 있게 해줘서 불안감을 줄이고 더 나은 사용자 경험을 제공할 수 있기 때문이죠.

가장 좋은 방법은 단순히 로딩 중이라는 표시만 보여주는 게 아니라, 의미 있는 로딩 상태를 디자인하는 거예요. 예를 들어, 뼈대 화면(skeleton)을 보여준다거나, 돌아가는 스피너(spinner)를 사용하는 게 대표적이고요. 좀 더 나아가서는 다음에 보여질 화면의 핵심 요소, 예를 들어 커버 사진이나 제목 같은 작은 부분을 먼저 보여주면 훨씬 자연스러운 느낌을 줄 수 있습니다.

개발 중에는 React Devtools를 활용하면 컴포넌트의 로딩 상태를 미리 보고, 어떻게 동작하는지 자세하게 확인할 수 있어요. 덕분에 사용자에게 완성도 높은 로딩 UI를 더 쉽게 만들 수 있답니다.

간단히 정리해볼게요.

| 팁                    | 설명                                                        |
| --------------------- | ----------------------------------------------------------- |
| 즉시 로딩 상태        | 네비게이션 후 바로 보여지는 대체 UI                         |
| 의미 있는 로딩 디자인 | 스켈레톤, 스피너, 다음 화면 일부 요소(제목, 이미지 등) 활용 |
| 개발 시 활용 도구     | React Devtools로 로딩 상태 미리보기 및 검사                 |

이런 방식으로 사용자에게 앱이 멈추지 않고 잘 작동 중임을 자연스럽게 알릴 수 있어요. 여러분도 한 번 적용해보세요!
