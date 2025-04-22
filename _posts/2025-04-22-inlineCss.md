---
title: "Next.js 15에서 Inline CSS 작성하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:14
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "inlineCss"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/inlineCss"
isUpdated: false
---


# inlineCss

## 사용 방법

`inlineCss`는 Next.js에서 CSS를 외부 링크(`link` 태그) 대신에 내부 스타일(`style` 태그)로 인라인 처리해주는 실험적 기능이에요. 이걸 켜면, 보통 CSS 파일을 불러올 때 사용하는 `link` 태그 대신 `head` 태그 내에 직접 `style` 태그를 생성해서 CSS를 넣어줍니다.

### 왜 이 기능을 쓸까?

- **초기 렌더링 속도 개선**: 외부에서 CSS 파일을 불러오지 않고 바로 HTML 문서 안에 CSS가 들어가 있어서, 스타일 로딩에 걸리는 시간이 줄어들 수 있어요.
- **더 나은 퍼포먼스**: 네트워크 요청 수가 줄어드니 실제 렌더링 속도가 더 빨라질 가능성이 큽니다.
- **적용법도 간단**해요! next.config.js에 아래처럼 설정만 해주면 끝~

```js
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  experimental: {
    inlineCss: true,
  },
}

export default nextConfig
```

### 주의할 점!

- 아직 **실험적인 기능**이라서, 프로젝트에 바로 적용하기 전에 꼼꼼히 테스트해보는 걸 추천해요.
- 스타일 규모가 아주 크거나 CSS 파일이 많이 분할된 경우, HTML 문서 크기가 커질 수 있어서 오히려 성능에 악영향을 줄 수도 있어요.
- 자바스크립트 번들 사이즈와 별개로, CSS가 HTML에 포함되므로 SEO 측면에서는 스타일이 빨리 적용되어 긍정적일 수 있습니다.

Next.js에서 이런 기능들이 점점 늘어나면서, 우리는 더 편하게 최적화된 웹사이트를 만들 수 있죠! 혹시 CSS 인라인 처리에 관심 있으면 꼭 한 번 테스트해보세요. 도움이 필요하면 언제든 물어봐 주세요!

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

## 장단점 비교: 언제 인라인 CSS를 사용할까?

인라인 CSS를 사용하면 좋은 경우가 몇 가지 있어요. 간단히 정리해볼게요!

| 상황 | 설명 |
|---|---|
| **첫 방문자** | CSS 파일은 렌더링을 차단하는 리소스라서, 외부 CSS를 불러올 때 처음 방문자는 로딩 지연을 겪어요. 이때 CSS를 인라인으로 넣으면 처음부터 스타일이 바로 적용돼서 페이지 로드 속도가 확 좋아집니다. |
| **성능 지표 개선** | 네트워크 요청 횟수를 줄이면 FCP(First Contentful Paint)나 LCP(Largest Contentful Paint)같은 중요한 웹 성능 지표가 대폭 상승하죠. 인라인 CSS가 이 부분에서 효과적입니다. |
| **느린 네트워크 환경** | 와이파이가 약하거나 모바일 데이터 연결이 느릴 때, 네트워크 라운드 트립(왕복시간)이 꽤 크게 느껴져요. 인라인 CSS는 이런 환경에서 성능 향상이 두드러집니다. |
| **아토믹 CSS 번들 (예: Tailwind CSS)** | Tailwind처럼 유틸리티 중심의 CSS 프레임워크는 페이지에 필요한 스타일 양이 디자인 복잡도와 크게 무관해요. 즉, 스타일의 전체 크기가 작고 일정하다는 말인데요, 그래서 인라인으로 넣으면 네트워크 요청도 줄이고 페이로드도 가볍게 유지할 수 있어요! |

---

### 추가로 알아두면 좋은 팁!

- 인라인 CSS는 초반 로딩에 장점이 있지만, 너무 많은 스타일을 인라인으로 넣으면 HTML 파일 크기가 커져서 오히려 로드가 느려질 수 있어요. 적당한 선에서 재사용성이 낮거나 꼭 필요한 스타일만 넣는 게 좋아요.
- 그리고 자주 바뀌는 스타일에는 별도의 CSS 파일을 사용하는 게 캐싱 측면에서 유리합니다. 인라인 스타일은 캐시가 어려워서 매번 새로 받아야 하거든요.
- 결국 사용자 경험과 유지보수 편의성, 성능 지표 사이의 '적절한 균형'을 찾아야 하는 게 관건입니다.

이런 점들 참고해서 프로젝트 상황에 맞게 인라인 CSS를 써보세요! 배운 만큼 웹 페이지가 확실히 빨라지고 사용자 만족도도 올라갈 거예요.

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

### 언제 인라인 CSS를 피해야 할까?

인라인 CSS는 성능 향상에 꽤 도움이 되지만, 상황에 따라선 꼭 좋은 선택이 아닐 수 있어요. 어떤 경우인지 한번 살펴볼게요.

| 상황 | 이유 및 설명 |
|-------|-------------|
| **큰 CSS 번들** | CSS 파일이 너무 크면, 인라인으로 모두 넣을 경우 HTML 파일 크기가 크게 늘어나요. 그러면 초기 응답 시간(Time to First Byte, TTFB)이 느려지고, 인터넷 속도가 느린 사용자에겐 오히려 성능 저하로 이어질 수 있어요. |
| **동적 혹은 페이지별 CSS** | 페이지마다 스타일이 많이 달라지고, CSS가 동적으로 변하는 경우 인라인 CSS가 중복되고 과도하게 커질 수 있어요. 이렇게 되면 관리도 어렵고, 전반적인 로딩 효율도 떨어져요. |
| **브라우저 캐싱 활용 시** | 다시 방문하는 사용자가 많을 땐 외부 CSS 파일이 유리해요. 브라우저가 파일을 캐시해서 재접속 때 파일을 다시 다운로드하지 않아도 되니까 데이터 절약도 되고 빠르죠. 인라인은 이 캐싱 이점을 포기하는 셈이에요. |

---

내 개인적인 팁을 하나 덧붙이면, 단순히 인라인 CSS냐 아니냐로만 고민하기보단, 중요한 부분(Critical CSS)만 인라인 하고 나머지는 외부 stylesheet로 분리하는 ‘하이브리드’ 전략을 쓰면 좋아요. 이렇게 하면 첫 화면 렌더링 속도를 잡으면서도 캐싱의 장점도 누릴 수 있거든요.

여러분이 만드는 사이트나 앱의 특성에 맞게, 이런 장단점을 꼼꼼히 따져보시면서 유연하게 적용해 보세요!

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

> 참고할 점:
이 기능은 현재 실험 단계이며 몇 가지 알려진 제한 사항이 있어요:
- CSS 인라인 처리는 전역적으로 적용되며, 개별 페이지별 설정은 불가능해요.
- 초기 페이지 로드 시 스타일이 두 번 중복돼요. 한 번은 SSR용 `style` 태그 안에, 또 한 번은 RSC 페이로드 안에 포함돼요.
- 정적으로 렌더링된 페이지로 이동할 때는 중복을 피하기 위해 인라인 CSS 대신 `link` 태그를 사용해요.
- 이 기능은 개발 모드에서는 사용할 수 없고, 프로덕션 빌드에서만 작동해요.

---

조금 더 풀어서 설명해볼게요. 이 기능은 웹사이트의 성능 최적화를 위해 CSS를 HTML 내부에 직접 넣는 방식(인라인 CSS)을 사용해요. 인라인 CSS는 초기 로딩 속도를 높여주고, 스타일이 빠르게 적용되는 장점이 있죠. 다만 아직 실험적인 기능이라서, 스타일 중복 문제나 상황에 따라 인라인 대신 외부 링크를 사용하는 식으로 처리하는 등 몇 가지 제한이 있어요.

개발할 때는 이 기능을 동작시키기 힘드니, 실제 프로덕션 환경에서만 효과를 확인하는 게 좋아요. 앞으로 개선되면서 더 유연한 설정이 가능해지길 기대해봅니다.