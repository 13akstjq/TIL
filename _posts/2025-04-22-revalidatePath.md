---
title: "Next.js 15에서 revalidatePath를 활용해 데이터 재검증하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:45
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "revalidatePath"
link: "https://nextjs.org/docs/app/api-reference/functions/revalidatePath"
isUpdated: false
---


# revalidatePath 정리해보기

revalidatePath는 특정 경로에 대해 캐시된 데이터를 즉시 갱신(무효화)할 수 있게 해주는 함수예요.

> 참고하면 좋은 점:
revalidatePath는 해당 경로를 다시 방문할 때 캐시를 무효화시켜줘요. 즉, 함수 호출 후 바로 캐시가 갈아엎어지는 게 아니라, 다음에 그 경로에 접근할 때 무효화가 적용된다는 뜻이죠. 동적 경로 세그먼트로 호출해도 한꺼번에 여러 경로가 즉시 무효화되는 게 아니라 '다음 방문 시점'부터 적용돼요.

현재는 서버 액션 내에서 revalidatePath를 쓰면, 클라이언트 사이드 라우터 캐시에 있는 모든 경로가 무효화되는 임시 동작을 하고 있어요. 이 부분은 향후 개선되어서 특정 경로에만 적용되도록 바뀔 거랍니다.

한편 서버 사이드 라우트 캐시에서는 특정 경로만 무효화하는 걸로 동작해요.

---

## 파라미터

(여기에 파라미터 관련 내용이 따라올 예정인데, 현재는 정보가 없네요.)

---

### 팁 하나 덧붙이자면요

이 기능은 예를 들어 게시글을 수정했을 때 수정된 내용이 바로 캐시에 반영되도록 강제로 갱신하고 싶을 때 많이 쓰이죠. 단, 재방문 시에 무효화가 되니까 페이지를 사용자에게 리다이렉트하거나, 클라이언트에서 다시 방문하게 만들어야 캐시 갱신 효과를 바로 볼 수 있다는 점 잊지 마세요! 

무효화를 즉시 반영해주는 것과는 조금 다르니까, 상황에 따라 적절히 활용해보세요.

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

```ts
revalidatePath(path: string, type?: 'page' | 'layout'): void;
```

- **path**: 다시 유효성 검사를 하고 싶은 데이터와 연결된 파일 시스템 경로 문자열입니다. 예를 들어, `/product/[slug]/page`처럼 동적 세그먼트를 포함할 수 있고, 혹은 `/product/123` 같은 실제 라우트 경로를 쓸 수도 있어요. 1024자 미만이어야 하고, 대소문자를 구분합니다.
- **type**: (선택적) `'page'` 또는 `'layout'` 문자열로, 재유효성 검사를 할 경로 유형을 정하는 거예요. 경로에 동적 세그먼트가 있다면 반드시 이 파라미터를 넣어야 합니다. 반대로, 동적 페이지 경로의 실제 라우트 예시(`/product/1`)를 줄 때는 넣지 않아야 합니다.

## 반환값

`revalidatePath` 함수는 값을 반환하지 않습니다. 즉, `void` 타입이에요.

---

### 한 마디 더!

`revalidatePath`는 Next.js에서 ISR(Incremental Static Regeneration)을 쓸 때, 특정 경로나 레이아웃을 수동으로 재검증하고 싶을 때 유용합니다. 페이지를 변경했을 때 빌드 전체를 다시 할 필요 없이 부분적으로 빠르게 리프레시하는 느낌이라 생각하면 편해요.

예를 들어, 어떤 상품 디테일 페이지 내용을 업데이트했을 때 해당 상품 페이지의 캐시만 재검증해서 최신 상태로 만들 수 있죠. 동적 경로가 섞인 경우에는 `type`을 꼭 신경 써서 넣어줘야 원하는 경로가 제대로 갱신됩니다.

개발 중에는 이 함수를 잘 활용하면 페이지 리빌드 시간을 크게 단축시킬 수 있으니 꼭 기억해두세요!

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

### 특정 URL 재검증하기

```js
import { revalidatePath } from 'next/cache'

revalidatePath('/blog/post-1')
```

위 코드는 `/blog/post-1` 경로를 다음 페이지 방문 시 재검증하도록 설정하는 방법이에요. 즉, 사용자가 해당 URL에 접속하면 콘텐츠가 최신 상태인지 Next.js가 다시 확인해서 캐시를 갱신하게 되는 거죠.

추가로, revalidatePath는 아주 유용한 함수인데요, 페이지 빌드 후에 데이터가 바뀌었을 때 해당 페이지를 다시 유효성 검사해서 최신 내용을 보여주고 싶을 때 자주 사용됩니다. 예를 들어, 블로그 글을 수정하는 CMS 관리 페이지에서 이 함수를 호출하면 변경된 글이 사용자에게 바로 반영되게 할 수 있어요.

그리고 한 가지 팁을 더 드리자면, revalidatePath 외에도 revalidateTag 같은 함수도 있는데, 여러 페이지에서 공통으로 쓰이는 데이터(예: 헤더, 푸터 정보)를 태그 단위로 재검증할 때 편리합니다. 상황에 맞게 잘 활용해보세요!

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

### 페이지 경로 재검증 (Revalidating A Page Path)

```js
import { revalidatePath } from 'next/cache'

revalidatePath('/blog/[slug]', 'page')
// 또는 라우트 그룹을 사용할 때
revalidatePath('/(main)/blog/[slug]', 'page')
```

이 코드는 특정 페이지 파일과 일치하는 URL이 다음에 방문할 때 다시 검증하도록 만들어줍니다. 쉽게 말해, 해당 경로에 있는 페이지가 업데이트되었을 때, 다음 방문부터 최신 내용이 반영되도록 하는 기능이에요.

단, 중요한 점은 이 함수가 딱 지정한 경로에만 유효하다는 거예요. 예를 들어 `/blog/[slug]`를 재검증한다고 해서 `/blog/[slug]/[author]` 같은 하위 경로까지 자동으로 재검증되지 않습니다. 필요할 경우 하위 경로도 별도로 재검증해줘야 해요.

---

### 재검증에 대해 한마디 더!

이 기능은 SSR(서버 사이드 렌더링)이나 ISR(Incremental Static Regeneration)을 활용하는 프로젝트에서 자주 쓰입니다. 예를 들어, 블로그 글을 수정했을 때, 방문자가 새로 페이지를 요청하면 최신 버전을 보여주고 싶을 때 유용하죠.

하지만 너무 많은 경로에 대해 자주 재검증을 걸면 서버 부하가 늘 수 있으니, 꼭 필요한 경로에만 적절히 활용하는 게 좋습니다.

---

### 재검증 레이아웃 경로 (Revalidating A Layout Path)

(다음 섹션에서 계속 이어서 작성할게요!)

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

Next.js에서 제공하는 revalidatePath 함수에 대해 간단히 이야기해볼게요. 이 함수는 특정 경로와 그 경로에 연결된 레이아웃을 기준으로 페이지를 다시 검증(리빌드)하는 작업을 해줍니다.

```js
import { revalidatePath } from 'next/cache'

revalidatePath('/blog/[slug]', 'layout')
// 또는 경로 그룹 사용 시
revalidatePath('/(main)/post/[slug]', 'layout')
```

위 코드는 `/blog/[slug]`에 해당하는 레이아웃 파일을 기준으로, 해당 경로뿐만 아니라 그 경로 하위에 있는 다른 페이지들도 다음 방문 시 자동으로 다시 리빌드되게 해줍니다. 예를 들어 `/blog/[slug]/[another]` 같은 하위 경로도 포함되겠죠.

### 모든 데이터를 재검증 하는 방법

```js
import { revalidatePath } from 'next/cache'

revalidatePath('/', 'layout')
```

위처럼 루트 경로(`/`)에 대해 revalidatePath를 호출하면, 루트 레이아웃 아래에 속한 모든 페이지들이 다음 방문 시 다시 검증되어 최신 상태로 업데이트됩니다.

---

### 추가 팁!

- `revalidatePath`가 유용한 시점: 서버에서 어떤 데이터를 업데이트했는데 클라이언트 페이지에서 바로 최신 데이터가 반영되길 원할 때 써보세요.
- "layout" 옵션은 선택사항인데, 해당 경로의 레이아웃이 바뀌었을 때 그 변경 사항을 반영하기 위해 추가하는 경우가 많습니다.
- Next.js의 ISR(Incremental Static Regeneration)과 비슷한 개념이지만, 이 함수로 좀 더 세밀하게 특정 경로를 재검증할 수 있어서 편리해요.

실제로 프로젝트에서 데이터 변경이 잦거나 특정 유저 액션 후 페이지를 새로 고침 시 최신 데이터를 보여주고 싶다면 적극 활용해보시길 바랍니다!

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

클라이언트 사이드 라우터 캐시를 지우고, 다음에 페이지를 방문할 때 데이터 캐시를 다시 검증하는 작업에 대해 이야기해볼게요.

### 서버 액션(Server Action)

```js
'use server'

import { revalidatePath } from 'next/cache'

export default async function submit() {
  await submitForm()
  revalidatePath('/')
}
```

위 코드는 간단하게 설명하면, 폼 제출 같은 서버 액션이 일어난 후에 특정 경로('/')의 캐시를 강제로 다시 검증(revalidate)하도록 해요. 이렇게 하면 클라이언트 쪽에 저장된 라우터 캐시를 초기화하고, 데이터가 최신 상태인지 확인할 수 있어서 사용자에게 더 정확한 정보를 제공할 수 있답니다.

**조금 더 풀어서 설명하자면**, `revalidatePath()` 함수는 Next.js의 서버 캐시를 특정 경로 기준으로 새로 고치는 역할을 해요. 이 덕분에, 예를 들어 데이터가 업데이트된 페이지를 구독하고 있는 사용자가, 다음 번 방문 때 반드시 최신 내용이 보여지도록 할 수 있죠.

### Route Handler

아래는 Route Handler에서 비슷한 맥락으로 쓰이는 코드의 예시입니다.

```js
import { revalidatePath } from 'next/cache'

export async function POST(request) {
  const data = await request.json()
  // 데이터 저장 로직 등 처리
  await saveData(data)

  // '/' 경로 캐시 재검증
  revalidatePath('/')

  return new Response('Success', { status: 200 })
}
```

Route Handler에서도 POST 요청 처리 후에 동일하게 `revalidatePath`를 호출해서 특정 경로의 캐시를 새로고침할 수 있어요.

---

> **TIP!**
>
> Next.js 13부터 새로운 데이터 패칭 방식과 서버 액션이 도입되면서, 이런 재검증 로직이 더 간편해졌어요. 특히 ISR(Incremental Static Regeneration)을 활용해 캐시를 누적하고, 필요한 순간에만 갱신하는 전략 덕분에 성능과 UX가 훨씬 좋아졌답니다. 적절한 타이밍에 `revalidatePath`를 호출하는 것이 핵심이에요!

필요하면 댓글이나 DM으로 질문 주세요. 더 친절하게 설명해드릴게요!

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

요즘 Next.js에서 ISR(Incremental Static Regeneration)을 활용할 일이 많아지면서, 특정 경로를 서버 사이드에서 동적으로 다시 빌드(재검증) 하는 방법에 대한 궁금증이 많아졌어요. 위에 코드가 그런 상황에서 유용한 예시라서 좀 쉽게 풀어서 설명해 드릴게요.

```js
import { revalidatePath } from 'next/cache'
import type { NextRequest } from 'next/server'

export async function GET(request: NextRequest) {
  // URL 쿼리에서 'path' 파라미터를 받아온다
  const path = request.nextUrl.searchParams.get('path')

  if (path) {
    // 해당 path를 Next.js에서 다시 빌드하도록 요청
    revalidatePath(path)
    return Response.json({ revalidated: true, now: Date.now() })
  }

  // path 파라미터가 없으면 에러 메시지 반환
  return Response.json({
    revalidated: false,
    now: Date.now(),
    message: 'Missing path to revalidate',
  })
}
```

### 코드 설명

- `revalidatePath`: Next.js 13에서 도입된 function으로, ISR에서 특정 경로를 다시 빌드하도록 시그널을 넣어줍니다.
- 이 API는 `GET` 요청을 받아서, 쿼리스트링에 `?path=/some-route` 형식으로 재검증할 경로를 전달하면 동작해요.
- 만약 `path`가 없으면 재검증을 할 수 없으니, 메시지를 되돌려주는 안전장치가 있죠.

---

### 여기서 더 알아두시면 좋은 점!

- `revalidatePath`는 내부적으로 Next.js의 캐시를 무효화해서, 다음에 해당 경로로 접근할 때 최신 버전의 페이지가 보여지게 만들어 줍니다.
- 재검증 요청을 API route처럼 만들어 두면, 관리자 페이지에서 "컨텐츠 업데이트" 버튼을 눌러 해당 페이지를 동적으로 최신화할 수 있어요.
- 주의할 점은, `path` 값은 Next.js 안에서 존재하는 페이지 경로여야 제대로 동작합니다. 없는 경로를 넣으면 에러는 안 나지만, 의미가 없겠죠.

---

### 보너스: 서버 컴포넌트라면 이런 식으로 쓰기도

Next.js 13 앱 디렉토리의 서버 컴포넌트에서는 이런 API route 없이 `revalidatePath`를 쓸 수 있습니다. 예를 들어, 폼 제출 후 해당 경로를 다시 검증하고 싶은 경우 이렇게요:

```ts
import { revalidatePath } from 'next/cache';

export async function updatePost(data: FormData) {
  // 글 업데이트 로직 수행
  const postId = data.get('postId') as string;

  // 포스트 페이지 재검증 요청
  revalidatePath(`/posts/${postId}`);
}
```

---

요약하자면, 오늘 보여드린 코드는 Next.js에서 특정 경로의 ISR 캐시를 수동으로 갱신하고 싶을 때 참 잘 쓰이는 방법입니다. 참고하시고, 필요할 때 활용해 보세요~!