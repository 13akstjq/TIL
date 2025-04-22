---
title: "Next.js 15에서 expireTime 설정으로 세션 만료 관리하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:07
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "expireTime"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/expireTime"
isUpdated: false
---


# expireTime 설정하기

이번에는 Next.js에서 ISR(Incremental Static Regeneration) 기능을 사용할 때 CDN이 참고할 수 있도록 Cache-Control 헤더에 `stale-while-revalidate` 만료 시간을 직접 지정하는 방법에 대해 알아볼게요.

### expireTime은 뭐예요?

`expireTime`은 ISR이 작동할 때, CDN이나 브라우저가 캐시된 페이지를 얼마나 오래 '신선한' 상태로 간주할지 초 단위로 지정하는 값이에요. 이 시간이 지나면, 캐시된 페이지는 'stale'(오래된) 상태가 되고, 사용자 요청 시 백엔드에서 페이지를 재생성하면서 동시에 이전 페이지를 제공하게 됩니다. 이렇게 하면 사용자 경험도 부드럽고, 신선한 컨텐츠도 유지할 수 있죠.

### 설정 방법

`next.config.js` 파일에 아래처럼 `expireTime` 옵션을 추가하면 됩니다.

```js
module.exports = {
  // 캐시 유효 시간: 1시간(3600초)
  expireTime: 3600,
}
```

위 예시는 1시간 동안 캐시된 페이지를 신선한 상태로 유지한다는 의미예요. 필요에 따라 이 시간을 더 짧게 설정해 자주 업데이트되는 페이지에는 빠른 갱신을, 변동이 적은 페이지는 좀 더 길게 설정해서 효율적으로 캐시를 활용할 수 있습니다.

---

### 추가 팁!

- `stale-while-revalidate` 전략은 사용자에게 빠른 응답을 주면서도 백그라운드에서 페이지를 재생성하므로, ISR을 적극 활용하는 사이트라면 꼭 캐시 시간도 신경 써주는 게 좋아요.
- CDN마다 이 헤더를 어떻게 해석하는지 조금씩 다를 수 있으니, 사용하는 CDN 문서도 한 번씩 확인해보는 걸 추천합니다.
- `expireTime`을 너무 길게 하면 업데이트가 늦어질 수 있고, 너무 짧으면 오히려 백엔드에 부하가 걸릴 수 있으니 적당한 균형을 찾아보세요.

이제 `expireTime`으로 한층 똑똑하고 빠른 ISR 환경을 만들어 보세요!

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

이번에 Cache-Control 헤더를 보낼 때, 만료 시간이 단순히 고정된 값이 아니라 특정 재검증(revalidate) 기간에 맞춰서 계산된다는 사실을 알려줄게요.

예를 들어, 어떤 경로(path)에 대해 재검증 기간을 15분(900초)으로 설정했고, 만료 시간은 1시간(3600초)으로 정했다면, 생성되는 Cache-Control 헤더는 이렇게 될 거예요:

| 헤더값                  | 설명                                                   |
|-------------------------|--------------------------------------------------------|
| s-maxage=900            | 캐시된 응답을 15분간(900초) 동안 새로 고침 없이 사용   |
| stale-while-revalidate=2700 | 만료 후에도 45분간(3600 - 900 = 2700초) 캐시된 콘텐츠를 '오래된 상태로' 사용하며 백그라운드에서 재검증 수행 |

여기서 중요한 점은 stale-while-revalidate 기간이 만료 시간에서 재검증 시간을 뺀 값이라는 거예요. 즉, 캐시는 만료되기 전 15분 동안은 완벽히 신선한 상태로, 그 이후 45분 동안은 다소 '오래된' 상태로 사용자에게 제공되면서 백그라운드에서 새로운 데이터를 가져와서 캐시를 업데이트하는 방식입니다.

이렇게 하면 사용자 입장에서는 캐시가 만료되자마자 바로 느려지는 게 아니라, 조금은 오래된 데이터로도 빠르게 응답받으면서 동시에 서버에서는 캐시를 새로 고치는 작업을 할 수 있어서 UX가 훨씬 개선돼요.

추가로, Cache-Control 헤더를 제대로 활용하면 CDN이나 프록시 서버에서 캐시 관리가 훨씬 효율적이니 꼭 이 부분을 이해하고 적용해보시길 추천합니다!