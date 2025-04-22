---
title: "Next.js 13에서 generateStaticParams 사용법 정리"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:37
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "generateStaticParams"
link: "https://nextjs.org/docs/app/api-reference/functions/generate-static-params"
isUpdated: false
---


# generateStaticParams 함수 알아보기

Next.js에서 동적 라우트(dynamic route)를 사용할 때, `generateStaticParams` 함수를 활용하면 **빌드 시점에 미리 정적 페이지들을 생성**할 수 있어요. 덕분에 사용자가 페이지를 요청할 때마다 서버에서 동적으로 처리하는 게 아니라, 빠르게 미리 만들어 둔 페이지를 보여줄 수 있답니다.

---

## 어떻게 동작하나요?

```js
// [slug] 같은 동적 경로에 들어갈 param들을 미리 만들어 반환하는 함수예요!
export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json())
 
  return posts.map((post) => ({
    slug: post.slug,
  }))
}
 
// 미리 뽑아낸 param들로 여러 버전의 페이지가 생성돼요
export default async function Page({
  params,
}: {
  params: Promise<{ slug: string }>
}) {
  const { slug } = await params
  // 해당 slug를 이용해 데이터를 가져오거나 렌더링을 진행해요
}
```

이렇게 하면 `posts` 데이터에서 각 `slug`를 뽑아내서, 각 슬러그 별로 페이지가 **스태틱하게 생성**되죠.

---

## 알아두면 좋은 점

| 포인트 | 설명 |
|---|---|
| `dynamicParams` 옵션 | `generateStaticParams`로 생성되지 않은 동적 구간에 방문했을 때 어떻게 동작할지 설정 가능해요. |
| ISR(Incremental Static Regeneration) 설정 | 만약 런타임에 경로의 재검증이 필요하다면, 빈 배열을 반환하거나 `export const dynamic = 'force-static'`를 사용해야 해요. |
| 실행 시점 | - 개발 모드(`next dev`)에서는 라우트를 방문할 때마다 `generateStaticParams`가 호출돼요.<br>- 빌드 시점(`next build`)에는 관련 레이아웃이나 페이지가 생성되기 전에 한 번 호출돼요.<br>- ISR에서는 다시 호출되지 않아요. |
| 기존 함수와 비교 | 기존 Pages Router의 `getStaticPaths` 함수 대신 사용된답니다. |

---

## TIP! 더 편리하게 사용하려면?

1. **빨리 빌드하는 방법**: 너무 많은 페이지를 한 번에 만드는 경우 빌드 시간이 길어져서 시간이 한참 걸릴 수 있어요. 필요한 핵심 데이터만 뽑거나, 자주 바뀌지 않는 부분만 `generateStaticParams`로 처리하는 게 좋아요.

2. **ISR과 함께 쓰기**: `generateStaticParams`로 만드는 정적 페이지가 오래되어 조금씩 바뀌는 데이터라면, ISR 설정으로 일정 시간마다 다시 빌드 없이 업데이트되게 하는 것도 좋아요.

3. **`dynamicParams` 활용법**: 만약 generateStaticParams에 없는 슬러그가 들어오는 상황을 대비해, `dynamicParams: true` 옵션을 설정해 동적 경로를 SSR으로 처리할 수도 있어요.

---

Next.js 13의 `app` 디렉토리를 사용한다면 이 기능은 꼭 익혀 두세요! 덕분에 사용자에게 훨씬 빠른 페이지 로딩 속도와 좋은 경험을 제공할 수 있답니다.😉

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

## 파라미터(Parameters)

`options.params` (선택 사항)

만약 라우트(route)에 여러 개의 동적 세그먼트(dynamic segment)가 있고, 각각 `generateStaticParams`를 사용한다면, 자식의 `generateStaticParams` 함수는 부모가 생성한 파라미터 집합(params)마다 한 번씩 실행됩니다.

여기서 중요한 점은, 자식 세그먼트에서 사용하는 `params` 객체는 부모의 `generateStaticParams`에서 만들어진 파라미터들을 포함하고 있다는 거예요. 즉, 부모가 생성한 파라미터를 활용해서 자식 세그먼트의 파라미터를 만들 수 있다는 뜻이죠.

---

예를 들어, 부모 라우트에서 카테고리를 생성하고, 자식 라우트에서 해당 카테고리 내의 상품 ID를 생성하는 경우를 생각해 볼 수 있습니다. 이렇게 하면 부모-자식 관계를 유지하면서도 각각의 동적 경로를 효율적으로 생성할 수 있겠죠.

이 구조는 Next.js 같은 프레임워크에서 정적 사이트 생성(Static Site Generation) 시 매우 유용하니, 동적 라우팅을 할 때 꼭 기억해두시면 좋습니다!

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

## Returns (반환값)

`generateStaticParams` 함수는 배열을 반환해야 하는데, 이 배열 안에는 각 경로(dynamic route)의 동적 세그먼트(dynamic segment)가 담긴 객체들이 들어 있습니다.

- 배열 안의 각 객체는 해당 경로의 동적 세그먼트 이름과, 그 세그먼트에 채워질 값을 프로퍼티로 가지고 있어요.
- 여기서 프로퍼티의 이름은 세그먼트 이름과 똑같고, 값은 해당 세그먼트에 들어가야 할 실제 값이 됩니다.

아래 표를 보시면 이해가 더 쉽습니다.

| 예시 경로                         | `generateStaticParams`가 반환하는 타입         |
|----------------------------------|---------------------------------------------|
| `/product/[id]`                  | `{ id: string }[]`                           |
| `/products/[category]/[product]` | `{ category: string, product: string }[]`   |
| `/products/[...slug]`             | `{ slug: string[] }[]`                        |

---

### 조금 더 풀어보자면...

- `[id]`처럼 하나의 세그먼트가 들어가는 경로는, 객체 안에 그 세그먼트 이름(`id`)을 키로 두고 문자열 값을 넣으면 됩니다.
- `[category]`와 `[product]` 같이 여러 동적 세그먼트가 있을 땐, 각각 키로 넣고 값도 각각 넣으면 돼요.
- 그리고 `[...slug]`처럼 'catch-all' 세그먼트는 배열 형태의 문자열로 값을 넣어줘야 합니다!

이걸 잘 활용하면 Next.js에서 빌드 타임에 모든 필요한 페이지를 미리 생성할 수 있어서, 성능 최적화에 큰 도움이 됩니다. 꼭 만들어 보시면서 이해해보세요!

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

Next.js 13에서 동적 라우팅을 할 때, 미리 정적인 페이지를 생성하고 싶다면 `generateStaticParams`라는 함수를 활용할 수 있어요. 이 함수에서 반환하는 파라미터들을 기반으로 정적 페이지를 미리 만들어주는 거죠. 어떻게 쓰는지 한 번 살펴볼게요!

---

## 단일 동적 세그먼트 (Single Dynamic Segment)

예를 들어, 상품(id)에 따라 페이지를 미리 생성한다고 하면 이렇게 작성할 수 있어요:

```js
export function generateStaticParams() {
  return [{ id: '1' }, { id: '2' }, { id: '3' }]
}

// 이렇게 하면 총 세 개의 페이지가 빌드 타임에 생성됩니다:
// - /product/1
// - /product/2
// - /product/3
export default async function Page({
  params,
}: {
  params: Promise<{ id: string }>
}) {
  const { id } = await params
  // id를 활용해서 데이터 페칭 또는 렌더링 작업을 수행
}
```

여기서 주목할 점은 `params`가 `Promise`라는 점! Next.js 13에서는 비동기 params를 지원하니까, 필요한 데이터를 미리 가져오는 작업을 할 때 유용합니다.

---

## 다중 동적 세그먼트 (Multiple Dynamic Segments)

이번엔 URL에 동적 세그먼트가 2개 이상인 경우예요. 예를 들어 카테고리와 상품 ID가 함께 있을 때:

```js
export function generateStaticParams() {
  return [
    { category: 'a', product: '1' },
    { category: 'b', product: '2' },
    { category: 'c', product: '3' },
  ]
}

// 이렇게 하면 총 세 개의 페이지가 생성됩니다:
// - /products/a/1
// - /products/b/2
// - /products/c/3
export default async function Page({
  params,
}: {
  params: Promise<{ category: string; product: string }>
}) {
  const { category, product } = await params
  // category와 product 값을 바탕으로 페이지 렌더링
}
```

이처럼 여러 개의 동적 세그먼트가 있을 때도 `generateStaticParams`에서 각각의 조합을 배열로 반환해주면 빌드 시에 모두 커버되는 페이지를 만들어줍니다.

---

## 개인적인 팁

- `generateStaticParams`는 꼭 필요한 페이지를 미리 생성하는 용도로 써야 해요. 너무 많은 조합을 만들면 빌드 시간이 엄청 길어질 수 있으니까 주의!
- 만약 데이터가 너무 많거나 자주 변동된다면, SSR(Server Side Rendering)이나 ISR(Incremental Static Regeneration)을 함께 활용하는 것도 좋은 방법입니다.
- 타입스크립트를 쓴다면 `params` 타입을 명확히 지정해두면 코드 자동완성이나 안정성 측면에서 더 좋아요.

다음 프로젝트에 이 기능 써보면 SEO와 성능에 좋은 긍정적인 효과를 직접 느낄 수 있을 거예요!

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

## Catch-all Dynamic Segment 알아보기

위 예제를 보면, Next.js 같은 프레임워크에서 동적 경로를 한꺼번에 처리하는 방식을 보여주고 있어요. `slug`라는 배열을 이용해 여러 레벨의 URL 세그먼트를 잡아낼 수 있죠.

```js
export function generateStaticParams() {
  return [{ slug: ['a', '1'] }, { slug: ['b', '2'] }, { slug: ['c', '3'] }]
}

// 이렇게 반환된 params로 세 가지 페이지가 정적 생성돼요:
// - /product/a/1
// - /product/b/2
// - /product/c/3

export default async function Page({
  params,
}: {
  params: Promise<{ slug: string[] }>
}) {
  const { slug } = await params
  // 실제 페이지 내용 처리
}
```

이처럼 `generateStaticParams` 함수가 여러 경로에 대한 파라미터를 배열로 반환하면, 빌드 시점에 이 경로들이 미리 렌더링됩니다. 그래서 방문자가 해당 URL에 접속하면 서버에서 빠른 응답을 받을 수 있죠.

### 여기서 잠깐!

- **`slug`가 배열인 이유**: 2단계 이상의 동적 경로를 한 번에 잡기 위해서예요. 예를 들어 `/product/a/1`처럼 두 개의 세그먼트가 있으면 `slug`는 `['a', '1']`이 됩니다.
- **Static Generation & Dynamic Routing**: 이 패턴은 빌드 시점에 생성 가능한 경로만 미리 만들고, 그렇지 않은 경로는 런타임에서 처리할 수도 있어요. 필요에 따라 혼합해서 쓰면 좋아요.

## 예시

### Static Rendering

아래는 위 코드를 활용해서 실제 페이지를 정적으로 렌더링하는 간단한 예시를 생각해볼 수 있습니다.

| 경로          | 설명                    |
| ------------- | ----------------------- |
| /product/a/1  | slug: ['a', '1']        |
| /product/b/2  | slug: ['b', '2']        |
| /product/c/3  | slug: ['c', '3']        |

이런 식으로 동적 세그먼트를 유연하게 다룰 수 있는 구조가 되겠죠?

---

동적 라우팅과 정적 생성은 페이지 성능 최적화에 큰 도움이 돼서 꼭 익혀두면 좋아요. 다음 글에서는 이걸 이용해서 더 복잡한 예제도 한번 다뤄볼게요!

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

#### 빌드 시 모든 경로 미리 생성하기

Next.js에서 페이지를 미리 정적으로 생성하려면, `generateStaticParams` 함수에 생성할 경로(path) 전체 목록을 넘겨줘야 해요. 이렇게 해두면 빌드 타임에 모든 페이지가 미리 렌더링 되어서 빠른 로딩 속도를 기대할 수 있답니다.

예를 들어, 외부 API에서 게시물 목록을 받아와 각 게시물의 슬러그를 기반으로 페이지를 생성하는 코드는 아래와 같아요:

```js
export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json())
 
  return posts.map((post) => ({
    slug: post.slug,
  }))
}
```

이렇게 하면 `posts` 배열 안에 있는 모든 게시물의 `slug`가 각각 정적으로 생성되어, 빌드 시점에 모든 페이지가 만들어져요.

---

#### 참고: 일부 경로만 빌드 시 생성하기

몇 만 개, 몇 십 만 개의 경로가 있을 때는 모든 경로를 빌드 시 생성하는 게 현실적이지 않을 수 있어요. 이럴 땐 일부 경로만 빌드 타임에 미리 생성하고, 나머지는 런타임에 동적으로 처리하는 방법도 있어요.

예를 들어, 인기 게시물만 미리 생성하고 나머지는 필요할 때 렌더링하거나, fallback 옵션을 사용하는 방법이 있죠.

이 부분은 상황에 따라 유연하게 고민해서 적용하면 되니, 다음에 더 자세히 다뤄볼게요!

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

빌드 시점에 일부 경로만 정적으로 미리 만들어 두고, 나머지 경로는 첫 방문 시에 서버에서 렌더링하고 싶다면, `generateStaticParams` 함수에서 일부 경로만 반환하면 됩니다.

```js
export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json())

  // 첫 10개의 포스트만 빌드 타임에 미리 렌더링
  return posts.slice(0, 10).map((post) => ({
    slug: post.slug,
  }))
}
```

이렇게 하면 상위 10개의 포스트 페이지는 미리 만들어지고, 나머지 페이지는 아직 만들어지지 않은 상태가 됩니다.

그런데 이렇게 되면 아직 정적으로 생성되지 않은 다른 동적 경로에 방문했을 때 어떻게 할지 정해야 하죠. 여기서 `dynamicParams` 옵션을 사용하면 동적 경로가 미리 생성되어 있지 않은 경우의 동작을 제어할 수 있습니다.

```js
// 상위 10개를 제외한 나머지는 404 페이지를 보여줌
export const dynamicParams = false

export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json())
  const topPosts = posts.slice(0, 10)

  return topPosts.map((post) => ({
    slug: post.slug,
  }))
}
```

- `dynamicParams = false`로 설정하면, 미리 생성되지 않은 동적 경로에 접근할 경우 404 에러를 띄워줍니다.
- 반대로 `dynamicParams = true` 혹은 기본값으로 설정하면, 해당 경로를 처음 방문할 때 서버가 동적으로 렌더링해주고 이후에는 캐싱될 수 있습니다.

이 기능 덕분에 빌드 시간을 줄이면서도 원하는 일부 페이지만 미리 렌더링하는 하이브리드 방식을 유연하게 사용할 수 있어요.

---

### 추가 팁!

- 만약 여러분의 데이터가 자주 변하거나 무한히 많다면, 모든 페이지를 빌드 시점에 미리 생성하는 건 비효율적일 수 있어요.
- 이런 경우 `dynamicParams: true`를 활용해서 사용자가 방문할 때마다 필요한 페이지만 서버에서 렌더링 하거나, ISR(Incremental Static Regeneration) 같은 기술과 조합해 보면 좋습니다.
- `generateStaticParams`에서 반환하는 경로의 객체 모양은 라우팅에 따라 달라지니, 꼭 라우트 파라미터 이름과 일치하도록 작성해야 해요.

이렇게 Next.js(혹은 비슷한 프레임워크)에서는 정적 생성과 동적 렌더링을 조합해 효율적인 페이지 전략을 짤 수 있으니 프로젝트 상황에 맞게 잘 활용해보세요!

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

#### 모든 경로를 런타임 시 처리하기

처음 방문할 때 모든 경로를 정적으로 렌더링하려면, `generateStaticParams` 함수에서 빈 배열을 반환하면 됩니다. 이렇게 하면 빌드 시점에는 렌더링할 경로가 없고, 방문하는 순간에 경로가 렌더링되죠. 또는 `export const dynamic = 'force-static'`을 사용해서 강제로 정적 렌더링을 지정할 수도 있어요.

```js
export async function generateStaticParams() {
  return []
}
```

> 알아두면 좋은 점: `generateStaticParams` 함수는 항상 배열을 반환해야 합니다. 비어있는 배열이어도 괜찮아요. 만약 배열이 아닌 다른 값을 반환하면 Next.js가 그 경로를 동적 렌더링하게 됩니다.

추가로, 이 점이 중요한 이유는 빌드 타임에 모든 경로를 미리 처리하지 않고, 필요할 때만 렌더링하도록 하여 빌드 시간을 단축하고 서버 리소스를 효율적으로 사용할 수 있기 때문입니다. 특히 사용자마다 동적으로 생성되는 페이지가 많을 때 유용하죠.

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

```js
export const dynamic = 'force-static'
```

### 지정되지 않은 경로에 대해 렌더링 비활성화하기

런타임에 지정되지 않은 경로가 정적으로 렌더링되는 것을 막으려면, 라우트 세그먼트에 `export const dynamicParams = false` 옵션을 추가하면 됩니다. 이 설정을 사용하면 `generateStaticParams`에서 제공한 경로만 서비스되고, 지정하지 않은 경로는 404 에러가 나거나(캐치올 라우트의 경우 해당 경로와 매칭됨) 처리됩니다.

이 설정은 특히 미리 정해진 경로만 정적으로 생성하고 싶을 때 유용해요. 예를 들어, 블로그 게시글 경로가 미리 모두 정해져 있고, 없는 글에 대한 요청은 404를 보여주고 싶을 때 쓸 수 있죠.

### 여러 개의 동적 세그먼트 다루기

Next.js의 라우팅 시스템은 여러 개의 동적 세그먼트를 한 경로에 넣는 것도 지원해요. 예를 들어, `[category]/[postId]` 같은 식으로요. 이 경우 각 동적 세그먼트에 대응하는 파라미터를 `generateStaticParams` 함수에서 모두 반환해 줘야 정적 생성이 정상적으로 이뤄집니다.

```js
export async function generateStaticParams() {
  return [
    { category: 'tech', postId: 'nextjs-guide' },
    { category: 'life', postId: 'morning-routine' },
  ];
}
```

이렇게 하면 `tech/nextjs-guide`, `life/morning-routine` 두 경로가 미리 정적으로 생성되죠.

동적 세그먼트를 여러 개 쓸 때는 각 세그먼트에 대응하는 파라미터를 적절히 관리하는 게 중요해요. 그리고 `dynamicParams = false`와 조합하면, 정해진 경로 외에는 모두 404 처리되게 설정할 수 있답니다.

---

추가로, Next.js의 동적 라우팅과 정적 생성은 SEO에도 굉장히 유리해서, 미리 생성된 페이지가 빠르게 로드되고 검색엔진 최적화에도 도움을 줘요. 그래서 동적 경로라도 미리 경로를 정의해 정적으로 생성하는 방식을 권장합니다. 다만 모든 경로를 미리 알 수 없거나 너무 많다면, SSR과 ISR의 적절한 사용도 고려해 보세요!

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

동적 세그먼트에 대해 params를 생성할 때, 현재 레이아웃이나 페이지 위쪽에서만 가능하고, 아래쪽에서는 불가능하다는 점 알아두면 유용해요. 예를 들어, `app/products/[category]/[product]` 경로를 생각해보죠:

- `app/products/[category]/[product]/page.js`에서는 `[category]`와 `[product]` 두 동적 세그먼트 모두에 대한 params를 생성할 수 있어요.
- 반면에 `app/products/[category]/layout.js`에서는 오직 `[category]`에 대한 params만 생성 가능하답니다.

여기서 중요한 부분은, 동적 세그먼트가 여러 개인 경로라면 params를 생성하는 방법이 두 가지로 나뉜다는 거예요.

### 동적 세그먼트 params 생성 방법 1: 아래에서 위로 (Bottom Up)

아래쪽(페이지 혹은 더 깊은 레벨)부터 params를 만들어서 상위 레이아웃으로 전달하는 식이죠. 이 방식은 여러 동적 세그먼트가 있을 때 가장 직관적이에요. 예를 들면, `[product]/page.js`에서 `[product]` params를 만들고, 그 위쪽 `[category]/layout.js`에서 `[category]` params를 생성하는 식입니다.

사실 이런 구조는 실무에서도 자주 쓰이는데요, 이유는 개별 페이지에서 가장 상세한 params를 만들고, 레이아웃은 좀 더 상위 레벨에서 필요한 범위의 params만 관리하기 때문이에요.

---

추가로, Next.js 13 앱 디렉토리 구조에서 동적 세그먼트와 params 생성이 왜 이렇게 제한되는지 궁금해하시는 분들도 많아요. 이건 경로 매칭과 렌더링 흐름이 위에서 아래로 진행되기 때문인데요. 상위 레이아웃에서 params를 정하지 않고서 하위 경로에서 마음대로 바꾸면 전체 UI가 예상치 못한 상태가 될 수 있거든요. 그래서 안정적이고 예측 가능한 렌더링을 위해 이런 규칙이 만들어졌다고 보면 돼요.

아참, 만약 여러 동적 세그먼트가 복잡하게 얽힌 상황이라면, 서버 컴포넌트에서 데이터를 패칭하면서 params를 조합하는 방식도 고려해보세요. 데이터를 불러오는 동시에 필요한 여러 params를 만들어내니까 코드도 더 깔끔해지고 관리도 쉬워질 거예요!

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

리액트 라우팅할 때, 특히 Next.js 같은 프레임워크에서 동적 라우트 여러 개를 한꺼번에 생성하는 경우가 많죠? 예를 들어 `[category]` 와 그 하위 경로인 `[product]`를 함께 동적으로 만들 때 어떻게 하면 좋을지 알아볼게요.

```js
// [category]/[product] 경로를 위한 정적 파라미터 생성 함수
export async function generateStaticParams() {
  const products = await fetch('https://.../products').then(res => res.json());

  return products.map(product => ({
    category: product.category.slug,  // 카테고리 슬러그
    product: product.id,               // 제품 아이디
  }));
}

export default function Page({
  params,
}: {
  params: Promise<{ category: string; product: string }>
}) {
  // params.category와 params.product를 이용해 페이지 내용 구성 가능
}
```

### 여기서 중요한 포인트!

- `generateStaticParams`를 통해 `category`, `product` 두 개의 파라미터를 한꺼번에 뽑아서 반환하죠.
- 반환 형식은 배열 안에 객체 형태로 `{ category, product }`를 넣어줘야 함!
- 이렇게 하면 Next.js가 빌드 시점에 모든 동적 경로를 정적으로 생성해줍니다.

---

### 상위 경로부터 하위 경로를 순차적으로 생성하는 법

아래는 **상위 segment(`[category]`)를 먼저 생성하고**, 그 결과를 기반으로 하위 segment(`[product]`)를 생성하는 방법이에요.

```js
// 먼저 카테고리 목록을 가져옵니다.
async function getCategories() {
  const categories = await fetch('https://.../categories').then(res => res.json());
  return categories;
}

// 이후 카테고리 별 제품 목록을 가져옵니다.
async function getProductsByCategory(categorySlug) {
  const products = await fetch(`https://.../categories/${categorySlug}/products`).then(res => res.json());
  return products;
}

export async function generateStaticParams() {
  const categories = await getCategories();
  
  const params = [];
  for (const category of categories) {
    const products = await getProductsByCategory(category.slug);
    for (const product of products) {
      params.push({
        category: category.slug,
        product: product.id,
      });
    }
  }
  return params;
}
```

이렇게 하면 카테고리부터 쭉 받아서, 각 카테고리마다 제품 리스트를 가져와 해당 조합을 모두 만들어 낼 수 있죠!

---

### 보너스 꿀팁 ⭐

- 여러 비동기 호출이 있을 땐 `Promise.all`을 활용하면 병렬처리가 가능해서 더 빠르게 데이터를 받아올 수 있어요.
- `generateStaticParams`가 너무 무거워지면 빌드 시간이 길어질 수 있으니, 필요한 데이터만 꼭 선별해 불러오는 게 중요해요.
- Next.js 13부터 `generateStaticParams`가 점점 더 강력해지고 있는데, 최신 문서 참고하는 것도 추천드립니다!

---

자, 이렇게 동적 라우트 여러개 한꺼번에, 그리고 부모-자식 관계로 연동해서 파라미터 생성하는 방법을 알아봤습니다. 필요에 따라 딱 맞게 조합해서 사용해보세요! 궁금한 점 있으면 언제든 질문해 주세요~

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

여러분, Next.js에서 동적 라우트와 관련해서 `generateStaticParams`를 어떻게 활용하는지 궁금하신 적 있나요? 오늘은 부모-자식 동적 라우팅에서 `generateStaticParams` 함수가 어떻게 작동하는지 쉽게 풀어드릴게요!

---

우선, 부모 라우트가 있다면, 이 부모가 있는 카테고리(`category`)를 기준으로 다음과 같이 `generateStaticParams`를 만들어줍니다.

```js
// [category]에 해당하는 세그먼트 생성 → 예: /category/샌드위치
export async function generateStaticParams() {
  const products = await fetch('https://.../products').then((res) => res.json())
 
  return products.map((product) => ({
    category: product.category.slug,
  }))
}
 
export default function Layout({
  params,
}: {
  params: { category: string }
}) {
  // ...
}
```

이 코드는 외부 API에서 상품 정보를 받아 카테고리별로 경로를 정적으로 생성합니다. 예를 들어, "샌드위치", "음료" 같은 카테고리를 미리 만들어서 각 경로에 대응시켜주는 거죠.

---

이제 자식 라우트 `[product]`가 있다고 해볼게요. 여기서 중요한 점!

부모의 `generateStaticParams` 함수가 만들어낸 각 `category` 세그먼트별로, 자식 라우트의 `generateStaticParams` 함수가 **각각 한 번씩 실행**됩니다. 즉, 부모 카테고리에 맞게 자식 데이터도 동적으로 생성할 수 있어요.

```js
// 부모 카테고리 params를 받아서 그 하위의 상품들로 [product] 세그먼트 생성
export async function generateStaticParams({
  params: { category },
}: {
  params: { category: string }
}) {
  const products = await fetch(
    `https://.../products?category=${category}`
  ).then((res) => res.json())
 
  return products.map((product) => ({
    product: product.id,
  }))
}
 
export default function Page({
  params,
}: {
  params: { category: string; product: string }
}) {
  // ...
}
```

예를 들어, `category`가 "샌드위치"라면, 그 안에 속한 제품 아이디들을 가져와서 `/샌드위치/제품ID` 형태의 경로를 만들어 주는 거죠.

---

### 팁!

1. `params` 타입 지정할 때, Promise가 아니라 바로 `{ category: string }` 형태로 명확히 적어주는 것이 편해요. 타입스크립트에서 헷갈릴 수 있거든요.
2. 부모-자식 관계에서 데이터 흐름을 잘 생각해서 API 요청을 최소화하면 성능에 도움이 됩니다.
3. `generateStaticParams`는 빌드 타임에 한 번만 실행되니, 자주 바뀌는 데이터는 `getStaticPaths`(Next.js 12 이전)나 SSR로 고민해보세요.

---

요약하자면, Next.js의 동적 라우팅에서 부모 세그먼트 `generateStaticParams`가 만들어낸 경로마다 자식 세그먼트의 `generateStaticParams`가 각각 호출되어, 계층적으로 경로를 생성합니다. 덕분에 복잡한 URL 구조를 쉽게 다룰 수 있답니다!

여러분도 이 구조를 잘 활용해서 멋진 다이나믹 웹사이트 만들어 보세요~! 😄

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

> 참고할 점: fetch 요청은 generate-로 시작하는 함수들, Layout, Page, Server Component 전반에 걸쳐 같은 데이터를 요청하면 자동으로 메모이제이션(memoization)이 됩니다. fetch를 사용할 수 없는 경우에는 React cache를 활용할 수 있어요.

## 버전 히스토리

| 버전       | 변경사항                   |
|------------|---------------------------|
| `v13.0.0`  | `generateStaticParams` 도입 | 

이 내용은 Next.js 같은 최신 프레임워크에서 중요한데요, fetch 요청이 중복되지 않고 효율적으로 관리돼서 성능이 꽤 좋아진답니다. 그리고 `generateStaticParams`는 정적 생성 시 파라미터를 미리 지정할 수 있게 해주는 함수로, 미리 페이지를 만들어 빠르게 제공할 수 있게 해줘요. 이렇게 바뀌면서 서버와 클라이언트 간의 데이터 전달이 좀 더 깔끔해졌다고 보면 됩니다!