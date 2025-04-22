---
title: "Next.js 15에서 sitemap.xml 자동으로 생성하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:54
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "sitemap.xml"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap"
isUpdated: false
---


# sitemap.xml

sitemap.(xml|js|ts)은 검색 엔진 크롤러가 사이트를 더 효율적으로 색인할 수 있도록 도와주는, Sitemaps XML 형식을 따르는 특별한 파일이에요.

### Sitemap 파일(.xml)

작은 규모의 앱이라면, 루트 디렉토리에 sitemap.xml 파일을 만들어 두는 게 가장 간단한 방법이에요. 이렇게 하면 크롤러가 바로 이 파일을 찾아서 사이트 구조를 쉽게 파악할 수 있답니다.

---

### 참고로 알아두면 좋은 점!

- sitemap.xml에 사이트 내 주요 URL들을 담으면, 구글 같은 검색 엔진이 어떤 페이지를 우선적으로 크롤링할지 효율적으로 판단해요.
- 페이지가 너무 많으면 하나의 sitemap.xml에 다 담기 어려운데, 이럴 땐 sitemap index 파일을 사용하거나 여러 개의 sitemap 파일을 나누어 관리할 수도 있어요.
- 웹사이트가 자주 바뀌는 경우엔 sitemap.xml에 `lastmod` 태그를 넣어서 마지막 변경 시점을 알리는 것도 좋아요. 그러면 크롤러가 변경된 부분을 빠르게 반영할 수 있어요.

마지막으로, sitemap.xml을 만드는 데 너무 복잡하게 생각하지 마세요. 간단하게 시작하고, 사이트가 커지면 점차 확장하는 방향으로 가면 충분합니다!

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

sitemap.xml 파일을 직접 작성하는 것도 방법이지만, 프로젝트 규모가 커지면서 수동으로 관리하기엔 점점 힘들어집니다. 그래서 요즘엔 코드로 sitemap을 자동 생성하는 방식을 많이 사용해요.

```js
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://acme.com</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
    <changefreq>yearly</changefreq>
    <priority>1</priority>
  </url>
  <url>
    <loc>https://acme.com/about</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://acme.com/blog</loc>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.5</priority>
  </url>
</urlset>
```

위처럼 정적인 sitemap도 좋지만, JavaScript나 TypeScript를 활용하면 더 유연하게 sitemap을 관리할 수 있죠.

---

## 코드로 sitemap 만들기 (.js, .ts)

예를 들어, `sitemap.js` 또는 `sitemap.ts` 같은 파일을 만들어서 거기서 URL 배열을 내보내는 함수를 작성할 수 있어요. TypeScript를 사용하면 `Sitemap` 타입도 제공되니까 타입 안정성도 챙길 수 있고요.

```ts
import { Sitemap } from 'some-sitemap-package'; // 예시, 실제로는 사용중인 라이브러리 따라 다름

const sitemap: Sitemap = [
  {
    loc: 'https://acme.com',
    lastmod: new Date().toISOString(),
    changefreq: 'yearly',
    priority: 1.0
  },
  {
    loc: 'https://acme.com/about',
    lastmod: new Date().toISOString(),
    changefreq: 'monthly',
    priority: 0.8
  },
  {
    loc: 'https://acme.com/blog',
    lastmod: new Date().toISOString(),
    changefreq: 'weekly',
    priority: 0.5
  }
];

export default function getSitemap() {
  return sitemap;
}
```

> **참고!** `sitemap.js` 파일은 특별한 Route Handler의 역할을 하며, 기본적으로 캐싱이 되기 때문에 사이트 성능에도 도움이 됩니다. 다만, 동적 API를 사용하거나 dynamic config 옵션이 있으면 캐싱이 적용되지 않을 수 있답니다.

---

### 추가 팁

- **자동 갱신:** 블로그처럼 새 글이 자주 올라오는 사이트는 게시글이 추가될 때마다 sitemap을 자동으로 업데이트하는 게 좋아요.
- **우선순위(priority):** 검색 엔진에게 어떤 페이지가 더 중요한지 알려주는 역할을 해요. 1.0이 가장 높은 값이고 0.0에 가까울수록 낮아요.
- **변경 빈도(changefreq):** 페이지가 얼마나 자주 변경되는지 알려주면 검색 엔진이 크롤링 빈도를 조절하는 데 도움이 돼요.

이렇게 코드로 sitemap을 관리하면, 추가하거나 수정할 때 훨씬 편하고 실수를 줄일 수 있어요. 꼭 한번 활용해 보세요!

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

Next.js의 `MetadataRoute.Sitemap` 타입으로 작성한 자바스크립트 객체를 실제 XML 사이트맵 형식으로 어떻게 변환하는지 보여주는 예제인데요.

코드를 보면 사이트맵에 들어갈 URL 리스트와 각 URL에 대한 메타정보(마지막 수정일, 변경 빈도, 우선순위)를 객체 배열로 반환하고 있어요. 그걸 XML `<urlset>` 태그 내부에 `<url>`단위로 나눠서 넣으면, 검색엔진이 이해할 수 있는 표준 사이트맵 파일이 완성됩니다.

---

### 자바스크립트 객체 ⇨ XML 사이트맵 대응표

| 자바스크립트 속성명 | XML 사이트맵 태그명  | 설명                         |
|---------------------|---------------------|------------------------------|
| url                 | `<loc>`             | URL 위치                     |
| lastModified        | `<lastmod>`         | 마지막 수정일 (ISO 8601 표기)|
| changeFrequency     | `<changefreq>`      | URL 변경 빈도 ('yearly', 'monthly', 'weekly' 등) |
| priority            | `<priority>`        | 우선순위 (0.0 ~ 1.0 사이 숫자) |

---

### 추가 팁: Image Sitemap 만들기

사이트맵에 이미지가 포함된 페이지를 별도로 표현할 때는 **Image Sitemap**이라고 불러요. 이미지를 효과적으로 검색엔진에 알릴 수 있어서 SEO에 도움이 되는 중요한 기능이죠.

예를 들어, 이미지 정보를 담는 XML 구조는 다음과 같아요.

```js
<url>
  <loc>https://example.com/page-with-image</loc>
  <image:image>
    <image:loc>https://example.com/images/image1.jpg</image:loc>
    <image:caption>Image Caption Here</image:caption>
    <image:title>Image Title Here</image:title>
  </image:image>
</url>
```

이미지 정보를 추가할 때는 각 `<url>` 안에 `<image:image>` 태그를 중첩시키고, 내부에 이미지 URL, 캡션, 타이틀 등을 넣어 줍니다.

---

Node.js나 Next.js 환경에서 이런 XML을 프로그래밍적으로 생성할 때는 보통 라이브러리를 쓰거나, 직접 문자열 템플릿을 만들어서 변환하죠. 여러분도 이 자료를 참고해서 자동으로 사이트맵을 만들면 워낙 편하니 꼭 도전해보길 추천할게요! 검색엔진 최적화(SEO)에 매우 큰 도움이 됩니다.

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

이미지 사이트맵을 만들 때는 `images` 속성을 활용할 수 있어요. 이미지가 포함된 사이트맵은 구글 검색에서 이미지 색인에 도움을 주니까, 이미지가 많은 사이트를 운영한다면 꼭 활용해보길 추천해요. 자세한 내용은 구글 개발자 문서(Developer Docs)에서 확인할 수 있습니다.

아래는 Next.js에서 `MetadataRoute.Sitemap` 타입을 사용해 이미지 사이트맵을 만드는 예제 코드예요:

```js
import type { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
  return [
    {
      url: 'https://example.com',
      lastModified: '2021-01-01',
      changeFrequency: 'weekly',
      priority: 0.5,
      images: ['https://example.com/image.jpg'],
    },
  ]
}
```

이 코드를 통해 생성되는 사이트맵 XML은 다음과 같이 생겼답니다:

```js
<?xml version="1.0" encoding="UTF-8"?>
<urlset
  xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
  xmlns:image="http://www.google.com/schemas/sitemap-image/1.1"
>
  <url>
    <loc>https://example.com</loc>
    <image:image>
      <image:loc>https://example.com/image.jpg</image:loc>
    </image:image>
    <lastmod>2021-01-01</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.5</priority>
  </url>
</urlset>
```

### 이미지 사이트맵, 이래서 좋아요!

| 장점 | 설명 |
|---|---|
| 더 나은 색인 | 구글이 이미지도 정확히 인식해서 검색 결과에 노출 가능성이 높아져요. |
| 트래픽 증가 | 이미지 검색을 통한 방문자 유입이 늘어나서 사이트 트래픽 증대에 도움이 됩니다. |
| SEO 강화 | 콘텐츠의 시각적 요소가 잘 반영되니, 전체 SEO 점수에도 긍정적 영향을 줍니다. |

> **Tip!**  
> 이미지 URL은 절대 경로로 명확하게 지정해 주세요. 이미지 파일이 자주 업데이트되는 경우에는 `lastModified` 값도 신경 써서 맞춰 주면 더 좋아요.

이왕 이미지 많은 사이트를 운영 중이라면, 이렇게 이미지 사이트맵까지 챙겨서 검색 노출, 방문자 수 두 마리 토끼 잡아보시길 바랍니다!

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

### 비디오 사이트맵 (Video Sitemaps)

웹사이트에 비디오 콘텐츠가 많다면, 비디오 전용 사이트맵을 만들어 검색 엔진이 영상을 더 잘 인식하고 노출할 수 있게 할 수 있어요. Next.js 같은 현대적인 프레임워크에서는 `videos` 프로퍼티를 활용해서 비디오 사이트맵을 쉽게 생성할 수 있는데요. 구글 공식 개발자 문서에서 자세한 내용을 참고할 수 있으니 꼭 한 번 확인해보세요!

아래 코드는 Next.js에서 비디오 사이트맵을 만드는 예시입니다:

```js
import type { MetadataRoute } from 'next'
 
export default function sitemap(): MetadataRoute.Sitemap {
  return [
    {
      url: 'https://example.com',                  // 페이지 URL
      lastModified: '2021-01-01',                  // 마지막 수정일
      changeFrequency: 'weekly',                    // 변경 빈도
      priority: 0.5,                               // 우선순위
      videos: [                                    // 비디오 정보 배열
        {
          title: 'example',                        // 영상 제목
          thumbnail_loc: 'https://example.com/image.jpg', // 썸네일 이미지 URL
          description: 'this is the description', // 영상 설명
        },
      ],
    },
  ]
}
```

### 비디오 사이트맵을 만들 때 알아두면 좋은 점

- `lastModified`와 `changeFrequency`는 검색 엔진에 갱신 주기를 알려주는 역할을 합니다. 자주 업데이트되는 콘텐츠면 보다 최신 정보를 반영하도록 설계하는 게 좋아요.
- `priority` 값은 0에서 1 사이로, 같은 사이트 내에서 해당 URL의 상대적 중요도를 나타내는데, 영상이 중요한 페이지라면 0.7 이상을 주는 것도 괜찮아요.
- `videos` 배열 안에는 여러 개의 비디오 정보를 넣을 수 있으므로, 한 페이지에 여러 영상이 있다면 모두 추가할 수 있어요.
- `thumbnail_loc` (썸네일 위치)은 영상 내용과 잘 맞는 정사각형 또는 16:9 비율의 고화질 이미지를 사용하는 게 노출 효과 면에서 유리합니다.

---

### 참고로, 비디오 사이트맵을 만들면 어떤 점이 좋나요?

- 구글이 사이트 내 비디오를 더 잘 파악해서 검색 결과에 리치 스니펫(동영상 미리보기)을 보여 줄 확률이 증가합니다.
- 유튜브 같은 외부 서비스 대신 자체 호스팅 영상이 있을 때 특히 검색 노출에 도움이 됩니다.
- 영상의 메타 정보를 명확하게 전달함으로써 사용자 경험 향상에도 기여할 수 있어요.

비디오를 많이 다루는 개발자나 콘텐츠 제작자라면 꼭 도입해보길 추천드립니다!

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

안녕하세요! 오늘은 **로컬라이즈된 사이트맵(Localized Sitemap)**을 만드는 방법에 대해 알려드릴게요.

---

### 1. 기본 비디오 사이트맵 예시

먼저, XML 형태로 작성된 비디오 사이트맵 예시를 보실게요.

```js
<?xml version="1.0" encoding="UTF-8"?>
<urlset
  xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
  xmlns:video="http://www.google.com/schemas/sitemap-video/1.1"
>
  <url>
    <loc>https://example.com</loc>
    <video:video>
      <video:title>example</video:title>
      <video:thumbnail_loc>https://example.com/image.jpg</video:thumbnail_loc>
      <video:description>this is the description</video:description>
    </video:video>
    <lastmod>2021-01-01</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.5</priority>
  </url>
</urlset>
```

- 여기선 `<loc>` 태그에 URL이 들어가고,
- `<video:video>` 안에 비디오 타이틀, 썸네일, 설명이 포함되어 있죠.
- `<lastmod>`, `<changefreq>`, `<priority>` 같은 검색엔진 최적화 태그들도 추가됐어요.

---

### 2. Next.js에서 로컬라이즈된 사이트맵 생성하기

이제 Next.js 기반 프로젝트에서, 여러 언어별 사이트맵을 자동으로 생성하는 방법을 알려드릴게요.

```ts
import type { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
  return [
    {
      url: 'https://acme.com',
      lastModified: new Date(),
      alternates: {
        languages: {
          es: 'https://acme.com/es',
          de: 'https://acme.com/de',
        },
      },
    },
    {
      url: 'https://acme.com/about',
      lastModified: new Date(),
      alternates: {
        languages: {
          es: 'https://acme.com/es/about',
          de: 'https://acme.com/de/about',
        },
      },
    },
    {
      url: 'https://acme.com/blog',
      lastModified: new Date(),
      alternates: {
        languages: {
          es: 'https://acme.com/es/blog',
          de: 'https://acme.com/de/blog',
        },
      },
    },
  ]
}
```

- `lastModified`엔 최신 업데이트 시간을 넣고,
- `alternates.languages` 속성으로 각 언어별 페이지 URL을 명시해주는게 포인트입니다.
- 이렇게 하면 구글 같은 검색엔진이 다국어 페이지를 인식해 SEO에 도움이 돼요!

---

### 사이트맵에 대해 더 알아두면 좋은 점

| 제목              | 설명                                                         |
|-----------------|------------------------------------------------------------|
| 사이트맵 역할       | 검색엔진 크롤러에게 내 사이트 페이지 구조를 알려줘요                 |
| 로컬라이즈 사이트맵 | 다국어 사이트에서 각 언어별 URL을 명확히 표시해서 SEO를 극대화해요    |
| Next.js 지원      | 최신 Next.js는 `app` 디렉터리에 `sitemap.ts`를 추가해 쉽게 구현 가능 |
| lastModified 쓰임  | 페이지가 언제 업데이트됐는지 알려줘 크롤러가 효율적으로 크롤링하게 도움 |

---

### 마무리

만약 여러분의 사이트가 여러 언어로 운영된다면, 이렇게 로컬라이즈된 사이트맵은 필수라고 할 수 있어요.  
특히 Next.js 같이 최신 프레임워크에선 손 쉽게 관리할 수 있으니 꼭 활용해보세요!

궁금한 점이나 어려운 점 있으면 편하게 댓글 남겨주시고, 즐거운 개발 하시길 바랄게요 :)

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
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9" xmlns:xhtml="http://www.w3.org/1999/xhtml">
  <url>
    <loc>https://acme.com</loc>
    <xhtml:link
      rel="alternate"
      hreflang="es"
      href="https://acme.com/es"/>
    <xhtml:link
      rel="alternate"
      hreflang="de"
      href="https://acme.com/de"/>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
  <url>
    <loc>https://acme.com/about</loc>
    <xhtml:link
      rel="alternate"
      hreflang="es"
      href="https://acme.com/es/about"/>
    <xhtml:link
      rel="alternate"
      hreflang="de"
      href="https://acme.com/de/about"/>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
  <url>
    <loc>https://acme.com/blog</loc>
    <xhtml:link
      rel="alternate"
      hreflang="es"
      href="https://acme.com/es/blog"/>
    <xhtml:link
      rel="alternate"
      hreflang="de"
      href="https://acme.com/de/blog"/>
    <lastmod>2023-04-06T15:02:24.021Z</lastmod>
  </url>
</urlset>
```

### 다중 사이트맵 생성하기

대부분의 웹사이트는 하나의 사이트맵만으로 충분하지만, 큰 규모의 웹 애플리케이션에서는 사이트맵을 여러 개로 나눠야 할 때가 있어요. 검색 엔진이 너무 큰 파일을 처리하기 어려워할 수 있고, 사이트맵 크기 제한(50,000 URL 또는 50MB uncompressed)을 초과할 우려도 있거든요.

그래서 사이트맵을 나누는 방법은 크게 두 가지가 있습니다:

| 방법           | 설명                                                                                   |
| -------------- | -------------------------------------------------------------------------------------- |
| 여러 개의 사이트맵 파일 생성 | 사이트를 여러 개의 사이트맵 파일로 나누어 각각 관리하는 방법. 예를 들어, /sitemap1.xml, /sitemap2.xml처럼 분리 가능해요. |
| 사이트맵 인덱스 파일 활용   | 여러 개의 사이트맵 파일을 묶어주는 ‘사이트맵 인덱스 파일’을 만들어 검색 엔진에 알려주는 방법입니다. 인덱스 파일은 각 사이트맵 파일의 위치를 포함합니다.  |

> 참고로, 사이트맵 인덱스 파일은 아래처럼 `<sitemapindex>` 태그를 사용해 작성합니다.

```js
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <sitemap>
    <loc>https://acme.com/sitemap1.xml</loc>
    <lastmod>2023-04-01</lastmod>
  </sitemap>
  <sitemap>
    <loc>https://acme.com/sitemap2.xml</loc>
    <lastmod>2023-04-05</lastmod>
  </sitemap>
</sitemapindex>
```

이렇게 하면 검색 엔진은 인덱스 파일만 방문해 여러 사이트맵을 효과적으로 크롤링할 수 있어요. 

---

사이트맵 나누기 방법을 이해하면, 큰 웹사이트 SEO 관리에 훨씬 유리해집니다. 필요에 따라 적절히 활용해 보세요! 혹시 궁금한 점 있으면 언제든 물어봐 주세요~

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

사이트맵을 여러 개로 나누는 방법에 대해 이야기해볼게요. Next.js에서 사이트맵은 기본적으로 app 디렉토리에 sitemap.xml 또는 sitemap.js(ts) 같은 파일을 생성해서 만들 수 있는데, 여기서 더 나아가 여러 경로 세그먼트에 걸쳐 여러 개의 사이트맵을 나누어 관리할 수 있어요. 예를 들어, app/sitemap.xml, app/products/sitemap.xml처럼 각각 독립적으로 관리하는 방식이죠.

또 다른 방법으로는 `generateSitemaps` 함수를 사용하는 건데요, 이 함수는 여러 개의 사이트맵을 동적으로 생성할 때 유용합니다. 간단히 말하면, `generateSitemaps`에서 사이트맵 ID 배열을 반환하고, 거기에 맞춰 각 ID마다 별도의 사이트맵을 만드는 겁니다.

아래 예제를 볼까요?

```js
import type { MetadataRoute } from 'next'
import { BASE_URL } from '@/app/lib/constants'
 
export async function generateSitemaps() {
  // 전체 상품 수를 가져와서 사이트맵 개수를 산정할 수 있겠지만 여기선 예시로 4개의 ID를 만들었어요.
  return [{ id: 0 }, { id: 1 }, { id: 2 }, { id: 3 }]
}
 
export default async function sitemap({
  id,
}: {
  id: number
}): Promise<MetadataRoute.Sitemap> {
  // 구글은 한 사이트맵에 최대 50,000 URL을 허용해요.
  const start = id * 50000
  const end = start + 50000
  // 데이터베이스 쿼리를 통해 특정 범위의 상품을 받아옵니다.
  const products = await getProducts(
    `SELECT id, date FROM products WHERE id BETWEEN ${start} AND ${end}`
  )
  // 상품마다 URL과 마지막 수정일을 반환하세요.
  return products.map((product) => ({
    url: `${BASE_URL}/product/${product.id}`,
    lastModified: product.date,
  }))
}
```

위 방식대로 하면 실제 생성되는 사이트맵 파일은 `/product/sitemap/0.xml`부터 `/product/sitemap/3.xml`까지 각 ID별로 존재하게 됩니다.

### 참고 팁!
- 사이트맵을 여러 개로 나누면 검색엔진 크롤러가 한꺼번에 너무 많은 URL을 받지 않아서 효율적이에요.
- `generateSitemaps`와 `sitemap` 함수를 함께 써서 동적으로 필요한 만큼 파일을 만들 수 있으니, 상품이나 게시글이 엄청 많을 때 특히 유용합니다.
- `/robots.txt` 파일에 이런 사이트맵 URL들을 모두 등록하거나, 사이트맵 인덱스를 만들어서 크롤러에게 알려주는 것도 잊지 마세요!

이제 대규모 프로젝트에서 사이트맵 관리가 훨씬 수월해지겠죠? 궁금한 점 있으면 언제든 물어봐요!

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

generateSitemaps API에 대한 더 자세한 정보는 공식 문서를 참고하시면 좋아요.

## 반환값

`sitemap.xml`, `sitemap.ts`, 또는 `sitemap.js`에서 기본으로 내보내는 함수는 아래처럼 특정 속성을 가진 객체들의 배열을 반환해야 해요:

```ts
type Sitemap = Array<{
  url: string
  lastModified?: string | Date
  changeFrequency?:
    | 'always'
    | 'hourly'
    | 'daily'
    | 'weekly'
    | 'monthly'
    | 'yearly'
    | 'never'
  priority?: number
  alternates?: {
    languages?: {
      [languageCode: string]: string
    }
  }
}>
```

### 속성 설명
- `url`: 필수! 사이트맵에 등록할 URL입니다.
- `lastModified`: 해당 페이지가 마지막으로 수정된 날짜를 나타내요. 문자열(ISO 포맷)이나 Date 객체 모두 가능해요.
- `changeFrequency`: 검색 엔진에게 페이지가 얼마나 자주 변경되는지 알릴 때 쓰입니다. 기본값은 없으므로 필요할 때만 넣으시면 돼요.
- `priority`: URL 우선순위를 0.0에서 1.0 사이의 숫자로 지정합니다. 기본값은 0.5이고, 높을수록 중요하다고 간주해요.
- `alternates.languages`: 다국어 페이지가 있을 때, 각 언어별 URL을 명시할 수 있어요. 예를 들면 `{ en: "/en/page", ko: "/ko/page" }` 이런 식입니다.

---

### 팁 + @
사이트맵을 잘 작성해두면 검색 엔진 최적화(SEO)에 큰 도움이 된답니다. 특히 `lastModified`나 `changeFrequency`를 적절히 지정해 두면 크롤러가 페이지 업데이트를 빠르게 인지해서 노출이 더 좋아질 수 있어요.  
또한 다국어 사이트라면 `alternates.languages`를 꼭 활용해 보세요. 구글 등에서 올바르게 다국어 페이지를 인식하는 데 도움이 됩니다.

혹시 사이트맵 자동 생성 도구를 사용 중이라면, 이런 반환 객체 형식을 참고해서 커스터마이징 해보는 것도 좋겠죠?ㅎㅎ

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

| 버전       | 변경사항                                       |
|------------|--------------------------------------------|
| `v14.2.0`  | 다국어 지원 기능 추가                          |
| `v13.4.14` | 사이트맵에 `changeFrequency`와 `priority` 속성 추가 |
| `v13.3.0`  | 사이트맵 기능 도입                             |

---

간단하게 버전별 주요 변경사항을 정리해봤어요. 특히 사이트맵 관련 기능이 점점 강화되고 있다는 걸 볼 수 있는데요, 웹사이트 SEO 최적화에 관심이 있다면 `changeFrequency`(변경 빈도)와 `priority`(우선순위) 속성이 어떤 의미인지 꼭 알아두시는 걸 추천드려요. 이 속성들을 잘 활용하면 검색 엔진이 내 사이트를 더 잘 이해하고, 중요한 페이지를 더 빠르게 크롤링하도록 도와준답니다!