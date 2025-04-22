---
title: "Next.js 15에서 generateSitemaps 기능 쉽게 구현하는 2025년 최신 가이드"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:36
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "generateSitemaps"
link: "https://nextjs.org/docs/app/api-reference/functions/generate-sitemaps"
isUpdated: false
---


# generateSitemaps 함수 소개

웹사이트를 운영하다 보면, 검색엔진 최적화(SEO)를 위해 여러 개의 사이트맵(sitemap)을 만들어야 할 때가 있는데요. 이때 `generateSitemaps` 함수를 사용하면 여러 개의 사이트맵을 손쉽게 생성할 수 있어요.

## 반환값(Returns)

`generateSitemaps` 함수는 사이트맵을 여러 개 만들고, 각 사이트맵에 대해 `id`라는 프로퍼티를 가진 객체들의 배열을 반환합니다. 쉽게 말해, 생성된 사이트맵들의 고유 식별자 리스트를 받아볼 수 있다는 뜻이죠.

| 반환 유형   | 설명                                |
| --------- | --------------------------------- |
| 배열(Array) | 각각의 사이트맵을 나타내는 객체들로 구성됨 |
| 객체(Object) | 각 객체는 `id` 프로퍼티를 포함          |

---

### 추가 팁!

- 사이트맵을 여러 개로 나누는 것은 대규모 웹사이트에서 자주 쓰이는 전략입니다. 구글 같은 검색엔진은 한 사이트맵 파일에 너무 많은 URL이 들어가면 효율적으로 크롤링하지 못하거든요.
- `id` 값을 활용해서 나중에 각 사이트맵을 동적으로 불러오거나 관리할 때 용이합니다.
- 만약 사이트맵을 자동 생성할 수 있는 라이브러리나 툴을 사용 중이라면, 이 같은 함수는 내부적으로 페이지 URL들을 분배하고 각 파일을 생성하는 역할을 담당합니다.

웹사이트 규모가 커질수록 사이트맵 관리가 쉽지 않은데, 이런 함수를 잘 활용하면 관리도 편하고 SEO에도 훨씬 도움이 되니 꼭 참고하세요!

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

## URL 경로

생성된 사이트맵은 `/.../sitemap/[id].xml` 경로에서 확인할 수 있어요. 예를 들어, `/product/sitemap/1.xml` 같은 식이죠.

## 예시

사이트맵을 나누고 싶다면 `generateSitemaps` 함수에서 사이트맵 ID를 포함한 객체 배열을 반환하면 돼요. 그리고 이 ID를 활용해서 각각 고유한 사이트맵을 생성할 수 있답니다.

아래 표를 보면 좀 더 이해하기 편할 거예요.

| Sitemap ID | 생성되는 사이트맵 파일 경로    |
|------------|------------------------------|
| 1          | /product/sitemap/1.xml        |
| 2          | /product/sitemap/2.xml        |
| 3          | /product/sitemap/3.xml        |

이렇게 ID를 기준으로 사이트맵을 나누면, 대형 웹사이트에서도 효율적으로 검색 엔진에 페이지를 노출시킬 수 있어요. 대략 한 사이트맵에 50,000개 정도의 URL까지만 포함하는 게 권장되니까요.

만약 사이트맵이 너무 커서 관리가 힘들다면, 이렇게 ID를 쪼개서 여러 개로 만드는 방법을 꼭 활용해 보세요!

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

이 코드는 Next.js에서 동적 사이트맵을 생성하는 간단한 예제예요. 사이트맵은 검색 엔진이 내 사이트를 효율적으로 크롤링할 수 있도록 도와주는 중요한 역할을 하죠. 여기선 제품 목록(product)을 50,000개씩 나눠서 각각의 사이트맵을 만들어주는 구조예요.

```js
import { BASE_URL } from '@/app/lib/constants'
 
export async function generateSitemaps() {
  // 전체 상품 수를 불러와서 사이트맵 페이지 개수를 계산하는 대신, 임시로 4개의 사이트맵 페이지를 반환
  return [{ id: 0 }, { id: 1 }, { id: 2 }, { id: 3 }]
}
 
export default async function sitemap({
  id,
}: {
  id: number
}): Promise<MetadataRoute.Sitemap> {
  // 구글은 사이트맵에 최대 50,000개의 URL만 허용하니 이걸 기준으로 상품을 나눠서 가져옴
  const start = id * 50000
  const end = start + 50000
  const products = await getProducts(
    `SELECT id, date FROM products WHERE id BETWEEN ${start} AND ${end}`
  )
  return products.map((product) => ({
    url: `${BASE_URL}/product/${product.id}`,
    lastModified: product.date,
  }))
}
```

### 중요한 포인트
- 사이트맵을 너무 크게 만들면 구글에서 인식하기 어려우니 50,000개씩 분할하는 게 기본입니다.
- `generateSitemaps` 함수는 사이트맵의 페이지 목록을 반환하는 역할을 하고, 실제 각 페이지에 어떤 URL이 들어갈지 `sitemap` 함수에서 처리하죠.
- 실제 환경에서는 상품 총 개수를 DB에서 조회해 그에 맞게 페이지 수를 동적으로 만들어줘야 해요.

---

## 버전 히스토리

| 버전 | 변경사항 |
|-------|---------|
| `v15.0.0` | `generateSitemaps` 함수가 개발 환경과 프로덕션 환경에서 일관된 URL을 생성하도록 개선됨 |
| `v13.3.2` | `generateSitemaps` 함수 도입. 개발 시 `/.../sitemap.xml/[id]` URL을 통해 동적으로 생성된 사이트맵 확인 가능 (예: `/product/sitemap.xml/1`) |

---

### 추가 팁
- 사이트맵 URL을 서비스 루트에 쉽게 추가하고 싶다면, Next.js 내의 API Route를 사용하거나 서버리스 함수(Serverless Function)를 활용할 수도 있어요.
- 사이트맵 생성 시 추천하는 방법 중 하나는 정기적으로 크론 잡으로 상품 리스트 업데이트에 맞춰 사이트맵을 재생성하는 거예요.
- 큰 사이트라면 사이트맵 인덱스 파일을 만들어서 여러 개의 사이트맵 파일을 참조하는 전략도 있어요.

사이트맵은 SEO 최적화에도 굉장히 중요하니, 꼭 내 사이트 규모에 맞게 적절한 구조로 관리해 보세요!