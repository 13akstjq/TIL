---
title: "웹 SEO를 위한 robots.txt 작성과 활용법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:53
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "robots.txt"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots"
isUpdated: false
---


# robots.txt 파일 쉽게 만들기

웹사이트를 운영하다 보면, 검색 엔진 봇들이 우리 사이트를 어떻게 크롤링할지 정하는 게 중요한데요. 그럴 때 쓰는 게 바로 **robots.txt** 파일이에요. 이 파일을 웹사이트의 루트 디렉터리에 넣으면, 검색 엔진 크롤러에게 “여기까지는 들어와도 돼”, “여기는 접근하지마”라고 알려줄 수 있죠.

---

## 기본적인 robots.txt 예시

```js
User-Agent: *
Allow: /
Disallow: /private/

Sitemap: https://acme.com/sitemap.xml
```

- `User-Agent: *` : 모든 크롤러에 적용한다는 뜻!  
- `Allow: /` : 사이트 전체를 접근허용해요.  
- `Disallow: /private/` : `/private/` 경로 아래는 크롤링 금지!  
- `Sitemap` : 사이트맵 URL을 알려주면 봇들이 페이지 구조를 더 잘 알 수 있답니다.

---

## 알아두면 좋은 점!

- robots.txt 파일은 “규칙”이지만, 강제성이 없다 보니 악의적인 봇은 무시할 수도 있어요.  
- CSS, JS 같은 리소스가 차단되지 않게 하시는 게 좋아요. 구글 등 주요 검색 엔진은 이런 파일을 분석해서 페이지를 제대로 이해하거든요.  
- 꼭 사이트 루트에 위치해야 하며, 경로를 잘못 넣으면 크롤러가 못 찾을 수 있어요.  
- 사이트맵 URL 적는 것도 함께 하면 SEO에 도움 됩니다!

robots.txt는 단순하면서도 웹사이트 검색 노출을 관리하는 데 중요한 파일이니, 위 내용을 참고해서 꼭 한 번만 세팅해 보세요! 그럼 여러분 사이트에 맞는 최적의 SEO 환경을 만들 수 있을 거예요. 😄

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

## Robots 파일 생성하기

웹사이트를 검색엔진이 어떻게 크롤링해야 하는지 알려주는 `robots.txt` 파일을 Next.js에서 쉽게 만들 수 있어요. 이때 `robots.js` 혹은 `robots.ts` 파일을 만들어서 `Robots` 객체를 반환하면 됩니다.

```js
import type { MetadataRoute } from 'next'

export default function robots(): MetadataRoute.Robots {
  return {
    rules: {
      userAgent: '*',       // 모든 봇에 대해 적용
      allow: '/',           // 루트 디렉토리는 크롤링 허용
      disallow: '/private/', // '/private/' 경로는 크롤링 금지
    },
    sitemap: 'https://acme.com/sitemap.xml', // 사이트맵 위치 지정
  }
}
```

### 알아두면 좋은 점 🌟
- `robots.js`는 Next.js의 **특별한 Route Handler**인데, 기본적으로 캐싱이 되어 있어요.  
- 다만 동적 API(dynamic API)나 동적 구성(dynamic config)을 사용할 경우에는 캐싱되지 않습니다.  
- 이 방식이 더 관리하기 편하고, 직접 `public/robots.txt` 파일을 만들지 않아도 되니 개발 속도가 빨라져요.

### 개인적으로 팁!
- 사이트에 아주 민감한 정보가 있다면, `.env` 파일이나 서버 설정으로 크롤링 금지 경로를 관리하는 것도 좋아요.  
- 그리고 구글 서치 콘솔 같은 도구에 사이트맵 URL을 등록해두면 검색엔진 최적화에도 도움이 됩니다.

이렇게 설정만 하면 여러분 사이트에 맞는 `robots.txt`를 편리하게 관리할 수 있어요! 🕷️🚫

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

출력 예시:

```js
User-Agent: *
Allow: /
Disallow: /private/

Sitemap: https://acme.com/sitemap.xml
```

### 특정 사용자 에이전트 맞춤 설정하기

검색 엔진 봇마다 사이트 크롤링 방식을 다르게 설정하고 싶을 때가 있죠? 이런 경우에는 `rules` 속성에 사용자 에이전트(User-Agent) 배열을 전달해서 각각을 개별적으로 지정할 수 있어요. 예를 들어, 이렇게 활용할 수 있습니다:

```js
rules: [
  {
    userAgent: ['Googlebot', 'Bingbot'],
    allow: ['/public'],
    disallow: ['/private']
  },
  {
    userAgent: ['*'],
    disallow: ['/admin']
  }
]
```

위처럼 하면 `Googlebot`과 `Bingbot`은 `/public` 경로를 허용하고 `/private`는 차단하지만, 모든 다른 봇들은 `/admin` 경로만 차단하도록 설정할 수 있죠.

추가 팁으로, 잘 구성된 `robots.txt`는 검색엔진 최적화(SEO)에도 도움을 주니 꼭 꼼꼼히 작성해 보세요! 그리고 필요하다면 사이트맵 위치도 명확히 표시해주면 크롤러가 사이트 구조를 더 잘 파악할 수 있습니다.

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

이번에는 Next.js에서 `MetadataRoute.Robots` 타입을 이용해 robots.txt를 설정하는 방법에 대해 살펴볼게요.

위 예제 코드를 보면 `robots()` 함수가 `MetadataRoute.Robots` 타입 객체를 반환하고 있습니다. 이 객체는 검색 엔진 크롤러들이 어떤 경로를 탐색할 수 있는지 세밀하게 조절할 수 있어요.

```js
export default function robots(): MetadataRoute.Robots {
  return {
    rules: [
      {
        userAgent: 'Googlebot',
        allow: ['/'],
        disallow: '/private/',
      },
      {
        userAgent: ['Applebot', 'Bingbot'],
        disallow: ['/'],
      },
    ],
    sitemap: 'https://acme.com/sitemap.xml',
  }
}
```

이 설정은 다음과 같은 의미를 갖는데요:

| User-Agent  | Allow | Disallow   |
|-------------|-------|------------|
| Googlebot   | /     | /private/  |
| Applebot    |       | /          |
| Bingbot     |       | /          |

- Googlebot에게는 전체 경로를 허용하지만 `/private/`는 막습니다.
- Applebot과 Bingbot은 사이트 전체 접근 금지입니다.
- Sitemap 위치도 명시되어 있어 크롤러가 쉽게 사이트맵을 찾을 수 있어요.

이렇게 하면 robots.txt는 다음과 같이 생성됩니다:


User-Agent: Googlebot
Allow: /
Disallow: /private/

User-Agent: Applebot
Disallow: /

User-Agent: Bingbot
Disallow: /

Sitemap: https://acme.com/sitemap.xml


### 여기서 알아두면 좋은 점
- `allow`와 `disallow`를 배열로 줄 수도 있어서 여러 경로를 지정할 수 있어요.
- `userAgent`도 문자열 하나나 배열로 여러 봇을 지정 가능해서 편리하죠.
- 사이트맵 URL 꼭 포함하는 걸 추천합니다. 크롤러가 사이트 구조를 더 빠르게 파악하거든요.

이 기능은 Next.js 13부터 정식 지원하면서 서버 컴포넌트 안에서 손쉽게 robots.txt 처리를 할 수 있어요. 예전엔 따로 파일을 만들어야 해서 좀 번거로웠는데, 이제 훨씬 깔끔하게 관리할 수 있답니다.

필요하면 robots.txt뿐만 아니라 `MetadataRoute` 타입으로 `favicon`이나 `manifest`도 관리할 수 있으니까 Next.js 공식 문서도 한번 살펴봐 보세요!

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
type Robots = {
  rules:
    | {
        userAgent?: string | string[]
        allow?: string | string[]
        disallow?: string | string[]
        crawlDelay?: number
      }
    | Array<{
        userAgent: string | string[]
        allow?: string | string[]
        disallow?: string | string[]
        crawlDelay?: number
      }>
  sitemap?: string | string[]
  host?: string
}
```

## 버전 히스토리

| Version   | Changes               |
|-----------|-----------------------|
| `v13.3.0` | `robots`가 도입되었습니다. |

---

### robots 타입 살짝 풀어보기!

위 타입은 주로 웹사이트의 `robots.txt` 설정을 타입스크립트로 표현한 거예요. 웹 크롤러가 어떤 페이지는 크롤링해도 되고, 어떤 페이지는 금지해야 할 때 `robots.txt`를 사용하는데요, 이런 설정을 코드로 관리할 때 유용하죠.

- `rules`는 크롤러 별로 어떤 경로를 허용(`allow`)하거나 차단(`disallow`)할지 설정해요.
- `userAgent`는 어떤 크롤러에 대한 규칙인지 지정하는 부분이라, Googlebot, Bingbot 등 특정 크롤러 이름을 넣을 수 있어요. 배열로 여러 크롤러를 지정할 수도 있고요.
- `crawlDelay`는 크롤러가 요청 사이에 얼마나 기다려야 하는지 초 단위로 설정합니다.
- `sitemap`은 사이트맵 URL을 명시해 크롤러가 더 효율적으로 페이지를 찾게 돕고,
- `host`는 사이트의 대표 도메인을 지정하는 값이에요.

이 타입으로 `robots.txt`를 프로그램matically(프로그램적으로) 생성하거나, 동적으로 관리하는 툴을 만들 때 큰 도움이 될 거예요!

필요하면 제가 간단한 예제도 함께 만들어볼게요. 관심 있으면 알려주세요!