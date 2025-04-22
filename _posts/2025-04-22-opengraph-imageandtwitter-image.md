---
title: "Next.js 15에서 OpenGraph-Image와 Twitter-Image 설정하는 방법 "
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:51
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "opengraph-image and twitter-image"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image"
isUpdated: false
---


# opengraph-image와 twitter-image 파일 컨벤션

웹 개발할 때, 내가 만든 페이지를 누군가 SNS나 메신저에서 공유할 때 보여지는 썸네일 이미지, 한 번쯤 신경 써봤죠? 이때 중요한 게 바로 Open Graph 이미지와 Twitter 카드 이미지입니다. 이걸 제대로 설정해주면 공유할 때 내 콘텐츠가 더 눈에 띄고 프로페셔널해 보여요.

여기서 소개할 opengraph-image와 twitter-image 파일 컨벤션은, 특정 라우트(경로)별로 이런 이미지를 쉽게 설정하는 방법이에요. 즉, 페이지별로 다르게 썸네일을 지정할 수 있다는 뜻이죠.

---

## Open Graph와 Twitter 이미지 설정 방법

개발자나 블로거들이 흔히 쓰는 방법은 두 가지예요:

(아래에서 자세하게 설명할게요!)

---

> 참고: Open Graph(Open Graph Protocol)는 페이스북, 카카오톡 같은 소셜 미디어가 공유 링크를 표시할 때 사용하는 메타 데이터 규격이에요.  
> Twitter 카드도 이름 그대로 트위터에서 공유 링크 시 보이는 카드형 이미지나 요약 정보를 위한 메타 태그입니다.

다음 메시지에서 두 가지 방법에 대해서 더 자세히 정리해드릴게요!

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

개발자 분들, 안녕하세요! 오늘은 Next.js에서 이미지를 다루는 방법에 대해 이야기해볼게요. 특히, 라우트 세그먼트(route segment)를 위한 오픈그래프(Open Graph) 이미지 설정하는 방법에 집중해보겠습니다.

---

## 이미지 파일로 이미지 설정하기 (.jpg, .png, .gif)

Next.js에서는 라우트 세그먼트에 공유할 이미지를 쉽게 설정할 수 있어요. 방법은 간단히 세그먼트 폴더 안에 `opengraph-image` 또는 `twitter-image`라는 이름의 이미지 파일을 넣어주면 됩니다.

예를 들어, `/about` 페이지에 대한 오픈그래프 이미지를 설정하고 싶다면 `/about/opengraph-image.jpg` 파일을 넣으면 돼요.

그럼 Next.js가 해당 이미지를 자동으로 인식해서, 여러분의 앱 `head` 태그 안에 적절한 메타 태그를 삽입해 줍니다. 즉, `<meta property="og:image" content="...">` 같은 태그들이 자동으로 생성되는 거죠!

이 방식의 장점은 별도의 코드 작업 없이 이미지만 해당 위치에 두면 알아서 처리해준다는 거예요. 덕분에 SEO나 공유하기 좋은 이미지 설정을 간편하게 할 수 있죠.

---

### 참고로, 이미지 파일명과 경로가 정말 중요해요!

- 파일명은 **반드시 정확**하게 `opengraph-image` 또는 `twitter-image` 여야 합니다.
- 지원되는 확장자는 `.jpg`, `.png`, `.gif` 등 표준 이미지 포맷들입니다.
- 여러 이미지 포맷 중에 최적화된 포맷을 선택하는 것도 중요해요. 예를 들어, 투명 배경이 필요하면 PNG, 사진이라면 JPG가 적합합니다.

---

다음에는 이미지 파일을 직접 업로드하지 않고, 자바스크립트 코드로 이미지를 생성하는 방법도 소개해 드릴게요! (예: .js, .ts, .tsx 파일 사용)

그럼 여기까지 알아두시면 라우트별 공유 이미지 설정은 문제없겠네요. 즐코딩! 🚀

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

아래는 파일 네이밍 규칙과 지원하는 파일 형식을 정리한 표입니다. 참고로 파일 크기 제한도 있으니 꼭 확인해주세요!

| File convention            | Supported file types               |
|---------------------------|----------------------------------|
| [opengraph-image](#opengraph-image)       | `.jpg`, `.jpeg`, `.png`, `.gif`         |
| [twitter-image](#twitter-image)           | `.jpg`, `.jpeg`, `.png`, `.gif`         |
| [opengraph-image.alt](#opengraph-imagealttxt) | `.txt`                              |
| [twitter-image.alt](#twitter-imagealttxt)      | `.txt`                              |

> 알아두면 좋은 점:
> - `twitter-image` 파일 크기는 5MB를 넘으면 안 됩니다.
> - `opengraph-image` 파일 크기는 8MB를 넘지 않아야 합니다.
> - 만약 이 크기 제한을 넘으면 빌드가 실패하니 주의하세요!

---

### opengraph-image

`opengraph-image.(jpg|jpeg|png|gif)` 형식의 이미지를 원하는 라우트 세그먼트에 추가할 수 있습니다.

---

추가로, opengraph 이미지와 트위터 이미지 모두 소셜 미디어에서 링크를 공유할 때 미리보기 이미지로 쓰이니, 가능한 한 눈에 띄고 관련성이 높은 이미지로 준비하는 게 좋아요. 그리고 alt 텍스트 파일(`.txt`)을 함께 제공하면 이미지가 잘 로드되지 않을 때 대체 텍스트로 활용할 수 있어서 접근성 측면에서도 도움이 됩니다. 

파일 크기가 크면 로딩 시간이 길어질 수 있으니 이미지 최적화도 꼭 챙기시길 바래요!

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

이 코드는 웹 페이지의 메타 태그를 설정하는 예시로, 특히 SNS 공유 시에 어떤 이미지가 어떻게 표시될지 지정하는 부분이에요. 조금 더 쉽게 설명드릴게요!

### og:image (Open Graph 이미지)

```html
<meta property="og:image" content="<generated>" />
<meta property="og:image:type" content="<generated>" />
<meta property="og:image:width" content="<generated>" />
<meta property="og:image:height" content="<generated>" />
```

- `og:image`: 페이스북, 카카오톡 같은 SNS에서 공유될 때 썸네일로 사용할 이미지 URL이에요.
- `og:image:type`: 이미지의 타입(jpeg, png 등)을 알려주는 태그에요.
- `og:image:width`, `og:image:height`: 이미지의 가로, 세로 크기를 픽셀 단위로 알려줘요.

이걸 잘 설정하면 공유할 때 이미지가 깨지거나 엉뚱한 이미지가 나오는 걸 막을 수 있어요.

---

### twitter-image (트위터 전용 이미지)

```html
<meta name="twitter:image" content="<generated>" />
<meta name="twitter:image:type" content="<generated>" />
<meta name="twitter:image:width" content="<generated>" />
<meta name="twitter:image:height" content="<generated>" />
```

- `twitter:image`: 트위터에서 공유 시 사용할 이미지 URL입니다.
- `twitter:image:type`, `twitter:image:width`, `twitter:image:height`: Open Graph와 비슷하게 이미지 타입과 크기를 알려줘요.

트위터는 트위터 카드라는 시스템이 있는데, 이 태그를 넣어줘야 멋진 카드 형태로 공유할 수 있어요.

---

### 추가 TIP!

- 보통 og 이미지와 twitter 이미지 주소를 동일하게 쓸 때가 많지만, 필요에 따라 다른 이미지를 지정해도 돼요.
- 이미지 권장 크기는 SNS마다 조금씩 다르긴 하지만, 최소한 1200x630(px) 정도를 권장해요. 그래야 고해상도 화면에서도 잘 보여요.
- 이미지 URL은 절대경로(https://... )로 넣어야 하므로 주의하세요!

### 요약

| 태그 종류         | 목적                         | 속성 예시                          |
|-----------------|----------------------------|---------------------------------|
| og:image        | 페이스북, 카카오톡 공유 이미지 | content, type, width, height    |
| twitter:image   | 트위터 공유 이미지           | content, type, width, height    |

이렇게 설정해주면 공유할 때 더 깔끔하고 전문적인 모습으로 보여질 수 있어요! 나중에 내 블로그나 서비스에 SNS 공유 기능 붙일 때 꼭 참고하세요.

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

### opengraph-image.alt.txt 파일 활용법

웹사이트를 운영하다 보면 **Open Graph 이미지**(SNS에 공유할 때 미리보기 이미지)와 함께 그 이미지의 대체 텍스트(alt text)를 정의해야 할 때가 있어요. 대체 텍스트는 이미지가 로드되지 않을 때 보여지거나, 스크린 리더 사용자에게 이미지 내용을 설명해주는 중요한 역할을 하죠.

이때 **opengraph-image.alt.txt** 파일을 사용하는 방법이 있어요. Open Graph 이미지 파일(jpg, jpeg, png, gif)이 있는 폴더 혹은 경로와 같은 위치에 이 텍스트 파일을 넣어서 이미지의 alt 텍스트를 지정할 수 있습니다.

예시를 들어볼게요.

```js
About Acme
```

이렇게 텍스트 파일에 이미지에 대한 설명을 적어두면, HTML 헤더 안에 다음과 같은 메타 태그가 자동으로 생성될 수 있어요.

```html
<meta property="og:image:alt" content="About Acme" />
```

이 메타 태그는 검색 엔진 최적화(SEO)에도 도움이 되고, 접근성(Accessibility) 측면에서도 매우 유용합니다.  
물론, 직접 `<meta>` 태그를 HTML에 넣어줘도 되지만, 파일로 관리하면 여러 이미지의 alt 텍스트를 일괄 관리하기 더 편리해요.

---

**추가 팁!**  
- alt 텍스트는 이미지의 내용을 간결하고 명확하게 설명하는 문장이 좋아요.  
- 너무 길거나 장황하면 오히려 독이 될 수 있으니 적당히 짧고 핵심을 담는 게 중요합니다.  
- Open Graph 태그를 잘 관리하면 페이스북, 트위터, 카카오톡 등 다양한 SNS에서 링크 공유 시 예쁘고 의미 있는 미리보기를 보여줄 수 있습니다.

따라서, `opengraph-image.alt.txt` 파일을 잘 활용해서 더 완성도 높고 친절한 웹 콘텐츠를 만들어보세요!

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

트위터 이미지를 사용할 때 이미지 설명(alt text)을 꼭 챙기는 게 좋아요. 왜냐하면 이미지가 제대로 로딩 안 될 때 사용자에게 대체 텍스트로 보여줄 수 있고, 접근성 측면에서도 도움이 되거든요.

여기서 `twitter-image.alt.txt`라는 파일은 여러분이 올린 트위터 이미지 파일(jpg, png, gif 등)과 같은 폴더(경로)에 두면 돼요. 이 파일 안에는 이미지에 대한 간단한 설명을 담아두는 거죠.

예를 들어, 이미지가 "About Acme"이라는 내용을 담았다면,  
`twitter-image.alt.txt` 안에는 이렇게 적어요:


About Acme


그리고 만약 HTML 메타 태그로 직접 추가할 경우라면 이렇게 쓸 수 있답니다:

```html
<meta property="twitter:image:alt" content="About Acme" />
```

사실 이 작업이 왜 중요하냐면, 트위터 같은 소셜 미디어 플랫폼에서 이미지를 공유할 때 설명이 있으면 더 접근성 높고, 사용자 경험도 좋아져요. 또한 이미지가 어떤 내용인지 검색 엔진 최적화(SEO)에도 조금 도움될 수 있답니다.

간단하지만 놓치기 쉬운 부분, 꼭 챙겨주세요!

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

## 코드로 이미지 생성하기 (.js, .ts, .tsx)

보통 이미지는 이미지 파일을 그대로 사용하는 경우가 많지만, 코드로 이미지를 직접 생성하는 것도 가능합니다.

예를 들어, 특정 라우트 세그먼트에 공유 이미지(opengraph-image나 twitter-image)를 만들고 싶다면, `opengraph-image`나 `twitter-image`라는 이름의 라우트를 만들고 거기서 기본(default)으로 내보내는 함수를 작성하면 돼요.

그리고 아래 표처럼 파일명 컨벤션과 지원하는 파일 타입도 정해져 있습니다:

| 파일명 컨벤션         | 지원하는 파일 타입       |
|--------------------|-------------------|
| `opengraph-image`  | `.js`, `.ts`, `.tsx` |
| `twitter-image`    | `.js`, `.ts`, `.tsx` |

---

### 조금 더 알아두면 좋은 팁!

- 이렇게 코드로 이미지를 생성하면, 동적으로 텍스트, 배경색, 아이콘 등을 상황에 맞게 커스터마이징할 수 있어요.
- 예를 들어 블로그 글마다 다른 오픈그래프 이미지를 자동으로 생성하고 싶을 때 굉장히 유용하죠.
- React 컴포넌트 형식으로 이미지를 만들면서, SVG나 Canvas API를 활용하거나, headless 브라우저에서 캡처하는 방법도 있어요.
  
직접 만들어 보면 생각보다 재미있고 신기하니 한 번 도전해 보세요!

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

좋은 정보!

Next.js에서 이미지를 생성할 때 기본적으로 정적으로 최적화(빌드 시 생성 및 캐시)된다는 점을 알고 있으면 좋아요. 단, Dynamic API나 캐시되지 않는 데이터를 사용하면 동적으로 처리됩니다. 그리고 한 파일 내에서 여러 이미지를 만들고 싶다면 `generateImageMetadata`를 활용할 수 있고요.

특히 `opengraph-image.js`와 `twitter-image.js` 같은 라우트 핸들러(Route Handlers)는 기본적으로 캐시 처리되지만, Dynamic API 또는 다이나믹 설정을 사용하면 예외입니다.

### 가장 간단한 이미지 생성법: ImageResponse API

`next/og` 패키지의 `ImageResponse`를 사용하면 아주 쉽게 이미지 생성이 가능해요. 아래 예제를 한 번 볼게요.

```js
import { ImageResponse } from 'next/og'
import { readFile } from 'node:fs/promises'
import { join } from 'node:path'

// 이미지 메타 데이터
export const alt = 'About Acme'
export const size = {
  width: 1200,
  height: 630,
}

export const contentType = 'image/png'

// 이미지 생성 함수
export default async function Image() {
  // 폰트 불러오기 (process.cwd()는 Next.js 프로젝트 루트 디렉터리)
  const interSemiBold = await readFile(join(process.cwd(), 'assets/Inter-SemiBold.ttf'))

  return new ImageResponse(
    (
      <div
        style={{
          fontSize: 128,
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        About Acme
      </div>
    ),
    {
      ...size,
      fonts: [
        {
          name: 'Inter',
          data: interSemiBold,
          style: 'normal',
          weight: 400,
        },
      ],
    }
  )
}
```

여기서 중요한 점은 직접 폰트를 로드해서 이미지 안에 텍스트를 쓸 때 폰트가 제대로 렌더되도록 하는 거예요. 그리고 스타일을 인라인으로 줘서 기본적인 레이아웃도 잡을 수 있죠.

만들어진 이미지는 Open Graph 혹은 트위터 카드 메타 태그에서 사용할 수 있는데요:

```html
<meta property="og:image" content="<generated>" />
<meta property="og:image:alt" content="About Acme" />
<meta property="og:image:type" content="image/png" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
```

이렇게 넣어주면 SNS에 공유될 때 예쁜 미리보기 이미지가 나타납니다.

---

### 추가 팁!

- 이미지 크기(`width`, `height`)는 SNS 공유용으로 권장되는 크기를 따라야 해요. 보통 1200x630이 표준입니다.
- `ImageResponse`의 JSX 내부에서는 스타일이나 레이아웃도 마음껏 커스터마이징할 수 있으니, 브랜드 컬러나 로고 넣기도 어렵지 않아요.
- 폰트 파일은 프로젝트에 포함시키거나 CDN에서 로드해도 되고, 성능을 위해 필요한 폰트만 정확히 불러오는 게 좋아요.
- 서버 컴포넌트에서만 사용 가능하다는 것도 기억하세요!

이렇게 Next.js의 이미지 생성 기능을 활용하면, SEO와 SNS 공유에서 경쟁력을 크게 높일 수 있습니다. 개발하면서 꼭 한번 써보세요!

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

### Props

기본으로 내보내는 함수가 받는 props는 다음과 같아요:

#### params (선택 사항)

이건 동적 라우트 파라미터들을 담고 있는 객체예요. 루트 세그먼트부터 opengraph-image나 twitter-image 세그먼트까지의 경로에 있는 모든 동적 파라미터들이 들어있답니다.

---

추가로 설명하자면, Next.js 같은 프레임워크에서 동적 라우트(dynamic route)는 URL 일부가 변할 수 있을 때 사용되는데, 예를 들어 `/posts/[id]`처럼 `id`가 동적으로 변하죠. 이런 경우 이 params 객체를 통해 해당 URL에서 어떤 값이 왔는지 쉽게 받아서 처리할 수 있답니다.

Markdown 표로 정리하면:

| Prop   | 설명                                                                    | 선택 여부  |
|--------|-------------------------------------------------------------------------|------------|
| params | 동적 라우트 파라미터들. 루트부터 현재 세그먼트까지의 경로 파라미터들이 포함돼 있음. | 선택 사항   |

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

Next.js 13의 App 디렉토리에서 Open Graph 이미지를 동적으로 생성할 때, `params`를 받아서 경로에 맞는 데이터를 처리하는 방식을 알아볼게요.

```js
export default function Image({ params }: { params: { slug: string } }) {
  // ...
}
```

위 예시는 `params` 안에 `slug`라는 문자열이 들어오는 컴포넌트의 기본 형태예요. 이 `params`는 URL 경로를 기반으로 자동으로 전달되죠.

아래는 라우트별 `params` 값과 매칭되는 예시입니다.

| Route                                 | URL           | `params`                  |
|-------------------------------------|---------------|---------------------------|
| `app/shop/opengraph-image.js`          | `/shop`       | `undefined`               |
| `app/shop/[slug]/opengraph-image.js`   | `/shop/1`     | `{ slug: '1' }`           |
| `app/shop/[tag]/[item]/opengraph-image.js` | `/shop/1/2` | `{ tag: '1', item: '2' }` |

즉, 동적 라우트(`[slug]` 등)를 사용하면 해당 경로 변수들이 `params` 객체로 들어와서 사용할 수 있어요.

그리고 중요한 점! 이 Open Graph 이미지를 만드는 컴포넌트는 기본적으로 다음 중 하나를 반환해야 해요.

- `Blob`
- `ArrayBuffer`
- `TypedArray`
- `DataView`
- `ReadableStream`
- `Response`

이 중 하나를 반환하면 Next.js가 이미지로 자동 변환해주는 거죠.

---

### 좀 더 쉽게 설명하자면

라우팅에 따라 Open Graph용 이미지 생성 로직을 분리할 수 있는데, URL에 포함된 변수들을 `params`로 받아서 이미지 내용에 반영할 수 있어요. 예를 들어 `/shop/1`이라면 `slug`가 `'1'`이 들어와서 해당 제품 정보를 사용해 이미지를 그리는 식입니다.

또한 반환 타입에 제약이 있는데, 이 반환값을 Next.js가 받아서 최종 이미지 파일을 만들어 주기 때문에 꼭 위 타입 중 하나로 반환해야 하고, 보통은 `Response` 객체를 많이 써요. 

이미지 생성에 대해 익숙하지 않다면, `@vercel/og` 같은 유틸리티를 사용해서 React 컴포넌트처럼 그려주고 그걸 바로 이미지로 변환시켜 주는 라이브러리도 추천드립니다.


---

### 정리

- `params`에는 URL 동적 세그먼트 값들이 담긴다.
- 경로에 따라 `params`가 자동으로 전달된다.
- Open Graph 이미지 함수는 Blob, Response 등 이미지를 표현할 수 있는 타입을 반환해야 한다.
- 동적 라우트에 맞게 이미지를 동적으로 생성하는 데 딱 맞는 구조다.

이제 이렇게 받은 `params`로 원하는 이미지 내용을 자유롭게 변경해 보세요! Next.js의 App 디렉토리 덕분에 라우트별 이미지 생성이 훨씬 간편해졌습니다.

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

> 참고로, ImageResponse는 이 반환 타입을 만족합니다.

### Config exports

opengraph-image 또는 twitter-image 경로에서 alt, size, contentType 변수를 export해서 이미지 메타데이터를 선택적으로 설정할 수 있어요.

| Option      | Type                                                                                                        |
|-------------|-------------------------------------------------------------------------------------------------------------|
| [alt](#alt) | `string`                                                                                                    |
| [size](#size) | `{ width: number; height: number }`                                                                        |
| [contentType](#contenttype) | `string` - [이미지 MIME 타입 (참고 문서)](https://developer.mozilla.org/docs/Web/HTTP/Basics_of_HTTP/MIME_types#image_types) |

---

조금 덧붙여서 설명하자면, alt는 이미지가 로드되지 않거나 스크린 리더를 사용할 때 중요한 역할을 하는 텍스트라서 SEO나 접근성을 위해 꼭 설정하는 걸 권장합니다. size는 이미지의 가로, 세로 크기를 명시해서 클라이언트가 적절한 공간을 미리 잡을 수 있게 도와주고요. contentType은 이미지의 MIME 타입으로, 예를 들어 `image/png`, `image/jpeg` 등이 있는데, 이걸 정확히 지정해주면 브라우저에서 이미지 처리할 때 더 효율적으로 작동합니다.

MIME 타입에 대해 잘 모르겠으면 위 MDN 링크 한 번 확인해보세요. 이미지뿐만 아니라 다양한 파일 타입에 대해 표준화된 정보를 얻을 수 있어서 개발할 때 많이 도움이 됩니다!

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

### alt 속성에 대해 알아볼까요?

웹에서 이미지를 사용할 때 `alt` 속성은 정말 중요해요. 간단히 말해, `alt`는 이미지가 표시되지 않을 때 대신 보여주는 텍스트인데요, 시각장애가 있는 분들이 스크린리더를 통해 이미지를 이해할 수 있도록 도와주는 역할도 해줘요.

예를 들어, 리액트 컴포넌트에서 `alt` 텍스트를 변수로 지정하는 방법은 이렇게 할 수 있어요:

```js
export const alt = 'My images alt text'

export default function Image() {
  return <img src="/path/to/image.jpg" alt={alt} />
}
```

또한, 소셜 미디어에서 공유할 때 사용하는 Open Graph 태그에도 `alt` 속성을 넣어줘서 이미지에 대한 추가 정보를 제공할 수 있답니다.

```html
<meta property="og:image:alt" content="My images alt text" />
```

이렇게 하면, 페이스북이나 트위터 같은 곳에서 이미지를 불러올 때 이 설명을 활용할 수 있어요.

> 참고로! `alt` 텍스트는 이미지 내용을 간결하면서도 충분히 설명할 수 있게 작성하는 게 좋아요. 그냥 '사진' 보다는 ‘서울의 남산타워 야경’처럼 구체적으로 적으면 효과적이에요.

---

### size 속성도 짚어볼게요

이미지의 크기를 다룰 때도 중요하게 생각해야 할 게 바로 `size`에요. 보통 HTML에서는 `<img>` 태그의 `width`, `height`로 크기를 조절하지만, 요즘은 반응형 디자인 때문에 CSS나 스타일 속성으로 크기를 정하는 경우가 많죠.

```html
<img src="/path/to/image.jpg" alt="sample image" width="200" height="150" />
```

하지만 이렇게 하게 되면 이미지 본연의 비율이 유지되지 않을 수 있으니, CSS에서 `max-width: 100%; height: auto;` 같은 설정으로 너비에 맞게 자동 조절하도록 하는 게 더 유연해요.

```css
img {
  max-width: 100%;
  height: auto;
}
```

또, 만약 리액트에서 사이즈 관련 값을 변수로 관리한다면 이렇게 활용할 수 있어요.

```js
export const size = {
  width: 200,
  height: 150
};

export default function Image() {
  return <img src="/path/to/image.jpg" alt="sample image" width={size.width} height={size.height} />;
}
```

> 작은 팁 하나! 이미지 크기를 지정할 때는 실제 이미지 파일의 크기와 맞추거나, 리사이징해서 용량 낭비를 막는 게 페이지 로딩 속도에 도움이 돼요.

---

요약하자면, ‘alt’는 접근성과 SEO를 위해 꼭 필요한 속성이고, ‘size’는 디자인과 퍼포먼스에 직결된 중요한 부분이랍니다. 이미지 작업할 때 이 두 가지 꼭 챙기세요!

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

안녕하세요! 오늘은 웹 개발에서 자주 사용하는 이미지 메타(tag) 관련 내용을 조금 더 친근한 느낌으로 정리해볼게요.

---

### 이미지 크기 정보 — size 객체

```js
export const size = { width: 1200, height: 630 }
 
export default function Image() {}
```

위 코드는 보통 이미지의 크기를 미리 정의할 때 사용해요. 특히 소셜 미디어나 SEO(Open Graph) 최적화할 때 이미지 크기를 명시하는 게 좋은데요. 여기서 `width`가 1200, `height`가 630인 픽셀 단위 크기를 지정했네요. 이 크기는 많은 SNS나 페이스북, 트위터에서 권장하는 OG 이미지 크기와도 잘 맞아서, 공유했을 때 이미지가 잘 보이도록 도와줍니다.

---

### 메타 태그로 이미지 크기 설정하기 (Open Graph)

```html
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
```

위 메타 태그들은 HTML 문서 안에 넣어서 페이스북 같은 소셜 미디어가 이미지를 정확한 크기로 인식하게 도와줍니다. 

> 만약 이 부분을 안 넣으면, 요즘 SNS 플랫폼들이 자동으로 이미지 크기를 추정할 수는 있지만, 정확한 크기를 명시해주면 훨씬 깔끔하게 미리보기가 나와요!

---

### 이미지 타입 명시 — contentType

```js
export const contentType = 'image/png'
 
export default function Image() {}
```

`contentType`은 이미지 파일 형식을 알려주는 변수입니다. 보통 png, jpg, jpeg, gif 등이 있어요. 클라이언트나 서버가 이미지를 처리할 때 타입을 알아야해서 명시해두면 좋아요.

---

## 한 눈에 보는 정리 테이블

| 속성             | 값           | 설명                                   |
|------------------|--------------|--------------------------------------|
| size.width       | 1200         | 이미지 가로 픽셀                      |
| size.height      | 630          | 이미지 세로 픽셀                      |
| og:image:width   | 1200         | Open Graph 메타 태그 내 이미지 가로 크기 |
| og:image:height  | 630          | Open Graph 메타 태그 내 이미지 세로 크기 |
| contentType      | 'image/png'  | 이미지 파일 포맷 (PNG/JPEG 등)       |

---

### 추가 팁!

- **이미지 크기**: OG 이미지 권장 크기는 보통 `1200x630` 픽셀이 가장 흔한데, 비율이 약 1.91:1 이거든요. 이 비율을 맞추면 SNS 공유 시 이미지가 잘려서 보이는 일 없이 깔끔하게 노출됩니다.

- **Content-Type 중요성**: 서버에서 이미지 응답을 보낼 때도 이 MIME type을 정확하게 설정해주면, 클라이언트가 이미지를 올바르게 해석할 수 있습니다.

- **Next.js 유저라면**: image 컴포넌트 내에 `width`, `height`를 고정해주면 레이아웃 shift 없이 좋은 퍼포먼스를 낼 수 있어요.

---

그럼 오늘 정리한 내용을 바탕으로 여러분이 만든 페이지가 소셜미디어에서 더 예쁘고 명확하게 보이길 바랄게요! 궁금한 점 있으면 댓글로 남겨주세요 :)

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
<meta property="og:image:type" content="image/png" />
```

#### Route Segment Config (라우트 세그먼트 설정)

opengraph-image와 twitter-image는 특별한 Route Handlers인데요, 이것들은 Pages나 Layouts에서 사용하는 것과 똑같은 라우트 세그먼트 설정 옵션들을 사용할 수 있어요.

### 예시들

---

여기서 잠깐! "Route Segment Config"는 Next.js나 비슷한 프레임워크에서 사용할 때, 각 URL 경로나 페이지 구간(segment)에 설정을 적용하는 방법이에요. 그래서 og:image나 twitter-image 같은 메타정보를 동적으로 설정하고 싶을 때 활용하면 굉장히 유용하답니다.

예를 들면, 특정 페이지에 맞춰서 og:image를 다르게 지정하고 싶을 때, 바로 이 설정을 이용해서 쉽게 처리할 수 있어요.

Markdown 형식으로 표기하면 아래와 같은 식으로 말이죠:

| 설정 이름               | 설명                              | 예시 값         |
|-----------------------|---------------------------------|---------------|
| `content`             | 실제 메타 태그에 들어갈 값         | `image/png`    |
| `property="og:image:type"` | Open Graph 프로토콜에서 이미지 타입 정의 | `image/png`    |

필요하면 직접 이미지 타입 외에 사이즈 정보나 용량 같은 추가 데이터도 넣을 수 있으니 참고하세요!

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

#### 외부 데이터 사용하기

이번 예제에서는 `params` 객체와 외부 데이터를 활용해서 이미지를 생성하는 방법을 보여드릴게요.

> 알아두면 좋은 점: 기본적으로 이 방식으로 생성된 이미지는 정적으로 최적화돼요. 필요에 따라 fetch 옵션이나 라우트 세그먼트 옵션을 조정해서 이 동작을 바꿀 수 있습니다.

```js
import { ImageResponse } from 'next/og'
 
export const alt = 'About Acme'
export const size = {
  width: 1200,
  height: 630,
}
export const contentType = 'image/png'
 
export default async function Image({ params }: { params: { slug: string } }) {
  // params.slug 값에 따라 외부 API에서 포스트 데이터를 가져옵니다.
  const post = await fetch(`https://.../posts/${params.slug}`).then((res) =>
    res.json()
  )
 
  return new ImageResponse(
    (
      <div
        style={{
          fontSize: 48,
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        {post.title}
      </div>
    ),
    {
      ...size,
    }
  )
}
```

---

조금 더 풀어서 설명하면, 이 코드는 URL에서 전달받은 `slug` 값을 이용해 외부 API에서 해당 글(post) 정보를 불러와요. 그리고 그 글의 제목을 이미지 안에 크게 띄우는 거죠.

여기서 `ImageResponse`는 Next.js의 Open Graph 이미지 생성 도구인데, 기본적으로 서버 사이드에서 실행되고 HTML 같은 JSX를 이미지로 렌더링해 줍니다. 그래서 동적으로 변하는 글 제목을 이미지에 넣을 수 있답니다.

참고로 `fontSize`, `background` 같은 인라인 스타일을 `style` 객체 안에 제대로 넣어줘야 하고, `width`, `height` 등 여러 스타일을 조합해 원하는 레이아웃으로 만들 수 있어요.

---

추가 팁: 외부 데이터를 불러올 때 에러 핸들링을 꼭 해주는 게 좋아요. 예를 들어 API가 응답하지 않거나 데이터가 없을 경우를 대비해 기본값을 넣거나, 사용자에게 알림을 줄 수 있죠. 그래야 이미지 생성이 실패하지 않고 원활히 처리됩니다 :)

그리고 만약 이미지가 너무 복잡해지면, CSS 스타일을 따로 작성하거나 React 컴포넌트로 분리해서 관리하는 것도 좋은 방법이에요. 이렇게 하면 가독성도 좋아지고 유지보수도 쉬워진답니다!

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

#### Node.js 런타임에서 로컬 자산 사용하기

이번 예제에서는 Node.js 런타임을 이용해 로컬 파일 시스템에서 이미지를 읽어오고, 그 이미지를 `img` 태그의 `src` 속성에 `ArrayBuffer` 형태로 전달하는 방법을 보여드릴게요. 여기서 중요한 점은, 로컬 자산(이미지 파일 등)은 프로젝트 최상위 경로(root)를 기준으로 두어야 한다는 거예요. 예제 소스 파일이 어디에 있든 상관없답니다.

아래 코드를 보면 `readFile`로 이미지를 불러오고, `Uint8Array.from`을 이용해 `ArrayBuffer`로 변환하는 과정이 보이죠? 이렇게 하면 Next.js의 `ImageResponse`에 이미지를 전달할 수 있어요.

```js
import { ImageResponse } from 'next/og'
import { join } from 'node:path'
import { readFile } from 'node:fs/promises'
 
export default async function Image() {
  const logoData = await readFile(join(process.cwd(), 'logo.png'))
  const logoSrc = Uint8Array.from(logoData).buffer
 
  return new ImageResponse(
    (
      <div
        style={{
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        <img src={logoSrc} height="100" />
      </div>
    )
  )
}
```

---

### 여기서 조금 더 팁!

- `process.cwd()`는 현재 Node.js 실행 경로(보통 프로젝트 루트)를 반환해요. 때문에 로컬 자산은 항상 프로젝트 루트 기준으로 경로를 잡는 게 좋아요.

- `ArrayBuffer` 형태로 변환하는 이유는, 로컬 이미지 데이터를 바이너리 형태로 직접 다루면서 Next.js가 이미지를 처리할 수 있게 하기 위함입니다.

- 만약 이미지를 직접 HTML의 `img` 태그에서 사용하고 싶으면, `data URL`로 변환하는 방법도 있지만 이번 경우처럼 바이너리 데이터를 직접 사용하는 게 효율적일 수 있어요.

- 스타일링할 때 `style` 객체를 작성할 때는 반드시 중괄호(`{}`) 두 번 써서 JSX 객체 형태로 넣어야 하는 거, 잊지 마세요!

이렇게 하면 Next.js와 Node.js 런타임을 활용해 로컬 이미지를 자유롭게 서버사이드에서 렌더링해볼 수 있답니다. 앞으로 이미지 처리 함께 파헤쳐봐요!

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

| Version  | Changes                                  |
|----------|------------------------------------------|
| `v13.3.0` | `opengraph-image`와 `twitter-image` 기능 추가됨 |

이번에 `v13.3.0` 버전에서는 Open Graph 이미지와 트위터 이미지 기능이 새롭게 도입되었어요. 이 기능들은 주로 소셜 미디어에서 공유할 때 미리보기 이미지로 사용되는데, 덕분에 내 웹페이지가 좀 더 멋지고 눈에 띄게 보이도록 해줘요. 개인 블로그나 프로젝트 소개 페이지를 운영 중이라면 꼭 활용해보는 걸 추천합니다! 혹시 더 자세한 설정 방법이나 활용 팁이 궁금하면 댓글로 남겨주세요. 함께 공부해봐요!