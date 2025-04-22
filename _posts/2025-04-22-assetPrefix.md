---
title: "Next.js 15에서 assetPrefix 설정하여 CDN 연동하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:01
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "assetPrefix"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/assetPrefix"
isUpdated: false
---


# assetPrefix

> 주의: Vercel에 배포하면 Next.js 프로젝트에 전역 CDN이 자동으로 설정되기 때문에, Asset Prefix를 따로 수동으로 설정하지 않아도 됩니다.

> 참고로: Next.js 9.5 버전부터는 Base Path를 커스터마이징하는 기능이 추가됐어요. 이 기능을 이용하면 `/docs` 같은 서브 경로에 앱을 호스팅하기 적합해서, 이런 경우에는 Asset Prefix 대신 Base Path를 사용하는 걸 추천합니다.

## CDN 설정하기

자, 이제 직접 CDN 설정을 해보고 싶을 때 어떻게 하는지 살펴볼게요.

---

### assetPrefix란?

`assetPrefix`는 Next.js에서 빌드된 정적 자산들(js, css, 이미지 등)이 로드되는 기본 URL 경로를 지정할 때 사용해요. 예를 들어, CDN 도메인을 지정해두면 사용자들은 CDN에서 자산들을 빠르게 받아볼 수 있죠.

예전에는 배포 환경마다 직접 CDN 주소를 설정해야 했는데, 요즘은 Vercel을 사용하면 자동으로 글로벌 CDN이 세팅되니 크게 신경 쓰지 않아도 됩니다!

---

### 직접 CDN을 연결해야 하는 경우

만약 Vercel이 아닌 AWS S3, CloudFront, Netlify, 또는 다른 호스팅 환경을 사용한다면, 아래처럼 `next.config.js`에 `assetPrefix`를 설정하게 됩니다.

```js
module.exports = {
  assetPrefix: 'https://cdn.mydomain.com',
};
```

이 설정을 하면, 정적 자산들의 경로가 `https://cdn.mydomain.com/_next/static/...`으로 변경되어 CDN에서 서빙됩니다.

---

### 주의할 점

다만, 이렇게 `assetPrefix`를 설정하면 정적 파일뿐 아니라 내부 스크립트와 스타일도 모두 여기서 불러오기 때문에 CDN 도메인의 SSL이나 CORS 설정이 올바르게 되어 있어야 해요. 그렇지 않으면 자원이 제대로 로드 안 되는 문제가 발생할 수 있습니다.

---

### Base Path 활용하기

또 한 가지 중요한 기능! Next.js 9.5버전부터는 `basePath` 옵션도 사용할 수 있어요. `basePath`는 애플리케이션 URL의 기본 경로를 설정하는 건데, 보통 `/docs`나 `/blog` 같이 서브 디렉토리에 앱을 배포할 때 쓰입니다.

```js
module.exports = {
  basePath: '/docs',
};
```

이렇게 설정하면 페이지들이 모두 `/docs` 경로 하위에서 서비스되고, 라우팅 관리가 훨씬 수월해져요.

그리고 중요한 점! 이런 경우에는 굳이 `assetPrefix`를 쓰지 않고, `basePath` 만으로 충분하니 참고하세요.

---

## 정리하자면!

| 상황                                 | 권장 설정                  |
|----------------------------------|-------------------------|
| Vercel 배포                         | 자동 CDN 설정, assetPrefix 불필요 |
| 서브 경로(예: `/docs`)로 앱 배포           | basePath 활용              |
| 직접 CDN 도메인 연결 필요 (Vercel 외)    | assetPrefix에 CDN 주소 설정    |

---

내가 개인적으로 추천하는 팁은 Vercel이나 요즘 메이저 플랫폼들은 대부분 CDN을 자동으로 설정해주니 복잡하게 assetPrefix 건드리지 않고, 필요하면 basePath만 적절히 활용하는 게 가장 깔끔하다는 거예요.

혹시 여러분도 서브 디렉토리로 앱을 배포하거나 직접 CDN 연결해야 하는 상황이 생기면, 위 내용을 참고해서 차근차근 설정해 보세요! 문제 생기면 댓글로 물어봐도 좋고요 :)

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

CDN(Content Delivery Network)을 설정하는 방법에 대해 이야기해볼게요. 좋은 성능을 위해 정적 자산(이미지, JS, CSS 등)을 CDN으로 배포하면 사용자에게 훨씬 빠르게 콘텐츠를 전달할 수 있어요.

Next.js에서는 assetPrefix라는 설정을 이용해서 CDN 주소를 쉽게 지정할 수 있어요. 예를 들어, 여러분의 CDN 도메인이 `https://cdn.mydomain.com`이라면, 다음과 같이 `next.config.mjs` 파일에 설정할 수 있습니다.

```js
// @ts-check
import { PHASE_DEVELOPMENT_SERVER } from 'next/constants'
 
export default (phase) => {
  const isDev = phase === PHASE_DEVELOPMENT_SERVER
  /**
   * @type {import('next').NextConfig}
   */
  const nextConfig = {
    // 개발 환경에서는 assetPrefix를 사용하지 않고, 배포 환경에서만 CDN 도메인을 붙임
    assetPrefix: isDev ? undefined : 'https://cdn.mydomain.com',
  }
  return nextConfig
}
```

이렇게 설정하면 Next.js는 자동으로 자바스크립트나 CSS 같은 정적 파일을 `/next/static/` 폴더에서 불러올 때 CDN 주소를 붙여 요청해줍니다.

예를 들어, 원래라면 `/next/static/chunks/main.js`로 요청할 파일이


https://cdn.mydomain.com/_next/static/chunks/main.js


이런 식으로 바뀌어서 CDN 서버에서 빠르게 제공되겠죠.

---

### 추가로 알아두면 좋은 점들

- **환경별 분리**: 개발 단계에선 로컬에서 바로 파일을 불러오는 게 편하니까 assetPrefix를 사용하지 않고, 실제 배포 때만 CDN 주소를 사용하도록 phase를 분리하는 게 일반적이에요.
- **CDN 설정**: Next.js 쪽에서 assetPrefix만 설정한다고 CDN이 완성되는 건 아니에요. CDN 제공 업체(Cloudflare, AWS CloudFront 등)에서 원본(origin)을 Next.js가 호스트 중인 도메인(예: 내 서버 도메인)으로 설정해야 합니다. 즉, CDN 주소로 들어온 요청을 실제 Next.js 서버로 전달해주는 역할이 필요해요.
- **_next 폴더 주의**: Next.js가 내부적으로 사용하는 `_next` 경로는 정적 파일 및 페이지 정보를 담고 있어서, assetPrefix 설정 시 해당 경로가 포함되니 CDN에서도 해당 경로들을 캐싱하도록 설정하는 게 좋아요.

---

CDN을 붙여놓으면 사용자의 로드 속도가 확실히 개선되니까 배포할 땐 꼭 한번 시도해보세요! 만약 추가로 리소스 압축이나 캐시 설정 등을 손보고 싶다면, CDN 설정 메뉴에서 조절하면 되고, Next.js에서도 `next.config.js`로 다양한 빌드 옵션을 더 줄 수 있습니다. 앞으로도 여러 팁 공유할게요!

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

`table` 태그를 마크다운 형식으로 바꿔서 설명해볼게요.

먼저, CDN에 파일을 업로드할 때 중요한 건 `.next/static/` 폴더 안에 있는 내용만 올려야 한다는 점이에요. 이걸 올릴 때는 URL 경로와 맞춰서 `_next/static/` 형태로 업로드해야 하죠. 다른 `.next/` 폴더 전체를 올리면 서버 쪽 코드나 중요한 설정들이 공개될 수 있으니까 절대 업로드하지 마세요!

요약하자면, 이렇게 생각하면 편해요:

| 할 일                     | 설명                                    |
|--------------------------|---------------------------------------|
| 업로드할 폴더              | `.next/static/` 폴더 안의 파일들만            |
| 업로드 경로               | CDN 상에서 `_next/static/` 경로로 업로드       |
| 업로드하지 말아야 할 것     | `.next/` 폴더 전체 (서버 코드, 설정 등 민감한 내용 포함) |

이 부분은 Next.js 같은 프레임워크를 사용할 때 꼭 주의해야 하는 보안 포인트 중 하나예요. CDN에서 정적 파일만 안전하게 제공하고, 서버 쪽 코드는 절대 노출되지 않게 하는 거죠.

만약 CDN 설정이 처음이라면, 사용하는 CDN 서비스에 따라 조금씩 방법이 다르니 공식 문서나 가이드를 참고하는 게 좋아요. AWS S3, CloudFront, Netlify, Vercel 등 흔히 쓰이는 CDN 서비스들은 각각 업로드 방법과 경로 규칙을 제공하니까요.

추가로, `.next/static/` 폴더는 빌드 시 자동으로 생성되는 정적 자원들이 들어가기 때문에, 빌드(F5)를 할 때마다 최신 버전으로 갱신되는 게 포인트입니다. 따라서 CDN 캐싱 정책도 적절히 설정해줘야 오래된 파일이 서빙되는 걸 방지할 수 있어요!

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

assetPrefix 설정은 _next/static 경로에 요청하는 파일들에는 적용되지만, 다음 경로들에는 영향을 주지 않아요:

- public 폴더 내의 파일들; 이 파일들을 CDN으로 제공하고 싶다면, assetPrefix를 사용하는 것만으로는 부족하고 직접 경로 앞에 CDN prefix를 붙여줘야 해요.

조금 더 쉽게 말하자면, Next.js에서 assetPrefix를 설정하면 빌드 후 생성되는 정적 파일들(예: _next/static 안에 있는 JS, CSS 파일)에 대해 해당 prefix가 자동으로 적용돼요. 하지만 public 폴더 안에 넣은 이미지나 favicon 같은 파일들은 assetPrefix 설정과 상관없이 그냥 루트 경로(/)를 기준으로 접근해요. 그래서 public 폴더 자산을 CDN에서 직접 서비스하고 싶다면, 예를 들어 이미지 태그에 `${assetPrefix}/image.png`처럼 prefix를 직접 넣어줘야 한다는 점, 꼭 기억하세요!

이 과정에서 흔히 헷갈리기도 하는데, 실제 배포 환경에서 자주 실수하는 부분이라 꼭 확인하는걸 추천드려요! CDN으로 빠르게 자산을 제공하고 싶다면 public 폴더 자산도 prefix를 잘 적용해주는게 중요하답니다.