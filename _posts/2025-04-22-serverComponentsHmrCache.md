---
title: "Next.js 15 서버 컴포넌트 HMR 캐시 동작 원리와 최적화 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:25
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "serverComponentsHmrCache"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/serverComponentsHmrCache"
isUpdated: false
---


# serverComponentsHmrCache 옵션 알아보기

안녕하세요! 오늘은 Next.js에서 실험적으로 제공하는 `serverComponentsHmrCache` 옵션에 대해 알려드릴게요. 이 옵션은 로컬 개발 환경에서 서버 컴포넌트(Server Components)의 `fetch` 응답을 HMR(Hot Module Replacement, 핫 모듈 교체) 과정 중에 캐싱해줘서, API 호출 비용도 줄이고 응답 속도도 빠르게 해준답니다.

---

## serverComponentsHmrCache가 뭘까?

기본적으로 개발 중에 코드를 수정하면 HMR 기능이 작동하면서 줄줄이 새로고침 됐었죠? 그런데 이때마다 서버 컴포넌트에서 fetch 요청이 다시 일어나 느려질 수 있고, 과도한 API 호출로 비용이 발생할 수 있어요.

이 옵션을 켜두면, 로컬에서 HMR 리프레시가 일어날 때 fetch가 반환한 데이터를 캐시에 저장해두고 재활용해요. 그래서 매번 API를 새로 호출하지 않고 빠르게 데이터를 불러올 수 있답니다.

---

## 기본 동작

- HMR 캐시는 모든 fetch 요청에 적용돼요.
- 심지어 `cache: "no-store"` 옵션이 붙은 요청도 캐시가 사용돼서 **새로운 데이터가 바로 반영되지 않을 수 있어요.**
- 단, 브라우저에서 페이지를 이동하거나 전체 새로고침을 하면 캐시는 초기화됩니다.

---

## 이 옵션 끄고 싶으면?

`next.config.js` 파일에 아래처럼 설정하면 돼요:

```js
// next.config.js
module.exports = {
  experimental: {
    serverComponentsHmrCache: false,
  },
}
```

---

## 팁! 알아두면 좋은 점

- 이 옵션은 오로지 로컬 개발 환경에서만 의미 있어요. 실제 배포 시에는 캐시가 적용되지 않습니다.
- HMR 중 데이터가 제대로 갱신되어야 할 때는 끄는 게 좋습니다. 예를 들어, 실시간으로 변하는 데이터를 테스트하거나, 캐시 때문에 최신 상태가 보이지 않을 때요.
- 서버 컴포넌트와 클라이언트 컴포넌트가 섞여 있는 프로젝트에서, 서버 컴포넌트가 데이터 로딩을 담당한다면 속도 향상을 꽤 느낄 수 있어요.

---

요약하자면, `serverComponentsHmrCache` 옵션은 개발 생산성을 올려주고 API 사용 비용을 아껴주는 좋은 도구예요. 다만, 데이터 갱신이 중요한 경우에는 캐시를 조심해서 사용하시고요! 궁금한 점 있으면 댓글로 남겨주세요~

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

Next.js에서 `serverComponentsHmrCache` 설정을 꺼두는 방법에 대해 이야기해볼게요.

```js
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  experimental: {
    serverComponentsHmrCache: false, // 기본값은 true입니다
  },
}

export default nextConfig
```

위 코드는 Next.js 설정에서 `serverComponentsHmrCache`를 `false`로 설정하는 예제인데요. 이 값은 기본적으로 `true`로 되어 있어서 서버 컴포넌트의 핫 모듈 리로딩을 캐시를 활용해 빠르게 처리해줍니다. 하지만 개발 상황이나 특정 버그를 해결할 때 이 캐시를 끄고 싶을 때가 있죠.

### 참고할 만한 팁!

개발할 때 fetch 요청의 캐시 적중(hit)과 실패(miss)를 제대로 추적하고 싶다면 `logging.fetches` 옵션을 켜보세요. 이렇게 하면 콘솔에서 캐시가 잘 작동하는지, 아니면 새로 fetch하는지를 한눈에 볼 수 있어서 디버깅할 때 훨씬 편합니다.

```js
const nextConfig: NextConfig = {
  experimental: {
    logging: {
      fetches: true,
    },
  },
}
```

이 설정을 켜두면 개발 중 네트워크 호출 캐시 상태를 실시간으로 파악할 수 있으니까, fetch가 잘 캐시되고 있나 궁금할 때 도움됩니다.

---

### 추가로 알아두면 좋은 점

- `serverComponentsHmrCache`는 서버 컴포넌트 개발자 경험 향상을 위한 옵션이에요. 이걸 꺼두면 개발 중 일부 반응 속도가 느려질 수 있지만, 환경에 따라서는 문제 해결에 도움이 됩니다.
- Next.js 최신 버전이나 기능 업데이트 때, experimental 옵션이 변할 수도 있으니 공식 문서를 꼭 확인하세요.

이 정도만 알아두면 `experimental` 섹션에서 실험적인 기능 다룰 때 도움이 될 거예요. 행복한 Next.js 개발하세요! 🚀