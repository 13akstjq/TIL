---
title: "Next.js 15에서 메타데이터 추가 및 OG 이미지 생성하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:18
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to add metadata and create OG images"
link: "https://nextjs.org/docs/app/getting-started/metadata-and-og-images"
isUpdated: false
---

# 메타데이터 추가와 OG 이미지 만들기 방법

웹 개발하다 보면 SEO(검색 엔진 최적화)나 SNS 공유 시 내 페이지가 더 멋지게 보이도록 메타데이터와 OG(Open Graph) 이미지를 설정하는 게 정말 중요하죠. Next.js에서는 이런 걸 엄청 쉽게 할 수 있도록 메타데이터용 API들을 제공해요.

예를 들면,

- **정적 메타데이터 객체(static metadata object)**
- **동적으로 메타데이터를 생성하는 함수(generateMetadata)**
- 그리고 정적 혹은 동적으로 생성되는 파비콘(favicon)이나 OG 이미지를 넣기 위한 특별한 파일 규칙

이런 옵션들을 사용해서, Next.js가 알아서 해당 페이지에 필요한 `<head>` 태그를 생성해줘요. 그래서 개발자 도구에서 살펴보면 자동으로 쭉 생성된 걸 확인할 수 있답니다.

---

## 좀 더 자세히 설명해볼게요!

### 1. 정적 메타데이터 객체 (Static Metadata)

간단하게 `metadata`라는 객체를 만들고, 그 안에 title, description, og:image 같은 정보를 딱 고정시켜 넣을 수 있어요.

```js
export const metadata = {
  title: "내 Awesome 페이지",
  description: "여기는 내 멋진 Next.js 앱이에요.",
  openGraph: {
    images: ["/og-image.png"],
  },
};
```

이렇게 해놓으면 해당 정보가 자동으로 `<head>` 안에 들어가서 SEO 효과도 보고, 페이스북이나 트위터에 공유할 때 예쁜 카드로 나와요.

### 2. 동적 메타데이터 생성 (Dynamic generateMetadata)

예를 들어, 블로그 포스트마다 제목이나 설명이 다를 때, 함수 형태로 만들면 요청할 때마다 데이터를 받아서 메타데이터를 바꿔줄 수 있어요.

```js
export async function generateMetadata({ params }) {
  const post = await getPostData(params.id);

  return {
    title: post.title,
    description: post.summary,
    openGraph: {
      images: [post.ogImage],
    },
  };
}
```

이렇게 하면 매번 해당 글에 맞는 메타데이터가 동적으로 들어가니까, SEO랑 SNS 공유가 훨씬 좋아지죠.

### 3. 파비콘과 OG 이미지 자동설정

Next.js에서는 `favicon.ico` 같은 파일을 프로젝트 루트에 두거나, OG 이미지를 특정 경로에 두면 자동으로 인식해줘요.

특히, OG 이미지는 요즘 꼭 넣어야 하는데, 일부러 직접 `<meta property="og:image" ...>` 태그 안 넣어도 Next.js가 알아서 해주는 점이 편리해요.

---

## 함께 알아두면 좋은 팁

- **OG 이미지 사이즈**: 일반적으로 1200x630 픽셀 정도가 적당해요. 너무 작으면 카드가 깔끔하게 안 나오고, 너무 크면 로딩에 부담이 돼요.
- **SEO와 UX도 같이 챙기자**: 메타데이터는 검색 결과뿐만 아니라, 실제 사용자가 공유 링크를 클릭하는 경험에도 영향을 줘요. 좋은 제목과 설명, 이미지가 방문자 증가로 이어질 수 있어요.
- **Next.js 문서 참고**: Next.js 공식 문서에 메타데이터 관련 가이드가 아주 잘 정리되어 있으니, 실습해보면서 익히면 더 도움이 될 거예요.

---

메타데이터와 OG 이미지를 신경 쓰면, 우리 앱이나 웹사이트가 한층 더 프로페셔널하고, 사용자에게 친숙하게 다가갈 수 있으니 꼭 적용해보세요!

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

## 기본 필드 (Default fields)

웹사이트를 만들 때, 라우트(route)에 메타데이터(metadata)를 따로 정의하지 않아도 자동으로 추가되는 기본 메타 태그(meta tag)가 두 가지 있어요. 이 두 가지는 웹 페이지가 제대로 보이고 작동하는 데 꼭 필요한 친구들입니다.

- **`<meta charset="utf-8" />`**
  - 이 태그는 웹페이지에서 사용되는 문자 인코딩을 UTF-8로 설정해줘요.
  - UTF-8은 한글, 영어, 이모지 등 다양한 문자를 모두 지원하기 때문에 대부분의 웹사이트에서 기본으로 사용하고 있답니다.
- **`<meta name="viewport" content="width=device-width, initial-scale=1" />`**
  - 스마트폰이나 태블릿 같은 다양한 디바이스에서 화면 크기와 초기 확대 비율(scale)을 조절해주는 태그예요.
  - 만약 이 태그가 없다면, 모바일 화면에서 회원가입 폼이나 메뉴가 너무 작게 보이거나, 반대로 너무 커져서 사용하기 어려워질 수 있어요.

```html
<meta charset="utf-8" /> <meta name="viewport" content="width=device-width, initial-scale=1" />
```

> 참고로, 다양한 메타 태그들이 있지만 이 두 개는 모든 HTML 문서에 꼭 넣어줘야 하는 기본 중의 기본입니다. 덕분에 사용자들이 어느 디바이스로 접속해도 글자가 깨지지 않고, 화면도 적절하게 보여서 사용자 경험(UX)이 좋아지죠!

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

메타데이터(metadata)를 다룰 때, 정적인 메타데이터(static metadata)와 동적으로 생성되는 메타데이터(generated metadata)를 정의하는 방법이 있어요. 정적인 메타데이터는 정해진 값 그대로 사용하고, 동적인 메타데이터는 컴포넌트 내 함수로 데이터를 생성해서 설정할 수 있죠.

---

## 정적 메타데이터 (Static Metadata)

정적 메타데이터를 만들고 싶다면, `layout.js`나 `page.js` 같은 정적인 파일에서 `Metadata` 객체를 `export` 하면 됩니다. 예를 들어, 블로그 페이지에 제목과 설명을 추가하고 싶을 때 코드는 이렇게 작성할 수 있어요:

```js
import type { Metadata } from "next";

export const metadata: Metadata = {
  title: "My Blog",
  description: "...",
};

export default function Page() {}
```

위 코드를 보면 `metadata`라는 이름으로 메타데이터를 내보내고 있고, 타입도 명확히 `Metadata`로 지정해줬습니다. 이렇게 하면 Next.js가 해당 페이지를 렌더링할 때 이 메타데이터를 자동으로 인식해서 `<head>` 태그에 적절한 메타 정보를 넣어주죠.

---

### 추가 팁!

- `title`은 화면의 탭 제목으로도 노출되니, 페이지별로 명확하고 간결하게 작성하는 게 좋아요.
- `description`은 SEO에 중요한 역할을 하니까, 페이지 내용과 맞는 적절한 설명을 넣어줘야 합니다.
- 메타데이터는 페이지가 최초로 로드될 때만 적용되므로, 자주 바뀌는 값은 동적 메타데이터 방식을 사용하세요.

정리하자면, 페이지마다 변하지 않는 고정된 정보(예: 사이트 이름, 정적 설명)를 넣을 땐 `metadata` 객체를 이용하는 것이 가장 손쉽고 직관적입니다. 다음에는 동적으로 메타데이터를 생성하는 방법도 알려드릴게요!

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

generateMetadata 함수로 동적인 메타데이터를 쉽게 가져올 수 있어요. 예를 들어 특정 블로그 포스트의 제목과 설명을 API에서 받아오고 싶다면 이렇게 작성하면 됩니다:

```js
import type { Metadata, ResolvingMetadata } from 'next'

type Props = {
  params: Promise<{ id: string }>
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>
}

export async function generateMetadata(
  { params, searchParams }: Props,
  parent: ResolvingMetadata
): Promise<Metadata> {
  const slug = (await params).slug

  // API에서 포스트 정보 가져오기
  const post = await fetch(`https://api.vercel.app/blog/${slug}`).then(res =>
    res.json()
  )

  return {
    title: post.title,
    description: post.description,
  }
}

export default function Page({ params, searchParams }: Props) {}
```

### 여기서 조금 더 알려드리자면

- `generateMetadata`는 Next.js에서 페이지에 필요한 메타 정보를 서버사이드에서 미리 가져와서 SEO에 유리하게 사용할 수 있게 해줘요.
- `params`와 `searchParams`는 라우팅 정보와 쿼리 스트링 정보를 Promise 형태로 받아오는데, 이걸 `await` 해서 실제 값을 꺼내 써야 합니다.
- 메타데이터엔 `title`, `description` 외에도 `openGraph`, `twitter` 같은 소셜 미디어용 메타 정보도 포함할 수 있어요.
- 만약 API가 느리거나, 자주 바뀌지 않는 경우엔 캐싱 전략도 고민해보세요. 서버 부담도 줄이면서 사용자 경험을 더 좋게 만들 수 있답니다.

이렇게 `generateMetadata`를 잘 활용하면 각 페이지마다 동적인 메타데이터를 쉽게 관리할 수 있어서 SEO 최적화에 큰 도움이 돼요!

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

Next.js 내부에서는 메타데이터를 UI와 별도로 스트리밍하고, 메타데이터가 준비되는 대로 HTML에 바로 주입하는 방식으로 동작해요. 덕분에 사용자에게 더 빠르게 정보를 보여줄 수 있답니다.

### 데이터 요청 memoization (메모이제이션)

메타데이터를 렌더링할 때와 페이지를 렌더링할 때 동일한 데이터를 여러 번 요청해야 하는 상황이 생길 수 있어요. 이럴 때 매번 중복해서 데이터를 요청하면 비효율적이죠. 그래서 React의 `cache` 함수를 활용해 데이터를 한번만 가져오고 결과를 메모이제이션해서 재사용할 수 있어요.

예를 들어, 블로그 포스트 정보를 메타데이터 생성과 페이지 렌더링에 모두 사용해야 할 때 이렇게 작성할 수 있습니다:

```js
import { cache } from "react";
import { db } from "@/app/lib/db";

// getPost 함수는 두 번 호출되지만, 실제로는 한 번만 실행돼요
const getPost = cache(async (slug) => {
  const res = await db.query.posts.findFirst({ where: eq(posts.slug, slug) });
  return res;
});
```

이렇게 하면 같은 `slug`에 대해 여러 번 호출해도 데이터베이스 쿼리는 한 번만 실행되니까 성능이 쑥~ 좋아지죠.

---

추가로, Next.js 13 이상부터 데이터 패칭에서 이런 memoization을 자주 활용할 텐데, 이 기능 덕분에 서버 측 렌더링 시 효율적으로 데이터를 관리할 수 있어요. 만약 `getPost`를 여러 컴포넌트에서 공유하고 싶다면 별도의 유틸 파일에 묶어두고 재사용하는 것도 좋은 패턴입니다.

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

자, 이번에는 Next.js에서 동적으로 메타데이터를 생성하는 방법과 함께 파비콘(Favicon)을 추가하는 기본적인 방법에 대해 살펴보려고 해요.

---

## 1. 동적 메타데이터 생성하기

```tsx
import { getPost } from "@/app/lib/data";

export async function generateMetadata({ params }: { params: { slug: string } }) {
  // URL 파라미터(slug)를 이용해서 해당 게시글 데이터를 가져와요
  const post = await getPost(params.slug);
  return {
    title: post.title, // 동적으로 페이지 타이틀 설정
    description: post.description, // 동적으로 메타 설명 설정
  };
}

export default async function Page({ params }: { params: { slug: string } }) {
  const post = await getPost(params.slug);
  return <div>{post.title}</div>;
}
```

위 코드는 Next.js 13의 새로운 라우팅 방식에서 메타 태그를 동적으로 생성하는 대표적인 예시예요. `params.slug`를 통해 URL에 맞는 글 내용을 서버 사이드에서 불러온 다음, 해당 글 제목과 설명을 메타 태그에 넣어 주는 방식이죠.

> 🔥 참고: `generateMetadata` 함수는 페이지 렌더링 전에 호출되므로, SEO 최적화에 아주 유리합니다.

---

## 2. 파비콘(Favicon) 추가하기

파비콘은 웹 브라우저 탭이나 북마크에 표시되는 작은 아이콘이에요. 사이트의 아이덴티티를 표현하는 데 아주 중요한 역할을 하죠. 간단하게 적용하는 방법을 알려드릴게요.

### 방법

- `favicon.ico` 파일을 프로젝트의 **루트 폴더 (`app` 폴더)**에 넣어주세요.
- Next.js는 기본적으로 루트에 있는 `favicon.ico`를 자동으로 인식해서 `<head>`에 추가해 줍니다.

만약 `favicon.ico` 대신 PNG 등 다른 형식을 쓴다면, `app/head.tsx` 혹은 `app/layout.tsx` 내에서 직접 설정해야 해요.

```tsx
// app/head.tsx 예시
export default function Head() {
  return (
    <>
      <link rel="icon" href="/favicon.ico" />
      {/* 다른 메타 태그들도 여기에 추가 가능 */}
    </>
  );
}
```

---

## 추가 팁!

- 파비콘을 만드는 데 어려움을 겪는다면 [favicon.io](https://favicon.io/) 같은 온라인 툴을 써서 쉽게 만들 수 있어요.
- 요즘에는 다양한 해상도(16x16, 32x32, 48x48, 64x64 등)로 미리 준비하는 것이 좋고, 모바일 앱 아이콘이나 Apple 터치 아이콘용으로 별도 이미지를 준비하는 경우도 많답니다.

---

## 요약

| 내용                 | 설명                                                                   |
| -------------------- | ---------------------------------------------------------------------- |
| 동적 메타데이터 생성 | `generateMetadata` 함수로 SEO 최적화와 동적 페이지 제목/설명 설정 가능 |
| 파비콘 추가          | `favicon.ico`를 `app` 폴더에 추가하거나 `<head>`에서 직접 링크를 명시  |

---

![파비콘 설정 이미지](/TIL/img/2025-04-22-HowtoaddmetadataandcreateOGimages_0.png)

파비콘과 메타데이터를 제대로 설정하면 SEO뿐 아니라 사용자 경험도 훨씬 좋아지니 꼭 챙겨보시길 바랄게요!  
필요하면 댓글로 질문 남겨 주세요! 😊

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

> 프로그래밍으로도 파비콘(favicon)을 자동 생성할 수 있어요. 더 자세한 내용은 파비콘 관련 문서를 참고해 보세요.

## 정적 Open Graph 이미지 만들기

Open Graph(OG) 이미지는 SNS에서 내 사이트를 대표하는 이미지예요. 내 앱에 정적인 OG 이미지를 추가하려면, 앱 폴더(root 디렉토리)에 `opengraph-image.png` 파일을 넣으면 돼요.

예를 들어 이렇게 생긴 이미지가 해당 파일일 수 있겠죠:

| ![Open Graph 이미지 예시](https://your-site.com/assets/img/2025-04-22-HowtoaddmetadataandcreateOGimages_1.png) |
| :------------------------------------------------------------------------------------------------------------: |
|                                            _Open Graph 이미지 예시_                                            |

이렇게 OG 이미지를 설정해 두면, 페이스북이나 트위터 같은 SNS에 내 웹사이트 링크를 공유할 때 예쁜 썸네일로 보여줘서 사용자 경험을 훨씬 더 좋게 만들 수 있답니다.

추가로, OG 이미지는 보통 1200x630 픽셀 정도 크기를 권장해요. 그래야 각 SNS에서 최적화되어 잘 보이거든요. 또한, 만약 동적인 OG 이미지를 만들고 싶다면, 코드로 이미지를 생성하는 방법도 있으니, 나중에 활용해 보시면 좋겠네요!

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

특정 경로에 대해 OG 이미지를 추가하고 싶다면, 해당 경로 폴더 안에 opengraph-image.png 파일을 만들어주면 돼요. 예를 들어, /blog 경로에 맞는 OG 이미지를 추가하고 싶다면 blog 폴더 안에 opengraph-image.jpg 파일을 넣으면 됩니다.

<img src="/TIL/assets/img/2025-04-22-HowtoaddmetadataandcreateOGimages_2.png" />

폴더 구조에서 더 깊숙한 경로에 있는 이미지가 우선해서 적용된다는 점, 기억하세요!

> 참고로 jpeg, png, webp 같은 다른 이미지 포맷도 지원되니까 상황에 맞게 사용하면 됩니다. 더 자세한 내용은 Open Graph Image 문서를 참고해 보세요.

추가로 팁을 드리자면, 여러 경로에 공통으로 쓰이는 기본 OG 이미지를 루트 폴더에 넣고, 그 이미지를 기본값처럼 설정해놓으면 관리가 훨씬 수월해요. 필요한 특정 페이지에는 별도의 이미지를 넣어서 덮어쓰면 되고요. 이렇게 하면 SEO 최적화와 SNS 공유 시 더 세련된 미리보기 이미지를 보여줄 수 있답니다!

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

## 동적으로 생성하는 오픈 그래프(Open Graph) 이미지 만들기

Open Graph 이미지는 페이스북, 트위터 같은 소셜 미디어에 링크를 공유할 때 보이는 미리보기 이미지라고 생각하면 편해요. 이 이미지를 글마다 다르게 자동 생성하면 훨씬 프로페셔널해 보이죠? Next.js의 `ImageResponse` 생성자를 사용하면 JSX와 CSS로 동적인 이미지를 쉽게 만들 수 있어요.

### 사용법 예시

아래 코드는 블로그 포스트마다 고유한 OG 이미지를 생성하는 방법입니다. 블로그 폴더 내에 `opengraph-image.ts` 파일을 만들고, `next/og`에서 `ImageResponse`를 불러와 사용해요.

```js
import { ImageResponse } from "next/og";
import { getPost } from "@/app/lib/data";

// 이미지 메타 정보
export const size = {
  width: 1200,
  height: 630,
};

export const contentType = "image/png";

// 이미지 생성 함수
export default async function Image({ params }: { params: { slug: string } }) {
  const post = await getPost(params.slug);

  return new ImageResponse(
    (
      // ImageResponse에서 그릴 JSX 요소
      <div
        style={{
          fontSize: 128,
          background: "white",
          width: "100%",
          height: "100%",
          display: "flex",
          alignItems: "center",
          justifyContent: "center",
        }}
      >
        {post.title}
      </div>
    )
  );
}
```

### 코드 설명

| 항목          | 설명                                                                        |
| ------------- | --------------------------------------------------------------------------- |
| `size`        | 생성할 이미지 크기 설정 (픽셀 단위)                                         |
| `contentType` | 이미지 포맷 (ex. 'image/png')                                               |
| `Image` 함수  | 요청 파라미터로 슬러그를 받아 포스트 데이터 조회 후, 제목을 이미지로 렌더링 |

### 더 알아두면 좋은 팁들

- **CSS 스타일링 자유자재**  
  위 예제에선 아주 간단하게 이미지를 그렸지만, `ImageResponse`로는 거의 대부분의 CSS 스타일을 적용할 수 있어요. 배경 색상, 폰트, 레이아웃, 심지어 그림자효과나 그라디언트도 가능하죠.

- **더 정교한 디자인을 원한다면?**  
  외부 폰트를 사용하거나 SVG 아이콘, 이미지 등을 조합하고 싶다면, `next/og`에서 지원하는 한도 내에서 최대한 활용해보세요. 단, 웹 폰트 로딩 시에는 약간의 설정이 필요할 수 있습니다.

- **이미지 캐싱 고려하기**  
  동적으로 이미지가 생성되다 보니, 잘못 설정하면 성능이나 비용 측면에서 문제가 될 수 있어요. Next.js의 Incremental Static Regeneration이나 Cache-Control 헤더 등을 적절히 활용해 캐싱 전략을 짜는 걸 추천합니다.

이렇게 동적으로 생성하는 Open Graph 이미지로 SNS에 공유될 때 더 눈에 띄는 내 컨텐츠를 만들어 보세요!

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

ImageResponse는 flexbox, 절대 위치 지정(absolute positioning), 커스텀 폰트, 텍스트 줄바꿈, 텍스트 중앙 정렬, 중첩 이미지 같은 일반적인 CSS 속성을 지원해요. 어떤 CSS 속성들이 지원되는지 전체 목록도 확인할 수 있으니 필요할 때 참고하면 좋습니다.

> 참고할 점:
> Vercel OG Playground에서 다양한 예제를 직접 확인해볼 수 있어요.

내부적으로 ImageResponse는 @vercel/og, Satori, 그리고 Resvg 라이브러리를 사용해서 HTML과 CSS를 PNG 이미지로 변환합니다. 다만, flexbox와 일부 CSS 속성만 지원하기 때문에 display: grid 같은 고급 레이아웃은 작동하지 않는다는 점 기억하세요.

쉽게 말해, 복잡한 레이아웃보다는 깔끔하게 정리된 flexbox 기반 레이아웃에 더 적합하다고 보면 됩니다. 만약 이미지 생성에 관심 있다면 Vercel OG Playground에서 실습해보며 감을 잡는 걸 추천해요!
