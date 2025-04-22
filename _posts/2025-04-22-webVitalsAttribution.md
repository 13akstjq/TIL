---
title: "webVitalsAttribution을 활용한 웹 성능 지표 측정 및 분석 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:34
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "webVitalsAttribution"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/webVitalsAttribution"
isUpdated: false
---


# webVitalsAttribution

웹 바이탈(Web Vitals) 문제를 디버깅할 때, 문제의 원인을 정확히 파악할 수 있다면 훨씬 도움이 되죠. 예를 들어, Cumulative Layout Shift(CLS) 같은 경우, 가장 큰 레이아웃 이동을 일으킨 최초의 요소가 무엇인지 알고 싶을 때가 있어요. 또는 Largest Contentful Paint(LCP)라면, 페이지에서 LCP에 해당하는 요소가 무엇인지 식별하고 싶을 때가 있죠. 특히 LCP 요소가 이미지인 경우, 이미지 리소스의 URL을 알면 어느 자산을 최적화해야 할지 찾기가 훨씬 수월해집니다.

이렇듯 Web Vitals 점수에 가장 큰 영향을 준 요소를 ‘attribution(기여 요소 파악)’이라고 하는데요, 이 기능을 사용하면 PerformanceEventTiming, PerformanceNavigationTiming, PerformanceResourceTiming 같은 좀 더 상세한 실시간 성능 로그 정보를 얻을 수 있습니다.

참고로 Next.js에서는 attribution 기능이 기본적으로 꺼져 있습니다. 필요할 때 특정 메트릭에 대해 아래처럼 next.config.js 파일에 설정해서 켤 수 있어요.

```js
// next.config.js
module.exports = {
  experimental: {
    webVitalsAttribution: ['CLS', 'LCP', 'FID'], // 원하는 메트릭만 지정 가능합니다.
  },
}
```

---

### 추가 팁

- attribution 기능을 켜면 좀 더 상세한 데이터가 들어오기 때문에 성능 분석에 큰 도움이 되지만, 동시에 약간의 오버헤드가 발생할 수 있으니 개발이나 디버깅 환경에서 주로 사용하는 걸 추천해요.
- 다양한 툴(예: Lighthouse, Chrome DevTools 등)에서도 Web Vitals를 측정할 수 있지만, Next.js와 같은 프레임워크에서 attribution을 활용하면 문제의 근원을 더 빠르게 찾는 데 큰 도움이 됩니다.
- LCP 같은 경우 이미지라면 로딩 최적화(예: webp 변환, 사이즈 조절, lazy loading 등)를 우선 고려해보는 것도 좋겠죠?

웹 바이탈 디버깅을 좀 더 똑똑하게 하는 기본 팁으로 기억해두세요!

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

여러분, Next.js에서 웹 성능 지표(Web Vitals)를 추적할 때 `experimental.webVitalsAttribution` 옵션을 사용해보셨나요? 

예를 들어, 아래와 같이 `next.config.js` 파일에 설정할 수 있어요:

```js
module.exports = {
  experimental: {
    webVitalsAttribution: ['CLS', 'LCP'],
  },
}
```

이 설정은 CLS(Cumulative Layout Shift)와 LCP(Largest Contentful Paint) 지표에 대한 Attribution(원인 추적)을 활성화합니다. 

참고로, `webVitalsAttribution`에 들어갈 수 있는 값들은 Next.js가 정의한 `NextWebVitalsMetric` 타입에 명시된 모든 웹 바이탈(Web Vitals) 메트릭들이에요. 대표적으로는 다음과 같답니다:

| 메트릭           | 설명                                         |
| ---------------- | -------------------------------------------- |
| CLS              | 누적 레이아웃 이동 (시각적 안정성)            |
| LCP              | 가장 큰 콘텐츠 페인트 (페이지 로딩 성능)       |
| FID              | 첫 입력 지연 (사용자 반응성)                   |
| FCP              | 첫 콘텐츠 페인트 (첫 번째 의미 있는 화면 출력) |
| TTFB             | 첫 바이트까지 시간 (네트워크 지연 측정)        |

이 설정은 아직 'experimental' 단계라서, 지속적으로 업데이트될 수 있다는 점 참고해주세요. 그리고 웹 바이탈 지표 중 어떤 것이 내 앱에 가장 중요한지 고민해보고, 필요한 것만 골라서 추적하는 게 좋아요.

혹시 이 옵션을 활성화하면, Next.js가 어떻게 성능 데이터를 수집하고, 어디에 어떻게 보여주는지 궁금하다면 알려주세요! 다음글에서는 실제 수집된 성능 데이터를 분석하고 최적화하는 방법에 대해 같이 얘기해 볼게요.