---
title: "nextjs 15 서버 컴포넌트에서 데이터 업데이트 하는 방법 "
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:15
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to update data"
link: "https://nextjs.org/docs/app/getting-started/updating-data"
isUpdated: false
---


# 데이터 업데이트하는 방법

Next.js에서 데이터를 업데이트할 때는 React의 Server Functions를 활용할 수 있어요. 이번 글에서는 Server Functions를 어떻게 만들고, 호출하는지 쉽고 간단하게 살펴볼게요.

## Server Functions 만들기

Server Function은 `use server`라는 지시어(directive)를 사용해서 정의할 수 있어요. `async` 함수 맨 위에 `use server`를 적으면 그 함수가 Server Function으로 인식돼요. 또는 파일 맨 위에 `use server`를 적으면, 그 파일이 내보내는(export) 모든 함수들이 Server Function으로 동작하게 돼요.

---

> 예를 들어, 아래처럼 쓸 수 있습니다.

```js
'use server'

export async function updateData() {
  // 서버에서 실행할 로직
}
```

---

이렇게 하면 프론트엔드에서 API를 호출하는 느낌 대신, 마치 서버 안에서 직접 호출하는 것처럼 데이터를 처리할 수 있어요. 서버에서 실행되기 때문에 보안이나 성능 측면에서도 이점이 크고, 코드도 깔끔해집니다.

혹시 여기서 `use server`가 뭔지 궁금할 수도 있는데, 이건 Next.js 13 이후에 도입된 기능으로, React 컴포넌트 내에서 서버 전용 함수를 만드는데 사용하는 문법이에요. 쉽게 말해 서버에서만 실행되는 함수임을 표시하는 태그라고 생각하면 됩니다.

다음엔 이 Server Function을 실제 컴포넌트에서 어떻게 호출하는지도 보여드릴게요!

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

이번 글에서는 Next.js 13부터 새롭게 도입된 Server Actions, 즉 서버에서 직접 실행되는 함수 작성법에 대해 살펴볼게요. 특히, Server Functions를 어떻게 작성하고, 이걸 Server Component 안에 어떻게 쉽게 넣을 수 있는지 가볍게 정리해봤습니다.

---

### Server Functions 기본 작성법

예를 들어, 게시글을 생성하거나 삭제하는 함수를 작성할 때 아래처럼 `FormData`를 받아서 처리할 수 있어요.

```js
export async function createPost(formData: FormData) {
  'use server'   // 여기가 포인트! 이 지시어로 서버에서 실행됨을 명시
  const title = formData.get('title')
  const content = formData.get('content')

  // 여기서 데이터베이스에 글 저장하거나,
  // 혹은 캐시를 재검증(revalidate)하는 작업을 할 수 있죠
}

export async function deletePost(formData: FormData) {
  'use server'
  const id = formData.get('id')

  // 삭제 처리 및 캐시 갱신
}
```

`'use server'`를 함수 맨 위에 적으면, 이 함수가 클라이언트가 아닌 서버에서 실행될 함수임을 명확히 하게 됩니다. 이게 Next.js가 Server Actions를 인식하고 별도로 처리하게 만들어주죠.

---

### Server Functions를 Server Component 안에 작성하기

서버 컴포넌트 내부에 이런 Server Action 함수를 바로 써도 OK인데요, 이런 식으로 함수 위에 `'use server'` 지시어를 붙이면 됩니다.

```js
export default function Page() {
  // 서버에서 실행될 함수 (Server Action)
  async function createPost(formData: FormData) {
    'use server'
    // ...게시글 생성 로직
  }

  return <></>
}
```

이렇게 하면 해당 함수를 컴포넌트 바깥으로 따로 빼지 않아도 되고, 컴포넌트와 함수의 관계를 내비치기 좋아요.

---

### 알아두면 좋은 팁

- Server Actions로 넘겨받는 `formData`는 `<form>`에서 전송한 데이터를 쉽게 받아서 쓸 수 있어서, API 엔드포인트 없이도 폼 제출을 깔끔하게 처리할 수 있어요.
- 서버 함수는 클라이언트 코드에 포함되지 않으니, 보안상 민감한 작업을 하기에 좋아요.
- 데이터 변경 후에는 꼭 필요한 경우 `revalidatePath()` 같은 Next.js의 캐시 재검증 함수를 호출해 UI에 최신 상태가 반영되게 만들어야 해요.

---

다음 포스팅에서는 Server Actions를 실제 폼과 연동해서 구현하는 간단한 예시도 준비해볼게요. 서버와 클라이언트 경계를 넘나드는 작업이 점점 더 좋아지고 있어 기대해 주세요! 😊

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

### 클라이언트 컴포넌트(Client Components)

서버 함수(Server Functions)는 클라이언트 컴포넌트 안에서 직접 정의할 수는 없어요. 하지만 서버 함수가 담긴 파일에 `'use server'`라는 지시어를 맨 위에 붙여서 내보내면, 클라이언트 컴포넌트에서 이 함수를 불러와서 실행할 수 있답니다!

예를 들어, 이런 식으로 서버 함수가 정의되어 있어요:

```js
'use server'

export async function createPost() {
  // 서버에서 처리할 로직
}
```

그리고 클라이언트 컴포넌트에서는 이렇게 import해서 사용할 수 있죠:

```js
'use client'

import { createPost } from '@/app/actions'

export function Button() {
  return <button formAction={createPost}>Create</button>
}
```

여기서 핵심은, `createPost` 같은 함수는 실제로는 서버에서 작동하지만, 클라이언트 컴포넌트가 그 함수를 호출하는 이벤트를 연결해준다는 점이에요. 덕분에 UI는 클라이언트에서 매끄럽게 돌아가면서도, 중요한 데이터 처리나 DB 작업 등은 안전하게 서버에서 수행할 수 있죠.

추가로 알아두면 좋은 점!
- `'use server'` 디렉티브가 있는 파일은 Next.js가 서버 전용이라는 걸 인지해서 클라이언트 번들에서 제외해줘서 코드가 더 깔끔해져요.
- `formAction` prop을 사용하면 폼 제출 시 서버 함수를 바로 트리거할 수 있어서 React의 상태 관리 없이도 서버와 통신하기 편해요.
- 이렇게 Server Function을 클라이언트 컴포넌트에서 활용하면, 복잡한 API 라우트 없이도 양방향 데이터 흐름을 간단히 구현할 수 있습니다.

이 방식 덕분에 Next.js에서 서버/클라이언트 코드 구분이 더 명확하고, 개발자는 로직을 더 효율적으로 나눌 수 있답니다!

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

## 서버 함수 호출하기

서버 함수를 호출하는 방법은 크게 두 가지가 있어요:

- 서버 컴포넌트와 클라이언트 컴포넌트에서의 폼(form) 사용
- 클라이언트 컴포넌트에서 이벤트 핸들러 사용

### 폼(Form)이용하기

폼 태그는 HTML에서 서버와 데이터를 주고받을 때 가장 기본적인 방법 중 하나인데요, Next.js 같은 프레임워크에서 서버 컴포넌트나 클라이언트 컴포넌트 내에서 쉽게 사용할 수 있어요. 폼이 제출되면 지정한 서버 함수가 호출되고, 그 서버 함수 안에서 데이터를 처리할 수 있죠.

예를 들어, 간단한 로그인 폼을 만들어 서버 함수로 데이터를 보내는 경우를 생각해보면 이해가 쉬워요. 폼 안에 input 필드를 넣고, submit 버튼을 누르면 서버 함수가 자동으로 실행되는 거죠.

또한, 요즘은 fetch API나 axios 같은 클라이언트 사이드 호출 방식을 많이 쓰긴 하지만, 폼을 이용한 방법은 여전히 직관적이고 간단하게 서버에 데이터를 보내는 좋은 방법이에요. 특히 서버 컴포넌트가 지원되는 환경에서는 폼을 통해 자연스럽게 서버 함수를 호출할 수 있다는 점 꼭 기억하세요!

필요하면 다음에 폼 예제 코드를 공유할게요.

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

React에서 `form` 태그를 확장해서 Server Function을 HTML `action` 속성으로 바로 호출할 수 있다는 사실, 알고 계셨나요?

보통 우리가 폼을 제출할 때는 프론트엔드에서 이벤트 핸들러를 따로 만들어서 처리하곤 하죠. 하지만 React에서는 폼의 `action` 속성에 서버에서 실행할 함수를 직접 연결할 수 있어요. 이 함수는 폼 제출 시 자동으로 `FormData` 객체를 받아서 안에 담긴 값들을 쉽게 꺼내 쓸 수 있답니다.

예를 들어, 아래처럼 `createPost`라는 서버 함수를 `action`에 할당하면, 폼이 제출될 때 자동으로 `createPost`가 호출되면서 제출한 데이터가 `FormData` 형태로 전달돼요.

```jsx
import { createPost } from '@/app/actions'

export function Form() {
  return (
    <form action={createPost}>
      <input type="text" name="title" />
      <input type="text" name="content" />
      <button type="submit">Create</button>
    </form>
  )
}
```

서버 함수는 이렇게 작성할 수 있습니다:

```js
'use server'

export async function createPost(formData: FormData) {
  const title = formData.get('title')
  const content = formData.get('content')

  // 여기서 DB에 저장하거나, 캐시를 재검증하는 작업을 할 수 있어요.
}
```

### 조금 더 알아두면 좋은 팁!

- `FormData.get()` 메서드는 반환값이 `FormDataEntryValue | null`이기 때문에, 실제 값이 문자열인지 확인하거나 기본값을 설정하는 게 좋습니다.
- 서버 함수 내에서는 서버 전용 코드(ex. 데이터베이스 쿼리, 파일 시스템 접근)를 자유롭게 사용할 수 있어요.
- 클라이언트에서 별도의 API 호출 없이도 서버로 데이터를 보낼 수 있어서 코드가 더 깔끔해지고 유지보수가 쉬워집니다.
- 물론, 복잡한 폼 유효성 검사나 사용자가 바로 피드백을 받아야 하는 경우엔 클라이언트 측에서 미리 체크하는 걸 추천합니다.

이 방식은 특히 Next.js 같은 React 기반 프레임워크에서 유용하게 쓰이는데요, 서버 함수와 폼을 연동하는 새로운 흐름을 경험해보고 싶다면 한 번 시도해보세요!

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

> 참고할 점: action prop에 전달될 때 Server Functions는 Server Actions라고도 불러요.

### 이벤트 핸들러(Event Handlers)

클라이언트 컴포넌트에서 서버 기능(Server Function)을 호출할 때는 onClick 같은 이벤트 핸들러를 활용할 수 있어요. 예를 들어, 좋아요 버튼을 만들고 클릭할 때마다 서버에 좋아요 수를 업데이트하는 기능을 구현해볼게요.

```jsx
'use client'

import { incrementLike } from './actions'
import { useState } from 'react'

export default function LikeButton({ initialLikes }: { initialLikes: number }) {
  const [likes, setLikes] = useState(initialLikes)

  return (
    <>
      <p>Total Likes: {likes}</p>
      <button
        onClick={async () => {
          // 서버에 좋아요 증가 요청을 보내고, 업데이트 된 좋아요 수를 받아서 상태를 갱신해요
          const updatedLikes = await incrementLike()
          setLikes(updatedLikes)
        }}
      >
        Like
      </button>
    </>
  )
}
```

여기서 중요한 점은, 클라이언트 컴포넌트 내부에서 직접 서버 사이드 코드를 호출할 수 없기 때문에, `incrementLike` 같은 서버 기능을 `actions`라는 별도의 파일에서 정의해두고 import해서 사용한다는 거예요. 그리고 비동기 처리를 위해 `async/await`를 활용한 점도 기억해두면 좋아요.

또한, 이렇게 서버 함수 호출 시 상태를 업데이트하는 패턴은 사용자 인터랙션에 실시간 피드백을 주는 데 아주 유용합니다. 만약 서버 함수 호출 중에 로딩 상태 표시가 필요하다면, `useState`로 로딩 상태를 관리해서 버튼을 비활성화하거나 스피너를 보여주는 것도 좋은 UX를 위한 팁이에요!

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

### 대기 상태 표시하기

서버 함수를 실행하는 동안, React의 `useActionState` 훅을 사용해서 로딩 인디케이터를 보여줄 수 있어요. 이 훅은 `pending`이라는 불리언 값을 반환하는데, 이 값이 `true`일 때는 작업이 진행 중임을 뜻하죠.

예를 들어, 게시글 생성 버튼을 눌렀을 때 로딩 스피너를 보여주는 컴포넌트 코드는 아래와 같습니다:

```js
'use client'

import { useActionState } from 'react'
import { createPost } from '@/app/actions'
import { LoadingSpinner } from '@/app/ui/loading-spinner'

export function Button() {
  const [state, action, pending] = useActionState(createPost, false)

  return (
    <button onClick={async () => action()}>
      {pending ? <LoadingSpinner /> : 'Create Post'}
    </button>
  )
}
```

여기서 중요한 점은 `pending` 값 덕분에 사용자에게 작업이 진행 중이라는 피드백을 줄 수 있다는 거예요. 보통 서버와의 통신이 느릴 때는 이렇게 대기 상태를 표시해주는 게 UX 측면에서 정말 중요하답니다.

또 한 가지 팁을 드리자면, 이 훅은 작업이 끝난 후 결과값이나 에러 상태도 함께 관리할 수 있어서, 로딩뿐만 아니라 성공, 실패 상태에 따른 UI 업데이트도 함께 처리할 수 있어요. 필요에 따라 `state`를 활용해보세요!

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

### 캐시 재검증하기

데이터를 업데이트한 뒤, Next.js 캐시를 재검증(revalidate)해서 최신 데이터를 보여주고 싶을 때가 있죠? 그럴 땐 Server Function 내부에서 `revalidatePath`나 `revalidateTag` 함수를 사용하면 간편하게 해결할 수 있어요.

```js
import { revalidatePath } from 'next/cache'

export async function createPost(formData: FormData) {
  'use server'
  
  // 데이터 업데이트 작업 수행
  // ...
  
  // 특정 경로에 대한 캐시를 재검증해서 최신 데이터 반영
  revalidatePath('/posts')
}
```

위 예제처럼, 글을 새로 만들거나 수정한 뒤 `/posts` 경로의 캐시를 재검증하면 사용자에게 항상 최신 게시물 목록을 보여줄 수 있답니다.

> 추가 팁!  
> 비슷하게 `revalidateTag`를 사용하면 태그 단위로 캐시를 재검증할 수도 있는데, 이 방법은 특정 데이터 그룹을 관리할 때 유용해요. 상황에 맞게 선택해서 사용해보세요.

### 리다이렉트하기

서버 함수에서 작업을 마친 후 사용자를 다른 페이지로 이동시키고 싶을 때는 어떻게 할까요? Next.js에서는 `redirect` 함수를 활용하면 쉽게 처리할 수 있답니다.

예를 들어, 글 작성 후 게시물 목록 페이지로 이동시키고 싶을 때:

```js
import { redirect } from 'next/navigation'

export async function createPost(formData: FormData) {
  'use server'
  
  // 데이터 생성 로직
  // ...
  
  // 작업 후 /posts 페이지로 리다이렉트
  redirect('/posts')
}
```

`redirect` 함수를 호출하면, 클라이언트가 자동으로 지정한 경로로 이동하게 됩니다. 이 방법 덕분에 별도의 클라이언트 사이드 코드 없이도 깔끔한 흐름 제어가 가능해요.

---

이처럼 Next.js의 Server Function 내에서 캐시 재검증과 리다이렉트를 함께 활용하면, 데이터 일관성을 유지하면서 사용자 경험도 한층 좋아지니 꼭 알아두세요!

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

업데이트 작업을 수행한 후에 사용자를 다른 페이지로 이동시키고 싶을 때가 있죠? 그럴 때는 Next.js의 Server Function 안에서 `redirect` 함수를 사용할 수 있어요.

아래 예시 코드를 한 번 볼게요!

```js
'use server'

import { redirect } from 'next/navigation'

export async function createPost(formData: FormData) {
  // 데이터 업데이트 작업 수행
  // ...
  
  // 업데이트 후 /posts 페이지로 리디렉션
  redirect('/posts')
}
```

여기서 포인트는, `redirect`가 호출되면 그 즉시 클라이언트에게 지정한 URL로 이동하라고 지시한다는 점이에요. 그래서 서버 함수 내에서 업데이트가 끝난 뒤에 자연스럽게 다른 페이지로 사용자를 안내할 수 있죠.

또 한 가지 팁! `redirect`를 사용할 때, 이 함수는 호출과 동시에 훅을 멈추고 이동을 처리하기 때문에 그 이후 코드는 실행되지 않는다는 점 기억해주세요. 만약 안전하게 특정 조건에서만 리디렉션을 하고 싶다면, 조건문 안에 넣어서 사용하면 돼요.

요약하자면, Next.js 13 이상의 서버 함수에서 업데이트 후 바로 페이지 전환을 원할 땐 `redirect`를 활용해보세요. 페이지 새로고침 없이 부드럽게 이동할 수 있어서 사용자 경험도 좋아진답니다!