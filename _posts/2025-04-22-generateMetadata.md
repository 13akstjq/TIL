---
title: "Next.js 15에서 generateMetadata 함수로 SEO 메타데이터 자동 생성하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 03:05
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "generateMetadata"
link: "https://nextjs.org/docs/app/api-reference/functions/generate-metadata"
isUpdated: false
---


# generateMetadata

이번 글에서는 Next.js에서 메타데이터를 설정하는 방법 중 Config 기반 메타데이터 옵션인 `generateMetadata`와 정적인 `metadata` 객체에 대해 알아볼게요.

```js
import type { Metadata } from 'next'

// 정적 메타데이터 예시
export const metadata: Metadata = {
  title: '홈페이지 제목',
}

// 동적 메타데이터 예시
export async function generateMetadata({ params }) {
  return {
    title: '동적 페이지 제목',
  }
}
```

> 알아두면 좋은 점:
> - `metadata` 객체와 `generateMetadata` 함수는 **서버 컴포넌트**에서만 지원돼요.
> - 한 라우트 세그먼트에서 `metadata` 객체와 `generateMetadata` 함수를 동시에 내보낼 수는 없습니다.

---

### 좀 더 자세히 설명해볼게요!

Next.js 13부터는 페이지별 메타데이터를 쉽게 설정하는 방법으로 `metadata` 객체 또는 `generateMetadata` 함수를 제공해요.

- **`metadata` 객체**: 빌드 시점에 정적으로 설정되는 메타데이터로, 변하지 않는 값을 넣을 때 사용해요.
- **`generateMetadata` 함수**: 요청에 따라 동적으로 값을 만들거나, API 데이터 등을 기반으로 메타데이터를 생성해야 할 때 사용해요.

예를 들면 블로그 게시글 같은 경우, 글 ID를 URL 파라미터로 받아서 `generateMetadata`에서 제목과 설명을 동적으로 세팅할 수 있죠.

```js
export async function generateMetadata({ params }) {
  const post = await getPost(params.id);
  return {
    title: post.title,
    description: post.summary,
  }
}
```

---

### 참고사항

- 이 기능들은 **서버 컴포넌트(Server Components)**에서만 동작해요. 클라이언트 컴포넌트에서는 지원되지 않으니 주의하세요.
- 한 경로에서 `metadata`와 `generateMetadata`를 동시에 export하면 충돌이 나므로, 하나만 골라서 사용해야 합니다.

메타데이터는 SEO에 매우 중요한 역할을 하니, 빌드시점에 고정 값으로 넣을지, 동적으로 데이터를 가져와서 세팅할지 상황에 맞게 잘 선택해 사용하면 좋겠죠?

다음 글에서는 실제 프로젝트에서 어떻게 활용하는지 더 구체적인 예제도 소개해볼게요!

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

## 메타데이터 객체

Next.js에서 정적인 메타데이터를 정의할 때는 `layout.js`나 `page.js` 파일에서 `Metadata` 객체를 export 하면 됩니다.

```js
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: '내 멋진 페이지 제목',
  description: '페이지에 대한 간단한 설명을 적어주세요',
}

export default function Page() {}
```

이렇게 하면 해당 페이지에 맞는 SEO-friendly한 메타데이터가 자동으로 적용돼서, 검색 엔진 최적화(SEO)에 도움이 되고, 소셜 미디어에 공유될 때도 깔끔하게 노출돼요.

### 참고로
`.metadata` 객체에는 더 많은 필드들이 있어요. 예를 들어 `keywords`, `author`, `openGraph`, `twitter` 같은 다양한 메타데이터 옵션을 넣을 수 있는데요, 이것들은 페이지를 더 풍부하게 만들어주고 SNS 공유 시 미리보기 정보를 더 세밀하게 제어할 수 있어요.

아래는 자주 사용하는 메타데이터 필드를 간단히 정리한 표예요:

| 필드명        | 설명                                  |
| ------------- | ------------------------------------- |
| `title`       | 페이지 제목                           |
| `description` | 페이지 설명                           |
| `keywords`    | SEO를 위한 키워드 배열                |
| `author`      | 페이지 제작자 정보                    |
| `openGraph`   | 오픈 그래프 프로토콜에 따른 메타데이터 |
| `twitter`     | 트위터 카드용 메타데이터              |

더 자세한 내용은 공식 문서의 [Metadata Fields](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#fields) 항목을 참고하면 좋아요.

---

정리하자면, Next.js에서 페이지별 메타데이터를 간단하게 관리하려면 `metadata` 객체를 export하는 것만으로 충분합니다. 이렇게 하면 최신 SEO 기술도 쉽게 적용할 수 있어서, 별도의 헤드 태그를 직접 조작하는 번거로움을 줄일 수 있어요!

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

## generateMetadata 함수란?

Next.js에서 동적인 메타데이터를 설정할 때, 현재 라우트의 파라미터나 외부 데이터, 부모 컴포넌트의 메타데이터 정보를 기반으로 설정할 수 있어요. 이를 위해 `generateMetadata`라는 함수를 export해서 메타데이터 객체를 반환하면 됩니다.

예를 들어, 상품 페이지에서 URL 파라미터로 받은 상품 ID를 활용해 서버에서 상품 데이터를 가져와서 타이틀이나 Open Graph 이미지 정보를 동적으로 세팅할 수 있죠.

```js
import type { Metadata, ResolvingMetadata } from 'next'

// Props 타입 정의 - params와 검색 파라미터가 Promise로 들어오는 걸 확인하세요
type Props = {
  params: Promise<{ id: string }>
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>
}

export async function generateMetadata(
  { params, searchParams }: Props,
  parent: ResolvingMetadata
): Promise<Metadata> {
  // 라우트 파라미터 읽기
  const { id } = await params

  // 외부 API에서 상품 정보 가져오기
  const product = await fetch(`https://.../${id}`).then((res) => res.json())

  // 부모 메타데이터의 Open Graph 이미지 배열 불러오기 (없으면 빈 배열)
  const previousImages = (await parent).openGraph?.images || []

  // 동적으로 메타데이터 설정 (기존 이미지는 유지하면서 새 이미지 추가)
  return {
    title: product.title,
    openGraph: {
      images: ['/some-specific-page-image.jpg', ...previousImages],
    },
  }
}

// Page 컴포넌트 예시 - params, searchParams 받아올 수 있음
export default function Page({ params, searchParams }: Props) {}
```

### 주요 파라미터 설명

| 파라미터 | 설명 |
| -------- | ---- |
| `params` | URL 라우트 파라미터(예: `/product/[id]`에서 `id`). Promise로 감싸져서 옵니다. |
| `searchParams` | URL 쿼리스트링 파라미터 (`?category=shoes` 등). Promise 타입입니다. |
| `parent` | 부모 레이아웃 혹은 상위 컴포넌트에 설정된 메타데이터를 받아올 수 있어요. 기존 메타데이터를 활용해 확장할 때 유용합니다. |

---

> **꿀팁!**  
> `generateMetadata` 함수 내부에서 필요할 때 외부 API를 호출해 정보를 받아올 수 있기 때문에, 페이지마다 SEO 최적화된 메타 태그를 동적으로 설정할 수 있어요.  
>  
> 다만, 이 함수는 서버에서만 실행되므로, 클라이언트 전용 상태나 이벤트는 사용할 수 없고, 비동기 작업도 서버에서 처리됩니다.  
>  
> 그리고 `params`와 `searchParams`가 Promise 형태라는 점도 헷갈리지 말아요! `await`를 꼭 붙여서 값을 받아야 합니다.

---

이제 `generateMetadata`를 잘 활용해서, 검색 엔진 최적화와 SNS 공유에 딱 맞는 메타데이터를 페이지 단위로 동적 생성할 수 있겠죠? 계산된 데이터를 기반으로 메타데이터를 만드는 저만의 SEO 전략을 코딩에 적용해 보세요!

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

generateMetadata 함수에 대해 알아볼게요! 이 함수는 Next.js 같은 프레임워크에서 페이지별로 메타데이터를 동적으로 생성할 때 많이 사용됩니다. 조금 더 쉽게 설명하고, 함수가 받는 파라미터와 반환값에 대해 정리해볼게요.

---

## generateMetadata 함수 파라미터 설명

이 함수는 다음과 같은 파라미터를 받습니다. 보통 하나의 `props` 객체로 전달되는데, 그 안에 여러 속성이 들어 있어요.

| 파라미터명    | 설명                                                                                                              | 예시                                                                                 |
|---------------|-------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|
| `params`      | 현재 라우트에서 동적 세그먼트(즉, URL 경로에서 동적으로 변하는 부분)들의 값을 모아놓은 객체.                       | `/shop/1` → `{ slug: '1' }` <br> `/shop/1/2` → `{ tag: '1', item: '2' }` <br> `/shop/1/2` (catch-all) → `{ slug: ['1', '2'] }` |
| `searchParams`| URL 쿼리스트링(검색 파라미터)의 값들을 담은 객체입니다. 여러 개일 경우 배열로 들어올 수도 있어요.                  | `/shop?a=1` → `{ a: '1' }` <br> `/shop?a=1&b=2` → `{ a: '1', b: '2' }` <br> `/shop?a=1&a=2` → `{ a: ['1', '2'] }`              |
| `parent`      | 상위 라우트(segment)에서 이미 계산된 메타데이터를 Promise 형태로 받습니다. 상위의 메타데이터를 기반으로 추가 가공할 때 유용해요. | —                                                                                    |

---

## 참고 및 중요 포인트

- `params` 객체는 URL 경로의 동적 부분을 잡아주기 때문에, 예를 들어 `[slug]` 같은 동적 경로 세그먼트에 해당하는 값이 들어옵니다.
- `[...slug]` 같은 catch-all 세그먼트의 경우 배열로 넘어옵니다.
- `searchParams`는 쿼리스트링에서 하나뿐 아니라 여러 값도 받을 수 있어서 배열로 표현될 수도 있어요.
- `parent`는 재귀적 메타데이터 생성이 필요한 경우에 상위 메타데이터를 가져오는데, `await`을 사용해서 비동기 처리합니다.
- 반환값은 `Metadata` 객체로, 보통 `title`, `description`, `openGraph` 같은 SEO 관련 메타데이터를 포함시킵니다.

---

## `generateMetadata` 반환값 예시

다음은 `generateMetadata`가 반환할 수 있는 간단한 메타데이터 객체의 예시입니다.

```ts
{
  title: 'Shop - ' + params.slug,
  description: `This is the shop page for item ${params.slug}`,
  openGraph: {
    title: 'Shop OG Title',
    description: 'Open graph description',
    url: `https://example.com/shop/${params.slug}`,
  },
}
```

---

이 함수로 동적 라우팅에 맞춰 SEO 최적화나 페이지별 메타데이터 세팅이 가능해서 Next.js의 중요한 기능 중 하나예요! 만약 상위 메타데이터와 합칠 필요가 있다면 `parent`를 `await`해서 가져와서 병합해주면 되고요.

궁금한 점 있으면 언제든 질문해 주세요!

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

> 알아두면 좋은 점:
- 메타데이터가 런타임 정보에 의존하지 않는다면, generateMetadata 대신 static metadata 객체를 사용하는 게 좋아요.
- fetch 요청은 generateMetadata, generateStaticParams, Layout, Page, Server Component 등에서 같은 데이터를 요청할 땐 자동으로 메모이제이션(캐싱)됩니다. 만약 fetch를 쓸 수 없는 상황이라면 React cache를 활용할 수 있어요.
- searchParams는 오직 page.js 세그먼트에서만 사용 가능합니다.
- Next.js의 redirect()와 notFound() 메서드는 generateMetadata 안에서도 사용할 수 있어요.

## 메타데이터 필드

### title

title 속성은 문서의 제목을 설정하는 데 사용됩니다. 단순 문자열로 지정할 수도 있고, 선택적으로 템플릿 객체 형태로 정의할 수도 있어요.

---

여기서 조금 더 팁을 드리자면, Next.js에서 페이지 타이틀을 동적으로 관리할 때 템플릿 객체를 활용하면 일관된 제목 포맷을 유지하는 데 아주 유용합니다. 예를 들어, 모든 페이지 제목 뒤에 ‘ | 내사이트명’을 붙이고 싶다면 템플릿을 써보세요. 각 페이지에서 제목만 넘겨주면 되니까 관리가 훨씬 편해진답니다!

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

#### 문자열 (String)

Next.js나 React를 사용할 때, 주로 문자열을 다루는 방법에 대해 간단히 정리해볼게요!

예를 들어, 아래처럼 `metadata`라는 객체 안에 `title`이라는 문자열을 설정하는 코드가 있어요.

```js
export const metadata = {
  title: 'Next.js',
}
```

이 코드는 주로 페이지의 메타데이터를 설정할 때 씁니다. 예를 들면, 브라우저 탭에 표시되는 제목 같은 내용을 여기서 정의하죠.

그리고 HTML에서는 이렇게 `<title>` 태그 안에 직접 문자열을 넣어서 페이지 제목을 설정합니다.

```js
<title>Next.js</title>
```

이 두 가지 방식이 결국 같은 목적, 즉 ‘페이지 제목’을 설정하는 거라 이해하면 쉬워요.

---

### 조금 더 알아볼까요?

- **`metadata` 객체**: Next.js에서는 페이지별 메타데이터를 객체 형태로 내보내는 게 일반적이에요. 이렇게 하면 SEO 최적화나 소셜 미디어 공유 시에 필요한 정보를 손쉽게 관리할 수 있어요.
- **템플릿 객체 (Template object)**: 보통 ‘템플릿 객체’란 UI 요소나 설정값 등을 미리 정해놓은 객체 형태로 관리하는 것을 말해요. 예를 들면, 페이지마다 `metadata`를 객체로 만들어서 사용하는 게 좋은 예죠.

---

### 간단 정리

| 개념               | 설명                                                    |
|------------------|-------------------------------------------------------|
| 문자열 (String)     | 코드나 HTML 안에서 텍스트 데이터를 나타내는 기본 단위                    |
| `metadata` 객체     | 페이지 정보(제목, 설명 등)를 담는 JSON 형식의 설정 객체                       |
| `<title>` 태그       | HTML 문서의 제목을 브라우저 탭에 표시해주는 태그                         |
| 템플릿 객체 (Template object) | 미리 정의된 설정이나 데이터를 객체 형태로 만들어서 재사용하는 패턴        |

---

그러니까, 개발할 때는 문자열을 단순히 “글자”로 생각하지 말고, 객체나 태그와 같이 어디에 어떻게 쓸지, 또 왜 필요한지 항상 생각하면서 다루면 훨씬 효율적인 코드를 만들 수 있어요! 필요하면 문자열 조작 함수나 템플릿 리터럴로 동적 텍스트를 만들 수도 있고요.

궁금한 점 있으면 언제든 질문해주세요!

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

Next.js에서 `metadata` 객체를 이용해 페이지의 메타 정보를 설정할 때, `title` 속성은 여러 가지 형태로 설정할 수 있어요. 그중에서 `title.default`는 자식 라우트 세그먼트가 별도로 제목(title)을 정의하지 않았을 때 사용할 ‘기본 제목(fallback title)’ 역할을 해요.

예를 들어, 이런 코드가 있다고 가정해볼게요:

```js
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: {
    default: 'Acme',
  },
}
```

위와 같이 `default` 값을 지정하면, 자식 페이지에서 `title`을 따로 정의하지 않아도 `<title>Acme</title>`로 기본 타이틀이 세팅됩니다.

---

### 정리해볼게요!

| 옵션       | 설명                                                      | 예시                         |
|------------|-----------------------------------------------------------|------------------------------|
| `title.default` | 자식 라우트에서 제목을 지정하지 않을 때 보여지는 기본 타이틀 | `'Acme'`                     |
| `title.template`| 페이지 제목을 포맷팅할 때 쓰는 템플릿                      | `'%s | My Site'`              |
| `title.absolute`| 절대 제목으로 설정, 템플릿과 관계없이 고정된 제목 사용       | `'Welcome to Acme'`          |

예를 들어, `title.template`을 활용하면 동적인 제목을 예쁘게 꾸밀 수 있죠.

```js
export const metadata: Metadata = {
  title: {
    template: '%s | Acme',
    default: 'Acme',
  },
};
```

- `/about` 페이지에서 `title`을 `About Us`로 지정하면, `<title>About Us | Acme</title>`가 되고,
- `/contact` 같은 다른 자식 라우트가 타이틀을 따로 지정하지 않으면 `<title>Acme</title>`가 됩니다.

### 좀 더 팁!

1. `title.absolute`는 특정 페이지에서 무조건 고정된 타이틀을 써야할 때 쓸 수 있어요. 템플릿 무시하고 딱 하나의 제목만 보여줘야 할 때 유용합니다.
2. `title.default`는 여러 자식 라우트에 일괄적인 기본 타이틀을 넣고 싶을 때 딱이죠.
3. SEO 관점에서 각 페이지에 맞는 고유한 타이틀을 갖는 것이 좋지만, 없을 때 기본 타이틀을 두는 것도 좋은 습관입니다.

이렇게 `metadata`의 `title` 옵션을 잘 활용하면, 중복되는 코드 없이 깔끔하게 제목을 관리할 수 있어서 프로젝트가 더 효율적으로 운영돼요. 다음 번 Next.js 프로젝트에 꼭 적용해보세요!

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

Next.js에서 title.template을 활용하면 자식 라우트 세그먼트에 정의된 타이틀에 접두사나 접미사를 쉽게 붙일 수 있어요. 예를 들어, 자식 페이지의 타이틀에 ' | Acme'라는 접미사를 자동으로 붙이는 방식이죠.

```js
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: {
    template: '%s | Acme',
    default: 'Acme', // template을 쓸 때는 default 타이틀이 꼭 필요해요.
  },
}
```

그리고 자식 라우트에서는 이렇게 타이틀을 선언하면 아래처럼 렌더링 됩니다:

```js
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'About',
}

// 결과 : <title>About | Acme</title>
```

### 이 점 꼭 기억하세요!
- `title.template`은 자기 자신이 속한 라우트 세그먼트가 아니라 *자식* 라우트 세그먼트에만 적용돼요.
- 그래서 template을 사용할 때는 `title.default`를 반드시 설정해야 합니다.
- `layout.js` 안에서 `title.template`을 정의해도, 같은 라우트 세그먼트에 있는 `page.js`의 타이틀에는 적용되지 않아요.
- `page.js`에서 `title.template`을 정의해봤자 무의미해요, 페이지는 항상 마지막 세그먼트이기 때문이죠. 즉, 자식 라우트가 없으니 template이 적용될 곳이 없다는 이야기입니다.
- 타이틀이나 기본 타이틀(`title` 또는 `title.default`)을 아예 정의하지 않은 라우트에는 template이 먹히지 않아요.

---

### 추가 팁!
`title.template`을 잘 활용하면 사이트 전체의 타이틀 포맷을 통일하기 쉽고, SEO 관리에도 유리합니다. 예를 들어, 블로그 포스트마다 글 제목 뒤에 ' | 내 블로그명'을 붙이고 싶을 때 유용하죠. 게다가 유지보수하기도 정말 편해져요. 타이틀 형식을 하나만 바꾸면 모든 자식 페이지에 바로 반영되니까요.

혹시 라우트 구조가 복잡해서 타이틀 설정이 어려운 경우, `layout.js`와 `page.js`의 역할을 다시 한 번 점검해보면서 어느 위치에 타이틀 관련 코드를 넣어야 할지 고민해보는 걸 추천드려요!

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

`title.absolute`는 부모 세그먼트에 설정된 `title.template`을 무시하고, 절대적인 제목을 지정할 때 사용할 수 있어요. 쉽게 말해, 부모에서 정의한 제목 형식을 적용하지 않고 내가 원하는 제목을 딱 하나만 보여줄 수 있다는 뜻이죠.

예를 들어, 아래처럼 `metadata`에 `template`을 설정해 두면…

```js
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: {
    template: '%s | Acme',
  },
}
```

아래처럼 `absolute`를 쓰면, 부모가 세팅한 `%s | Acme` 형식을 무시하고 그냥 'About'이라는 제목만 보여줍니다.

```js
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: {
    absolute: 'About',
  },
}

// 출력 결과: <title>About</title>
```

---

### 알아두면 좋은 점 정리

| 위치       | 속성              | 설명                                                                                                                                                         |
|------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **layout.js** | `title` (string), `title.default` | 자식 세그먼트 중 제목을 따로 정하지 않은 곳에 기본 제목을 제공합니다. 부모의 `title.template`이 있으면 그 형식을 덧붙여서 보여줘요.                              |
|            | `title.absolute`  | 자식 세그먼트에 기본 제목을 설정하지만, 부모의 `title.template`을 무시합니다. 즉, 깔끔하게 절대 제목만 보여줘요.                                                  |
|            | `title.template`  | 자식 세그먼트용 새로운 제목 템플릿을 정의합니다.                                                                                                               |
| **page.js**  | `title` (string)  | 페이지(라우트)의 제목을 정의합니다. 부모의 `title.template`이 있으면 같이 적용돼요.                                                                           |
|            | `title.absolute`  | 페이지 제목을 절대적으로 지정하며, 부모의 `title.template`을 무시합니다.                                                                                       |
|            | `title.template`  | 적용되지 않습니다. 페이지는 항상 라우트의 마지막 세그먼트여서 템플릿을 사용할 수 없어요.                                                                        |

---

### 좀 더 살펴보기!

- `title.template`은 `%s` 자리에 해당 제목이 들어가도록 포맷을 만들어 줄 때 씁니다. 예를 들어, `'%s | 내 사이트'` 같은 형태죠. 그래서 자식 세그먼트에서 제목만 정의하면 자동으로 `| 내 사이트`가 붙어요.
- 반면 `title.absolute`는 그냥 깨끗하게 제목을 바꿔야 할 때 편리합니다. 예를 들어, "특별한 이벤트 페이지" 같이 이름만 딱 보여줘야 할 때 `title.template`을 쓰면 `특별한 이벤트 페이지 | 내 사이트`처럼 길어질 수 있는데 그걸 막아줍니다.
- layout 단위에서 잘 설계하면, 사이트 전체 제목 구조를 관리하기가 훨씬 수월해져요. 기본 템플릿은 내비두고, 특수 페이지에서만 `absolute`로 자유롭게 제목을 정하는 게 흔한 패턴입니다.

다시 요약하자면, `title.absolute`는 부모 제목 템플릿을 무시하고 딱 하나의 제목을 화면에 보여주고 싶을 때 사용하는 옵션이에요. next.js 앱에서 제목 관리할 때 요긴하게 쓰이니 기억해 두시면 좋아요!

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

### description 설명

```js
export const metadata = {
  description: 'The React Framework for the Web',
}
```

위 코드는 자바스크립트에서 `metadata`라는 객체에 웹 페이지나 앱의 설명(description)을 정의한 예시입니다. React나 Next.js 같은 프레임워크에서 메타데이터를 설정할 때 흔히 볼 수 있죠.

```html
<meta name="description" content="The React Framework for the Web" />
```

위 태그는 HTML `<head>` 영역에 들어가는 메타 태그로, 브라우저나 검색엔진이 페이지를 이해할 때 참고하는 설명을 담고 있어요. SEO(Search Engine Optimization)에도 꼭 필요한 부분이죠.

---

### 왜 description이 중요할까?

- **검색 결과 노출**: 구글이나 네이버 같은 검색엔진에서 내 페이지가 어떤 내용을 담고 있는지 설명해주기 때문에 검색 결과에 노출될 때 보이는 요약문으로 활용돼요.
- **사용자 클릭 유도**: 매력적인 설명은 검색 사용자들이 내 페이지를 클릭하도록 유도할 수 있어요.
- **공유 시 정보 제공**: SNS나 메신저에 링크를 공유할 때도 이 description이 설명으로 사용됩니다.

---

### Basic Fields

웹 메타데이터에는 description 외에도 여러 기본 필드들이 있어요. 대표적인 기본 필드를 표로 정리해볼게요.

| 필드명          | 설명                             | 예시                                |
|----------------|--------------------------------|-----------------------------------|
| title          | 페이지의 제목                   | "React로 만드는 웹 어플리케이션"   |
| description    | 페이지 설명                    | "The React Framework for the Web" |
| keywords       | 검색 키워드 (요즘엔 덜 쓰임)  | "React, JavaScript, Web Framework"|
| author         | 작성자 이름                   | "홍길동"                           |
| viewport       | 화면 크기의 기본 설정          | "width=device-width, initial-scale=1" |

---

### 팁: 메타데이터 잘 활용하는 법

1. **각 페이지마다 고유한 description 작성하기**  
   똑같은 설명을 여러 페이지에 쓰면 SEO에 좋지 않아요. 페이지마다 내용을 잘 반영해서 개성있게 써보세요.

2. **길이 조절**  
   너무 길면 검색 결과에서 잘리니까 150~160자 정도로 간결하게 작성하세요.

3. **중요 키워드 포함하기**  
   설명 안에 주요 키워드를 자연스럽게 넣으면 검색엔진에서 더 잘 인식합니다.

---

이렇게 메타테이터의 `description` 필드는 쉽지만 굉장히 중요한 부분이에요. 제대로 설정해두면 검색엔진에서의 인지도도 쑥쑥 올라가니 꼭 신경 써주세요!

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

안녕하세요! 오늘은 Next.js에서 메타데이터를 관리하는 방법에 대해서 이야기해볼게요. 특히 metadata 객체를 통해 SEO에 중요한 여러 정보를 한 번에 설정하는 방법과, 이 정보가 실제로 HTML 메타 태그로 어떻게 변환되는지 살펴보겠습니다.

먼저, Next.js에서 사용하는 metadata 객체를 보면 다음과 같아요:

```js
export const metadata = {
  generator: 'Next.js',
  applicationName: 'Next.js',
  referrer: 'origin-when-cross-origin',
  keywords: ['Next.js', 'React', 'JavaScript'],
  authors: [{ name: 'Seb' }, { name: 'Josh', url: 'https://nextjs.org' }],
  creator: 'Jiachi Liu',
  publisher: 'Sebastian Markbåge',
  formatDetection: {
    email: false,
    address: false,
    telephone: false,
  },
}
```

이렇게 설정해 주면 Next.js가 내부적으로 아래와 같은 메타 태그로 변환해주게 되죠:

| 태그 종류               | 내용                                                             |
|----------------------|----------------------------------------------------------------|
| `<meta name="application-name" content="Next.js" />` | 애플리케이션 이름을 지정해요.                                      |
| `<meta name="author" content="Seb" />`               | 저자 이름 (Seb)                                                  |
| `<link rel="author" href="https://nextjs.org" />`    | 저자 관련 URL (Josh의 프로필 링크)                               |
| `<meta name="author" content="Josh" />`               | 저자 이름 (Josh)                                                 |
| `<meta name="generator" content="Next.js" />`        | 생성 도구 (Next.js)                                              |
| `<meta name="keywords" content="Next.js,React,JavaScript" />` | 주요 키워드                                                    |
| `<meta name="referrer" content="origin-when-cross-origin" />` | 리퍼러 정책 설정                                               |
| `<meta name="color-scheme" content="dark" />`        | 컬러 스킴 (다크 모드)                                            |
| `<meta name="creator" content="Jiachi Liu" />`        | 콘텐츠 제작자                                                     |
| `<meta name="publisher" content="Sebastian Markbåge" />` | 콘텐츠 배급자                                                  |
| `<meta name="format-detection" content="telephone=no, address=no, email=no" />` | 자동 전화번호, 주소, 이메일 인식 비활성화 |

### 여기서 중요한 점!

- `authors` 배열 안에 객체를 넣으면 저자 정보뿐 아니라, URL 같은 추가 속성도 넣을 수 있다는 점!
- `formatDetection` 설정은 요즘 모바일에서 전화번호, 주소, 이메일 자동 링크 걸리는 걸 방지할 때 많이 쓰여요. 예를 들어, 아이폰은 자동으로 전화번호 인식을 하는데, 이걸 끄고 싶으면 metadata에 이렇게 설정하면 돼요.
- `referrer`는 보안을 위해서 추천하는 정책 중 하나예요.

---

### metadataBase란?

마지막으로 metadataBase라는 옵션에 대해 간단히 짚고 넘어갈게요.

metadataBase는 메타데이터에 사용하는 URL들이 절대 경로(완전한 URL)를 요구할 때, 기본 URL을 한 번에 지정해주는 편리한 옵션이에요.

예를 들어, 메타 태그에 이미지 URL을 넣거나 OG(Open Graph) 태그에 링크를 넣을 때 다음처럼 활용할 수 있죠:

```js
export const metadataBase = new URL('https://example.com')
```

이렇게 설정하면 상대 경로인 `/image.png`가 자동으로 `https://example.com/image.png`로 변환되어서 출력됩니다. 굉장히 편하죠?

---

### 맺으며

Next.js의 메타데이터 설정은 기본적인 SEO는 물론이고, 사용자 경험 개선에도 큰 도움이 됩니다. 이런 메타 정보를 꼼꼼하게 관리하는 습관이 좋은 웹사이트를 만드는 첫걸음이니 꼭 활용해보세요!

필요하다면 Open Graph, Twitter Card 같은 SNS용 메타 태그도 같이 설정할 수 있으니, 다음 글에서는 그런 부분도 다뤄볼게요~!

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

metadataBase 옵션에 대해 한 번 쉽게 정리해볼게요!

---

### metadataBase가 뭘까?

`metadataBase`는 URL 기반 메타데이터를 만들 때, **절대 URL 대신 상대 경로를 쓸 수 있게 해주는 옵션**이에요.  
예를 들어 `images: '/og-image.png'`라고 하면, 이 상대 경로가 `metadataBase`에 써둔 기본 URL과 합쳐져서 완전한 URL(`https://acme.com/og-image.png`)을 만들어줍니다.

---

### 사용 예시

```js
export const metadata = {
  metadataBase: new URL('https://acme.com'),
  alternates: {
    canonical: '/',
    languages: {
      'en-US': '/en-US',
      'de-DE': '/de-DE',
    },
  },
  openGraph: {
    images: '/og-image.png',
  },
}
```

위처럼 쓰면 Next.js가 최종 HTML에 다음과 같이 출력해줘요:

```html
<link rel="canonical" href="https://acme.com" />
<link rel="alternate" hreflang="en-US" href="https://acme.com/en-US" />
<link rel="alternate" hreflang="de-DE" href="https://acme.com/de-DE" />
<meta property="og:image" content="https://acme.com/og-image.png" />
```

---

### 알아두면 좋은 팁!

| 내용 | 설명 |
|---|---|
| 일반적으로 어디에 설정하나요? | `metadataBase`는 보통 `root app/layout.js`에 설정해서 모든 라우트에 적용합니다. |
| 어떤 필드에 적용되나요? | 절대 URL이 필요한 URL 기반 메타데이터들에 적용돼요. (Open Graph 이미지, canonical 링크, hreflang 등) |
| 복잡한 URL도 가능해요 | 서브도메인 포함(`https://app.acme.com`)이나, 베이스 경로 포함도 가능(`https://acme.com/start/from/here`)해요. |
| 절대 URL 쓰면? | 해당 필드는 `metadataBase`를 무시하고 절대 URL을 그대로 씁니다. |
| `metadataBase` 없으면 어떻게 되나요? | 상대 경로를 쓸 때 에러가 납니다. 빌드 오류가 나니까 꼭 설정해줘야 해요! |
| 슬래시 중복 처리 | `metadataBase`가 끝에 슬래시(`/`) 있어도, 상대경로가 앞에 슬래시 있어도 자동으로 정리해서 `//`가 생기지 않아요! |

---

### 마무리

`metadataBase`는 앱 전체 메타데이터를 관리할 때 정말 편리한 옵션이에요.  
특히 도메인이 바뀌거나 서브도메인이 추가될 때 한 곳만 바꾸면 되니까 유지보수도 쉬워집니다!

---

도움이 됐다면 좋겠네요~ 다음에 더 재미있는 개발 이야기로 찾아올게요! 😄

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

#### 기본값(Default value)

`metadataBase`를 따로 설정하지 않으면 기본값이 자동으로 지정돼요.

> **Vercel 환경 기준:**
> - 프로덕션 배포에서는 `VERCEL_PROJECT_PRODUCTION_URL` 값을 사용해요.
> - 프리뷰 배포에서는 `VERCEL_BRANCH_URL`이 우선시 되고, 이 값이 없으면 `VERCEL_URL`을 사용합니다.
> 
> 만약 위 환경 변수들이 없다면 기본값은 `http://localhost:${process.env.PORT || 3000}`이 됩니다. 이 덕분에 Open Graph 이미지가 로컬 환경에서 테스트할 때부터 Vercel 프리뷰, 프로덕션 환경 모두에서 잘 작동해요.
>
> 기본값을 덮어쓰고 싶다면, 환경 변수를 이용해 URL을 컴퓨팅하는 걸 추천해요. 이렇게 하면 개발(local), 스테이징, 프로덕션 환경에 맞춰 유동적으로 URL을 관리할 수 있어서 훨씬 편리하답니다.
> 
> 참고로, Vercel 관련 환경 변수는 [System Environment Variables](https://vercel.com/docs/concepts/projects/environment-variables#system-environment-variables) 문서에서 자세히 확인할 수 있어요.

#### URL 구성(URL Composition)  
(여기에서는 URL을 어떻게 조합하는지에 관한 내용이 나올 것 같네요. 필요하면 추가 설명해 드릴게요!)

---

추가 팁:  
`metadataBase`를 잘 활용하면 SEO, 소셜 미디어 공유 등에 쓰이는 Open Graph, Twitter Card 메타데이터 설정이 훨씬 편리해집니다. 특히, 여러 환경에 맞게 URL이 자동으로 바뀌니까 관리 부담도 줄어들죠!  
환경 변수 설정을 깜빡하지 말고, `.env` 파일 같은 곳에서 꼼꼼히 관리해 주세요.

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

URL 구성을 할 때, 개발자가 의도한 경로를 우선시하는 특징이 있습니다. 여기서 기본 디렉토리 탐색 방식과는 다르게 작동한다고 보면 돼요.

- `metadataBase`와 `metadata` 필드 사이에 있는 **슬래시(/)**는 자동으로 정리(정규화)돼요. 그래서 경로가 어색하게 겹치거나 빠지는 일이 없죠.
- 그리고 일반적으로 URL 전체 경로를 대체하는 절대 경로(`/`로 시작하는 경로)가 들어와도, 이걸 "상대 경로"로 해석해서 `metadataBase`의 끝부분부터 시작하는 경로로 간주해요.

### 예를 들어, 아래처럼 `metadataBase`를 설정했다고 가정해보죠:

```js
import type { Metadata } from 'next'

export const metadata: Metadata = {
  metadataBase: new URL('https://acme.com'),
}
```

이 경우, 만약 `metadata` 안에서 절대 경로처럼 보이는 `/about`이나 `/team` 같은 경로를 넣더라도, 실제로는 `https://acme.com/about`, `https://acme.com/team` 이런 식으로 `metadataBase`를 바탕으로 경로가 완성됩니다. 

이는 URL 조합 시 헷갈릴 수 있는 슬래시 중복 문제나, 경로가 엉뚱하게 바뀌는 문제를 방지해줘서 개발자가 의도한 URL을 정확하게 만들 수 있도록 도와줍니다.

---

추가로, Next.js에서 `metadataBase`를 사용하는 이유는 **SEO 최적화와 메타데이터 관리**를 좀 더 깔끔하고 명확하게 하기 위해서예요. 이걸 잘 활용하면 외부 링크나 공유할 때도 주소가 예상대로 동작해서 유용하니, 프로젝트에 맞게 꼭 써보길 추천합니다!

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

위에서 설명한 `metadataBase`를 상속받아 개별 메타데이터 필드가 자신의 값을 설정할 때, URL은 다음과 같이 해석돼요:

| `metadata` 필드      | 해석된 URL                       |
|---------------------|---------------------------------|
| `/`                 | `https://acme.com`               |
| `./`                | `https://acme.com`               |
| `payments`          | `https://acme.com/payments`      |
| `/payments`         | `https://acme.com/payments`      |
| `./payments`        | `https://acme.com/payments`      |
| `../payments`       | `https://acme.com/payments`      |
| `https://beta.acme.com/payments` | `https://beta.acme.com/payments` |

즉, 상대 경로를 적더라도 `metadataBase`가 `https://acme.com`이라면, 결국 그 기준에서 최종 URL이 해석된다는 뜻이에요. 완전한 절대 URL을 제공하면 그대로 사용되고, 상대 경로라면 기본 베이스를 기준으로 합쳐지게 되죠.

---

### openGraph 메타데이터 예시

```js
export const metadata = {
  openGraph: {
    title: 'Next.js',
    description: 'The React Framework for the Web',
    url: 'https://nextjs.org',
    siteName: 'Next.js',
    images: [
      {
        url: 'https://nextjs.org/og.png', // 반드시 절대 URL이어야 해요.
        width: 800,
        height: 600,
      },
      {
        url: 'https://nextjs.org/og-alt.png',
        width: 1800,
        height: 1600,
        alt: 'My custom alt', // 이미지가 로드 안 될 때 대체 텍스트용
      },
    ],
    videos: [
      {
        url: 'https://nextjs.org/video.mp4', // 절대 URL 필수
        width: 800,
        height: 600,
      },
    ],
    audio: [
      {
        url: 'https://nextjs.org/audio.mp3', // 절대 URL이어야 함
      },
    ],
    locale: 'en_US',
    type: 'website',
  },
}
```

Open Graph 메타데이터는 페이스북, 트위터 같은 소셜 미디어에서 공유할 때 정보가 풍부하게 나타나도록 도와줘요. 예를 들어, 사이트 제목, 설명, 메인 URL 뿐만 아니라 이미지, 동영상, 오디오까지 확장해서 넣을 수 있죠. 특히 이미지의 경우, 절대경로 URL을 사용하는 게 중요하고, 크기 정보와 대체 텍스트도 넣어주는 게 좋아요. 그래야 소셜 미디어가 올바르게 콘텐츠를 표시하거든요.

또, `locale` 속성은 콘텐츠 언어나 지역을 지정하는데, 이를 통해 다른 국가나 언어권에서 맞춤형으로 보이게 할 수 있어요. `type`은 공유하는 콘텐츠의 유형을 지정하는데, 예를 들어 `website`, `article`, `video.movie` 등이 있어요.

이렇게 메타데이터를 꼼꼼히 설정하면, 검색엔진 최적화(SEO)와 소셜 미디어에서의 콘텐츠 노출이 훨씬 좋아지니 꼭 활용해보세요!

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

오늘은 웹 페이스북, 트위터 등 소셜 미디어에서 내 사이트가 어떻게 보일지 정하는 데 핵심 역할을 하는 'Open Graph(OG) 메타태그'에 대해 얘기해보려고 해요. 그리고 Next.js에서 이걸 어떻게 손쉽게 다룰 수 있을지도 소개합니다.

---

## 1. 기본적인 Open Graph 메타태그 구조

웹 문서 `<head>` 안에 아래처럼 메타 태그를 넣으면, SNS에서 내 페이지가 공유될 때 제목, 설명, 이미지, 사이트 이름 등 다양한 정보를 잘 보여줄 수 있어요.

```html
<meta property="og:title" content="Next.js" />
<meta property="og:description" content="The React Framework for the Web" />
<meta property="og:url" content="https://nextjs.org/" />
<meta property="og:site_name" content="Next.js" />
<meta property="og:locale" content="en_US" />
<meta property="og:image" content="https://nextjs.org/og.png" />
<meta property="og:image:width" content="800" />
<meta property="og:image:height" content="600" />
<meta property="og:image" content="https://nextjs.org/og-alt.png" />
<meta property="og:image:width" content="1800" />
<meta property="og:image:height" content="1600" />
<meta property="og:image:alt" content="My custom alt" />
<meta property="og:video" content="https://nextjs.org/video.mp4" />
<meta property="og:video:width" content="800" />
<meta property="og:video:height" content="600" />
<meta property="og:audio" content="https://nextjs.org/audio.mp3" />
<meta property="og:type" content="website" />
```

> 여기서 중요한 점은 여러 개의 og:image 태그를 넣을 수 있다는 거예요! 이렇게 하면 SNS에서 선택적으로 보여줄 이미지가 다양해져서 좀 더 풍성한 미리보기를 만들 수 있죠.

---

## 2. 포스트 형식 메타태그 예시

블로그 글이나 뉴스 같은 ‘article’ 타입엔 아래처럼 좀 더 구체적인 메타 정보(작성일, 작성자 등)를 추가할 수 있어요.

```html
<meta property="og:title" content="Next.js" />
<meta property="og:description" content="The React Framework for the Web" />
<meta property="og:type" content="article" />
<meta property="article:published_time" content="2023-01-01T00:00:00.000Z" />
<meta property="article:author" content="Seb" />
<meta property="article:author" content="Josh" />
```

여기서도 작성자가 여러 명일 경우, author 태그를 여러 개 넣으면 됩니다.

---

## 3. Next.js 메타데이터 API로 간단하게 쓰기

Next.js 13부터 파일 기반 메타데이터 API 덕분에 이런 메타 태그를 자바스크립트 오브젝트 형태로 쉽게 선언 가능해요:

```js
export const metadata = {
  openGraph: {
    title: 'Next.js',
    description: 'The React Framework for the Web',
    type: 'article',
    publishedTime: '2023-01-01T00:00:00.000Z',
    authors: ['Seb', 'Josh'],
  },
}
```

이렇게만 하면 Next.js가 알아서 위의 `<meta>` 태그들을 자동으로 만들어 줍니다. 훨씬 편하죠?

---

## 4. 꿀팁: 이미지 메타데이터는 파일 기반 API 활용하기

Open Graph 이미지 메타정보를 직접 관리하는 건 정말 귀찮거든요. 예를 들어 이미지 파일이 바뀌거나 경로가 바뀌면 메타데이터도 수동으로 바꿔줘야 해서 실수가 잦아요.

Next.js 파일 기반 메타데이터 API를 사용하면:

- 실제 파일 시스템에서 이미지를 가져와서 맞는 크기, 타입 등을 자동으로 계산해줘요.
- 중복되거나 엉뚱한 메타 태그 생성을 방지해줘서 안정적입니다.

공식 문서를 참고해서 이미지 파일을 같이 관리하는 패턴으로 바꾸면 개발 생산성이 올라가니 꼭 써보세요!

---

## 마무리

- OG 태그는 SNS 공유 시 내 사이트가 멋지게 보이도록 만드는 필수 요소예요.
- 기본 태그부터 기사형 태그까지 상황에 맞게 넣으면 좋아요.
- Next.js 13 이상의 메타데이터 API를 활용하면 작업이 훨씬 수월해집니다.
- 이미지 메타 정보는 파일 기반 API를 이용해 관리하세요.

웹사이트를 좀 더 프로페셔널하게 보이게 만드는 작은 팁! 여러분의 프로젝트에도 꼭 적용해 보시길 바랍니다. 다음에도 좋은 웹 개발 이야기로 찾아올게요! 😄

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

### robots 메타 태그 설정하기

Next.js에서 SEO를 신경 쓸 때, `metadata` 객체의 `robots` 속성을 잘 활용하면 구글봇 같은 검색 엔진 크롤러들에게 페이지 인덱싱 방법을 알려줄 수 있어요. 예를 들어 아래처럼 설정할 수 있죠:

```js
import type { Metadata } from 'next'
 
export const metadata: Metadata = {
  robots: {
    index: true, // 페이지를 인덱싱 하라고 명시
    follow: true, // 페이지 내 링크들도 크롤링 하도록 허용
    nocache: false, // 캐싱 하지 말라는 지시어(false면 캐시 가능)
    googleBot: { // 구글봇에 대한 세부 설정
      index: true,
      follow: true,
      noimageindex: false, // 이미지 인덱싱 허용
      'max-video-preview': -1, // 비디오 미리보기 무제한
      'max-image-preview': 'large', // 큰 이미지 미리보기 허용
      'max-snippet': -1, // 스니펫 길이 제한 없음
    },
  },
}
```

이걸 HTML meta 태그로 나타내면 아래와 같아요:

```html
<meta name="robots" content="index, follow" />
<meta
  name="googlebot"
  content="index, follow, max-video-preview:-1, max-image-preview:large, max-snippet:-1"
/>
```

> **팁!**  
> robots 메타 태그는 SEO에 아주 중요한 역할을 해요.  
> - `index, follow`는 기본적으로 페이지와 링크들을 모두 크롤링해서 인덱싱하라는 뜻이고,  
> - `noindex`를 쓰면 검색결과에서 제외되기도 하니 주의가 필요해요.  
>  
> 구글봇에 따로 설정을 줄 수 있다는 점도 기억해두면 좋습니다.  
> 맞춤형 SEO가 필요할 때 활용하기 딱 좋거든요.

다음은 아이콘 관련 설정으로 넘어가 볼게요!

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

좋은 소식! 아이콘 설정할 때 되도록이면 파일 기반의 Metadata API를 사용하는 걸 추천해요. 설정 파일과 실제 아이콘 파일을 일일이 맞출 필요 없이, 이 API가 알아서 올바른 메타데이터를 생성해주거든요. 덕분에 실수도 줄고 관리도 편해져요.

예를 들어, 아래처럼 간단하게 아이콘 경로만 지정해주면:

```js
export const metadata = {
  icons: {
    icon: '/icon.png',
    shortcut: '/shortcut-icon.png',
    apple: '/apple-icon.png',
    other: {
      rel: 'apple-touch-icon-precomposed',
      url: '/apple-touch-icon-precomposed.png',
    },
  },
}
```

이렇게 실제 HTML 메타 태그가 자동으로 생성되죠:

```html
<link rel="shortcut icon" href="/shortcut-icon.png" />
<link rel="icon" href="/icon.png" />
<link rel="apple-touch-icon" href="/apple-icon.png" />
<link rel="apple-touch-icon-precomposed" href="/apple-touch-icon-precomposed.png" />
```

여기서 아이콘을 더 다양한 상황에 맞게 세분화해서 지정할 수도 있어요. 예를 들면 다크모드용 아이콘이나 여러 크기의 아이콘 등도 말이죠:

```js
export const metadata = {
  icons: {
    icon: [
      { url: '/icon.png' },
      new URL('/icon.png', 'https://example.com'), // 절대 URL도 가능해요
      { url: '/icon-dark.png', media: '(prefers-color-scheme: dark)' }, // 다크모드 전용 아이콘
    ],
    shortcut: ['/shortcut-icon.png'], // 배열 형태로도 지원하고
    apple: [
      { url: '/apple-icon.png' },
      { url: '/apple-icon-x3.png', sizes: '180x180', type: 'image/png' }, // 크기와 타입 지정
    ],
    other: [
      {
        rel: 'apple-touch-icon-precomposed',
        url: '/apple-touch-icon-precomposed.png',
      },
    ],
  },
}
```

잘 보시면, 각 아이콘에 크기, 미디어 쿼리, 타입 등 상황에 맞는 세부 옵션을 넣을 수 있어서 훨씬 유연하게 아이콘을 관리할 수 있어요. 특히 요즘 다크모드 지원이 필수인 만큼 `(prefers-color-scheme: dark)` 같은 미디어 쿼리를 활용하는 게 중요합니다.

아이콘 설정은 웹사이트의 첫인상에도 영향을 주니 꼼꼼하게 세팅해두면 좋아요! 나중에 favicon이 제대로 안 보인다고 당황하지 말고, 이런 메타데이터 API를 활용해보세요. 관리가 훨씬 쉬워질 거랍니다.

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

위 코드는 웹사이트에서 자주 사용하는 아이콘과 메타 태그 설정 부분인데요, HTML `<link>` 태그를 이용해 파비콘(favicon), 애플 터치 아이콘을 등록하는 예시입니다.

간단히 설명드리자면,

- `<link rel="shortcut icon" href="/shortcut-icon.png" />`: 가장 일반적인 파비콘 설정.
- `<link rel="icon" href="/icon.png" />`: 파비콘 설정 (기본).
- `<link rel="icon" href="https://example.com/icon.png" />`: 외부 URL에 있는 아이콘 설정 가능.
- `<link rel="icon" href="/icon-dark.png" media="(prefers-color-scheme: dark)" />`: 다크 모드일 때 사용할 파비콘.
- `<link rel="apple-touch-icon" href="/apple-icon.png" />` 및 관련 태그들: 아이폰, 아이패드 등 iOS 기기 홈 화면에 추가할 때 쓰이는 아이콘.

이 부분에서 알아두면 좋은 점 몇 가지!

| 항목 | 설명 |
| --- | --- |
| msapplication-* 메타 태그 | MS Edge(Chromium 기반)에서는 더 이상 지원하지 않으니 추가할 필요 없음 |
| themeColor 옵션 | Next.js 14부터는 `themeColor`가 deprecated 되었고, 대신 `viewport` 설정을 권장 |

📌 특히 다크 모드용으로 별도 파비콘을 설정한다든지, 애플 터치 아이콘에 사이즈를 명시해서 다양한 기기 대응을 하는 점이 요즘 트렌드고, 사용자 경험을 더 좋게 만듭니다.

---

> **추가 팁!**  
> 아이콘은 해상도가 높은 걸 준비하는 게 유리해요. 예를 들어 180x180 사이즈 정도면 모바일 기기에서 깨끗하게 보이고, 더 큰 아이콘을 준비하면 고화질 디스플레이에서 유용하죠.  
> 또, `manifest.json` 파일과 연동해 PWA(Progressive Web App)를 만들 때에도 아이콘 설정이 중요합니다.

이렇게 설정을 해두면, 다양한 브라우저와 디바이스에서 잘 작동하는 멋진 사이트 아이콘을 가질 수 있습니다! 필요하다면 상황에 맞게 `rel`이나 `sizes` 같은 속성도 조절해 보세요.

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

### colorScheme

> 더 이상 권장되지 않아요: Next.js 14부터는 metadata에서 `colorScheme` 옵션이 deprecated 되었어요. 대신에 **viewport 설정**을 사용하라고 하네요.

사실 `colorScheme`은 라이트 모드, 다크 모드 같은 테마를 간단하게 지정할 때 쓰였는데, 이제는 Next.js가 좀 더 유연하게 뷰포트 기반으로 처리하기 때문에 바뀐 거예요. 만약 이전에 `colorScheme` 쓰고 있었다면, 이제는 `viewport` 설정을 어떻게 주는지 살펴보는 걸 추천해요!

---

### manifest

웹 애플리케이션 매니페스트(manifest)는 웹 앱이 네이티브 앱처럼 동작할 수 있도록 도와주는 **설정 파일**이에요.  
말 그대로, 앱 이름, 아이콘, 시작 URL, 화면 회전 방식 등 사용자 경험에 관련된 여러 정보를 JSON 형태로 정의해두는 거죠.

이 매니페스트는 [Web Application Manifest 사양](https://developer.mozilla.org/en-US/docs/Web/Manifest)을 따르는데, 이를 통해 브라우저가 앱 설치 시 어떤 모습으로 표시할지, 시작할 때 어떤 화면크기로 열지 같은 걸 알 수 있어요.

---

추가로, Next.js에서 이 매니페스트를 다룰 때는 `public` 폴더에 `manifest.json` 파일을 넣고, 이걸 기본 HTML에 링크해주면 끝!  
```html
<link rel="manifest" href="/manifest.json" />
```

이렇게 하면 PWA(Progressive Web App)로서 한 단계 더 발전시킬 수 있으니, 관심 있으면 한번 도전해보세요!

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

안녕하세요 여러분! 이번에는 웹앱이나 사이트에서 자주 사용되는 `manifest` 관련 설정과 트위터 메타데이터에 대해 살펴볼게요.

---

먼저, Next.js 같은 프레임워크에서 `manifest`를 설정하는 방법입니다. 아래 코드는 `metadata` 객체 안에 `manifest` 속성을 넣는 예시인데요,

```js
export const metadata = {
  manifest: 'https://nextjs.org/manifest.json',
}
```

이렇게 하면 Next.js가 이 `manifest.json` 파일을 웹 앱의 매니페스트로 인식해줍니다.

---

기존 HTML에서는 아래처럼 `<link>` 태그로 `manifest`를 연결했죠.

```js
<link rel="manifest" href="https://nextjs.org/manifest.json" />
```

즉, `manifest`는 PWA(Progressive Web App)를 만들 때 필수적인 구성 요소로, 앱의 아이콘, 이름, 시작 URL, 색상 같은 정보를 담고 있어요. 그래서 브라우저가 앱을 어떻게 실행할지, 어떤 버튼을 보여줄지 등의 정보를 확인할 수 있답니다.

---

### Twitter 메타데이터 (트위터 카드)

흥미로운 점은, 트위터의 메타데이터 스펙이 실제로는 X(예전 트위터)뿐만 아니라 여러 플랫폼에서 활용된다는 거예요.

예를 들어, `twitter:card`, `twitter:title`, `twitter:description` 같은 태그를 넣으면 트윗이 공유될 때 예쁘게 카드 형태로 보이고, 검색 엔진에도 긍정적인 영향을 미칩니다.

추가 팁으로는 아래처럼 Next.js의 `metadata` 객체에 `twitter` 속성을 활용할 수도 있어요:

```js
export const metadata = {
  twitter: {
    card: 'summary_large_image',
    site: '@your_twitter_handle',
    title: '이 페이지의 제목',
    description: '페이지 설명을 적어주세요',
    images: ['https://example.com/image.png'],
  },
}
```

이렇게 하면 별도의 `<meta>` 태그를 일일이 작성하지 않아도 되니 훨씬 편리합니다.

---

### 요약

아래 표에 간략히 정리해봤어요!

| 설정 방법          | 설명                                     |
|-------------------|----------------------------------------|
| `metadata.manifest` | Next.js에서 PWA 매니페스트 JSON 파일 경로 설정  |
| `<link rel="manifest" ... />` | 전통적인 HTML에서 매니페스트 연결 태그          |
| `metadata.twitter` | 트위터(X) 카드 메타데이터 자동 생성 및 관리      |

---

오늘은 여기까지! PWA나 소셜 공유 시 내 사이트를 더 멋지게 보이게 만들고 싶다면 `manifest` 설정과 트위터 카드 메타데이터를 꼭 활용해보세요. 질문 있으면 댓글로 남겨주세요~ :)

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

트위터 카드를 활용해서 여러분의 웹페이지가 트위터에서 더 멋지게 보이도록 할 수 있어요. 특히 링크가 공유될 때 이미지, 제목, 설명 등이 깔끔하게 표시되어서 클릭률도 올라가겠죠?

이번에 소개할 건 Next.js 공식 문서 기준으로 작성한 Twitter Card 메타 태그예요. Next.js에서 `metadata` 객체 안에 트위터 정보를 넣는 방식인데, 이걸 페이지에 적용하면 트위터가 이 정보를 보고 카드 형태로 링크를 꾸며줍니다.

---

### 1. `summary_large_image` 카드 타입  
트윗에 큰 이미지와 함께 짧은 제목, 설명을 노출하는 카드 타입이에요.

```jsx
export const metadata = {
  twitter: {
    card: 'summary_large_image',            // 카드 타입 지정
    title: 'Next.js',                        // 카드 제목
    description: 'The React Framework for the Web', // 카드 설명
    siteId: '1467726470533754880',          // 사이트 트위터 ID (숫자)
    creator: '@nextjs',                      // 작성자 트위터 계정
    creatorId: '1467726470533754880',       // 작성자 트위터 ID (숫자)
    images: ['https://nextjs.org/og.png'],  // 반드시 절대 URL로 이미지 지정
  },
}
```

위처럼 설정하면 내부적으로는 아래처럼 메타 태그가 생성됩니다:

```html
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:site:id" content="1467726470533754880" />
<meta name="twitter:creator" content="@nextjs" />
<meta name="twitter:creator:id" content="1467726470533754880" />
<meta name="twitter:title" content="Next.js" />
<meta name="twitter:description" content="The React Framework for the Web" />
<meta name="twitter:image" content="https://nextjs.org/og.png" />
```

> **팁:** `images`는 배열로 받지만, URL은 반드시 절대 경로(https://~~)여야 하므로 로컬 경로나 상대 경로는 사용하면 안 됩니다.

---

### 2. `app` 카드 타입  
앱 카드 타입은 모바일 앱 링크를 추가해 앱 설치를 유도할 때 유용해요. 예를 들어 iPhone, iPad, 구글플레이 각각에 맞는 앱 스토어 URL을 넣을 수 있죠.

```jsx
export const metadata = {
  twitter: {
    card: 'app',
    title: 'Next.js',
    description: 'The React Framework for the Web',
    siteId: '1467726470533754880',
    creator: '@nextjs',
    creatorId: '1467726470533754880',
    images: {
      url: 'https://nextjs.org/og.png',
      alt: 'Next.js Logo',                  // 이미지 대체 텍스트도 넣으면 좋아요
    },
    app: {
      name: 'twitter_app',
      id: {
        iphone: 'twitter_app://iphone',    // 앱 아이디 스킴
        ipad: 'twitter_app://ipad',
        googleplay: 'twitter_app://googleplay',
      },
      url: {
        iphone: 'https://iphone_url',      // iOS 앱스토어 URL
        ipad: 'https://ipad_url',          // iPad용 URL
      },
    },
  },
}
```

요런 식으로 하면 트위터 카드에 앱 다운로드 링크도 함께 추가되어 사용자가 트윗에서 바로 앱 설치 페이지로 이동할 수 있답니다.  

---

## 한눈에 보는 트위터 카드 옵션 정리

| 속성           | 설명                                        | 비고                         |
| -------------- | ------------------------------------------- | ---------------------------- |
| `card`         | 카드 유형 지정 (`summary_large_image`, `app` 등) | 타입에 따라 보이는 형태 변동 |
| `title`        | 카드 제목                                    | 70자 이내 권장               |
| `description`  | 카드에 보이는 간단 설명                       | 200자 이내 권장              |
| `siteId`       | 사이트 소유 트위터 ID                         | 숫자만 입력                  |
| `creator`      | 작성자 트위터 핸들 (예: `@nextjs`)          | 선택사항                    |
| `creatorId`    | 작성자 트위터 ID                              | 숫자만 입력                  |
| `images`       | 대표 이미지 URL (또는 객체로 `url`, `alt` 포함)| 반드시 절대 URL              |
| `app`          | 모바일 앱 정보 (앱 이름, ID, URL 등)          | 앱 카드 타입에서 사용         |

---

### 마무리

트위터 카드는 단순한 텍스트 링크보다 훨씬 풍성한 정보를 제공합니다. 블로그, 뉴스, 제품 소개 페이지 등 모든 공유 콘텐츠에 기본으로 적용하면 좋고, 특히 모바일 앱을 홍보할 때는 `app` 카드가 효과적이에요.

참고로, 메타 태그가 제대로 작동하는지 꼭 [Twitter Card Validator](https://cards-dev.twitter.com/validator)에서 테스트해보세요! URL을 입력하면 트위터에서 어떻게 보일지 미리 확인할 수 있으니까요.

자, 이제 여러분 웹사이트에 멋진 트위터 카드를 달아서 SNS 유입도 올리고, 사용자 경험도 업그레이드 해보세요! 필요하면 Next.js 공식 문서도 같이 참고하시길 추천합니다. 🚀

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

이번에 살펴볼 내용은 Next.js 프로젝트 안에서 Twitter 카드 메타 태그 설정과 관련된 부분이에요. 그리고 Next.js 14에서 바뀐 `viewport` 설정에 대한 알림도 함께 다뤄볼게요.

우선, 코드에서 메타 태그를 보면 주로 Twitter 카드용으로 쓰이는 정보들이 담겨 있어요. 트위터 카드란, 트위터에 URL을 공유할 때 보이는 미리보기 박스 같은 거죠. 여기서 중요한 속성들을 한 번 정리해볼게요.

| 메타 태그 속성명                 | 설명                                    |
|--------------------------------|---------------------------------------|
| `twitter:site:id`               | 트위터 사이트(계정) 고유 ID             |
| `twitter:creator`               | 컨텐츠를 만든 사람의 트위터 핸들         |
| `twitter:title`                 | 공유할 때 보여질 제목                     |
| `twitter:description`           | 공유할 때 보여질 설명                     |
| `twitter:card`                  | 카드 타입 (예: summary, app 등)          |
| `twitter:image`                 | 미리보기 이미지 URL                      |
| `twitter:image:alt`             | 이미지 대체 텍스트                       |
| `twitter:app:name:iphone`       | 아이폰 앱 이름                           |
| `twitter:app:id:iphone`         | 아이폰 앱 고유 ID/URI                    |
| `twitter:app:url:iphone`        | 아이폰 앱 연결 URL                       |
| (ipad, googleplay 관련 항목도 동일한 역할을 함)                |

이렇게 여러 플랫폼(iPhone, iPad, Android/GooglePlay) 별로 앱 이름과 URL을 명시해 주는 게 특징이에요. 만약 여러분 앱이 여러 플랫폼에서 동작한다면, 이런 메타 태그를 꼭 넣어주는 게 좋아요. 그럼 트위터가 공유 시 앱을 바로 연결해주니까요.

---

### Next.js 14부터 달라진 viewport 설정

그리고 예고처럼, Next.js 14부터는 `metadata` 안에서 `viewport` 옵션을 직접 쓰는 게 deprecated(사용 중단) 되었어요. 대신, Next.js는 이제 `next.config.js` 또는 `app/layout.tsx` 등에서 별도의 viewport 설정을 권장합니다.

예를 들어, 기존엔 이렇게 썼다면:

```js
export const metadata = {
  viewport: 'width=device-width, initial-scale=1',
};
```

이제는 HTML `<head>` 안에 직접 `<meta name="viewport" content="...">` 를 넣거나, Next.js가 제공하는 새로운 방식대로 설정하라는 거죠.

이 부분은 반응형 웹 개발 시 매우 중요한 설정이라 꼭 알아두세요!

---

### 추가 팁! Twitter Card Validator

트위터에서 내가 설정한 카드가 제대로 나오는지 확인하고 싶다면 [Twitter Card Validator](https://cards-dev.twitter.com/validator) 를 사용해보세요. URL 입력하면 실제 카드가 어떻게 나오는지 미리 보여줍니다.

---

그럼 오늘은 여기까지! 메타 태그 잘 활용해서 SNS 공유 효과도 쑥쑥 올려보세요~

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

요즘 웹사이트 최적화할 때 꼭 챙겨야 하는 메타 태그들, 여러분도 많이 들어보셨죠? 구글, 야ндекс, 야후 같은 검색엔진 인증부터, 애플 웹 앱 설정까지 다양한 메타 정보를 자바스크립트 객체 형태로 관리할 수 있는데요. 제가 한 번 직접 정리해보고, 어떤 식으로 활용하는지 쉽게 설명해 드릴게요!

---

## 1. 검색엔진 인증 메타 태그 관리하기

웹사이트를 운영하면 구글, 야ндекс, 야후 등 다양한 검색엔진에 ‘내 사이트 인증’ 절차를 거쳐야 하죠. 이렇게 해야 검색 결과에 잘 노출되고, 통계도 정확하게 받아볼 수 있어요.

```js
export const metadata = {
  verification: {
    google: 'google',           // 구글 사이트 인증 코드
    yandex: 'yandex',           // 야ндекс 사이트 인증 코드
    yahoo: 'yahoo',             // 야후 사이트 인증 코드
    other: {
      me: ['my-email', 'my-link'], // 기타 인증용 메타 정보
    },
  },
}
```

위처럼 `verification` 객체 안에 각 인증 키를 넣으면, 실제 HTML 메타 태그로 이렇게 변환됩니다:

```html
<meta name="google-site-verification" content="google" />
<meta name="y_key" content="yahoo" />
<meta name="yandex-verification" content="yandex" />
<meta name="me" content="my-email" />
<meta name="me" content="my-link" />
```

> **TIP:** `me` 메타 태그는 주로 이메일이나 개인 프로필 URL을 나타낼 때 사용해요. 검색엔진 최적화(SEO)를 넘어서 사이트 신뢰도에 도움 됩니다.

---

## 2. Apple Web App 을 위한 메타 태그 구성하기

아이폰, 아이패드 사용자 대상으로 웹 앱 모양을 좀 더 자연스럽게 만들고 싶다면 ‘appleWebApp’ 메타 정보가 매우 중요해요. 홈 화면에 웹 앱을 추가했을 때, 앱처럼 보여지는 설정들을 여기에 담을 수 있어요.

```js
export const metadata = {
  itunes: {
    appId: 'myAppStoreID',          // 앱스토어 ID
    appArgument: 'myAppArgument',   // 앱 열 때 전달하는 인자 (URL 스킴 등)
  },
  appleWebApp: {
    title: 'Apple Web App',         // 웹 앱 이름
    statusBarStyle: 'black-translucent', // 상태바 스타일(투명하거나 색상 지정)
    startupImage: [                 // 시작화면 이미지
      '/assets/startup/apple-touch-startup-image-768x1004.png',
      {
        url: '/assets/startup/apple-touch-startup-image-1536x2008.png',
        media: '(device-width: 768px) and (device-height: 1024px)',
      },
    ],
  },
}
```

위 설정을 넣으면, 애플 기기에서 다음과 같은 경험을 쉽게 만들 수 있어요:

- 홈 화면에 아이콘 추가했을 때 앱 이름과 상태바 모양 조정
- 앱 시작 시 딜레이 없이 로딩 화면(스플래시 이미지) 띄우기
- 앱스토어 연동해서 사용자 유입 매끄럽게

---

### 정리: 메타 태그는 ‘웹사이트를 검색엔진과 스마트 기기 환경에 맞게 최적화하는 히든 챔피언’

하나하나 보면 어렵거나 귀찮을 수 있지만, `metadata` 객체로 잘 구조화하면 관리도 편하고 재사용도 쉬워집니다. 특히 최신 프레임워크(Next.js 같은)에서는 이런 식으로 메타 정보를 코드로 정의하는 걸 권장하죠.

혹시 여러분도 사이트 SEO, 혹은 PWA/Apple Web App 최적화를 생각 중이라면, 위처럼 메타 태그 전략부터 차근차근 시작해 보시길 추천합니다!

---

참고로 다음 시간에는 이 `metadata` 객체를 실제로 어떻게 HTML `<head>`에 심고, 렌더링하는지 예제를 상세히 보여드릴게요. 기대 많이 해 주세요~

---

필요하다면 제가 직접 만든 메타 태그 템플릿도 공유해 드릴 수 있으니, 언제든 편하게 물어봐 주세요! 😊

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

메타 태그와 링크 태그를 통해 앱 관련 정보와 웹 앱 설정, 그리고 SEO에 중요한 alternates(대체 링크)를 어떻게 다루는지 예시를 보여드릴게요.

---

## 1. iOS/앱 관련 메타 태그 예시

```html
<meta
  name="apple-itunes-app"
  content="app-id=myAppStoreID, app-argument=myAppArgument"
/>
<meta name="mobile-web-app-capable" content="yes" />
<meta name="apple-mobile-web-app-title" content="Apple Web App" />
<link
  href="/assets/startup/apple-touch-startup-image-768x1004.png"
  rel="apple-touch-startup-image"
/>
<link
  href="/assets/startup/apple-touch-startup-image-1536x2008.png"
  media="(device-width: 768px) and (device-height: 1024px)"
  rel="apple-touch-startup-image"
/>
<meta
  name="apple-mobile-web-app-status-bar-style"
  content="black-translucent"
/>
```

### 설명!

- `apple-itunes-app`: 스토어 앱 아이디와 호출할 매개변수를 넣어, iOS에서 앱으로 바로 연결할 수 있게 해줍니다.
- `mobile-web-app-capable`: 웹앱을 홈화면에 추가하면 네이티브 앱처럼 풀스크린으로 보여주도록 설정.
- `apple-mobile-web-app-title`: 홈화면 아이콘 밑에 표시될 이름.
- `apple-touch-startup-image`: 앱 실행 시 보여줄 스플래시 이미지를 기기별 해상도와 맞춰서 지정해줄 수 있어요.
- `apple-mobile-web-app-status-bar-style`: 상태바 스타일을 지정하는데 `black-translucent`는 반투명 검정색을 의미.

---

## 2. SEO에 도움되는 alternates 설정

Next.js 등에서 `metadata` 객체를 활용하면 좀 더 깔끔하게 메타 태그를 관리할 수 있습니다.

### metadata 예시 (자바스크립트)

```js
export const metadata = {
  alternates: {
    canonical: 'https://nextjs.org',
    languages: {
      'en-US': 'https://nextjs.org/en-US',
      'de-DE': 'https://nextjs.org/de-DE',
    },
    media: {
      'only screen and (max-width: 600px)': 'https://nextjs.org/mobile',
    },
    types: {
      'application/rss+xml': 'https://nextjs.org/rss',
    },
  },
}
```

### 실제 HTML로 변환 시

```html
<link rel="canonical" href="https://nextjs.org" />
<link rel="alternate" hreflang="en-US" href="https://nextjs.org/en-US" />
<link rel="alternate" hreflang="de-DE" href="https://nextjs.org/de-DE" />
<link
  rel="alternate"
  media="only screen and (max-width: 600px)"
  href="https://nextjs.org/mobile"
/>
<link
  rel="alternate"
  type="application/rss+xml"
  href="https://nextjs.org/rss"
/>
```

### 설명!

- `canonical`: 중복되는 콘텐츠가 있을 때 대표 주소를 검색 엔진에 알려줘 SEO에 유리!
- `hreflang`: 언어별 페이지가 있을 때, 사용자 언어에 맞는 페이지로 안내해줍니다.
- `media`: 특정 미디어쿼리 조건에 맞는 별도의 페이지를 지정할 수 있어 반응형 페이지를 분리할 때 유용.
- `type`: RSS 피드 같은 타입별 대체 리소스 링크도 연결할 수 있습니다.

---

## 추가 팁!

- iOS 홈 화면 아이콘(`apple-touch-icon`)이나 안드로이드 웹앱 설정도 같이 해주면 모바일 환경에서 훨씬 앱 같은 느낌을 낼 수 있어요.
- SEO에서 `alternates`는 다국어 사이트, 반응형 사이트 운영 시 꼭 챙겨야 할 부분입니다.
- Next.js나 React 같은 프레임워크에선 `head` 혹은 `metadata` 설정을 통해 위 태그를 동적으로 관리하면 편리해요.

---

필요한 메타 태그나 링크 설정 어떻게 쓰는지 궁금하다면 언제든 물어봐주세요! 제가 직접 해보고 쓰는 만큼 친근하게 알려드릴게요 :)

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

### appLinks: 메타데이터에서 앱 링크 설정하기

안녕하세요 여러분! 오늘은 메타데이터(metadata) 안에 `appLinks`를 설정하는 방법에 대해서 쉽게 알려드릴게요. 여러분이 Next.js 같은 최신 프레임워크에서 앱과 웹을 연결할 때 꽤 유용한 부분이랍니다.

먼저, 아래 `appLinks` 객체를 보시면 iOS, Android, 그리고 Web을 위한 링크 설정을 각각 해 놓은 걸 볼 수 있어요.

```js
export const metadata = {
  appLinks: {
    ios: {
      url: 'https://nextjs.org/ios',
      app_store_id: 'app_store_id',
    },
    android: {
      package: 'com.example.android/package',
      app_name: 'app_name_android',
    },
    web: {
      url: 'https://nextjs.org/web',
      should_fallback: true,
    },
  },
}
```

- `ios`: 여기서는 iOS 앱 스토어 링크와 앱 ID가 들어가 있어요. 사용자가 iOS 디바이스에서 해당 링크를 열면 앱이 연결되거나 앱스토어로 이동하게 되죠.
- `android`: 패키지 이름과 앱 이름이 들어가 있습니다. 마찬가지로 안드로이드 사용자를 위한 정보예요.
- `web`: 웹 URL과 `should_fallback`이라는 옵션이 있어요. 이거는 모바일 앱이 설치 안 돼 있을 때 웹 사이트로 자동으로 넘어가게 하는 역할을 합니다.

그리고 이렇게 설정한 내용은 결국 아래처럼 HTML `<meta>` 태그로 변환이 됩니다. 이 메타 태그들은 Open Graph 프로토콜에 기반한 앱 링크 정보를 담고 있어서 SNS나 브라우저가 앱과 웹을 적절히 연결하게 해줍니다.

| 태그 속성                    | 내용                          |
|----------------------------|-----------------------------|
| `<meta property="al:ios:url" content="https://nextjs.org/ios" />`         | iOS 앱으로 연결되는 URL          |
| `<meta property="al:ios:app_store_id" content="app_store_id" />`          | iOS 앱스토어 ID                  |
| `<meta property="al:android:package" content="com.example.android/package" />` | 안드로이드 앱 패키지 이름          |
| `<meta property="al:android:app_name" content="app_name_android" />`      | 안드로이드 앱 이름                |
| `<meta property="al:web:url" content="https://nextjs.org/web" />`         | 웹 URL                         |
| `<meta property="al:web:should_fallback" content="true" />`               | 웹 fallback 허용 여부             |

---

### 추가 팁!

- **앱 링크 설정의 중요성:** 요즘은 SNS나 모바일 브라우저에서 링크를 누르면 바로 앱이 뜨길 원하시잖아요? 이런 `appLinks` 덕분에 사용자 경험이 확실히 좋아집니다.
- **앱이 없을 때 웹 fallback:** `should_fallback`을 `true`로 두면 사용자의 기기에 앱이 없어도 자연스럽게 웹으로 넘어가서 끊김없는 경험을 제공합니다.
- **아이콘이나 스플래시 스크린 같은 추가 설정도 가능**하니, 앱을 홍보할 때 같이 챙기시면 좋겠죠?

앱과 웹을 유기적으로 연결하고 싶을 때, 위처럼 `appLinks` 설정하는 것 잊지 마세요! 도움이 되셨다면 좋아요와 구독 부탁드려요~ 다음에 또 유용한 개발 정보로 찾아뵙겠습니다. :)

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

"archives"라는 건 역사적인 가치가 있는 기록, 문서, 혹은 기타 자료들을 모아놓은 컬렉션을 의미해요. 예를 들어, 오래된 사진, 편지, 공식 문서 같은 것들이 여기에 속하죠.

위에 있는 코드를 보면 두 가지 형태로 "archives"를 지정하는 방법이 나와 있어요.

첫 번째는 자바스크립트 객체 형태로, `metadata`라는 변수 안에 `archives`라는 배열 속성으로 URL을 넣고 있고요.

```js
export const metadata = {
  archives: ['https://nextjs.org/13'],
}
```

두 번째는 HTML의 `<link>` 태그를 써서 `"archives"`라는 관계를 명시하는 방법이에요.

```js
<link rel="archives" href="https://nextjs.org/13" />
```

이런 방식으로 사이트나 문서가 참조하는 중요한 역사적 자료 링크를 연결할 수 있죠.

### assets(에셋)

여기서 `archives`와 비슷하게 많이 쓰이는 `assets`라는 용어도 알아두면 좋아요. `assets`는 이미지, 스타일시트, 스크립트 등 웹사이트에서 사용하는 모든 '자산'들을 의미해요. 즉, 웹사이트를 구성하는 데 필요한 리소스 파일들이죠.

아래는 `assets`를 나타낼 때 사용하는 간단한 메타데이터 예시예요.

```js
export const metadata = {
  assets: ['https://example.com/assets/logo.png'],
}
```

---

추가로, 보통 역사적 기록이나 자료들을 다룰 때는 메타데이터를 잘 작성해두면 나중에 자료들을 검색하거나 관리하는 데 큰 도움이 돼요. 그래서 링크 태그나 자바스크립트 객체를 활용해 이런 정보를 명확하게 표시하는 게 중요합니다!

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

Next.js에서 `metadata`를 사용해서 페이지에 다양한 메타 정보를 추가할 수 있어요. 근데 `assets`와 `bookmarks`를 어떻게 쓰는지는 조금 헷갈릴 수 있죠. 여기서 간단하게 설명하고 예제도 보여드릴게요.

---

### 1. `assets` 설정하기

```js
export const metadata = {
  assets: ['https://nextjs.org/assets'],
}
```

`assets`는 페이지에서 사용할 외부 리소스(이미지, 폰트, 아이콘 등)를 미리 선언하는 역할을 해요. 이게 있으면, 브라우저가 미리 해당 리소스를 캐싱하거나 최적화된 방식으로 로딩할 수 있게 도와줍니다.

브라우저에 직접 `<link rel="assets" ... />` 태그를 추가하는 코드도 있는데, 보통 Next.js에서는 `metadata`를 통해 쉽게 설정할 수 있어요.

이걸 HTML로 보면 이렇게 생겼죠:

```html
<link rel="assets" href="https://nextjs.org/assets" />
```

---

### 2. `bookmarks` 설정하기

```js
export const metadata = {
  bookmarks: ['https://nextjs.org/13'],
}
```

`bookmarks`는 웹 브라우저에서 사용하는 북마크 리소스를 지정하는 메타데이터인데, 사실 웹 표준이나 브라우저 지원 면에서는 아직 널리 쓰이지 않는 편이에요. 그래서 굳이 사용할 필요는 없고, 특수한 경우가 아니라면 안 써도 무방합니다.

---

### 간단 정리

| 키워드    | 용도                                      | 브라우저 지원 및 유용성                  |
|----------|-----------------------------------------|----------------------------------------|
| `assets` | 외부 리소스를 사전에 선언해 최적화 지원            | Next.js에서 공식 지원, 실사용에서 유용함   |
| `bookmarks` | 북마크 관련 리소스 지정 (특정 브라우저에서 사용 가능) | 아직 표준 널리 쓰이지 않음, 보통 안 써도 됨 |

---

### 참고로

- Next.js에서 `metadata`는 SEO나 사용자 경험 개선에 중요한 역할을 해요.
- 공식 문서에서 지원하는 키만 사용하는 게 안전하고, `assets`는 그중 하나입니다.
- HTML `<link>` 태그를 직접 다룰 필요 없이, Next.js가 알아서 `metadata`를 바탕으로 최적의 구조로 만들어줍니다.

---

이렇게 `metadata`를 잘 활용하면 페이지가 더 빠르게 로드되거나, 검색 엔진 최적화에도 도움 될 수 있어요. 혹시 더 궁금한 점 있으면 알려주세요!

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

안녕하세요! 오늘은 HTML과 Next.js 메타데이터에 대해서 간단히 살펴볼게요. 코드 예시들을 보면서 자연스럽게 이해할 수 있도록 설명해 드릴게요.

먼저, `<link>` 태그로 북마크(bookmarks) 같은 걸 지정하는 부분인데요:

```html
<link rel="bookmarks" href="https://nextjs.org/13" />
```

여기서 `rel="bookmarks"`는 링크가 북마크 관련 링크임을 알려주는 역할이에요. 하지만 보통 웹 개발에서 이 속성은 잘 사용하지 않는 편이라, 만약 북마크를 위한 링크를 넣고 싶다면 보통 사용자 브라우저나 북마크 기능을 활용하는 게 좋겠죠.

---

다음은 Next.js에서 메타데이터(metadata)를 설정하는 부분입니다.

```js
export const metadata = {
  category: 'technology',
}
```

이 코드처럼 `metadata` 객체로 카테고리 같은 정보를 지정할 수 있어요. Next.js 13 버전부터 이런 식으로 페이지나 컴포넌트 단위 메타데이터 설정이 가능해져서 SEO(검색엔진최적화)에 아주 유용합니다.

---

그리고 보통 HTML 문서에는 다음처럼 `<meta>` 태그를 써서 카테고리를 명시하기도 하죠:

```html
<meta name="category" content="technology" />
```

이 메타 태그는 검색엔진이나 소셜 미디어가 내용을 이해하는 데 도움을 줄 수 있어요.

---

### 쉽게 정리한 내용

| 태그/코드                   | 역할 및 설명                                   |
|-----------------------------|----------------------------------------------|
| `<link rel="bookmarks" ...>` | 북마크 관련 링크를 지정 (실제로는 잘 쓰이지 않음) |
| `export const metadata = {...}` | Next.js에서 페이지 메타데이터 설정 (추천)          |
| `<meta name="category" ...>` | HTML 문서에서 카테고리 같은 메타 정보 명시          |

---

참고로, HTML 메타태그는 SEO를 위해 꼭 필요한 요소입니다. 특히 `description`, `keywords`, `author` 같은 태그들도 추가해주면 더 좋죠. Next.js 13부터는 이런 메타데이터를 코드 안에서 직접 관리할 수 있어 개발 생산성이 훨씬 올라가니 꼭 한 번 써보세요!

그럼 오늘 내용은 여기까지~ 즐거운 개발 되세요! 😊

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

### 페이스북 연동하기

웹페이지에 페이스북 앱이나 페이스북 계정을 연결해서 페이스북 소셜 플러그인(Facebook Social Plugins)을 활용할 수 있어요. 공식 페이스북 문서에서 더 자세한 내용을 확인할 수 있답니다.

> 참고할 점: `appId`와 `admins` 옵션은 둘 다 쓸 수 없고, 한 가지만 선택해서 써야 해요.

간단한 예시 코드를 보면 이렇게 구성할 수 있어요:

```js
export const metadata = {
  facebook: {
    appId: '12345678',  // 본인의 페이스북 앱 ID를 여기에 입력하세요
  },
}
```

---

#### 추가 팁!

- **appId**는 페이스북 개발자 페이지에서 앱을 생성하면 얻을 수 있어요. 이 ID를 넣으면 페이스북 로그인, 공유 버튼 등 다양한 플러그인을 웹에 쉽게 적용할 수 있답니다.
- 만약 개인 또는 조직의 페이스북 계정이 관리자로서 관련 페이지를 관리한다면 `admins` 옵션을 사용할 수도 있어요. 하지만 `appId`와 `admins`는 동시에 넣을 수 없으니 주의하세요.
- 페이스북 소셜 플러그인을 적용할 때는 도메인 설정도 잘 맞춰줘야 오류 없이 작동해요.

웹에 페이스북 기능을 심플하게 넣고 싶다면 이 설정부터 시작해 보세요!

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

여러분, 페이스북과 연동할 때 사용하는 메타 태그에 대해 이야기해볼게요.

---

먼저, 페이스북 앱 ID를 지정하는 태그는 이렇게 생겼어요:

```html
<meta property="fb:app_id" content="12345678" />
```

이건 페이스북 앱과 내 사이트를 연결할 때 자주 사용되는 태그입니다. 특히, 페이스북에서 제공하는 소셜 플러그인이나 오픈그래프(Open Graph) 기능을 쓸 때 꼭 필요해요.

---

그리고 또 하나, 페이스북 관리자 권한을 명시할 때는 이렇게 할 수 있어요:

```js
export const metadata = {
  facebook: {
    admins: '12345678',
  },
}
```

혹은 HTML에서는 다음처럼 쓸 수 있죠:

```html
<meta property="fb:admins" content="12345678" />
```

여기서 '12345678'은 페이스북 사용자 ID입니다. 이렇게 적어놓으면, 내가 해당 페이지에 대한 페이스북 통계나 관리를 더 쉽게 할 수 있어요.

---

추가로, 만약 여러 명의 fb:admins를 설정하고 싶다면, 배열을 사용하면 됩니다. 예를 들어 이렇게요:

```js
export const metadata = {
  facebook: {
    admins: ['12345678', '87654321', '11223344'],
  },
}
```

이 경우, 페이스북에서 여러 사람에게 관리자 권한을 줄 수 있어 협업 시에 아주 유용하죠.

---

정리하자면,

| 태그 종류          | 역할                        | 예시                                        |
|-------------------|----------------------------|-------------------------------------------|
| fb:app_id         | 앱 ID 등록, 페이스북 연동 필수    | `<meta property="fb:app_id" content="12345678" />` |
| fb:admins         | 페이지 관리자 지정               | `<meta property="fb:admins" content="12345678" />`   |
| fb:admins (배열)  | 여러 명의 관리자 지정            | admins: ['12345678', '87654321']             |

---

마지막으로 팁! 메타 태그는 HTML 헤드(head) 태그 안에 넣어야 제대로 작동하니, 꼭 위치를 신경 써주세요. 더불어 페이스북 디버거 툴 (Facebook Sharing Debugger)을 사용하면 제대로 설정됐는지 검증 가능하니 활용해 보시길 추천해요!

필요한 부분 있으면 댓글로 물어봐 주세요~ :)

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

안녕하세요! 이번에는 페이스북과 핀터레스트 같은 SNS 메타 태그를 다루는 방법을 간단히 살펴볼게요. 웹 개발할 때 메타데이터를 어떻게 설정하는지는 SEO나 소셜 미디어에서 내 콘텐츠가 어떻게 보일지에 큰 영향을 줍니다.

## Facebook 메타 태그 설정하기

먼저 `metadata` 객체를 사용해 페이스북의 `fb:admins`를 등록하는 예시입니다. 이 코드는 여러분이 페이스북 페이지의 관리자임을 인증하는 데 도움을 줘요.

```js
export const metadata = {
  facebook: {
    admins: ['12345678', '87654321'],
  },
}
```

이걸 HTML 메타 태그로 변환하면 이렇게 됩니다:

```html
<meta property="fb:admins" content="12345678" />
<meta property="fb:admins" content="87654321" />
```

페이스북에서 여러 명의 관리자를 등록할 때는 `<meta>` 태그를 각각 추가하는 형식이죠. 이 값들은 페이스북 개발자 페이지에서 본인 계정을 식별할 때 필요해요. 특히 오픈 그래프(Open Graph) 태그와 함께 쓴다면 내 페이지가 페이스북에 공유될 때 멋지게 표시할 수 있습니다.

Tip!  
`fb:admins` 외에도 `fb:app_id`(앱 ID)를 추가해주면 더 강력한 통계와 함께 페이스북 기능을 사용할 수 있어요.

## Pinterest Rich Pins 활성화하기

다음으로, 핀터레스트에서는 "Rich Pins"(리치 핀)이라는 기능이 있는데요, 이걸 통해 내가 만든 웹사이트 콘텐츠가 핀터레스트에서 더 풍부한 정보를 담아 보여질 수 있어요.

> 리치 핀은 예를 들면 상품, 레시피, 기사 등의 추가 정보가 포함되어서 사용자에게 더 많은 가치를 제공합니다.

웹페이지에서 리치 핀을 사용할지 말지를 설정할 수 있는데, 간단히 메타 태그를 추가하거나 핀터레스트 개발자 도구를 통해 활성화할 수 있어요.

예시로는 이런 식으로 메타 태그를 넣게 되죠:

```html
<meta property="og:type" content="product" />
<meta property="og:title" content="Awesome Product" />
<meta property="og:description" content="This product is really awesome because..." />
<meta property="og:image" content="https://example.com/product-image.jpg" />
<link rel="canonical" href="https://example.com/product-page" />
```

이런 오픈 그래프 태그들이 핀터레스트가 내 콘텐츠를 정확히 이해하고 풍성하게 보이도록 도와줍니다.

---

### 요약 정리

| 소셜 서비스 | 설정 방법                          | 주요 역할                                  |
|-------------|----------------------------------|-------------------------------------------|
| Facebook    | `<meta property="fb:admins" />` | 관리자 인증 및 공유 시 페이지 신뢰성 향상  |
| Pinterest   | 오픈 그래프 태그 추가           | 리치 핀 활성화로 콘텐츠 정보 풍성하게 표시 |

---

메타 태그를 잘 활용하면 내 사이트가 SNS에서 더 멋지게 보이고, 방문자에게 신뢰감을 줄 수 있어요. 블로그나 쇼핑몰을 운영하신다면 꼭 챙겨야 할 부분이니 한번 적용해보시길 추천드릴게요! 필요하면 오픈 그래프 태그, 트위터 카드 태그 같은 다른 메타 데이터도 같이 공부해보면 좋습니다 :)

궁금한 점 있으면 언제든 물어보세요!

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

자바스크립트 코드에서 메타데이터를 설정하는 방법과 HTML 메타 태그를 직접 작성하는 방법을 비교해볼게요.

```js
export const metadata = {
  pinterest: {
    richPin: true,
  },
}
```

위 코드는 프로젝트 안에서 메타데이터를 설정하는 대표적인 예입니다. 여기서는 Pinterest의 Rich Pin 기능을 활성화 한다는 걸 나타내죠.

반면에 HTML에서는 이렇게 작성할 수 있겠죠:

```html
<meta name="pinterest-rich-pin" content="true" />
```

이렇게 하면 브라우저나 크롤러가 해당 페이지에서 Rich Pin 기능을 인식할 수 있어요.

---

그리고 여기서 중요한 점! 

> **other** 옵션을 활용하면 프로젝트에서 기본적으로 지원하지 않는 커스텀 메타 태그들도 추가할 수 있어요.

즉, 공식적으로 제공되는 메타데이터 옵션 외에 브랜드에서 특별하게 필요한 메타 태그가 있거나, 아직 최신 메타 태그가 공식 지원되기 전에 먼저 써야 하는 상황에도 유용하답니다.

---

참고로 메타데이터는 검색 엔진 최적화(SEO), SNS 공유, 웹사이트의 기능 확장 등 다양한 용도로 활용되기 때문에, 어떤 메타 태그를 쓸지 꼼꼼하게 고민해보는 게 좋아요. 특히 Pinterest 같은 SNS 메타 태그는 이미지 노출과 관련된 중요한 역할을 하니까, 꼭 신경 써서 세팅해보시길 바랍니다!

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

이 내용은 React나 Next.js 같은 프레임워크에서 메타 태그(metadata)를 설정하는 방법에 관한 거예요. 메타 태그는 웹페이지의 정보를 검색 엔진이나 브라우저에 알려주는 중요한 역할을 하죠.

우선, `metadata` 객체에서 `other`라는 키로 커스텀 메타 태그를 정의할 수 있어요.

```js
export const metadata = {
  other: {
    custom: 'meta',
  },
}
```

위 코드는 결국 HTML로 변환될 때 아래처럼 된다고 볼 수 있어요.

```html
<meta name="custom" content="meta" />
```

만약 같은 이름의 메타 태그를 여러 개 만들어야 한다면, 값에 배열을 넣으면 됩니다.

```js
export const metadata = {
  other: {
    custom: ['meta1', 'meta2'],
  },
}
```

이렇게 하면 HTML에서는 다음과 같이 두 개의 메타 태그가 만들어져요:

```html
<meta name="custom" content="meta1" />
<meta name="custom" content="meta2" />
```

---

### 추가 팁!

1. **메타 태그에 다양한 정보 넣기**  
   `name`과 `content` 외에도 `http-equiv`나 `charset` 등의 속성도 있는데, 프레임워크 문서에서 지원하는 범위 내에서 활용해보세요.

2. **여러 메타 태그 관리하기**  
   SEO 최적화와 SNS 공유를 위한 `og:title`, `og:description`, `twitter:card` 같은 메타 태그도 함께 작성해야 한다면, `metadata` 객체 안에 따로 정의해놓고 관리하면 편리해요.

3. **동적 메타 태그**  
   만약 페이지마다 동적으로 메타 태그를 바꿔야 할 때는, 정적인 `export const metadata`뿐 아니라 React의 상태(state)나 훅을 사용해서 동적으로 설정하는 방법도 공부해보세요.

메타 태그를 잘 활용하면 사용자 경험과 검색엔진에서의 가시성 모두 향상시킬 수 있으니, 꼭 챙겨서 활용해보시길 바랍니다!

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
<meta name="custom" content="meta1" /> <meta name="custom" content="meta2" />
```

## 지원하지 않는 메타데이터 유형

아래 메타데이터 타입들은 현재 Next.js에서 내장 지원을 하지 않아요. 하지만 그렇다고 아예 사용할 수 없는 건 아니에요. 이런 메타 태그들은 레이아웃(layout)이나 개별 페이지 내부에서 직접 렌더링할 수 있습니다.

| 메타데이터                    | 권장 방법                                                                                       |
|------------------------------|------------------------------------------------------------------------------------------------|
| `<meta http-equiv="...">`     | `<meta http-equiv>`는 HTTP 헤더 역할을 하는데, 여기서는 직접 태그 사용보다는 <a href="/docs/app/api-reference/functions/redirect"><code>redirect()</code></a>, <a href="/docs/app/building-your-application/routing/middleware#nextresponse">미들웨어</a>, <a href="/docs/app/api-reference/config/next-config-js/headers">보안 헤더 설정</a> 같은 방법으로 대응하는 걸 권장해요. |
| `<base>`                     | `<base>` 태그는 레이아웃이나 페이지 컴포넌트 안에서 직접 렌더링하세요.                                |
| `<noscript>`                 | `<noscript>` 태그 역시 레이아웃이나 페이지 내에서 렌더링하는 게 좋아요.                              |
| `<style>`                   | Next.js에서 스타일을 적용하는 방법에 대해선 <a href="/docs/app/building-your-application/styling/css">여기</a>서 확인해보세요.                         |
| `<script>`                  | 스크립트 사용에 관한 자세한 내용은 <a href="/docs/app/building-your-application/optimizing/scripts">이 문서</a>에서 다룹니다.                          |
| `<link rel="stylesheet" />`  | 스타일시트는 레이아웃 또는 페이지에서 직접 `import` 하여 적용하는 것을 권장합니다.                    |
| `<link rel="preload" />`     | React에서 preload를 사용하려면 <a href="#link-relpreload">ReactDOM preload 메서드</a>를 활용하세요.           |
| `<link rel="preconnect" />`  | 비슷하게 preconnect가 필요하면 <a href="#link-relpreconnect">ReactDOM preconnect 메서드</a>를 사용하세요.      |
| `<link rel="dns-prefetch" />`| DNS 미리 조회가 필요할 때는 <a href="#link-reldns-prefetch">ReactDOM prefetchDNS 메서드</a>를 참고하면 됩니다.     |

---

### 조금 더 쉽게 이해하기

Next.js가 내부적으로 지원하지 않는 태그들은 꼭 페이지나 레이아웃 컴포넌트에서 직접 넣어야 한다고 기억해 주세요. 그리고 `<meta http-equiv>` 같은 헤더 관련 태그들은 서버 설정이나 Next.js의 자체 헤더 설정 기능으로 대체하는 편이 훨씬 안정적이고 최적화된 방법입니다.

또한 CSS나 스크립트 같은 리소스들은 `import`나 Next.js에서 권장하는 관리 방식을 따르는 게 가장 좋은데요, 이렇게 하면 빌드 과정에서 최적화가 자연스럽게 따라와서 성능에 큰 도움이 된답니다.

저도 처음에는 `<style>`이나 `<script>`, `<meta>` 태그 작성하는 게 익숙치 않았는데, 공식 문서들을 보면서 Next.js 스타일에 맞게 사용하는 방법을 익히니 프로젝트 관리가 훨씬 편해졌어요. 여러분도 Next.js와 함께라면 점점 더 깔끔하고 빠른 웹 앱을 만들 수 있을 거예요!

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

### 리소스 힌트(Resource hints)란?

웹 페이지를 빠르게 불러오기 위해 브라우저에 외부 리소스가 곧 필요할 거라는 신호를 줄 수 있어요. 이때 사용하는 게 바로 `<link>` 태그의 `rel` 속성 키워드들이에요. 예를 들어, `preload`, `preconnect`, `dns-prefetch` 같은 것들이 있죠. 브라우저는 이 힌트를 받으면 미리 리소스를 준비해서 페이지 로딩 속도를 확 끌어올려준답니다.

그런데 보통 이런 힌트는 메타데이터 API에 직접 포함하기 어려워요. 하지만 React에서 새롭게 제공하는 ReactDOM 메서드를 통해 문서의 `<head>`에 안전하게 이 힌트를 삽입할 수 있어요. 아래처럼 사용하면 되죠!

```jsx
'use client'

import ReactDOM from 'react-dom'

export function PreloadResources() {
  ReactDOM.preload('https://example.com/script.js', { as: 'script' })        // 스크립트 미리 로드
  ReactDOM.preconnect('https://fonts.googleapis.com', { crossOrigin: 'anonymous' }) // 사전에 서버 연결
  ReactDOM.prefetchDNS('https://cdn.example.com')                          // DNS 조회 미리하기

  return null
}
```

#### 각 메서드가 하는 일 정리

| 메서드             | 역할 설명                           | 자주 쓰는 용도                         |
|------------------|---------------------------------|---------------------------------|
| `ReactDOM.preload`    | 해당 리소스를 미리 다운로드해서 준비 | 스크립트, 스타일시트, 폰트 선로딩           |
| `ReactDOM.preconnect` | 리소스를 제공하는 서버와 미리 연결       | 외부 폰트, API 서버 등 네트워크 연결 앞당기기    |
| `ReactDOM.prefetchDNS`| 도메인 이름 해석(DNS 조회)을 미리해서 속도 향상 | 외부 CDN, API 서버 DNS 조회 시간 단축         |

---

### 추가 팁!

- **preload**는 실제 리소스를 미리 다운받기 때문에, 정말 필요한 리소스에만 적용하는 게 좋아요. 너무 많이 사용하면 오히려 네트워크에 부담이 될 수 있거든요.
- **preconnect**와 **prefetchDNS**는 리소스 요청 자체보다는 연결을 준비하는 단계라서 비용이 적은 편이에요. 많이 사용해도 크게 문제없지만 무조건 남발하는 건 피하는 게 좋아요.
- React 18부터 ReactDOM에 이런 메서드가 생기면서, 서버 컴포넌트나 클라이언트 컴포넌트에서 동적으로 리소스 선로딩을 관리하기 훨씬 편해졌답니다.

리소스 최적화가 생각보다 쉽지 않은데, 이런 기능으로 페이지 속도를 한층 끌어올려 보세요! 😊

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

웹 페이지를 렌더링하는 동안 리소스를 미리 불러오는 방법에 대해 알아볼게요. 조금 더 빠르게 페이지를 띄우고 싶을 때 유용한 팁입니다!

---

### React에서 리소스 미리 불러오기

React에서 `ReactDOM.preload` 함수를 사용하면, 특정 자원을 미리 로드할 수 있어요.

```js
ReactDOM.preload(href: string, options: { as: string })
```

- `href`: 미리 불러올 리소스 URL
- `options.as`: 리소스 타입(ex. `script`, `style`, `image` 등)

이렇게 하면 해당 리소스가 실제로 필요할 때보다 훨씬 일찍 다운로드가 시작돼서, 렌더링 속도가 빨라져요.

---

### HTML `<link rel="preload">` 태그

브라우저 표준으로 미리 불러오기 할 때는 `<link>` 태그를 사용해요.

```html
<link rel="preload" href="..." as="..." />
```

- `href`: 리소스 주소
- `as`: 불러올 리소스 종류(ex. `script`, `style`, `image`, `font` 등)

이 방법도 리소스를 빠르게 준비시키는 데 효과적입니다.

---

### 추가 팁: preconnect

리소스를 미리 불러오기 전에, 네트워크 연결부터 미리 열어두는 방법도 있어요. `preconnect`를 사용하면 DNS 조회나 TLS 핸드쉐이크 같은 초기 연결 과정을 미리 수행해서 연결 시간을 줄일 수 있답니다.

```html
<link rel="preconnect" href="https://example.com" />
```

이렇게 하면 브라우저가 해당 도메인과의 초기 연결을 미리 처리해줘서, 리소스를 실제로 요청할 때 더 빠르게 받을 수 있어요.

---

### 마무리

- **preload**: 리소스 자체를 미리 다운로드 시작
- **preconnect**: 해당 도메인과의 네트워크 연결을 미리 시작

둘 다 적절히 활용하면, 페이지 로딩 시간 단축에 큰 도움이 돼요. 특히, 외부 CDN에서 스크립트나 폰트, 이미지 같은 무거운 리소스를 불러올 때 효과가 좋으니 참고하세요!

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

여러분, 웹 페이지를 좀 더 빠르게 로딩하고 싶을 때 사용할 수 있는 기술 중에 '프리커넥트(preconnect)'와 '프리페치(prefetch)'가 있어요. 이번 글에서는 React에서 해당 기능을 어떻게 활용할 수 있는지 간단하게 정리해볼게요.

---

### 1. Preconnect (도메인 미리 연결하기)

웹 브라우저가 리소스를 요청하기 전에 DNS 조회, TCP 핸드셰이크, TLS 협상 등을 미리 처리하도록 하는 기능입니다. 쉽게 말해서 "아, 곧 여기서 리소스를 받아올 거니까 미리 연결 준비 해둘게!"라는 거죠.

HTML에서는 보통 이렇게 사용해요:

```html
<link rel="preconnect" href="https://example.com" crossorigin />
```

React에서는 다음과 같이 사용할 수 있습니다:

```js
ReactDOM.preconnect('https://example.com', { crossOrigin: 'anonymous' });
```

`crossOrigin` 옵션은 리소스가 CORS 정책에 따라 로드돼야 할 때 설정해주면 되고, 보통 `anonymous`나 `use-credentials` 중 하나를 골라 사용합니다.

---

### 2. Prefetch DNS (도메인 이름 해석 미리 하기)

이건 조금 더 가볍게 DNS 조회만 미리 해두는 기능이에요. 브라우저가 도메인 주소를 미리 해석해서 나중에 리소스를 요청할 때 시간을 절약할 수 있죠.

React에서 이렇게 사용 가능해요:

```js
ReactDOM.prefetchDNS('https://example.com');
```

---

### 👉 왜 이런 기능들을 써야 할까?

- **로딩 속도 개선**: 외부 리소스(이미지, 폰트, 스크립트 등)를 불러올 때 필요한 연결 과정을 미리 끝내서 사용자 체감 속도를 높일 수 있어요.
- **경쟁 상태 완화**: 여러 리소스를 동시에 요청하는 상황에서 연결 비용을 줄여서 네트워크 경합을 줄여줍니다.

---

### 참고로 알아두면 좋은 점!

- `preconnect`는 HTTPS, 크로스오리진 리소스에서 특히 효과적이에요.
- 너무 많이 쓰면 오히려 오버헤드가 발생할 수 있으니 꼭 필요한 도메인에만 적용하세요.
- `prefetchDNS`는 프리커넥트보다는 가벼운 작업입니다. 가능하면 `preconnect`를 먼저 고려하세요.

---

요즘은 React 자체에서 이런 네트워크 최적화 기능을 간편하게 지원해 준다는 점이 참 편리하고, 서버 사이드 렌더링(SSR) 환경이나 Next.js 같은 프레임워크에서도 적절히 활용하면 더욱 좋습니다.

다음에는 실제 프로젝트에 적용하면서 생길 수 있는 팁이나 주의사항도 공유할게요! 궁금한 점 있으면 댓글로 남겨주세요~ 😊

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
<link rel="dns-prefetch" href="..." />
```

> 알아두면 좋은 점:  
이 메서드들은 현재 클라이언트 컴포넌트에서만 지원됩니다. 하지만 초기 페이지 로드는 여전히 서버 사이드 렌더링으로 이루어집니다.  
Next.js에 내장된 기능들(예: next/font, next/image, next/script)은 관련 리소스 힌트를 자동으로 처리해주니 참고하세요.

## 타입 종류

메타데이터에 타입 안정성을 추가하고 싶다면 `Metadata` 타입을 사용할 수 있습니다.  
IDE에서 내장된 TypeScript 플러그인을 사용 중이라면 타입을 따로 명시할 필요는 없지만, 명시적으로 추가하고 싶다면 그렇게 해도 괜찮습니다.

---

### 추가 팁!  
`dns-prefetch`는 브라우저가 DNS 조회를 미리 해두게 해서 첫 요청 시간을 줄여주는 역할이에요.  
외부 리소스를 많이 사용하는 페이지에서는 이걸 적절히 활용하면 로딩 속도 체감에 큰 도움이 됩니다!  
그리고 Next.js가 자동으로 대부분 처리해주긴 하지만, 커스텀 도메인이나 특수한 경우에는 직접 넣어줘야 할 수도 있어요.  

---

필요하면 저도 다음 글에서 `preconnect`, `prefetch`, `preload` 같은 리소스 힌트 비교와 함께 실제 사용법도 자세히 다뤄볼게요!

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

### metadata 객체

```js
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'Next.js',
}
```

Next.js에서 페이지에 메타데이터를 설정할 때 `metadata` 객체를 이렇게 정의할 수 있어요. 예를 들어, `title`을 지정해서 브라우저 탭에 표시되는 제목을 쉽게 바꿀 수 있죠. 정적으로 고정된 메타데이터가 필요할 때 주로 활용합니다.

### generateMetadata 함수

#### 일반 함수(Regular function)

generateMetadata 함수는 동적으로 메타데이터를 생성할 때 사용하는데, 페이지가 렌더링되기 전에 호출돼서 필요한 정보를 가져오거나 계산해서 메타데이터를 만들 수 있답니다. 예를 들어, API에서 데이터를 불러와서 제목이나 설명을 동적으로 넣을 때 유용하죠.

```js
import type { Metadata } from 'next'
import type { Params } from 'next/navigation'

export async function generateMetadata({ params }: Params): Promise<Metadata> {
  const data = await fetch(`https://api.example.com/posts/${params.id}`)
  const post = await data.json()

  return {
    title: post.title,
    description: post.summary,
  }
}
```

> 이렇게 하면 각 게시글마다 고유한 제목과 설명을 메타데이터에 넣을 수 있어 SEO에도 좋아요!

> 참고로, `generateMetadata`는 페이지가 서버에서 렌더링될 때만 동작하기 때문에, 클라이언트 사이드에서 동적으로 변경하는 메타데이터와는 목적이 조금 다르다는 점 기억해 주세요.

요약하자면:  
- **metadata 객체**는 정적인 메타데이터용  
- **generateMetadata 함수**는 동적으로 메타데이터를 생성할 때 사용  

다음에는 `generateMetadata`의 화살표 함수 버전이나 비동기 처리에 대해 또 얘기해볼게요!

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

Next.js에서 `generateMetadata` 함수는 페이지 메타데이터(예: 제목, 설명 등)를 정의할 때 사용돼요. 이 함수는 동기 혹은 비동기 형태로 작성할 수 있는데, 어떤 차이가 있는지 한 번 살펴볼게요.

---

## 기본 동기 함수

```js
import type { Metadata } from 'next'
 
export function generateMetadata(): Metadata {
  return {
    title: 'Next.js',
  }
}
```

여기서는 `generateMetadata`를 동기 함수로 정의했어요. 단순히 객체를 반환하니까, 렌더링 과정에서 바로 메타 데이터를 사용할 수 있죠. 페이지 제목만 간단히 설정할 때 유용해요.

---

## 비동기 함수 (Async)

```js
import type { Metadata } from 'next'
 
export async function generateMetadata(): Promise<Metadata> {
  return {
    title: 'Next.js',
  }
}
```

메타데이터 생성에 API 호출이나 DB 조회 같은 비동기 작업이 필요할 땐, async 함수로 정의할 수 있어요. Promise를 반환하면 Next.js가 알아서 기다려 준답니다.

---

## 세그먼트 Props와 함께 사용하기

Next.js에서는 라우트 세그먼트(예: `[slug]` 같은 동적 경로)의 정보를 `generateMetadata`에 넘겨줄 수 있어요. 이를 통해 각 페이지마다 맞춤 메타데이터 생성이 가능하죠.

```ts
import type { Metadata } from 'next'

type Params = {
  params: {
    slug: string
  }
}

export function generateMetadata({ params }: Params): Metadata {
  return {
    title: `Post - ${params.slug}`,
  }
}
```

이렇게 하면 URL 경로에 따라 적절한 제목을 동적으로 설정할 수 있어 편리합니다.

---

### 요약 📌

| 형태            | 특징                          | 사용 상황                       |
|-----------------|-----------------------------|------------------------------|
| 동기 함수       | 즉시 메타데이터 반환           | 간단한 메타데이터 설정           |
| 비동기 함수     | Promise 반환, await 가능       | API 호출 등 비동기 로직 필요 시  |
| 세그먼트 포함    | 라우트 파라미터 활용 가능       | 동적 경로에 따라 메타데이터 달리할 때 |

---

Next.js에서 메타데이터를 유연하게 다루는 방법, 어렵지 않죠? 페이지마다 맞춤형 SEO 설정을 할 때 이 기능을 꼭 활용해 보세요! 필요하면 next.js 공식 문서도 참고하면 도움이 많이 될 거예요. 😊

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

요즘 Next.js에서 메타데이터를 다루는 방법이 많이 바뀌었죠? 이번 글에서는 Next.js에서 메타데이터를 생성하고 사용하는 기본적인 방법과, 부모 메타데이터를 활용하는 방법까지 간단하게 살펴볼게요.

---

## 기본 메타데이터 생성하기

아래 코드를 보시면 `generateMetadata`라는 함수를 정의해서 메타데이터를 리턴하고 있어요.

```js
import type { Metadata } from 'next'
 
type Props = {
  params: Promise<{ id: string }>
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>
}
 
export function generateMetadata({ params, searchParams }: Props): Metadata {
  return {
    title: 'Next.js',
  }
}
 
export default function Page({ params, searchParams }: Props) {}
```

여기서 중요한 점은:

- `generateMetadata` 함수가 `Metadata` 타입 객체를 반환한다는 것
- 이 메타데이터는 페이지 렌더링 시 Next.js가 자동으로 HTML `<head>`에 넣어준다는 것

`params`나 `searchParams`를 사용해 동적으로 메타데이터를 바꿀 수도 있어요.

---

## 부모 메타데이터와 합치기

Next.js는 중첩된 레이아웃이나 페이지 구조를 지원하기 때문에, 상위 레이아웃에서 정의한 메타데이터와 병합해서 사용하는 경우가 많죠. 이런 경우엔 `generateMetadata` 함수에서 `parent`라는 인자를 받아서 처리할 수 있어요.

```js
import type { Metadata, ResolvingMetadata } from 'next'
 
export async function generateMetadata(
  { params, searchParams }: Props,
  parent: ResolvingMetadata
): Promise<Metadata> {
  return {
    title: 'Next.js',
  }
}
```

- `parent`는 상위 레이아웃이 전달하는 메타데이터를 포함한 객체라서, 이 데이터를 참고하거나 확장해서 메타데이터를 만들 수 있어요.
- 보통 `async` 함수로 작성하는데, `parent` 메타데이터를 비동기로 받아오기 때문이에요.

---

## 자바스크립트 프로젝트에서 활용하기

타입스크립트를 사용하지 않는 자바스크립트 프로젝트에서도 거의 같은 방식으로 사용할 수 있어요. 타입 선언 부분을 제외하면 되고, `generateMetadata` 함수만 잘 활용하면 되죠.

```js
export async function generateMetadata({ params, searchParams }, parent) {
  return {
    title: 'Next.js',
  }
}
```

---

### 추가 팁!

- 메타데이터는 `title` 뿐만 아니라 `description`, `openGraph`, `icons` 등 다양한 속성을 설정할 수 있어요.
- Next.js 13 이상부터 공식 지원하는 메타데이터 API라서, SEO나 SNS 공유 시 훨씬 효율적이에요.
- 페이지나 레이아웃 단위로 메타데이터를 구분해서 작성하면 유지보수가 편해지니 꼭 활용해 보세요.

---

| 구분            | 설명                                                      |
| --------------- | --------------------------------------------------------- |
| `generateMetadata` | 페이지나 레이아웃에서 메타데이터를 생성하는 함수               |
| `params`        | URL 파라미터 정보를 담고 있음                               |
| `searchParams`  | URL 쿼리스트링 정보를 담고 있음                             |
| `parent`        | 상위 레이아웃의 메타데이터 정보를 포함하는 객체 (비동기 가능) |

---

다음번에는 메타데이터 속성별 구체적 예시와 SEO에 어떻게 활용할 수 있는지 알려드릴게요. 필요하면 궁금한 점 편하게 질문해주세요! 😄

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

JavaScript 프로젝트에서 타입 안정성을 높이고 싶을 때, JSDoc을 활용할 수 있어요.

```js
/** @type {import("next").Metadata} */
export const metadata = {
  title: 'Next.js',
}
```

위 예시는 Next.js 프로젝트에서 메타데이터 객체에 타입을 지정하는 방법이에요. 이렇게 하면 에디터에서 자동 완성도 지원되고, 실수로 메타데이터 키를 잘못 작성하는 실수를 줄일 수 있답니다.

---

## 스트리밍 메타데이터(Streaming metadata)

Next.js에서 `generateMetadata` 함수가 반환하는 메타데이터는 클라이언트에게 스트리밍 방식으로 전송돼요. 즉, 메타데이터가 준비되는 즉시 HTML에 주입되기 때문에 페이지 로딩 속도나 SEO 측면에서 유리하답니다.

이 방식 덕분에 페이지가 완전히 렌더링되기 전에 메타데이터가 미리 전달되면서 브라우저와 검색엔진이 올바른 정보를 빠르게 인식할 수 있어요.

추가로, 스트리밍 방식 덕분에 대규모 프로젝트에서 메타데이터 처리도 효율적으로 수행되니, Next.js를 사용할 때 꼭 기억해두면 좋은 팁입니다!

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

페이지 메타데이터는 주로 봇이나 크롤러를 대상으로 하기 때문에, Next.js는 JavaScript를 실행하고 전체 페이지 DOM을 검사할 수 있는 봇(예: Googlebot)에 대해 메타데이터를 스트리밍 방식으로 제공합니다. 하지만 메타데이터가 없으면 안 되는 HTML 제한 봇(예: Twitterbot)에게는 여전히 페이지 렌더링을 막는 블로킹 방식으로 메타데이터를 제공하게 됩니다. 이런 봇들은 크롤링할 때 JavaScript를 실행할 수 없기 때문이죠.

Next.js는 자동으로 들어오는 요청의 유저 에이전트(User Agent)를 감지해서 스트리밍 메타데이터를 줄지, 아니면 블로킹 메타데이터를 줄지 판단합니다.

그런데, 혹시 Next.js가 기본으로 인식하는 봇 리스트 외에 내가 원하는 봇을 따로 지정하고 싶다면, next.config.js 파일 안에 `htmlLimitedBots` 옵션을 써서 직접 정할 수 있어요. 이 옵션에 정규표현식 패턴으로 유저 에이전트를 지정하면, 일치하는 봇들은 페이지를 요청할 때 무조건 블로킹 메타데이터를 받게 됩니다.

예를 들어, `MySpecialBot`, `MyAnotherSpecialBot`, `SimpleCrawler`라는 봇을 직접 지정하려면 이렇게 작성하면 됩니다:

```js
import type { NextConfig } from 'next'

const config: NextConfig = {
  htmlLimitedBots: 'MySpecialBot|MyAnotherSpecialBot|SimpleCrawler',
}

export default config
```

### 추가로 알아두면 좋은 점!
- 이 기능 덕분에 트위터 같은 JavaScript를 실행하지 못하는 봇들이 페이지 메타데이터를 제대로 받아갈 수 있어, 공유 시에 미리보기 정보가 깨지지 않아요.
- `htmlLimitedBots` 옵션에 너무 많은 봇을 넣으면 서버 리소스가 더 소모될 수 있으니 꼭 필요한 봇만 추가하는 게 좋습니다.
- Next.js 13 이상의 App Router에서 적용되는 중요한 설정이며, 메타데이터 처리 최적화를 통해 SEO와 소셜 공유 품질을 높이고 싶을 때 활용해보세요!

필요한 봇 패턴을 유연하게 커스터마이징해서, 내 웹사이트가 다양한 봇 환경에서도 깔끔하게 잘 작동하도록 만드는 팁이었습니다 :)

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

htmlLimitedBots 설정을 지정하면 Next.js의 기본 유저 에이전트(user agent) 리스트를 덮어쓰게 되어, 어떤 봇이 이 동작을 선택할지 완전히 제어할 수 있습니다. 이 기능은 다소 고급스러운 설정이므로, 대부분의 경우 기본값으로도 충분합니다.

아래는 버전별 주요 변경사항입니다:

| 버전       | 변경사항                                                                                                                   |
|------------|----------------------------------------------------------------------------------------------------------------------------|
| v15.2.0    | `generateMetadata`에 스트리밍 지원이 도입되었습니다.                                                                          |
| v13.2.0    | `viewport`, `themeColor`, `colorScheme` 설정이 더 이상 권장되지 않고, 대신 [viewport 구성](https://nextjs.org/docs/app/api-reference/functions/generate-viewport) 방식으로 대체되었습니다.  |
| v13.2.0    | `metadata`와 `generateMetadata` 기능이 새롭게 도입되었습니다.                                                                 |

덧붙여서, Next.js에서 메타데이터 관련 기능들이 점점 세분화되고 발전하면서, 메타데이터를 동적으로 생성하거나 스트리밍하는 기능들도 지원하고 있어 SEO 최적화나 성능 향상에 도움을 줄 수 있답니다. 앞으로 최신 Next.js 기능을 다룰 때는 이런 변화들을 잘 활용해보시는 걸 추천드려요!