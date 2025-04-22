---
title: "Next.js 15에서 ImageResponse를 활용해 이미지 생성 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:41
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "ImageResponse"
link: "https://nextjs.org/docs/app/api-reference/functions/image-response"
isUpdated: false
---


# ImageResponse 사용법 간단 정리

Next.js의 `ImageResponse`는 JSX와 CSS를 활용해 동적인 이미지를 만들어낼 수 있는 아주 멋진 기능이에요. 특히 소셜미디어용 이미지, 예를 들어 Open Graph 이미지나 트위터 카드 이미지를 자동으로 생성할 때 많이 쓰이죠.

---

## ImageResponse 생성자 옵션

```js
import { ImageResponse } from 'next/og'

new ImageResponse(
  element: ReactElement,
  options: {
    width?: number = 1200, // 이미지 가로 크기 (기본 1200px)
    height?: number = 630, // 이미지 세로 크기 (기본 630px)
    emoji?: 'twemoji' | 'blobmoji' | 'noto' | 'openmoji' = 'twemoji', // 이모지 스타일 선택 가능
    fonts?: {
      name: string,        // 폰트 이름
      data: ArrayBuffer,   // 폰트 데이터 (ArrayBuffer 포맷)
      weight: number,      // 폰트 두께
      style: 'normal' | 'italic' // 폰트 스타일 지정
    }[],
    debug?: boolean = false, // 디버그 모드 활성화 여부

    // HTTP 응답에 쓰일 추가 옵션들
    status?: number = 200,       // HTTP 상태 코드
    statusText?: string,         // 상태 메시지
    headers?: Record<string, string> // 헤더 정보
  },
)
```

---

## 간단 설명

- **element**: JSX 요소를 넘겨주면 그걸로 이미지를 만들어냅니다.
- **width / height**: 생성될 이미지의 크기를 결정합니다. 보통 소셜 미디어용 이미지 사이즈로 1200x630을 많이 사용해요.
- **emoji**: 이모지 스타일을 지정할 수 있어요. 원하는 이모지 스타일을 골라서 좀 더 개성있는 이미지를 만들어보세요.
- **fonts**: 원하는 커스텀 폰트를 불러와서 쓸 수도 있습니다. ArrayBuffer 포맷으로 폰트 데이터를 준비해야 하는데, 웹에서 폰트를 직접 임포트해서 사용하는 것과는 조금 다르니 참고하세요.
- **debug**: 이미지 생성할 때 콘솔이나 에러 정보가 더 많이 보고 싶다면 true로 설정하면 됩니다.
- **status / statusText / headers**: HTTP 응답 설정용. 이미지 API를 만들 때 상태 코드를 따로 지정하거나 헤더를 붙이고 싶을 때 사용해요.

---

## 추가 팁!

- 커스텀 폰트를 적용하려면 미리 폰트를 ArrayBuffer로 변환하는 과정이 필요해요. 예를 들어, `fetch`로 폰트 파일을 받은 뒤 `arrayBuffer()`를 호출해서 준비하면 됩니다.
- 이모지 스타일을 바꾸면 이미지 분위기가 확 달라져서, 브랜딩에 맞게 적용하면 좋아요.
- 디버그 모드를 켜면 어떤 CSS나 요소가 문제인지 더 쉽게 파악할 수 있어서 개발 초반에 유용해요.

---

Next.js에서 동적 이미지 생성하는 게 필요하다면 `ImageResponse` 한 번 꼭 써보세요! 특히 자동으로 OG 이미지를 만들어주는 환경에서는 시간을 엄청 아껴준답니다.

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

## 지원하는 CSS 속성들

Satori의 문서를 참고하면 어떤 HTML과 CSS 기능들이 지원되는지 자세히 확인할 수 있어요. 직접 사용해보면서 어떤 스타일들이 잘 먹히는지도 체크해보시면 좋습니다.

## 버전 히스토리

| 버전       | 변경 사항                                                      |
|------------|---------------------------------------------------------------|
| `v14.0.0`  | `ImageResponse`가 `next/server`에서 `next/og`로 이동되었습니다.   |
| `v13.3.0`  | `ImageResponse`를 `next/server`에서 임포트할 수 있게 되었습니다.  |
| `v13.0.0`  | `@vercel/og` 패키지를 통해 `ImageResponse`가 도입되었습니다.       |

요즘 Next.js에서 이미지 관련 기능들이 점점 발전하고 있는데요, 특히 `ImageResponse` 같은 기능은 동적으로 이미지를 생성할 때 굉장히 편리해요. 다만 버전에 따라서 임포트 경로나 패키지가 달라질 수 있으니 사용 전 문서를 한번만 꼭 확인하는 것을 추천합니다. 이 부분을 미리 숙지해두면 버전 업그레이드 시에 불필요한 에러를 줄일 수 있거든요!