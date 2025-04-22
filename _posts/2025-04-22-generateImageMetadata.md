---
title: "Next.js 15에서 generateImageMetadata 함수 사용하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 03:04
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "generateImageMetadata"
link: "https://nextjs.org/docs/app/api-reference/functions/generate-image-metadata"
isUpdated: false
---


# generateImageMetadata 함수 소개

여러분, 이미지 메타데이터를 다룰 때 직접 하나하나 값을 하드코딩하기 귀찮거나, 여러 버전의 이미지를 한꺼번에 처리하고 싶었던 적 있으신가요? 바로 그럴 때 `generateImageMetadata` 함수를 활용하면 정말 편리해요!

이 함수는 하나의 이미지를 다양한 버전으로 만들어내거나, 한 라우트 세그먼트에 여러 이미지를 반환할 수 있도록 도와줘요. 특히 아이콘 같이 자주 쓰이면서도 크기나 형식이 달라지는 이미지 작업에 아주 유용하답니다.

## 파라미터 (Parameters)

`generateImageMetadata` 함수에 넘겨줄 수 있는 주요 파라미터들은 아래와 같아요:

| 파라미터 이름 | 설명 |
| --- | --- |
| `src` | 기본 이미지 경로 또는 URL (필수) |
| `sizes` | 생성할 이미지 크기 배열 (예: `[16, 32, 64]`) |
| `formats` | 생성할 이미지 형식 배열 (예: `['png', 'webp']`) |
| `outputDir` | 변환 이미지가 저장될 디렉터리 경로 |
| `quality` | 이미지 압축 품질 (0~100) |
| `background` | 배경색 설정 (특히 투명하지 않은 이미지에 유용) |

위 내용은 가장 기본적이고 자주 사용하는 옵션들을 포함하고 있어요. 이외에도 상황에 따라 추가 설정이 가능하니 공식 문서를 참고하면 더 깊이 이해할 수 있습니다.

---

### 실무에서 이렇게 활용해보세요!

1. **아이콘 다양화**  
   앱이나 웹사이트 내에서 동일한 아이콘을 크기별, 포맷별로 자동 생성해서 사용하면 코드가 더 깔끔해지고, 다양한 기기 대응이 쉬워져요.

2. **SEO와 성능 최적화**  
   다양한 해상도와 포맷(WebP 등)을 지원하면 사용자의 네트워크 환경이나 브라우저에 맞춰 최적화된 이미지를 제공할 수 있어, 페이지 로딩 속도와 SEO에 도움이 됩니다.

다음번에는 이 함수를 실제 코드 예제와 함께 '어떻게 사용하는지' 자세히 다뤄볼게요! 질문이나 궁금한 점 있으면 언제든 댓글 남겨주세요 :)

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

#### params (선택 사항)

`params`는 동적 라우트 매개변수를 포함하는 객체인데요, 이 객체는 루트 세그먼트에서부터 `generateImageMetadata` 함수가 호출된 세그먼트까지의 매개변수를 담고 있어요.

예를 들어, 아래처럼 `generateImageMetadata` 함수에 `params`를 전달받을 수 있습니다.

```js
export function generateImageMetadata({
  params,
}: {
  params: { slug: string }
}) {
  // ...
}
```

이때, 라우트와 URL, 그리고 `params`의 예시는 다음 표처럼 이해하시면 편해요.

| Route                       | URL        | params              |
|-----------------------------|------------|---------------------|
| `app/shop/icon.js`           | `/shop`    | `undefined`         |
| `app/shop/[slug]/icon.js`    | `/shop/1`  | `{ slug: '1' }`     |
| `app/shop/[tag]/[item]/icon.js` | `/shop/1/2` | `{ tag: '1', item: '2' }` |

즉, URL 경로에 따라 `params` 객체 안에 전달되는 키와 값이 달라지는 거죠.

참고로, Next.js 같은 프레임워크에서는 이 `params`를 통해 동적으로 페이지를 구성할 때 유용하게 쓰여요. 특히 여러 단계의 동적 세그먼트가 있을 경우, 이 객체를 활용해 필요한 데이터를 가져오거나 메타데이터를 생성할 수 있답니다.

또 한 가지 팁을 드리자면, `params`가 `undefined`인 경우도 있으니 함수 내에서 안전하게 처리해주는 게 좋아요! 예를 들면 다음과 같이요:

```js
export function generateImageMetadata({ params }) {
  const slug = params?.slug ?? 'default';
  // slug를 안전하게 사용
}
```

이렇게 하면 예상치 못한 에러를 방지할 수 있답니다!

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

## 리턴값 설명

`generateImageMetadata` 함수는 이미지 메타데이터를 담은 객체들의 배열을 반환해야 합니다. 여기서 각 객체는 이미지의 대체 텍스트(`alt`), 크기(`size`) 같은 정보를 포함하고 있어요. 그리고 꼭 포함해야 할 값 중 하나가 `id`인데, 이 값은 이미지 생성 컴포넌트에서 props로 전달받게 됩니다.

아래는 반환하는 객체 구조를 간단히 정리한 표예요:

| Image Metadata Object | Type                      |
|----------------------|---------------------------|
| `id`                 | `string` (필수)            |
| `alt`                | `string`                   |
| `size`               | `{ width: number; height: number }` |
| `contentType`        | `string`                   |

### 예제 코드

```js
import { ImageResponse } from 'next/og'
 
export function generateImageMetadata() {
  return [
    {
      contentType: 'image/png',
      size: { width: 48, height: 48 },
      id: 'small',
    },
    {
      contentType: 'image/png',
      size: { width: 72, height: 72 },
      id: 'medium',
    },
  ]
}
 
export default function Icon({ id }: { id: string }) {
  return new ImageResponse(
    (
      <div
        style={{
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
          fontSize: 88,
          background: '#000',
          color: '#fafafa',
        }}
      >
        Icon {id}
      </div>
    )
  )
}
```

---

### 조금 더 설명을 덧붙이자면

- `generateImageMetadata` 함수는 이미지가 다양할 때 각각에 맞는 메타정보를 한꺼번에 관리할 수 있어 편리합니다.
- 각 이미지별 `id`는 컴포넌트에서 어떤 이미지를 렌더할지 구분하는 역할을 해요. 이 `id`를 이용해 동적으로 이미지 내용을 바꾸거나 다르게 스타일링할 수 있습니다.
- `contentType`은 보통 이미지 포맷을 지정하는데, 예를 들어 `image/png`나 `image/jpeg` 등을 쓸 수 있어요.
- `alt` 값은 UI 접근성을 높이기 위해 넣으면 좋습니다. 이미지 로딩 실패 시나 스크린 리더 사용자의 이해를 도와주니까요.

그리고 스타일링 부분에서, 인라인 스타일에 중괄호 `{}`를 빼먹을 수 있는데 꼭 유의하세요! 예제 코드처럼 객체형태로 잘 감싸야 합니다.

필요에 따라 더 다양한 메타데이터를 포함시켜 유연하게 확장하는 것도 추천드려요~!

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

### 예제

#### 외부 데이터를 활용한 이미지 생성

이번 예제에서는 `params` 객체와 외부 데이터를 사용해 라우트 세그먼트에 대해 여러 개의 Open Graph 이미지를 생성하는 방법을 보여드릴게요.

```js
import { ImageResponse } from 'next/og'
import { getCaptionForImage, getOGImages } from '@/app/utils/images'

export async function generateImageMetadata({
  params,
}: {
  params: { id: string }
}) {
  const images = await getOGImages(params.id)

  return images.map((image, idx) => ({
    id: idx,
    size: { width: 1200, height: 600 },
    alt: image.text,
    contentType: 'image/png',
  }))
}

export default async function Image({
  params,
  id,
}: {
  params: { id: string }
  id: number
}) {
  const productId = (await params).id
  const imageId = id
  const text = await getCaptionForImage(productId, imageId)

  return new ImageResponse(
    (
      <div
        style={
          {
            // ...
          }
        }
      >
        {text}
      </div>
    )
  )
}
```

---

위 예제는 `params.id`를 기반으로 여러 Open Graph 이미지를 생성해주는 방식이에요. 먼저 `generateImageMetadata` 함수에서 외부 유틸 함수 `getOGImages`를 호출해 이미지 메타데이터들을 불러옵니다. 그리고 각각의 이미지를 원하는 크기와 alt 텍스트, 콘텐츠 타입과 함께 반환하죠.

또한, 이미지 생성 함수 `Image`에서는 각각의 이미지 ID와 제품 ID를 받아서 해당 이미지에 들어갈 텍스트 캡션을 외부에서 가져와 렌더링합니다.

> 참고로 이런 방식을 사용하면 한 가지 라우트에 여러 OG 이미지를 동적으로 생성할 수 있어서, SNS에서 링크를 공유할 때 다양한 시각적 표현이 가능해집니다.  
>  
> 그리고 `ImageResponse`는 Next.js의 `next/og` 모듈에서 제공하는 기능으로, JSX를 기반으로 이미지를 그릴 수 있게 해줘요. 평소 HTML이나 React를 다루는 것처럼 편하게 이미지 레이아웃도 조절 가능하니 참 편리합니다.

여기에 좀 더 스타일을 입히거나, 이미지 안에 다른 컴포넌트나 아이콘, 배경색 등을 넣어서 더욱 멋진 OG 이미지를 만들 수도 있겠네요. 이렇게 동적으로 메타데이터와 이미지를 생성하는 것은 SEO뿐만 아니라, 사용자 경험 향상에도 크게 도움을 줄 수 있습니다!

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

## 버전 히스토리

| 버전      | 변경 사항                       |
|-----------|-------------------------------|
| v13.3.0   | `generateImageMetadata` 기능 추가 |

여기서 `generateImageMetadata`라는 기능이 새롭게 도입된 걸 확인할 수 있어요. 혹시 이미지 처리나 메타데이터 자동 생성과 관련된 작업을 한다면, 이 기능이 꽤 유용할 거예요. 나중에 직접 사용해보고 어떤 점이 좋은지, 어떻게 활용할 수 있는지도 다뤄볼게요!