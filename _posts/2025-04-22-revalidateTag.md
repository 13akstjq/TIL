---
title: "Next.js 15에서 특정 캐시 태그 갱신하는 방법 (revalidateTag)"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:46
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "revalidateTag"
link: "https://nextjs.org/docs/app/api-reference/functions/revalidateTag"
isUpdated: false
---


# revalidateTag

`revalidateTag`은 특정 캐시 태그에 해당하는 데이터를 필요할 때 바로 지워서 새롭게 갱신할 수 있도록 도와주는 기능이에요.

> 알아두면 좋은 점:  
`revalidateTag`는 캐시를 바로 무효화하는 게 아니라, 해당 경로를 다음에 방문할 때 무효화가 이루어진다는 점이에요. 즉, 동적 라우트 세그먼트에 대해 `revalidateTag`를 호출해도 한꺼번에 많은 재검증이 바로 일어나는 것이 아니라, 경로를 실제로 다음에 방문할 때 무효화가 일어나는 방식입니다.

## Parameters (매개변수)

| 이름       | 타입      | 설명                                              |
|------------|-----------|---------------------------------------------------|
| tag        | string    | 무효화하려는 캐시 태그 이름                        |

---

### 좀 더 팁을 드리자면

`revalidateTag`는 데이터가 자주 바뀌는 페이지에서 유용해요. 예를 들어, 뉴스 사이트에서 특정 카테고리의 최신기사를 업데이트할 때, 전체 페이지를 모두 새로 고치기보다 관련된 태그만 무효화해서 효율적으로 캐시를 관리할 수 있죠.

그리고 동적 라우트가 많은 사이트에서는 무분별하게 캐시를 바로 무효화하는 것보다 이렇게 "다음 방문 시 무효화"하는 방식이 서버 부하를 줄이고, 사용자 경험도 부드럽게 만들어준답니다. 꼭 기억해두세요!

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

요즘 웹 개발하면서 캐시 재검증(revalidation) 기능을 많이 쓰게 되는데, 여기서 `revalidateTag`라는 함수가 있어요. 이 함수는 특정 캐시 태그를 기반으로 데이터를 다시 검증하도록 도와줍니다.

```js
revalidateTag(tag: string): void;
```

- **tag**: 캐시와 연결된 태그 이름을 문자열로 넣어주면 되는데, 최대 256자까지 가능합니다. 그리고 대소문자를 구분하니 주의하세요.

또한, 데이터를 요청할 때 `fetch` 함수에서 태그를 붙여서 쓸 수도 있어요:

```js
fetch(url, { next: { tags: [...] } });
```

이렇게 하면 해당 요청과 연관된 캐시 태그를 쉽게 관리할 수 있어서, `revalidateTag`로 특정 태그가 붙은 모든 데이터를 한 번에 재검증할 수 있답니다.

---

### 추가 팁!

- 태그를 잘 관리하면 서버 부담을 줄이면서 최신 데이터를 유지할 수 있어요.
- 태그 이름은 간결하고 직관적으로 만드는 게 좋아요. 예를 들어, `user-profile-123` 같은 식으로요.
- 태그 재검증 기능은 Next.js 13 이하 버전에선 experimental로 제공되기도 하니, 항상 버전별 문서 확인도 해보세요!  

이처럼 캐시 관리에 태그를 활용하면, 데이터 일관성 유지와 성능 향상에 큰 도움이 된답니다. 꼭 활용해보세요!

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

## 반환값

`revalidateTag` 함수는 반환값이 없어요.

## 예제

### 서버 액션에서 사용하기

`revalidateTag` 함수를 서버 액션(Server Action) 내에서 사용할 때는 보통 데이터가 바뀌었을 때 특정 태그와 관련된 캐시를 재검증(revalidate)할 때 사용해요. 서버 쪽에서 데이터를 업데이트한 뒤 클라이언트 측에서 최신 데이터를 받도록 만드는 역할이죠.

예를 들어, 블로그 글을 수정하거나 댓글을 추가했을 때 관련된 페이지나 API 응답 캐시가 최신 상태로 유지되게 하기 위해 쓸 수 있어요.

```js
'use server';

import { revalidateTag } from 'next/cache';

export async function updatePost(data) {
  // 데이터 업데이트 로직 (DB에 저장 등)
  
  revalidateTag('blog-post');

  // 반환값이 없는 함수입니다.
}
```

> TIP: `revalidateTag`는 반환값이 없어서 함수 호출 직후엔 결과를 받을 필요가 없고, 재검증 작업은 백그라운드에서 조용히 처리돼요. 그래서 함수 호출 뒤 별도의 에러 핸들링도 필요 없는 편이랍니다.

이렇게 서버 액션과 함께 쓰면, 사용자 입장에선 데이터가 실시간처럼 빠르게 반영되는 경험을 할 수 있어서 좋아요!

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

여러분, 이번에는 Next.js에서 서버에서 캐시 관리를 좀 더 세밀하게 할 수 있는 `revalidateTag` 기능에 대해 이야기해볼게요.

기본적으로 Next.js에서는 페이지나 데이터를 변경하면 전체를 다시 빌드하거나 리렌더링하기도 하죠? 근데 때로는 특정 태그에 해당하는 데이터만 부분적으로 다시 캐시를 갱신하고 싶을 때가 있어요. 이럴 땐 `revalidateTag`를 사용하면 아주 유용합니다.

---

### 1. 폼 제출 후 캐시 태그 재검증하기

```js
'use server'

import { revalidateTag } from 'next/cache'

export default async function submit() {
  await addPost()           // 새로운 게시글 추가 함수 (가정)
  revalidateTag('posts')    // 'posts'라는 태그가 붙은 부분만 캐시를 재검증해줘요
}
```

여기서 `addPost`는 새 글을 데이터베이스나 저장소에 추가하는 비동기 함수라고 생각하면 됩니다. 글이 성공적으로 추가된 뒤, `revalidateTag('posts')`를 호출해서 'posts' 태그가 걸린 데이터나 페이지 캐시만 다시 유효하게 만들어줘요. 강력하죠?

---

### 2. Route Handler에서 동적으로 태그 재검증

```js
import type { NextRequest } from 'next/server'
import { revalidateTag } from 'next/cache'

export async function GET(request: NextRequest) {
  const tag = request.nextUrl.searchParams.get('tag')  // 쿼리에서 태그 가져오기
  revalidateTag(tag)                                   // 그 태그에 대해 재검증 호출
  return Response.json({ revalidated: true, now: Date.now() })
}
```

URL 쿼리 파라미터로 태그 값을 받아와서 해당 태그만 똑똑하게 재검증하는 API예요. 예를 들어 `/api/revalidate?tag=posts`로 요청하면 'posts' 태그를 가진 캐시만 갱신되겠죠.

---

### 왜 태그별 재검증이 좋은가요?

- **성능 최적화**: 전체 데이터를 다시 빌드하거나 전부 캐시 무효화하는 대신, 관련된 작은 단위만 관리할 수 있어서 서버 부담이 줄어들어요.
- **빠른 피드백 루프**: 자주 변경되는 데이터만 신속하게 반영할 수 있어 사용자 경험이 좋아집니다.
- **코드 가독성 & 유지보수성 향상**: 데이터 모델별로 태그를 분리해서 관리하면, 어떤 부분이 업데이트됐는지 명확해져서 협업에도 유리해요.

---

저는 개인적으로 이 기능을 도입할 때, 아래 몇 가지도 같이 생각해보는데요!

| 팁 | 설명 |
| --- | --- |
| 태그 네이밍 규칙 만들기 | `posts`, `users`, `comments` 등 명확하고 일관된 이름을 쓰면 좋습니다. |
| 태그별 작업 분리 | 특정 동작마다 어떤 태그를 재검증해야 하는지 문서화해두면 편해요. |
| 에러 핸들링 강화 | `revalidateTag`가 실패할 경우 로깅하거나 재시도 로직을 넣는 것도 추천합니다. |

---

Next.js에서 서버 캐시와 관련해 고민중이라면, `revalidateTag` 방법 꼭 써보세요! 이 작은 툴 하나로 데이터 최신화 작업이 훨씬 수월해질 겁니다. 나중에 더 구체적인 적용 사례도 공유해드릴게요. 궁금한 점 있으면 언제든 댓글로 남겨주세요!