---
title: "TypeScript와 ReactJS에서 에러 처리하는 5가지 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:16
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to handle errors"
link: "https://nextjs.org/docs/app/getting-started/error-handling"
isUpdated: false
---

# 에러 처리하는 법

에러는 크게 두 가지로 나눌 수 있어요: 예상 가능한 에러와 잡히지 않은 예외(uncaught exceptions)죠. 이번 글에서는 Next.js 앱에서 이런 에러들을 어떻게 다룰 수 있는지 쉽게 알려드릴게요.

## 예상 가능한 에러 처리하기

예상 가능한 에러는 앱이 정상적으로 작동하는 중에 발생할 수 있는 에러예요. 예를 들어 서버 쪽 폼 검증에서 실패할 때나, API 요청이 실패하는 경우죠. 이런 에러들은 명확하게 처리해서 클라이언트한테 적절하게 전달해줘야 해요.

사실 이런 에러들은 사용자 경험에도 영향을 많이 주기 때문에, 단순히 ‘문제가 발생했습니다’라고 하는 것보단, 어떤 문제가 있는지 구체적으로 알려주는 게 중요해요. 예를 들어, 폼에서 이메일 형식이 맞지 않으면 "이메일 형식이 올바르지 않습니다"라고 바로 알려주면 사용자가 수정할 수 있잖아요?

Next.js에서는 서버 사이드 코드에서 이런 에러들을 try-catch 문으로 잡아서, 적절한 HTTP 상태 코드와 메시지로 응답을 줄 수 있어요. 클라이언트 쪽에서는 그 응답을 받아서 UI에 에러 메시지를 띄우거나, 특정 행동을 유도하도록 하면 됩니다.

혹시 에러를 다루는 게 처음이라면, ‘에러 핸들링 레이어’를 따로 만들어서 코드를 깔끔하게 관리하는 것도 추천해요. 그래야 에러 처리 로직이 여러 군데 흩어지지 않고, 수정도 편리해요.

다음에는 잡히지 않은 예외, 즉 예기치 않은 오류가 발생했을 때 어떻게 대처하는지도 알려드릴게요!

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

### 서버 함수에서 예상 에러 처리하기

서버 함수(Server Functions)를 작성할 때, 예상되는 에러를 다루는 방법에 대해 이야기해볼게요. 보통 자바스크립트에서 에러가 발생하면 try/catch로 잡거나 `throw`를 해서 에러를 던지죠? 그런데 서버 함수에서는 이런 방식보다는 **예상 가능한 에러를 반환값(return value)**으로 모델링하는 게 좋아요.

예를 들어, 아래와 같이 `createPost`라는 서버 함수를 만들었을 때, API 호출이 실패하는 경우를 생각해볼게요.

```js
"use server";

export async function createPost(prevState: any, formData: FormData) {
  const title = formData.get("title");
  const content = formData.get("content");

  const res = await fetch("https://api.vercel.app/posts", {
    method: "POST",
    body: { title, content },
  });
  const json = await res.json();

  if (!res.ok) {
    return { message: "Failed to create post" };
  }
}
```

여기서 실패하면 그냥 `throw` 대신 `{ message: 'Failed to create post' }`라는 객체를 반환하고 있죠? 이렇게 하면 클라이언트 쪽에서 이 반환값을 이용해 에러 메시지를 쉽게 표시할 수 있어요.

#### 팁!

- 서버에서 에러를 `throw`하면 클라이언트 입장에선 뭘 잘못했는지 알기 어려울 수 있어요. 그래서 예상 가능한 에러들은 명확하게 데이터로 내려주는 게 UX에도 더 좋아요.
- 반대로, 정말 예상하지 못한 시스템 에러는 그대로 `throw`해서 호출 스택에서 잡히게 하는 게 관리하기 편합니다.
- 그리고, `useActionState` 훅을 사용하면 이런 반환값을 쉽게 받아서 처리할 수 있으니 한번 써보세요!

이런 스타일은 점점 더 많은 React 서버 컴포넌트 기반 환경에서 권장되는 패턴이니, 앞으로 서버 함수 작성할 때 참고하면 좋아요.

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

이번 시간에는 React의 `useActionState` 훅을 이용해서 폼 제출 시 발생할 수 있는 에러 메시지를 화면에 표시하는 방법을 함께 알아보겠습니다.

기본적으로 `useActionState`는 액션(action)의 상태를 추적할 수 있도록 도와줘서, 성공 여부나 에러 발생 시 상태를 쉽게 관리할 수 있답니다.

---

## 예제 코드 살펴보기

```jsx
"use client";

import { useActionState } from "react";
import { createPost } from "@/app/actions";

const initialState = {
  message: "",
};

export function Form() {
  // useActionState 훅 호출, createPost 액션을 인자로 전달
  const { state, pending } = useActionState(createPost);

  return (
    <form action={createPost}>
      <label htmlFor="title">Title</label>
      <input type="text" id="title" name="title" required />

      <label htmlFor="content">Content</label>
      <textarea id="content" name="content" required />

      <button disabled={pending}>Create Post</button>

      {/* 에러 메시지가 있다면 화면에 표시 */}
      {state?.message && <p style={{ color: "red" }}>{state.message}</p>}
    </form>
  );
}
```

- `useActionState(createPost)`를 호출하면 액션의 현재 상태(`state`)와 제출 진행 중 여부(`pending`)를 반환해줍니다.
- `state.message`에 에러 메시지가 담겨 있다면, 이를 사용자에게 보여줄 수 있죠.
- `button`은 제출하는 동안 중복 전송을 방지하기 위해 `disabled` 처리됩니다.

---

## Server Component에서 에러 핸들링

서버 컴포넌트에서 데이터를 불러올 때도 에러 처리는 중요하죠! 거기서는 서버에서 받은 응답을 활용해 조건부 렌더링을 하거나, 필요시 리다이렉션을 할 수 있습니다.

예를 들어:

```jsx
export default async function Page() {
  try {
    const data = await fetchData(); // 서버에서 데이터 가져오기
    return <DisplayData data={data} />;
  } catch (error) {
    // 에러 발생 시 에러 메시지 또는 리다이렉션 처리
    return <p>데이터를 불러오는 중 오류가 발생했습니다.</p>;
  }
}
```

이처럼 서버 컴포넌트 내부에서 `try-catch` 문으로 에러를 잡아서 사용자에게 피드백을 줄 수 있습니다.

---

## 정리하자면

- **useActionState** 훅을 활용하면 클라이언트 컴포넌트 내에서 액션 상태를 쉽게 관리하고, 에러 메시지를 화면에 표시할 수 있습니다.
- 제출 버튼에 진행 상태를 반영해 사용자 경험도 개선할 수 있죠.
- **서버 컴포넌트**에서는 데이터를 비동기로 가져올 때 에러를 잡아서 적절히 처리해야 하며, 이때 사용자에게 친절하게 에러를 알리는 UI를 만드는 게 중요합니다.

---

혹시 더 자세한 액션 상태 관리나 서버-클라이언트 간 에러 핸들링에 관해 궁금한 점이 있으면 언제든지 물어보세요! 여러분이 더 쉽게 React와 Next.js를 활용할 수 있도록 돕겠습니다 :)

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

자, 이번에는 Next.js에서 데이터를 가져오고, 만약 데이터가 없다면 404 페이지를 띄우는 방법에 대해 이야기해볼게요.

먼저, fetch API를 사용해서 데이터를 비동기로 가져오는 기본 예제부터 봅시다.

```js
export default async function Page() {
  const res = await fetch(`https://...`);
  const data = await res.json();

  if (!res.ok) {
    return "There was an error."; // 에러 발생 시 간단한 메시지 반환
  }

  return "..."; // 데이터를 성공적으로 받아오면 원하는 UI 반환
}
```

여기서 핵심은 서버 API를 호출하고, 응답이 제대로 오지 않으면 에러 메시지를 보여주는 거예요. 그런데, 만약 해당 페이지나 데이터가 없을 때는 ‘404 페이지’를 보여주는 방법도 필요하겠죠?

Next.js에서는 `notFound()` 함수를 이용해서 이런 상황을 처리할 수 있어요. 예를 들어, 블로그 포스트를 주소로 찾는 상황을 가정해보죠.

```js
import { notFound } from "next/navigation";
import { getPostBySlug } from "@/lib/posts";

export default async function Page({ params }: { params: { slug: string } }) {
  const { slug } = params;
  const post = getPostBySlug(slug);

  if (!post) {
    notFound(); // 데이터가 없으면 404 페이지로 이동
  }

  return <div>{post.title}</div>; // 포스트 타이틀 출력
}
```

여기서 중요한 점 몇 가지 짚고 넘어가면:

| 항목            | 설명                                                                                                                                         |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `notFound()`    | Next.js가 제공하는 내장 함수로, 호출 시 해당 페이지 대신 404 UI를 보여줘요. 그냥 반환하는 게 아니라 실제로 404 상태로 페이지가 렌더링됩니다. |
| `getPostBySlug` | 여긴 예시로 작성한 함수인데, 실제로는 슬러그(게시물 주소)를 기준으로 데이터를 찾아오는 로직입니다. 만약 없으면 `undefined` 혹은 `null` 반환  |
| `params`        | 동적 라우팅에서 URL 파라미터를 받을 때 사용해요. 예를 들어 `/posts/hello-world` 라면 slug는 `hello-world` 가 되겠죠                          |

추가로, `not-found.js` 또는 `not-found.tsx` 파일을 프로젝트에 만들어서 404 페이지의 UI를 커스터마이징 할 수도 있어요.

```jsx
export default function NotFound() {
  return (
    <div>
      <h1>페이지를 찾을 수 없어요 ㅠㅠ</h1>
      <p>요청하신 페이지가 존재하지 않거나 삭제되었습니다.</p>
    </div>
  );
}
```

이렇게 하면 프로젝트 전반에서 공통으로 쓰이는 404 페이지가 알아서 뜹니다.

정리해보면, 데이터가 없거나 잘못된 URL을 요청했을 때 사용자에게 잘못된 접근임을 알려주는 방법으로 `notFound()` 호출과 맞춤형 404 페이지를 함께 사용하면 훨씬 깔끔한 사용자 경험을 만들 수 있다는 점! 꼭 기억하세요~

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

```jsx
export default function NotFound() {
  return <div>404 - Page Not Found</div>;
}
```

## 처리하지 않은 예외(uncaught exceptions) 다루기

처리하지 않은 예외란, 애플리케이션의 정상적인 흐름에서는 발생하지 말아야 할 예상치 못한 오류를 뜻해요. 이런 예외들은 보통 에러를 던져서(error throwing) 처리하는데, 이 에러를 '에러 바운더리(Error Boundary)'에서 잡아내서 적절히 대응해주는 방식이 일반적입니다.

에러 바운더리는 리액트 컴포넌트에서 흔히 사용되는데, UI가 깨지지 않고 사용자에게 친절한 에러 메시지를 보여주거나, 로그를 남기거나, 대체 UI로 전환하는 데 활용할 수 있답니다.

### 중첩된 에러 바운더리(Nested error boundaries)

에러 바운더리를 한 군데만 두는 것보다, 애플리케이션 구조에 따라 여러 단계로 중첩시켜서 중요한 UI 부분별로 별도의 에러 처리 영역을 만드는 게 좋아요. 예를 들어, 페이지 전체를 감싸는 바운더리가 있고, 그 안에 각각의 위젯이나 컴포넌트 단위로 또 다른 바운더리가 존재하면, 특정 위젯에서 문제가 생겨도 페이지 전체가 깨지지 않고 해당 위젯 부분만 에러 UI로 교체할 수 있거든요.

이렇게 하면 사용자 경험도 개선되고, 디버깅할 때도 어떤 컴포넌트에서 문제가 발생했는지를 좀 더 명확히 파악할 수 있습니다.

---

참고로, 리액트에서 에러 바운더리를 구현할 때는 다음과 같은 라이프사이클 메서드를 활용해요:

- `static getDerivedStateFromError(error)`: 에러가 발생했을 때 상태를 업데이트하여 대체 UI를 렌더할 준비를 함
- `componentDidCatch(error, info)`: 에러 로깅 등 부가 작업 수행

그리고 함수형 컴포넌트만 쓴다면 `Error Boundary`는 아직 클래스 컴포넌트로 작성해야 한다는 점 기억하세요!  
리액트가 앞으로 훅 기반 에러 바운더리를 지원하면 훨씬 편리해지겠죠?

필요하면 에러 바운더리 예제도 공유해 드릴게요!

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

Next.js에서는 에러 바운더리(Error Boundaries)를 이용해 예기치 못한 에러를 잡아냅니다. 에러 바운더리는 자식 컴포넌트에서 발생한 오류를 잡아내서, 부러진 컴포넌트 트리 대신에 대체 UI(일종의 에러 화면)를 보여주게 해주죠.

이걸 구현하려면, 라우트 세그먼트(route segment) 안에 `error.js` 파일을 만들고 React 컴포넌트를 내보내면 됩니다. 예시는 아래와 같아요:

```js
'use client' // 에러 바운더리는 클라이언트 컴포넌트여야 합니다

import { useEffect } from 'react'

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  useEffect(() => {
    // 에러 리포팅 서비스에 로그를 보낸다고 생각해보세요
    console.error(error)
  }, [error])

  return (
    <div>
      <h2>문제가 발생했어요!</h2>
      <button
        onClick={
          // 다시 렌더링 시도해서 복구를 노립니다
          () => reset()
        }
      >
        다시 시도
      </button>
    </div>
  )
}
```

여기서 핵심은 `reset` 함수예요. 이 버튼을 누르면 해당 세그먼트를 다시 렌더링하면서 문제가 해결됐는지 재시도합니다.

그리고 재미있는 점! 에러는 가장 가까운 상위 에러 바운더리로 올라가서 처리돼요. 덕분에, 라우트 트리 계층별로 `error.js`를 여러 개 둬서 세밀하게 에러 처리를 할 수 있습니다. 예를 들어, 특정 페이지에서만 보여줄 맞춤형 에러 화면도 가능하다는 뜻이죠.

---

추가로 알려드리자면, Next.js에서는 페이지나 레이아웃 단위로 에러를 다룰 수 있어서 사용자 경험을 더 세련되게 만드는 데 큰 도움이 됩니당. 단, 에러 바운더리는 무조건 클라이언트 컴포넌트여야 하는 점은 꼭 기억해주세요!

꼭 한 가지 깜빡하지 말아야 할 팁은, 에러 바운더리 안에서도 에러는 다시 던져지지 않으니, 중요한 로그나 오류 추적(예: Sentry 같은 도구 활용)은 `useEffect` 같은 곳에서 해주는 게 베스트입니다.

필요할 때마다 `error.js` 만들어서 우아한 에러 처리를 해보세요! 사용자도, 개발자도 모두 행복해질 거예요 :)

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

<img src="/TIL/assets/img/2025-04-22-Howtohandleerrors_0.png" />

### 전역 에러 처리하기 (Global errors)

에러 처리하면 보통 각 컴포넌트 단위에서 하게 되지만, root 레이아웃 단계에서 전역적으로 에러를 잡고 싶을 때가 있어요. 이런 경우에는 `app` 루트 디렉터리에 `global-error.js` 파일을 만들어서 처리할 수 있습니다. 특히 다국어 지원 같은 국제화(i18n)를 적용해도 똑같이 쓸 수 있다는 점도 참고하세요.

이 전역 에러 UI 컴포넌트는 root 레이아웃이나 템플릿을 완전히 대체하는 역할을 하기 때문에, 직접 `html`과 `body` 태그를 포함해야 해요. 없으면 브라우저가 제대로 렌더링하지 않기 때문에 꼭 넣어줘야 합니다.

```js
'use client' // 에러 바운더리는 클라이언트 컴포넌트여야 합니다

export default function GlobalError({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    <html>
      <body>
        <h2>문제가 발생했어요!</h2>
        <button onClick={() => reset()}>다시 시도</button>
      </body>
    </html>
  )
}
```

### 핵심 포인트

- `'use client'` 선언으로 클라이언트 컴포넌트로 만들어야 에러 경계(Error Boundaries)가 정상 동작해요.
- 에러를 화면에 보여주고, 사용자가 다시 시도할 수 있도록 reset 함수도 전달받아 꼭 활용하세요.
- 전역 에러 UI는 root 레이아웃을 완전히 대체하므로 `html, body` 태그를 반드시 포함!

### 살짝 더!

만약 이 전역 에러 화면에 스타일을 입히고 싶다면, 전역 CSS를 불러오거나 `style` 태그를 넣는 방법이 있어요. 그리고 모던 웹 앱에서는 에러가 발생했을 때 단순히 메시지 보여주는 것 말고, 사용자 피드백을 받아 서버로 리포트 하거나, 자동 리프레시 기능을 추가하는 것도 생각해볼 만합니다.

즉, 전역 에러 처리는 앱 안정성을 향상시키는 훌륭한 도구니 적극 활용해보세요!
