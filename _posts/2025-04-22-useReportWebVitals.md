---
title: "Next.js 15에서 useReportWebVitals로 웹 성능 데이터 측정하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:51
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "useReportWebVitals"
link: "https://nextjs.org/docs/app/api-reference/functions/use-report-web-vitals"
isUpdated: false
---


# useReportWebVitals 훅 사용법

이번에는 Next.js에서 제공하는 `useReportWebVitals` 훅에 대해 알아볼게요. 이 훅은 웹사이트의 **Core Web Vitals**(핵심 웹 성능 지표)를 쉽게 측정하고, 이를 여러분이 사용하는 분석 도구나 커스텀 로그에 연동할 수 있도록 도와줘요.

### Core Web Vitals가 뭐냐고요?

간단히 말해, Core Web Vitals는 사용자 경험과 직결되는 성능 지표들입니다. 페이지 로딩 속도, 반응성, 시각적 안정성 등을 측정해서 구글 검색 순위에도 영향을 주죠. 그러니까 꼭 체크해야 하는 중요한 수치예요!

---

## 기본 사용법

```js
'use client'

import { useReportWebVitals } from 'next/web-vitals'

export function WebVitals() {
  useReportWebVitals((metric) => {
    console.log(metric)
  })

  return null
}
```

- `useReportWebVitals` 훅 안에 넘기는 콜백 함수에서 각 지표(metric)를 받아서 처리할 수 있어요.
- 여기선 간단하게 `console.log`로 결과를 확인하고 있지만, 이 자리에 GA(Google Analytics) 같은 외부 분석도구 연동 코드를 넣으면 됩니다.

---

## Layout에 적용하기

```js
import { WebVitals } from './_components/web-vitals'

export default function Layout({ children }) {
  return (
    <html>
      <body>
        <WebVitals />
        {children}
      </body>
    </html>
  )
}
```

- 일반적으로 페이지의 루트 컴포넌트나 레이아웃에 `WebVitals` 컴포넌트를 넣어서 전체 페이지에서 웹 바이탈 지표를 수집할 수 있도록 설정합니다.

---

## 추가 팁

- Core Web Vitals 값에는 **LCP(Largest Contentful Paint)**, **FID(First Input Delay)**, **CLS(Cumulative Layout Shift)** 등이 포함돼요.
- 지표 값을 서버로 보내려면 `fetch`나 `axios`를 활용해서 API에 전달하면, 실시간으로 사용자 경험 데이터를 모니터링할 수 있답니다.
- 만약 GA4를 쓰시는 분들에겐 [`gtag.event`를 활용하는 방법](https://developers.google.com/analytics/devguides/collection/gtagjs/events)도 추천해요.

---

이처럼 Next.js에서 기본으로 제공하는 `useReportWebVitals` 훅을 통해 간단하게 Core Web Vitals를 측정하고, 여러분만의 분석 체계에 연동해 보세요! 사용자의 체감 성능을 향상시키는 데 큰 도움이 될 거예요 :)

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

"useReportWebVitals" 훅을 사용할 때는 "use client" 디렉티브가 필요해요. 그래서 가장 효율적인 방법은, 이 훅을 사용하는 별도의 컴포넌트를 만들어서 루트 레이아웃에서 불러오는 거예요. 이렇게 하면 클라이언트 경계(client boundary)를 WebVitals 컴포넌트에만 딱 제한할 수 있어 성능 관리에도 도움이 되죠.

## useReportWebVitals 훅이 전달받는 metric 객체

이 훅에 넘겨주는 metric 객체에는 다음과 같은 여러 프로퍼티가 있어요:

| 프로퍼티명       | 설명                                                                                       |
|----------------|------------------------------------------------------------------------------------------|
| id             | 현재 페이지 로드 컨텍스트에서의 고유 식별자입니다.                                            |
| name           | 성능 지표의 이름이에요. 주로 웹 바이탈(Web Vitals) 지표들이고, TTFB, FCP, LCP, FID, CLS 등이 있어요.  |
| delta          | 이번 측정값과 이전 측정값 차이로, 보통 밀리초 단위입니다. 시간에 따라 얼마나 바뀌었는지 알려주죠.         |
| entries        | 이 성능 지표와 관련된 Performance Entry들의 배열입니다. 상세한 성능 이벤트 정보를 포함하고 있어요.       |
| navigationType | 측정을 트리거한 네비게이션 유형입니다. "navigate", "reload", "back_forward", "prerender" 중 하나가 될 수 있어요. |
| rating         | 성능 지표의 질적 평가로, "good", "needs-improvement", "poor" 중 하나입니다. 미리 정해진 임계값과 비교해 성능 상태를 보여주죠. |
| value          | 실제 측정값으로 보통 밀리초 단위의 성능 수치이며, 측정하는 지표에 따라 다양한 Performance API에서 가져옵니다. |

이렇게 상세한 정보가 있어서, 개발자가 웹사이트의 성능 상태를 구체적으로 파악하고 개선 작업을 할 때 큰 도움이 됩니다. 예를 들어, 사용자 경험에 중요한 LCP(최대 콘텐츠 표시 시간)나 FID(첫 입력 지연) 같은 지표를 실시간으로 모니터링하고, 문제가 있을 때 알림을 줄 수도 있죠.

또한, "navigationType" 덕분에 페이지 이동인지 새로고침인지, 혹은 뒤로가기인지 등 상황별 구분도 가능해 어떤 상황에서 성능 문제가 발생하는지 꼭 체크해야 할 부분이에요.

개발할 때 이 훅을 활용하면 사용자의 실제 경험에 가까운 성능 데이터를 얻을 수 있어요. 전통적인 Lighthouse 점수나 Lab 데이터보다 실제 사용자 환경을 반영하는 리얼월드 데이터 측정에 강력하니 참고하면 좋아요!

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

## Web Vitals란?

Web Vitals는 웹 페이지의 사용자 경험(User Experience, UX)을 측정하기 위해 만들어진 중요한 지표들의 모음이에요. 이 지표들을 통해 사용자가 웹사이트를 이용할 때 느끼는 체감 속도나 인터랙션의 원활함을 객관적으로 평가할 수 있죠.

대표적인 Web Vitals 지표들은 다음과 같아요:

| 지표명                | 설명                                     |
|---------------------|----------------------------------------|
| Time to First Byte (TTFB) | 서버가 첫 번째 바이트를 보내기까지 걸리는 시간                 |
| First Contentful Paint (FCP) | 페이지 내에서 처음으로 텍스트나 이미지 같은 콘텐츠가 렌더링 되는 시간  |
| Largest Contentful Paint (LCP) | 페이지 내에서 가장 큰 콘텐츠(보통 메인 이미지나 텍스트 블록)가 렌더링 되는 시간 |
| First Input Delay (FID)   | 사용자가 처음으로 페이지와 인터랙션할 때 발생하는 지연 시간             |
| Cumulative Layout Shift (CLS) | 페이지 렌더링 중 레이아웃이 얼마나 많이 이동하는지를 나타내는 누적 값      |
| Interaction to Next Paint (INP) | 사용자가 어떤 인터랙션을 했을 때 다음 렌더링이 시작될 때까지 걸리는 시간     |

이 지표들을 한 번에 관리할 때는 보통 `name` 프로퍼티를 활용해 각 메트릭을 구분해서 다룹니다.

---

### 조금 더 알아보기

- **TTFB**는 서버와 네트워크 상태에 크게 영향을 받기 때문에, 백엔드 최적화나 CDN 사용으로 개선할 수 있어요.
- **FCP**와 **LCP**는 실제 사용자가 보는 화면이 얼마나 빨리 뜨는지 보여줘서, 프론트엔드 최적화 포인트를 잡을 때 아주 유용하답니다.
- **CLS** 지표가 중요한 이유는, 페이지가 갑자기 팍! 움직이면 사용자 경험이 크게 떨어지기 때문이에요. 예를 들어, 광고 배너나 이미지가 늦게 로딩되면서 텍스트가 휙 움직이는 현상이 이에 해당하죠.
- **FID**와 **INP**는 사용자의 ‘반응성’을 보여주는 지표로, 버튼 클릭이나 입력 도중 느껴지는 딜레이를 줄일 때 활용해요.

웹 페이지 성능을 개선하려면 이 Web Vitals 지표들을 꾸준히 모니터링하고, 사용자 입장에서 어떤 부분이 불편한지 파악하는 게 중요해요. 구글 크롬의 Lighthouse 나 웹사이트 성능 측정 도구들을 활용하면 쉽게 확인할 수 있으니 꼭 써보시길 추천드립니다!

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
'use client'

import { useReportWebVitals } from 'next/web-vitals'

export function WebVitals() {
  useReportWebVitals((metric) => {
    switch (metric.name) {
      case 'FCP': {
        // FCP 결과를 처리하는 로직을 여기에 작성해보세요
        break
      }
      case 'LCP': {
        // LCP 결과를 처리하는 로직을 여기에 작성해보세요
        break
      }
      // 필요한 다른 Web Vitals 항목들도 추가할 수 있어요
    }
  })
}
```

## Vercel에서의 사용법

Vercel에서는 `useReportWebVitals` 훅 대신에 `@vercel/speed-insights` 패키지를 사용해 웹 성능을 측정하고 있어요. 그래서 만약 여러분이 Vercel 환경에서 작업 중이라면 이 패키지를 활용하는 게 더 적합할 수 있습니다. 하지만 로컬 개발 환경이나 다른 웹 성능 측정 서비스를 이용하는 경우에는 `useReportWebVitals` 훅이 매우 유용하답니다.

## 외부 시스템으로 결과 전송하기

웹 바이탈(Web Vitals) 데이터를 수집하는 것만으로는 충분하지 않죠. 이 데이터를 실제로 활용하려면 외부 모니터링 시스템(예: Google Analytics, Sentry, 직접 만든 서버 등)으로 보내야 합니다.

예를 들어, 간단히 Google Analytics에 보내는 코드는 다음과 같아요:

```js
useReportWebVitals((metric) => {
  window.gtag('event', metric.name, {
    event_category: 'Web Vitals',
    value: Math.round(metric.value),
    event_label: metric.id,
    non_interaction: true,
  })
})
```

- `metric.name`은 FCP, LCP, CLS 같은 웹 바이탈 이름이에요.
- `metric.value`는 측정된 값입니다.
- `metric.id`는 각각의 측정에 부여된 고유 ID인데, 중복 계산을 막는 데 도움이 돼요.
- `non_interaction: true`로 설정하면 사용자 세션에 영향을 않끼칩니다.

> 참고로, Web Vitals를 수집할 때는 네트워크 성능이나 사용자 경험에 영향을 최소화하기 위해 비동기로 데이터를 보내는 것이 좋아요.

---

**Tip!**  
Next.js 13부터는 `useReportWebVitals`를 `client component`에서만 사용할 수 있으니, 올바른 환경에서 사용하는지 꼭 확인하세요!

---

앞으로 웹 성능 측정 데이터를 잘 활용하면 사용자 경험을 크게 향상시킬 수 있으니 꼭 한번 적용해보시길 추천드려요! 😊

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

사이트 성능 측정, 즉 실제 사용자의 경험을 추적하고 싶을 때, 측정 결과를 원하는 서버로 전송할 수 있어요. 예를 들어, `useReportWebVitals` 훅에서 받은 메트릭 데이터를 서버에 보내는 코드는 아래와 같이 작성할 수 있답니다.

```js
useReportWebVitals((metric) => {
  const body = JSON.stringify(metric);
  const url = 'https://example.com/analytics';

  // `navigator.sendBeacon()` API가 있으면 사용하고,
  // 없으면 fetch()로 대체해요.
  if (navigator.sendBeacon) {
    navigator.sendBeacon(url, body);
  } else {
    fetch(url, { body, method: 'POST', keepalive: true });
  }
});
```

**팁!** 만약 Google Analytics(구글 애널리틱스)를 사용한다면, 수집한 메트릭의 `id` 값을 활용해서 퍼센타일 같은 상세한 통계값도 직접 계산할 수 있어요. 이렇게 하면 데이터 분석에 더 유용하답니다.

그리고, 구글 애널리틱스가 초기화되어있다면 `window.gtag` 함수로 바로 이벤트를 전송하는 방법도 있어요. 예를 들어:

```js
useReportWebVitals((metric) => {
  window.gtag('event', metric.name, {
    value: Math.round(metric.name === 'CLS' ? metric.value * 1000 : metric.value), // 값은 정수여야 해요
    event_label: metric.id, // 이 값은 페이지 로드마다 고유해요
    non_interaction: true, // 방문자 이탈률에 영향을 미치지 않아요
  });
});
```

여기서 CLS(누적 레이아웃 이동) 값은 소수점이고, 구글 애널리틱스 이벤트 값은 정수만 받기 때문에 곱하기 1000을 해서 변환한 점이 포인트예요.

### 참고할 점
- `navigator.sendBeacon()`은 사용자의 페이지가 언로드(unload)되어도 데이터를 신뢰성 있게 보내줘서, 웹 성능 측정에 아주 적합합니다.
- `fetch()`를 사용할 땐 `keepalive: true` 옵션을 붙여서, 페이지가 닫히거나 이동해도 전송 중단이 덜 하도록 설정합니다.
- Google Analytics의 `gtag`는 따로 라이브러리를 로드하거나 스니펫을 설치해야 동작하니, 사전에 환경 세팅이 필요해요.

실제 운영 중인 사이트에서 사용자 경험을 꾸준히 모니터링하고 싶다면, 위 코드를 응용해서 나만의 데이터 파이프라인을 구축해보는 것도 추천드려요! 다양한 지표를 모으면 성능 개선 포인트도 한눈에 보이고, 결과적으로 방문자 만족도도 높일 수 있답니다.