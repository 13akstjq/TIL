---
title: "Next.js 15에서 Incremental Static Regeneration(ISR)로 정적 웹사이트 업데이트 하기"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:01
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Incremental Static Regeneration (ISR)"
link: "https://nextjs.org/docs/app/building-your-application/data-fetching/incremental-static-regeneration"
isUpdated: false
---


# Incremental Static Regeneration (ISR)이란?

안녕하세요! 오늘은 **Incremental Static Regeneration (ISR)**에 대해 쉽고 간단하게 이야기해보려고 해요. ISR은 특히 Next.js 같은 프레임워크에서 자주 쓰이는 개념인데, 기본적으로 정적 페이지를 효율적으로 업데이트할 수 있게 해주는 방법이에요.

---

## ISR이 왜 좋을까?

- **전체 사이트를 다시 빌드하지 않아도 돼요!**  
  기존에 정적으로 생성된 페이지들을 모두 다시 생성하는 대신, 바뀐 페이지만 업데이트할 수 있답니다.

- **서버 부하가 줄어들어요.**  
  거의 대부분의 요청에 대해 이미 만들어진 정적 페이지를 바로 보여주기 때문에 서버가 한결 가벼워져요.

- **cache-control 헤더가 자동으로 달려요.**  
  페이지마다 적절한 캐시 정책이 적용되어서, 사용자에게 빠른 경험을 제공할 수 있어요.

- **콘텐츠가 많은 사이트도 걱정 끝!**  
  많은 페이지를 가진 사이트도 빌드 시간이 너무 길어지는 걸 피할 수 있어요.

---

## 간단한 예시

다음은 ISR을 활용한 아주 기본적인 Next.js의 `getStaticProps` 예시입니다.

```jsx
export async function getStaticProps() {
  const data = await fetchDataSomehow();

  return {
    props: {
      data,
    },
    // 페이지가 60초마다 다시 생성되도록 설정
    revalidate: 60,
  };
}
```

위에서 핵심은 `revalidate` 키예요. 여기서 60초를 지정했으니, 사용자가 페이지에 접속할 때마다 적어도 60초가 지난 후에는 새로 데이터를 가져와서 페이지를 다시 만들어줘요. 

즉, "최소 60초 주기로 페이지를 갱신한다" 라고 이해하면 돼요.

---

## 좀 더 알려드리는 팁!

- `revalidate`에 너무 짧은 시간을 설정하면, 서버 부하가 다시 올라갈 수 있으니 적당한 값을 찾는 게 중요해요.
- 현재 Next.js는 ISR을 사용하면서 자동으로 incremental cache를 관리해줍니다. 덕분에 개발자가 별도로 캐싱 로직을 신경 쓸 필요가 줄어듭니다.
- 빌드 후 데이터가 자주 변경되는 뉴스, 블로그, 마켓플레이스 같은 서비스에서 굉장히 빛을 발합니다.

---

자, 오늘은 ISR에 대해서 간단히 알아봤는데요. 정적 사이트의 장점과 동적인 데이터 갱신을 함께 누리고 싶은 분들에게 정말 추천드리는 기술이에요. 더 궁금한 점이나 실제 예제 궁금하면 언제든지 댓글 달아주세요! 😊

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

여러분, Next.js에서 동적 라우팅과 ISR(Incremental Static Regeneration)을 활용하는 예제를 하나 같이 살펴볼게요! 위에 작성된 코드를 바탕으로 우리 블로그 포스트 페이지를 만든다고 가정해봅시다.

### 이 코드가 뭘 하는 거냐면요

- `generateStaticParams` 함수 안에서 `https://api.vercel.app/blog` API를 호출해, 빌드 시점에 미리 25개의 블로그 포스트 경로를 생성해요.
- 이렇게 미리 생성된 페이지들은 사용자 요청 시 즉시 로드되고 캐시되죠.
- `revalidate = 60` 설정 덕분에, 60초마다 페이지가 백그라운드에서 다시 생성됩니다. 즉, 오래된 페이지를 보여주더라도 동시에 최신 페이지를 서버에서 준비시키는 거예요.
- 만약 아직 생성하지 않은 새로운 블로그 포스트 URL이 들어오면, `dynamicParams = true` 설정 때문에 404 대신 서버에서 해당 페이지를 “on-demand”로 생성해줍니다.

한 마디로, 정적 생성과 서버 사이드 렌더링의 장점을 적절히 섞어 어떤 페이지는 미리 생성해 빠르게 제공하고, 새 페이지는 요청이 들어올 때 만들어서 서비스하는 똑똑한 방식인 거죠!

---

### 조금 더 풀어서 설명해볼게요!

| 설정                     | 설명                                                        |
|-------------------------|-------------------------------------------------------------|
| `revalidate = 60`       | 한번 캐싱한 페이지는 60초간 다시 생성하지 않고 캐시를 사용함. 60초 후 첫 요청 때 백그라운드로 새 페이지를 생성함. |
| `dynamicParams = true`  | `generateStaticParams`에 없는 경로가 요청되면 404가 아닌 동적으로 페이지를 생성함. |
| `generateStaticParams`  | 빌드 타임에 미리 생성할 동적 경로 목록을 반환함. 이 예제는 모든 블로그 포스트 ID가 여기 포함됨. |
| `Page` 컴포넌트          | 요청된 `id` 파라미터로부터 해당 포스트를 API에서 받아와서 렌더링함. |

---

### 추가적으로 알아두면 좋은 점!

- 이 방식은 데이터가 자주 변하지 않는 콘텐츠에 딱 맞아요. 아주 빠른 실시간 업데이트는 어렵지만, 몇 분 단위 변경이라면 사용자 경험을 살릴 수 있습니다.
- 만약 완전 최신 데이터가 필요하면 `revalidate`를 0으로 두고 매 요청마다 서버 렌더링(SSR)을 할 수도 있어요. 하지만 속도는 느려질 수 있죠.
- Next.js 13버전 이상에서 도입된 이 방식은 ISR과 동적 라우팅을 동시에 처리할 때 좋은 선택이니, 블로그나 뉴스 사이트 같은 프로젝트에 활용해보세요!

---

필요하면 제가 이 코드를 기반으로 직접 해볼 수 있는 샘플 프로젝트도 만들어서 공유해드릴게요. 혹시 관련해서 궁금한 점, 더 깊게 알고 싶은 부분 있으면 편하게 물어봐 주세요!

참고로 공식 문서도 정말 잘 정리되어 있으니 한 번 쭉 읽어보시면 큰 도움이 됩니다.

---

## 참고 자료

- [Next.js Dynamic Routes & ISR 공식 문서](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes#incremental-static-regeneration)
- [Next.js 13 App Router 소개](https://nextjs.org/docs/getting-started/upgrade-guide#app-router)

다음에도 쓸모 있는 Next.js 팁으로 찾아올게요! :)

---

> 혹시 이 내용을 마크다운 문서로 쓰거나 블로그에 올리고 싶다면 추가 설명과 코드는 댓글로도 언제든 요청해 주세요!

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

### 라우트 세그먼트 설정(Route segment config)

라우트를 잘 관리하려면 설정할 수 있는 옵션들이 몇 가지 있는데요, 대표적으로 `revalidate`와 `dynamicParams`가 있어요.

| 옵션 이름       | 설명                                                         |
|----------------|--------------------------------------------------------------|
| revalidate     | 해당 라우트 데이터를 다시 검증하는 주기(초 단위)를 설정해요. 이걸 설정하면 데이터를 자동으로 갱신해서 최신 상태를 유지할 수 있답니다.  |
| dynamicParams  | 라우트에서 동적 파라미터를 사용할지 여부를 결정해요. 동적 파라미터란 URL 경로 내에서 변하는 값들을 말하죠(예: `/post/[id]`의 `id`). 이 옵션으로 다루는 방식을 설정할 수 있어요. |

---

### 함수 소개(Functions)

`revalidate`와 연관해서 사용할 수 있는 함수들도 있어요! 주로 캐시를 새로고침하거나 특정 경로를 다시 유효화하는 용도로 쓰이는데요.

| 함수 이름       | 설명                                                                                          |
|----------------|-----------------------------------------------------------------------------------------------|
| revalidatePath  | 특정 경로(path)에 대해 데이터를 재검증하도록 요청하는 함수예요. 예를 들어, 어떤 글을 수정한 후 그 경로를 갱신할 때 사용하죠.  |
| revalidateTag   | 태그(tag)를 이용해 어느 그룹에 속한 데이터들을 한 번에 다시 검증하도록 할 수 있어요. 여러 경로를 묶어서 관리할 때 유용해요.       |

---

### 덧붙여서

Next.js 같은 최신 웹 프레임워크들에서 이런 revalidation 시스템을 잘 활용하면, 서버의 부담을 크게 줄이면서도 사용자에게 항상 최신 데이터를 보여줄 수 있어요. 예를 들어, 자주 변하지 않는 페이지는 캐시를 오래 유지하고, 실시간으로 변하는 페이지는 자주 재검증해서 최적 성능을 유지할 수 있죠.

또한, `dynamicParams` 옵션을 올바르게 설정하면, 빌드 시에 미리 생성할 경로나 런타임에 처리할 경로를 적절하게 나눌 수 있어서 프로젝트 구조를 깔끔하게 유지하는데 큰 도움이 돼요.

이런 설정과 함수들을 잘 이해하고 활용해보시면, 더욱 빠르고 효율적인 웹앱을 만들 수 있답니다. 궁금하신 점 있으면 언제든 질문 주세요!

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

### 시간 기반 리밸리데이션(Time-based Revalidation)

이 예시는 `/blog` 경로에서 블로그 글 목록을 불러와 보여주는 방법이에요. 그리고 중요한 점은 페이지에 캐시된 데이터가 1시간마다 새로 고쳐진다는 거죠. 

어떻게 동작하냐면, 첫 방문 시 데이터를 불러와서 캐시에 저장합니다. 그 후 1시간이 지나 다음에 페이지를 방문하면, 기존 캐시를 바로 버리고 다시 가져오는 게 아니라, 사용자에게는 기존 페이지를 보여주면서 백그라운드에선 새로운 글 목록으로 페이지를 다시 생성해 캐시를 업데이트해요. 덕분에 사용자는 항상 빠르고 끊김 없는 경험을, 백엔드는 최신 데이터를 보장할 수 있답니다.

```js
interface Post {
  id: string
  title: string
  content: string
}
 
export const revalidate = 3600 // 3600초 = 1시간마다 캐시 무효화
 
export default async function Page() {
  const data = await fetch('https://api.vercel.app/blog')
  const posts: Post[] = await data.json()
  return (
    <main>
      <h1>Blog Posts</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </main>
  )
}
```

---

### 추가 팁!

- 여기서 `revalidate` 값에 넣는 숫자는 초(seconds) 단위예요. 3600은 1시간을 의미하니까, 10분마다 갱신하려면 600으로 바꾸면 되겠죠?
- 이렇게 시간 기반 갱신은 자주 변하지 않는 데이터에 딱이에요. 예를 들어 뉴스 헤드라인이나 프로필 정보처럼 자주 고쳐질 필요 없는 데이터를 캐시에 담기로 적합하죠.
- 만약 데이터가 수시로 업데이트되어 즉각 반영이 필요하면 `revalidate`를 사용하지 않고 서버 사이드 렌더링(SSR) 또는 클라이언트 측 데이터 패칭을 고려해보세요.
- `Next.js` 같은 최신 프레임워크를 쓰면 이런 캐시 전략을 아주 간편하게 구현할 수 있으니, 프로젝트에 따라 적절히 활용해보세요!

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

재검증(revalidation) 시간을 길게 설정하는 걸 추천해요. 예를 들어, 1초 대신 1시간으로 말이죠. 만약 더 세밀한 데이터 갱신이 필요하다면 on-demand revalidation(필요할 때만 재검증)를 고려해보세요. 그리고 실시간 데이터가 꼭 필요하다면, 동적 렌더링(dynamic rendering)으로 전환하는 것도 좋은 방법입니다.

### 필요할 때만 재검증: revalidatePath 활용법

좀 더 정확하게 데이터 재검증을 하고 싶다면, revalidatePath 함수를 써서 페이지를 필요할 때만 무효화시키는 방법이 있어요.

예를 들어, 새로운 게시글을 추가한 후에 이 Server Action을 호출하면, Server Component에서 fetch를 쓰든 DB에 직접 연결하든 상관없이 해당 경로(route)의 캐시를 지우고, Server Component가 새로운 데이터를 가져오도록 할 수 있습니다.

이렇게 하면 불필요한 데이터 갱신을 줄이면서도 필요한 순간에 최신 데이터를 보여줄 수 있어서 성능과 사용자 경험 두 마리 토끼를 잡을 수 있답니다. 

간단하게 요약하면:

| 상황                  | 추천 방법                         |
|---------------------|------------------------------|
| 데이터 갱신이 1초 단위로 자주 필요하다면 | on-demand revalidation 사용       |
| 실시간 데이터가 꼭 필요하다면        | 동적 렌더링(dynamic rendering) 고려 |
| 갱신 주기가 길어도 무방하다면       | 긴 revalidation 시간(예: 1시간) 설정 | 

이렇게 상황에 맞게 적절한 방법을 선택하는 게 중요해요!

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

여러분, Next.js에서 서버 측 캐시를 관리할 때 `revalidatePath` 함수만큼 유용한 게 없어요. 위 코드처럼 `/posts` 경로의 캐시를 무효화하고 싶을 때 아주 간단하게 쓸 수 있거든요.

```js
'use server'
 
import { revalidatePath } from 'next/cache'
 
export async function createPost() {
  // /posts 경로 캐시를 무효화하기
  revalidatePath('/posts')
}
```

이렇게 하면 새로운 게시글을 추가하고 나서 바로 `/posts`를 방문했을 때 최신 글 목록을 볼 수 있죠.

### 근데, 만약 경로 단위가 아니라 더 세밀한 캐시 무효화가 필요하다면?

바로 `revalidateTag`라는 기능을 써볼 수 있어요. 이건 fetch 요청에 태그를 달아서, 특정 태그가 붙은 데이터만 골라서 다시 불러오게 하는 방법이에요. 예를 들어, 게시글 하나하나에 태그를 붙여서 수정된 글만 빠르게 반영하도록 할 수 있죠.

#### 간단 예시

```js
'use server'

import { revalidateTag } from 'next/cache'

export async function updatePost(id) {
  // 특정 게시글에 붙은 태그를 기반으로 캐시를 무효화하기
  revalidateTag(`post-${id}`)
}
```

fetch를 할 때도 태그를 붙여줘야 하는데요,

```js
const post = await fetch(`/api/posts/${id}`, {
  next: { tags: [`post-${id}`] },
}).then(res => res.json())
```

이렇게 하면 이 게시글은 `post-123` 같은 식으로 태그가 붙고, 해당 게시글이 업데이트될 때만 캐시가 무효화되어 더 효과적으로 리소스를 관리할 수 있답니다.

---

### 정리

| 함수명           | 역할                           | 사용 케이스                               |
|----------------|------------------------------|----------------------------------------|
| `revalidatePath` | 경로 단위 캐시 무효화                | 페이지 전체를 새로 고쳐야 할 때                      |
| `revalidateTag` | 태그 단위 캐시 무효화                | 데이터 단위로 세밀하게 캐시를 관리하고 싶을 때          |

---

이렇게 `revalidateTag`를 활용하면, 대용량 데이터나 자주 변경되는 목록이 있을 때 정말 도움 많이 돼요. 오늘 바로 적용해보시고 캐시 전략 더 똑똑하게 짜보세요!

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

Next.js에서 데이터를 효율적으로 캐싱하고 다시 불러오는 방법에 대해 알아볼게요! 특히 `fetch` 함수와 ORM을 이용할 때, 그리고 캐시를 자동으로 재검증하는 방법까지 다뤄보겠습니다.

---

### 1. fetch 함수에 `next` 옵션으로 태그 지정하기

```js
export default async function Page() {
  const data = await fetch('https://api.vercel.app/blog', {
    next: { tags: ['posts'] }, // 여기에 태그를 넣어 캐시를 관리할 수 있어요
  })
  const posts = await data.json()
  // 이제 posts를 화면에 렌더링
}
```

- `next: { tags: ['posts'] }` 옵션을 주면, 이 요청이 `'posts'`라는 태그와 연결돼서 관리돼요.
- 나중에 `'posts'` 태그가 변경됐을 때 캐시를 재검증 할 수 있답니다.

---

### 2. ORM이나 DB 연결 시 `unstable_cache` 사용하기

```js
import { unstable_cache } from 'next/cache'
import { db, posts } from '@/lib/db'
 
const getCachedPosts = unstable_cache(
  async () => {
    return await db.select().from(posts)  // 데이터베이스에서 포스트를 조회
  },
  ['posts'],                   // 캐시 관련 태그
  { revalidate: 3600, tags: ['posts'] }  // 3600초(1시간)마다 재검증, tags와 연결
)
 
export default async function Page() {
  const posts = getCachedPosts()  // 캐시된 데이터를 불러옴
  // ...
}
```

- `unstable_cache` 함수는 캐시된 결과를 기억하며 해당 데이터를 재활용해요.
- `revalidate` 옵션으로 몇 초마다 다시 데이터를 불러올지 설정할 수 있어요.
- 태그를 사용해서 데이터를 더욱 세밀하게 관리 가능!

---

### 3. 캐시가 무효화되었을 때 재검증하기 - `revalidateTag` 활용

서버 액션(Server Actions)이나 라우트 핸들러(Route Handler) 안에서 다음과 같이 캐시 태그를 재검증할 수 있습니다:

```js
import { revalidateTag } from 'next/cache'

export async function POST(request) {
  // 예를 들어 데이터를 변경하는 로직
  // ...
  
  // 변경 후 'posts' 태그 관련 캐시를 재검증해서 최신 데이터 사용하도록 유도
  revalidateTag('posts')

  return new Response('Updated and cache revalidated')
}
```

- 업데이트가 발생하면 캐시 무효화를 하면서 관련된 태그를 재검증합니다.
- 이렇게 하면 데이터가 바뀌었을 때 페이지가 최신 상태로 렌더링돼요.

---

## 조금 더!

- 이 태그 기반 캐시 전략은 여러 페이지에서 같은 API 데이터를 공유할 때 정말 유용해요.
- `unstable_cache`는 아직 `unstable` 단계이니, 공식 문서나 업데이트 내용을 주의 깊게 지켜보는 게 좋아요.
- `fetch` 함수의 `next` 옵션 중에 `revalidate` 값을 직접 지정할 수도 있고, `tags`와 혼합해서 사용하면 훨씬 강력한 캐싱 전략을 구성할 수 있습니다.

잘 활용하면 페이지 성능과 데이터 일관성을 높일 수 있으니 여유 있을 때 한번 적용해보세요! 도움이 되셨으면 좋겠네요. 😊

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

이번에는 Next.js에서 서버 측 캐시를 다루는 방법 중에서, revalidateTag 함수를 사용하는 간단한 예제를 살펴보고, 오류 처리와 캐시 위치 커스터마이징에 대해 알아볼게요.

```js
'use server'
 
import { revalidateTag } from 'next/cache'
 
export async function createPost() {
  // 'posts'라는 태그가 붙은 모든 캐시 데이터를 무효화합니다.
  revalidateTag('posts')
}
```

위 코드에서 `revalidateTag('posts')`를 호출하면, 캐시에 'posts'라는 태그로 분류된 데이터가 모두 무효화되어 다음 요청 시 새로운 데이터를 다시 가져오도록 만듭니다. 이렇게 하면 게시글 리스트나 관련 콘텐츠가 업데이트될 때, 서버 캐시를 손쉽게 갱신할 수 있죠.

### 예기치 않은 예외 처리 (Handling uncaught exceptions)

만약 데이터 재검증 중에 에러가 발생한다면, Next.js는 바로 새로운 데이터 요청을 실패하더라도 이전에 성공적으로 생성된 데이터를 계속해서 캐시에서 제공합니다. 즉, 사용자에게는 데이터가 갑자기 사라지거나 오류가 노출되지 않고 안정적으로 서비스가 유지됩니다. 그리고 다음 요청 시점에 다시 재검증을 시도하니, 일시적인 문제라도 금방 해결됩니다.

이 부분이 꽤나 유용한데, 서버 오류로 인한 서비스 중단 없이도 캐시가 안정적으로 유지되고 복구되기 때문이죠.

### 캐시 위치 커스터마이징하기

Next.js 내장 캐시는 기본적으로 프로젝트 내 `.next/cache` 폴더에 저장됩니다. 하지만 상황에 따라 캐시 위치를 바꾸고 싶을 때가 있죠. 예를 들어, CI/CD 환경에서 캐시를 공유하거나 특정 디스크 경로에 캐시를 두고 싶을 때가 그렇답니다.

Next.js에서는 `next.config.js` 파일에서 `experimental` 설정 아래 `serverActions`와 같은 옵션을 다루듯, 캐시 관련 설정도 향후 업데이트를 기대할 수 있는데, 현재 공식 문서에서는 구체적인 캐시 위치 변경 방법은 제한적입니다. 그래도 곧 더 다양한 설정 옵션들이 추가될 예정이라서, 공식 문서를 꾸준히 확인하는 걸 추천드려요.

---

마지막으로 Next.js의 캐시 전략은 점점 발전 중이라서, 개발자가 직접 캐시 태그를 관리하며 성능을 최적화할 수 있는 부분이 늘고 있어요. 보다 유연하고 빠른 데이터 페칭을 위한 중요한 기능이니, 꼭 한번 직접 써보시면서 익혀두시면 좋을 것 같아요! 혹시 서비스 로딩 속도가 느리거나 데이터 최신화에 문제가 있을 때, 캐시 무효화 전략부터 점검해보세요 :)

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

Next.js에서 페이지를 캐싱하고 재검증하는 기능인 Incremental Static Regeneration(ISR)은 동일한 캐시를 공유해서 사용해요. 

만약 Vercel에 배포한다면, ISR 캐시는 자동으로 안정적인 저장소에 저장되어서 데이터가 안전하게 보존되죠. 그런데 직접 서버를 운영(셀프 호스팅)할 때는 ISR 캐시가 Next.js 서버 내의 파일 시스템(디스크)에 저장돼요. Pages Router와 App Router 둘 다 셀프 호스팅할 때 이 방식이 자동으로 적용됩니다.

또한 필요에 따라 Next.js 캐시 위치를 직접 설정할 수도 있어요. 이렇게 하면 캐시된 페이지와 데이터를 더 영속적인 저장소에 보관할 수 있고, 여러 컨테이너나 인스턴스에서 캐시를 공유하는 것도 가능해집니다. 더 자세한 설정 방법은 공식 문서를 참고해 보세요.

---

### 간단히 말해서!
- **Vercel 배포:** 캐시 자동 저장(내구성 있는 저장소)
- **셀프 호스팅:** 캐시가 서버 디스크에 저장
- **캐시 위치 변경 가능:** 여러 서버 간 캐시 공유 및 데이터 보존 용도

만약 캐시 문제가 발생하거나 페이지가 예상대로 재검증되지 않는다면 캐시 설정을 점검해 보는 게 좋겠죠? 다음번에는 흔한 이슈와 해결법도 함께 소개할게요!

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

### 로컬 개발 환경에서 캐시된 데이터 디버깅하기

fetch API를 사용할 때, 어떤 요청이 캐시된 것인지, 혹은 캐시되지 않은 것인지 확인하고 싶다면 추가적인 로깅(logging) 설정을 해보는 게 좋아요. 이렇게 하면 네트워크 요청이 실제로 어떻게 처리되는지 한눈에 파악할 수 있답니다. 

예를 들어, 아래처럼 `logging` 옵션을 설정하면 모든 fetch 호출의 전체 URL(fullUrl)을 로그로 남길 수 있어요.

```js
module.exports = {
  logging: {
    fetches: {
      fullUrl: true,
    },
  },
}
```

이 설정을 통해 "아, 이 요청은 캐시에서 바로 가져왔구나", "이 요청은 서버에 직접 다녀왔구나" 같은 정보를 쉽게 알 수 있게 됩니다. 특히 복잡한 캐싱 로직을 다룰 때, 문제를 빠르게 찾아내는 데 정말 유용하죠.

또한, 브라우저 개발자 도구(Network 탭)를 열어 보거나, 서버에서 캐시 헤더(Cache-Control, ETag 등)를 확인하는 것도 도움이 됩니다. 이런 방법들을 병행하면 캐시 동작을 더 명확히 이해할 수 있어요.

### 프로덕션 환경에서의 올바른 동작 확인하기

로컬 환경에서 잘 작동한다고 해도, 프로덕션 환경에서는 다른 조건들이 작용할 수 있어요. 그렇기 때문에 실제 운영 서버에서 캐시가 제대로 동작하는지 반드시 확인해야 합니다. 다음과 같은 점들을 체크해 보세요.

- **실제 네트워크 요청 확인**: 운영 중인 서버의 로그 혹은 브라우저 개발자 도구를 통해 매 요청마다 캐시가 제대로 적용되는지 봅니다.
- **HTTP 캐시 헤더 점검**: 서버가 올바른 캐시 정책을 설정했는지 확인해 보세요. 캐시 만료시간, 재검증(ETag, Last-Modified) 관련 헤더들이 적절히 사용되는지 중요합니다.
- **캐시 무효화 전략 검토**: 업데이트가 필요한 데이터가 있을 때 캐시가 제대로 초기화(unset)되는지 테스트해 보아야 합니다.
- **콘텐츠 배포 네트워크(CDN) 설정**: 만약 CDN을 사용한다면 CDN 캐시 정책도 함께 고려해야 하니, CDN 로그나 설정도 꼭 살펴보세요.

이처럼 디버깅할 때 로컬 개발 환경뿐 아니라 프로덕션 환경에서의 케이스까지 살펴보면, 예상치 못한 캐싱 문제를 미리 차단할 수 있어서 안정적인 서비스를 운영할 수 있답니다!

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

프로덕션 환경에서 페이지가 제대로 캐시되고 재검증되는지 확인하려면, 로컬에서 직접 테스트해보는 게 좋아요. 방법은 간단해요!

1. 먼저 `next build` 명령어로 프로젝트를 빌드합니다.
2. 그리고 `next start`로 프로덕션용 Next.js 서버를 실행해보세요.

이렇게 하면 ISR(Incremental Static Regeneration) 동작을 실제 프로덕션 환경처럼 경험할 수 있어요. 

그리고 개발하는 동안 캐시 동작을 좀 더 자세히 보고 싶다면, 프로젝트 루트에 있는 `.env` 파일에 아래 환경 변수를 추가해보세요.

```js
NEXT_PRIVATE_DEBUG_CACHE=1
```

이걸 넣으면 Next.js 서버 콘솔에서 ISR 캐시 히트(hit)와 미스(miss) 정보를 출력해줍니다. 덕분에 빌드 과정에서 생성된 페이지들과, 사용자 요청에 따라 어떤 경로들이 새롭게 생성되는지 세세하게 확인할 수 있죠.

---

추가로 팁 하나! 서버 로그를 보면서 페이지가 갱신되는 타이밍을 체크하면, 재검증 설정(`revalidate` 옵션)과 실제 동작이 잘 맞는지 쉽게 디버깅할 수 있어요. 혹시 ISR이 잘 작동하지 않는다고 느껴지면, 이 환경 변수 설정과 로그 확인부터 해보시길 추천합니다!

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

## 주의할 점 (Caveats)

- ISR(Incremental Static Regeneration)은 기본적으로 Node.js 런타임 환경에서만 지원돼요.
- Static Export 방식으로 프로젝트를 만들 때는 ISR을 사용할 수 없답니다.
- 정적으로 렌더링된 경로에서 여러 개의 fetch 요청을 하고 각각 다른 revalidate 주기를 설정했다면, 가장 짧은 시간이 ISR에 적용돼요. 하지만 Data Cache에서는 각각 설정한 재검증 주기가 따로 지켜진답니다.
- 만약 경로 내 fetch 요청 중에 revalidate 시간이 0이거나 명시적으로 no-store가 설정되어 있다면, 그 경로는 동적으로 렌더링돼요.
- 미들웨어(Middleware)는 on-demand ISR 요청에는 실행되지 않아요. 즉, 경로 재작성(path rewrites)이나 미들웨어 내 로직이 적용되지 않으니, 재검증할 때는 꼭 정확한 경로를 지정해야 해요. 예를 들어, /post-1로 rewrite된 경로가 아니라 /post/1과 같이 실제 경로를 사용해야 합니다.

---

## 버전 히스토리 (Version history)

| 버전       | 변경 사항                                      |
|------------|--------------------------------------------|
| v14.1.0    | 커스텀 `cacheHandler`가 안정화되었어요.          |
| v13.0.0    | App Router가 도입되었답니다.                      |
| v12.2.0    | Pages Router에서 On-Demand ISR이 안정화됐어요.    |
| v12.0.0    | Pages Router에 [Bot-aware ISR fallback](https://nextjs.org/blog/next-12#bot-aware-isr-fallback)이 추가되었죠. |
| v9.5.0     | Pages Router에 [안정화된 ISR](https://nextjs.org/blog/next-9-5)이 도입되었어요. |

---

이번 내용에서는 Next.js에서 ISR을 사용할 때 주의해야 할 점들과 버전별 주요 업데이트 소식을 함께 정리했는데요, 보통 ISR을 잘 활용하면 성능과 SEO 모두 잡을 수 있어 굉장히 유용해요. 다만 위에서 언급한 몇 가지 제한사항들을 꼭 숙지해서 개발 중 예상치 못한 문제가 생기지 않도록 하는 게 중요해요.

또한, 버전업이 자주 되기 때문에 새로운 기능이나 안정화된 기능들은 공식 블로그나 문서를 꾸준히 체크하는 걸 추천드려요!