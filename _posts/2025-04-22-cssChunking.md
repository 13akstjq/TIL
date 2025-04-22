---
title: "Next.js 15에서 cssChunking을 활용해 CSS 로딩 최적화하기"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:04
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "cssChunking"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/cssChunking"
isUpdated: false
---


# CSS Chunking이란?

CSS Chunking은 웹 애플리케이션의 성능을 높이기 위한 전략 중 하나예요. 간단히 말하면, CSS 파일을 여러 개의 작은 덩어리(Chunk)로 쪼개고, 필요한 부분만 골라서 로드하는 방식이죠. 예를 들어, 특정 페이지(라우트)를 볼 때 모든 CSS를 한꺼번에 불러오는 게 아니라 그 페이지에 필요한 CSS만 불러오게 해서 로딩 시간을 줄일 수 있어요.

Next.js에서는 `next.config.js` 파일 내에 실험적인 기능인 `experimental.cssChunking` 옵션으로 이 CSS Chunking 기능을 켤 수 있습니다. 기본값은 `true`로 되어 있어요.

```js
import type { NextConfig } from 'next'

const nextConfig = {
  experimental: {
    cssChunking: true, // 기본값이라 특별히 설정 안 해도 활성화됨
  },
} satisfies NextConfig

export default nextConfig
```

---

## 더 알아두면 좋은 점

- CSS Chunking을 활성화하면 사용자 경험이 개선되는 경우가 많아요. 특히 큰 프로젝트나 여러 페이지로 구성된 복잡한 앱에서 효과가 크죠.
- 하지만 작은 사이트에서는 chunk가 너무 잘게 쪼개져서 오히려 관리가 번거로울 수도 있어요.
- 이 기능이 아직 `experimental` 상태라서, 일부 환경에서는 예상치 못한 문제가 발생할 수도 있으니, 배포 전에 충분한 테스트가 필요해요.
- Next.js뿐만 아니라 다른 프레임워크나 번들러(예: Webpack)에서도 비슷한 CSS 분할 기능을 지원하니, 프로젝트에 맞게 선택하면 좋아요.

CSS Chunking을 활용해서 효율적인 스타일 로딩 전략을 세워보세요! 웹사이트가 한층 더 빨라질 거예요.

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

## 옵션 설명

- **true (기본값)**  
  Next.js는 가능한 한 CSS 파일을 병합하려고 시도해요. 파일 간의 명시적, 암묵적인 의존성을 import 순서에서 파악해서 CSS 청크(chunk)의 수를 줄이고, 결과적으로 요청(request) 수를 줄여주는 방식이에요.

- **false**  
  Next.js가 CSS 파일을 병합하거나 재정렬하지 않아요. 각 파일이 독립적으로 로드됩니다.

- **`strict`**  
  CSS 파일을 임포트된 순서 그대로 로드해줘요. 이 경우 청크와 요청 수가 늘어날 수 있지만, CSS가 예상치 못한 동작을 할 때 유용할 수 있어요.

---

### 언제 `strict` 모드를 쓸까?

예를 들어 `a.css`와 `b.css`를 서로 다른 파일에서 임포트하는데 순서가 다르다고 가정해봅시다. (어떤 파일에서는 `a.css`를 먼저 임포트하고, 다른 파일에서는 `b.css`를 먼저 임포트하는 경우) 기본 옵션인 `true`는 두 파일을 병합할 때 임포트 순서를 무시하고 병합할 수 있어요. 만약 `b.css`가 `a.css`에 의존한다면, 이 병합이 CSS 적용을 꼬이게 만들 수 있습니다.

이럴 때 `strict` 옵션을 사용하면 임포트 순서를 엄격히 지켜 각 CSS 파일이 실제 임포트된 순서대로 로드됩니다. 대신 청크와 요청 수가 늘어나죠.

---

### 결론 및 추천

대부분의 프로젝트에서는 **`true`(기본값)** 모드를 사용하는 걸 추천해요. 요청 수가 줄어들고 성능 향상에 도움되거든요. 다만, CSS가 복잡하고 의존성이 까다롭다면 `strict` 모드를 시도해보세요.

---

### 추가 팁: CSS 병합 최적화 고려하기

- CSS 파일이 많고, 임포트 순서가 복잡한 상황이라면 의존성 관계를 명확하게 정리해보세요.
- Styled Components, Emotion 같은 CSS-in-JS 라이브러리를 도입하면 이러한 문제를 깔끔하게 해결할 수도 있어요.
- 또한, Next.js 13부터는 app 디렉터리 기반의 새로운 라우팅과 CSS 처리 방법이 제공되니 최신 문서도 참고해 보는 걸 추천드립니다!