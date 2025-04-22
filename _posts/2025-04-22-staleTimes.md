---
title: "Nextjs에서 캐싱된 데이터 업데이트 하는 방법(staleTime)"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:27
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "staleTimes"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/staleTimes"
isUpdated: false
---


# staleTimes란?

staleTimes는 약간 실험적인 기능인데요, 클라이언트 사이드 라우터 캐시에서 페이지 세그먼트(segment)들을 캐싱할 수 있게 해줍니다. 쉽게 말해, 페이지를 다시 요청하지 않고도 미리 저장된 데이터를 활용하면서도, 일정 시간이 지나면 새로 데이터를 검증하도록 설정할 수 있다는 거죠.

이 기능을 쓰고 싶다면 Next.js 설정 파일(next.config.js)에서 `experimental` 옵션 안에 `staleTimes`를 이렇게 설정해주면 됩니다:

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    staleTimes: {
      dynamic: 30, // 동적 데이터는 30초마다 재검증
      static: 180, // 정적 데이터는 180초(3분)마다 재검증
    },
  },
}

module.exports = nextConfig
```

즉, 위 설정대로면 **동적 데이터는 30초**, **정적 데이터는 3분** 간 캐시에 보관됐다가 그 이후엔 데이터를 새로 검증합니다.

---

## 좀 더 쉽게 풀어볼게요

- **동적(dynamic)** 데이터는 자주 변할 가능성이 크니까, 캐시 유효기간을 짧게.
- **정적(static)** 데이터는 자주 변하지 않으니, 캐시 유효기간을 길게 잡는 거죠.

이걸 적절히 조절하면, 사용자 입장에서는 페이지가 훨씬 빠르게 로드되면서도, 너무 오래된 데이터가 보여지는 걸 방지할 수 있습니다.

---

## 왜 이 기능이 필요할까?

보통 Next.js에서 데이터 패칭(fetch) 후 렌더링할 때, 매번 서버에 새로 요청하면 속도가 느려지거나 서버 부담이 커질 수 있어요. 여기에 staleTimes를 활용하면, 어느 정도 기간 동안은 캐시된 내용을 쓸 수 있으니 성능 최적화에 크게 도움이 됩니다.

---

## 참고로!

- 아직 experimental 단계라서 Next.js 버전에 따라 동작 방식이 바뀔 수도 있고, 완전 안정적이지 않을 수도 있어요.
- 실제 프로젝트에서 도입할 땐, 충분히 테스트해서 데이터 신선도와 퍼포먼스 사이 균형을 잘 맞춰보세요.

staleTimes를 적용해서 캐시 전략을 잘 세워보면, 사용자 경험 향상에 큰 도움이 될 거예요!

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

링크 프리페칭(prefetching)에 따라 정적(static)과 동적(dynamic) 속성이 시간 단위(초)로 어떻게 적용되는지 알아볼게요.

| 속성       | 설명                                                                                              | 기본값                |
|------------|-------------------------------------------------------------------------------------------------|-----------------------|
| 동적 (dynamic)  | 페이지가 정적으로 생성되지 않았거나, 완전히 프리페치 되지 않은 경우 사용돼요. 예를 들어 `prefetch='true'`가 아닐 때요.      | 0초 (캐시 안 함)        |
| 정적 (static)   | 정적으로 생성된 페이지거나, Link 컴포넌트의 `prefetch` 속성이 `true`일 때, 또는 `router.prefetch`를 호출할 때 사용해요.  | 5분 (300초)             |

### 한마디 더!
- **로딩 경계(loading boundaries)는 설정된 정적 기간 동안 재사용 가능**하답니다.  
- 부분 렌더링에 영향을 미치지 않아요. 즉, 공유 레이아웃(shared layouts)은 네비게이션할 때마다 자동 재페칭되지 않고, 변경된 페이지만 다시 불러옵니다.  
- 백/포워드 캐시 동작에는 영향을 주지 않아서 레이아웃 변경이나 브라우저 스크롤 위치 유지에도 문제없어요!

이해를 돕기 위해 쉽게 말하자면, 정적 속성은 이미 예측해서 안전하다고 판단한 페이지를 일정 시간 동안 캐시에 저장해두는 것이고, 동적 속성은 그런 캐싱 없이 매번 새로 불러오는 방식이에요.

React 기반의 라우터나 Next.js 같은 프레임워크에서 클라이언트 사이드 라우팅 성능 최적화를 할 때 이런 프리페칭 설정이 굉장히 중요합니다. 캐시 기간을 어떻게 조절하느냐에 따라 유저 경험과 페이지 로딩 속도에 큰 차이가 나기 때문이죠.

추가로, Client Router Cache에 대해 더 자세히 알고 싶다면 공식 문서를 참고하는 걸 추천해요! 해당 기능이 어떻게 작동하고 구성 요소들이 어떻게 상호작용하는지 이해하는 데 큰 도움이 될 거예요.

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

### 버전 히스토리

| Version  | Changes                                  |
| -------- | ---------------------------------------- |
| v15.0.0  | `dynamic`한 `staleTimes` 기본값이 30초에서 0초로 변경됨 |
| v14.2.0  | 실험 중인 `staleTimes` 기능이 도입됨               |

---

여기서 `staleTimes`라는 건 데이터를 캐싱할 때 얼마나 오래 신선하다고 판단할지를 정하는 시간이에요. v14.2.0 버전부터 실험적으로 적용되었고, v15.0.0에서는 기본값이 30초에서 0초로 바뀌어서 캐시된 데이터가 바로 오래된 것으로 간주된다는 의미랍니다.

이 기능이 바뀌면서 데이터의 최신성을 더 중요하게 생각하는 환경에서는 유용할 수 있어요. 물론, 너무 짧으면 매번 데이터를 새로 받아오게 돼서 네트워크 비용이 늘어나니 상황에 맞게 잘 조절하는 게 필요해요. 나중에 이 부분을 직접 조정해보면서 최적화하는 것도 꽤 재밌는 작업이 될 거예요!