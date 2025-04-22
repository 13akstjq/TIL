---
title: "Next.js 15에서 cacheLife 설정하는 방법 및 캐시 수명 관리 방법 정리"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:57
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "cacheLife"
link: "https://nextjs.org/docs/app/api-reference/functions/cacheLife"
isUpdated: false
---

# cacheLife 함수 알아보기

안녕하세요, 개발 블로거입니다! 오늘은 `cacheLife`라는 함수에 대해 이야기해보려고 해요. 이 함수는 함수나 컴포넌트의 캐시 수명을 설정하는 데 쓰여요. 캐시 수명이란, 쉽게 말해 '데이터를 얼마나 오래 저장해서 재사용할지'를 결정하는 시간이죠.

이 `cacheLife` 함수는 `use cache` 지시어와 함께 사용해야 하며, 반드시 함수나 컴포넌트의 범위 안에서 호출해야 한다는 점 기억하세요.

## 사용법

`cacheLife`를 사용하려면 우선 프로젝트 설정 파일인 `next.config.js`에 `dynamicIO` 플래그를 활성화해야 합니다:

```js
// next.config.js
module.exports = {
  experimental: {
    dynamicIO: true,
  },
};
```

왜 이렇게 설정해야 하냐면, `dynamicIO` 플래그가 켜져야 동적인 입출력 처리와 캐싱 기능이 제대로 작동하기 때문이에요.

### 참고로!

만약 캐시를 너무 오래 유지하면 데이터가 오래된 상태일 수 있으니, 적절한 캐시 수명을 설정하는 게 중요해요. 반대로 너무 짧게 설정하면 캐시 효과가 떨어지고 서버 부하가 늘 수 있답니다.

추가로, React나 Next.js와 같은 프레임워크를 사용 중이라면, `cacheLife` 설정과 `use cache` 관련 지시어를 적절히 조합해 컴포넌트 렌더링 최적화를 도모할 수 있습니다.

다음에 좀 더 구체적인 사용 예제와 실전 팁을 공유할게요!

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

이번에 Next.js에서 소개된 새로운 기능 중 하나인 `dynamicIO`와 `cacheLife`에 대해 간단히 살펴볼게요. 직접 써보고 정리한 내용이니 참고해서 프로젝트에 적용해봐도 좋을 것 같아요!

---

### `dynamicIO` Experimental 옵션 설정하기

Next.js는 계속 발전 중이라 실험적인 기능도 종종 나옵니다. `dynamicIO`는 그중 하나인데, 이걸 켜면 IO 작업을 더 동적으로 처리할 수 있는 환경을 만들어줍니다.

```js
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  experimental: {
    dynamicIO: true,
  },
};

export default nextConfig;
```

- 위처럼 `next.config.js` 혹은 `next.config.ts`에서 `experimental.dynamicIO` 옵션을 `true`로 설정하면 돼요.
- 공식 문서에서는 이 옵션이 아직 실험 단계라서, 실제 서비스에서는 좀 더 안정화될 때까지 조심스럽게 사용하는 걸 권장합니다.

---

### `cacheLife` 함수 사용하기

`cacheLife`는 Next.js에서 새롭게 제공하는 캐싱 관련 유틸리티예요. 특히 서버 컴포넌트나 함수 내에서 캐싱 수명을 관리할 때 유용하죠.

```js
"use cache";
import { unstable_cacheLife as cacheLife } from "next/cache";

export default async function Page() {
  // cacheLife를 이용해 캐시 수명을 조절하는 로직 구현 가능
  return <div>Page</div>;
}
```

- 여기서 `use cache`라는 디렉티브도 눈에 띄는데, 이건 Next.js가 해당 모듈을 캐싱 대상으로 인식하게 해줘요.
- `unstable_cacheLife`는 아직 실험적인 API라는 뜻이라, 이름 앞에 `unstable_`가 붙어 있어요.
- 이 함수를 활용하면, 필요한 부분의 캐시 유지 시간을 섬세하게 조절할 수 있어요. 예를 들어, 데이터 페칭 결과를 일정 시간 동안만 캐싱하고 싶을 때 유용하답니다.

---

### 참고사항 및 팁

| 내용                                | 설명                                                               |
| ----------------------------------- | ------------------------------------------------------------------ |
| `dynamicIO` 기능의 목적             | I/O 작업을 더 동적으로 처리하여 효율적인 데이터 페칭과 렌더링 가능 |
| `cacheLife` 활용법                  | 캐시 수명을 직접 설정해 보다 세밀한 리소스 관리 가능               |
| 'unstable\_' 접두사 의미            | 아직 안정화되지 않은 API, 앞으로 변경될 가능성 있음                |
| Next.js 실험적 기능 활용시 주의사항 | 실제 서비스에 적용할 때는 충분한 테스트 후 사용하는 것이 좋음      |

혹시 이 기능들을 실제로 프로젝트에 적용해보고 어려운 점이나 궁금한 점이 있으면 댓글로 남겨주세요! 저도 더 공부해서 좋은 팁들 공유할게요. Next.js 계속 발전하는 만큼, 우리도 함께 성장해보자구요!

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

### 기본 캐시 프로필 소개

Next.js에서는 다양한 시간대에 맞춘 이름 있는 캐시 프로필 세트를 제공해요. 만약 `use cache` 디렉티브와 함께 `cacheLife` 함수에 캐시 프로필을 지정하지 않으면, Next.js가 기본 캐시 프로필을 자동으로 적용해줍니다.

하지만 저는 항상 `use cache`를 사용할 때는 명시적으로 캐시 프로필을 지정해주는 걸 추천해요. 그래야 내가 원하는 캐싱 동작을 확실히 정의할 수 있거든요!

아래는 Next.js가 제공하는 기본 캐시 프로필들의 종류와 각 프로필이 가진 `stale`, `revalidate`, `expire` 타임라인, 그리고 설명입니다.

| 프로필    | stale | revalidate | expire | 설명                                                                   |
| --------- | ----- | ---------- | ------ | ---------------------------------------------------------------------- |
| `default` | 5분   | 15분       | 1년    | 자주 업데이트 필요 없는 콘텐츠에 적합한 기본 프로필                    |
| `seconds` | 0     | 1초        | 1초    | 거의 실시간 업데이트가 필요한 빠르게 변하는 콘텐츠에 적합              |
| `minutes` | 5분   | 1분        | 1시간  | 한 시간 내 자주 업데이트되는 콘텐츠에 적합                             |
| `hours`   | 5분   | 1시간      | 1일    | 하루 단위로 업데이트되며 약간 오래된 캐시가 허용되는 콘텐츠에 적합     |
| `days`    | 5분   | 1일        | 1주일  | 주 단위로 업데이트되며 하루 정도 오래된 캐시 정도 괜찮은 콘텐츠에 적합 |
| `weeks`   | 5분   | 1주일      | 30일   | 월 단위 업데이트이며 일주일 정도 캐시가 오래돼도 괜찮은 콘텐츠에 적합  |
| `max`     | 5분   | 30일       | 1년    | 거의 변경되지 않는 매우 안정적인 콘텐츠에 적합                         |

---

### 덧붙여서...

- `stale` 기간이란, 캐시된 데이터가 구식(stale)으로 간주되기 전까지의 시간입니다. 이 기간 동안은 캐시된 데이터를 사용하지만, `revalidate` 시점에는 백그라운드에서 새로운 데이터를 받아와서 캐시를 갱신하게 돼요.
- `revalidate`는 백그라운드에서 데이터를 다시 요청하는 시기를 뜻해요. 이때 응답은 즉시 캐시된 값으로 처리하고, 서버에서 신규 데이터를 받아 업데이트합니다.
- `expire`는 캐시가 완전히 만료되어 더 이상 사용할 수 없는 시점을 의미합니다.

만약 자주 업데이트가 필요한 뉴스 사이트나 주식 시세, 실시간 스포츠 결과 같은 데이터라면 `seconds`나 `minutes` 프로필을, 변화가 거의 없는 블로그 포스트나 정적인 이미지 리소스라면 `max` 프로필 같은 장기 캐싱을 선택하는 게 효율적이에요.

Next.js 캐시 기능은 이렇게 시간 기반 프로필을 잘 활용하면 사용자 경험도 개선하고 서버 부하도 줄일 수 있으니, 꼭 적절하게 설정해서 활용해 보시길 바랍니다!

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

캐시 프로필을 참조할 때 사용하는 문자열 값들은 그 자체로 특별한 의미를 가지기보다는, 코드 내에서 의미를 부여하는 라벨 역할을 해요. 덕분에 캐시된 콘텐츠를 더 쉽고 직관적으로 관리할 수 있답니다.

> 참고할 점: staleTimes와 expireTime 설정을 변경하면 기본 캐시 프로필의 stale과 expire 속성도 함께 업데이트돼요. 즉, 일관성 있게 캐시 설정을 관리할 수 있다는 뜻이죠.

### 커스텀 캐시 프로필 만들기

Next.js 프로젝트에서 커스텀 캐시 프로필을 만들어 사용하고 싶다면, `next.config.ts` 파일 내의 `cacheLife` 옵션에 직접 추가하면 돼요. 이렇게 하면 프로젝트 요구사항에 맞게 캐시 정책을 세밀하게 조정할 수 있답니다.

아래는 `cacheLife` 옵션을 활용해 여러 캐시 프로필을 정의하는 예시입니다:

```typescript
// next.config.ts
export default {
  experimental: {
    cacheLife: {
      shortCache: { staleTime: 60, expireTime: 120 }, // 짧게 유지되는 캐시 예시 (초단위)
      longCache: { staleTime: 3600, expireTime: 7200 }, // 오래 유지되는 캐시 예시
      noCache: { staleTime: 0, expireTime: 0 }, // 캐시를 사용하지 않는 프로필
    },
  },
};
```

이렇게 프로필을 만들고 나면, 데이터 요청이나 페이지 캐싱 시에 해당 프로필 이름(`shortCache`, `longCache` 등)을 참조해 필요한 캐시 정책을 간편하게 적용할 수 있죠.

> 팁! 캐시를 적절히 활용하면 서버 부하도 줄이고, 사용자 경험도 크게 향상시킬 수 있어요. 하지만 너무 오래된 캐시는 오히려 구버전 콘텐츠를 보여줄 수 있으니, 상황에 따라 적절한 `staleTime`과 `expireTime` 설정이 중요하답니다.

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

Cache profiles는 캐시와 관련된 몇 가지 속성을 담고 있는 객체입니다. 각 속성은 캐시가 어떻게 동작하는지 세밀하게 조절할 수 있게 도와주죠. 아래 표를 보면서 살펴볼게요.

| Property     | Value  | Description                                                                                                                                   | Requirement                          |
| ------------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| `stale`      | number | 클라이언트가 서버에 확인하지 않고 값을 캐시에 저장해두는 기간입니다.                                                                          | 선택 사항                            |
| `revalidate` | number | 서버에서 캐시를 새로 고칠 주기입니다. 리밸리데이트 중에는 오래된 값(stale)을 보여줄 수 있어요.                                                | 선택 사항                            |
| `expire`     | number | 값이 오래된 상태(stale)로 유지될 수 있는 최대 기간입니다. 이 기간이 지나면 동적(fetch)으로 전환됩니다. 반드시 `revalidate`보다 길어야 합니다. | 선택 사항 – `revalidate`보다 커야 함 |

---

여기서 조금 혼동될 수 있는 부분이 `stale` 속성과 `staleTimes` 설정인데요, 간단히 말씀드리면:

- `stale`은 클라이언트 사이드 라우터 쪽 캐시만 제어합니다. 즉, 특정 함수나 경로(route)별로 직접 '얼마 동안 캐싱할지'를 정할 수 있다는 거죠.
- 반면에 `staleTimes`는 전역(globa) 설정으로, 동적(dynamic) 데이터와 정적(static) 데이터 모두에 영향을 미칩니다.

---

그리고 한 가지 팁!  
`stale` 속성은 **Cache-control: max-age** HTTP 헤더 값을 설정하는 게 아닙니다. 이건 서버에서 보내는 캐시 지시자(header)이지만, `stale`은 오로지 클라이언트 라우터의 로컬 캐시 관리를 위한 설정이라는 점, 꼭 기억하세요.

이런 캐시 컨트롤 방법은 앱을 더 빠르게 만들면서도 최신 데이터를 적절히 유지하는 데 아주 유용해요. 각 상황에 맞게 잘 조절해보면서 퍼포먼스 최적화에 활용해보세요!

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

## 예시

### 재사용 가능한 캐시 프로필 정의하기

next.config.ts 파일에서 재사용 가능한 캐시 프로필을 정의할 수 있어요. 프로필 이름은 사용 목적에 맞게 지정하고, `stale`, `revalidate`, `expire` 속성들을 설정해주면 됩니다. 필요한 만큼 여러 개의 커스텀 캐시 프로필을 만들어 사용할 수 있어요. 각 프로필은 이름을 문자열로 `cacheLife` 함수에 전달해서 참조할 수 있답니다.

아래 예시는 14일간 캐시를 유지하고, 1일마다 재검증을 수행하는 `biweekly`라는 캐시 프로필을 정의한 코드입니다.

```typescript
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  experimental: {
    dynamicIO: true,
    cacheLife: {
      biweekly: {
        stale: 60 * 60 * 24 * 14, // 14일 동안 stale 상태 유지
        revalidate: 60 * 60 * 24, // 1일 간격으로 재검증
        expire: 60 * 60 * 24 * 14, // 14일 후 캐시 만료
      },
    },
  },
};

module.exports = nextConfig;
```

---

**참고로 캐시 관련 설정을 잘 해두면, 페이지 로딩 속도가 크게 향상되고 서버 부하도 줄일 수 있어요.** 특히 자주 변하지 않는 데이터라면 이런 캐시 프로필을 적극 활용해서 효율적인 서비스 운영을 할 수 있답니다.

단, `stale`, `revalidate`, `expire` 값은 상황에 맞게 조절해야 해요. 예측 가능한 사용자 트래픽, 데이터 갱신 빈도 등을 고려해 보세요. 너무 길게 설정하면 오래된 데이터가 노출될 수도 있으니까 주의가 필요해요!

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

위 예시는 14일 동안 캐시를 유지하고, 매일 업데이트를 확인하며, 14일이 지나면 캐시가 만료되도록 설정한 거예요. 이렇게 만든 캐시 프로필은 애플리케이션 전반에서 이름으로 참조해서 사용할 수 있답니다.

```js
"use cache";
import { unstable_cacheLife as cacheLife } from "next/cache";

export default async function Page() {
  return <div>Page</div>;
}
```

### 기본 캐시 프로필 덮어쓰기(오버라이딩)

Next.js에서 제공하는 기본 캐시 프로필은 캐시된 데이터가 얼마나 신선한지, 혹은 얼마나 오래된 것인지를 결정하는 데 도움을 줘요. 그런데 어떤 애플리케이션은 기본 프로필보다 더 세분화된 캐싱 전략이 필요할 수 있죠. 이럴 때는 내 입맛에 맞게 이름이 붙은(즉, 커스텀) 캐시 프로필을 만들어서 쓸 수 있어요.

예를 들어, 자주 업데이트되는 뉴스 섹션엔 'shortCache', 변경이 거의 없는 공지사항엔 'longCache' 같은 이름을 붙여서 관리하면 더 효율적이고 관리하기도 편하답니다. 이렇게 하면 빠른 캐시 만료가 필요한 곳과 오래 두어도 무방한 부분을 명확히 구분할 수 있어요.

> **참고 팁:** 캐시 프로필을 세밀하게 관리하면, 서버 부담을 줄이면서도 사용자에게 항상 신선한 콘텐츠를 제공할 수 있어요. 너무 보수적으로 캐시 기간을 설정하면 빈번한 요청 처리로 서버에 부담이 갈 수 있으니, 애플리케이션 특성에 맞게 적절히 조절하는 게 중요합니다!

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

next.js에서 기본으로 제공하는 캐시 프로필(named cache profiles)을 커스텀하고 싶을 때, 같은 이름으로 새 설정을 만들어서 덮어쓸 수 있다는 점, 알고 계셨나요? 기본 프로필을 그대로 쓰는 것도 좋지만, 프로젝트 상황에 맞게 적절히 조정하면 효율적인 캐시 관리가 가능해집니다.

예를 들어, 기본 “days” 캐시 프로필을 이렇게 오버라이딩할 수 있어요:

```js
const nextConfig = {
  experimental: {
    dynamicIO: true,
    cacheLife: {
      days: {
        stale: 3600, // 1시간 동안 stale 상태 유지
        revalidate: 900, // 15분마다 재검증
        expire: 86400, // 1일 후 만료
      },
    },
  },
};

module.exports = nextConfig;
```

여기서 좀 더 설명을 드리자면,

- `stale`: 캐시된 데이터가 조금 오래됐어도 바로 폐기하지 않고, 이 시간만큼은 stale 상태로 허용합니다. (즉, 약간 낡은 데이터도 사용할 수 있게끔 시간을 설정하는 거예요.)
- `revalidate`: 재검증(interval) 주기로, 이 시간이 지나면 캐시를 백그라운드에서 새로고침합니다.
- `expire`: 캐시가 완전히 만료되는 시간으로, 이 시간이 지나면 캐시를 폐기하고 새 데이터를 받아옵니다.

이런 설정 덕분에 데이터를 너무 자주 다시 받느라 리소스를 낭비하지 않고, 살짝 오래된 데이터도 쓸 수 있게 해서 효율성을 높일 수 있답니다.

### 캐시 프로필 직접 정의하기 (Defining cache profiles inline)

기본 프로필이 아니라 완전히 새롭게 캐시 프로필을 정의하고 싶을 때도 있습니다. 예를 들어, 'hours'라는 이름으로 6시간 단위의 캐시 전략을 직접 만들어볼 수 있죠.

```js
const nextConfig = {
  experimental: {
    dynamicIO: true,
    cacheLife: {
      hours: {
        stale: 600, // 10분간 stale 허용
        revalidate: 300, // 5분마다 재검증
        expire: 21600, // 6시간 후 만료
      },
    },
  },
};

module.exports = nextConfig;
```

이렇게 나만의 캐시 전략을 만들어두면, 복잡한 프로젝트에서 여러 페이지나 API에 맞게 캐시 설정을 다양하게 적용할 수 있어서 매우 유용해요.

> **팁:** 캐싱 전략을 짤 때, 너무 짧은 expire는 서버 부담을 늘리고, 너무 긴 expire는 사용자에게 오래된 데이터를 보여줄 위험이 생기므로 적절한 균형이 중요합니다!

필요에 따라 값들을 튜닝하면서 최적의 캐시 전략을 찾아가 보세요. 앞으로도 이런 유용한 실전 팁 계속 소개할게요! 궁금한 점 있으면 언제든 물어봐 주세요 :)

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

이번에는 Next.js에서 캐싱을 좀 더 세밀하게 제어하는 방법에 대해 이야기해볼게요. 특히 `cacheLife` 함수를 사용해서 원하는 캐시 프로필을 직접 설정하는 방법을 다뤄보겠습니다.

```js
"use cache";
import { unstable_cacheLife as cacheLife } from "next/cache";

export default async function Page() {
  return <div>Page</div>;
}
```

위 코드는 기본적인 캐시 설정 예시예요. 여기서 `cacheLife` 함수에 객체 형태의 설정값을 넘기면 내가 원하는 캐시 정책을 직접 만들 수 있어요. 그런데 중요한 점은 이렇게 만든 캐시 프로필은 해당 함수나 파일에 한해서만 적용된다는 거예요.

만약 여러분의 앱 전체에서 같은 캐시 정책을 반복해서 쓰고 싶다면, 매번 이렇게 작성하는 대신 `next.config.ts` 파일 안에 `cacheLife` 속성을 추가하는 게 좋은 방법입니다. 그러면 전역적으로 일관된 캐싱 전략을 관리할 수 있거든요.

---

### 좀 더 살펴보는 `use cache`와 `cacheLife`의 중첩 사용법

`'use cache'`는 Next.js에서 함수가 캐시 가능하다는 걸 알려주는 지시문이고, `cacheLife`는 그 캐시의 수명과 동작 정책을 좀 더 구체적으로 설정해주는 역할을 해요.

```js
"use cache";
import { unstable_cacheLife as cacheLife } from "next/cache";

// 예를 들어, 캐시 수명을 60초로 지정하고 싶을 때
const myCacheConfig = cacheLife({ maxAge: 60 });

export default myCacheConfig(async function Page() {
  return <div>캐시가 60초 동안 유지됩니다.</div>;
});
```

위처럼 `cacheLife`가 반환하는 함수로 래핑하면, 그 안 함수는 해당 캐시 정책을 따르게 됩니다.

---

### 추가 팁!

- **maxAge**: 캐시 데이터가 얼마동안 살아있을지 초 단위로 지정해요.
- **staleWhileRevalidate**: 캐시가 만료되더라도 새 데이터를 받아올 때까지 옛날 캐시를 계속 보여주는 설정이에요.
- **캐시 무효화**: 필요할 땐 특정 조건이나 API 호출 시 캐시를 초기화해줄 수도 있는데, 이 부분도 꼭 체크해보시면 좋아요.

React 컴포넌트나 API 라우트에서 잘 활용하면, 불필요한 데이터 재요청을 줄이고 사용자 경험을 개선할 수 있답니다.

필요하면 다음에 더 자세한 캐시 전략이나 예제도 정리해서 소개할게요!

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

같은 경로나 컴포넌트 트리 안에서 여러 캐싱 동작(caching behavior)을 정의할 때, 안쪽(내부) 캐시가 자신만의 cacheLife 프로필을 지정하면, 바깥쪽(외부) 캐시는 그 중 가장 짧은 캐시 지속시간을 자동으로 따르게 돼요. 단, 이건 바깥쪽 캐시가 명시적으로 cacheLife 프로필을 지정하지 않은 경우에만 적용됩니다.

예를 들어, 페이지에 `use cache` 지시어를 붙이면서 별도로 캐시 프로필을 지정하지 않으면, 기본값인 `cacheLife("default")`가 암묵적으로 적용돼요. 그런데 이 페이지에 불러오는 컴포넌트가 자체 캐시 프로필을 가진 `use cache`를 사용한다면, 내부와 외부 캐시 프로필이 비교되어서, 가장 짧은 시간이 적용됩니다.

간단히 말해, 여러 레이어의 캐시가 있으면 "가장 짧은 시간 기준"으로 캐싱 기간을 설정해서, 데이터를 너무 오래 쓰지 않도록 조절해 준다는 거죠.

---

예를 들어서 본 부모 컴포넌트 코드를 한번 보면:

```js
// Parent component
import { unstable_cacheLife as cacheLife } from "next/cache";
import { ChildComponent } from "./child";

cacheLife("days"); // "days" 기준 캐싱 지속시간 지정

return (
  <div>
    <ChildComponent />
  </div>
);
```

그리고 별도의 파일에서 임포트한 자식 컴포넌트는 다음처럼 정의해요.

(자식 컴포넌트 코드는 다음에 나올 거랍니다!)

---

### 추가 팁!

- 캐시 지속 시간을 지정할 때는 `"hours"`, `"minutes"`, `"seconds"`처럼 구체적으로 설정하셔도 되고, 직접 숫자 단위(예: `cacheLife(60 * 60 * 24)`)로 초 단위 값을 줄 수도 있습니다.
- 내부 컴포넌트의 캐시가 더 짧으면, 전체적으로 데이터 최신성이 더 잘 유지되기 때문에 업데이트 반영 속도가 빨라지죠.
- 반대로 외부에서 명시적으로 긴 cacheLife를 지정하면 내부 캐시는 무시되고 외부 걸린 시간 기준으로 유지돼요. 따라서 캐시가 꼬이지 않도록 캐시 설정을 꼼꼼하게 관리하는 게 중요합니다.

다음 메시지에 자식 컴포넌트 코드를 소개할게요!

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

안녕하세요! 오늘은 Next.js에서 제공하는 `unstable_cacheLife` 함수에 대해 살펴볼게요. 이 함수는 캐시 수명을 설정할 때 사용되는데, 아직 공식적으로 안정화되지 않은 기능이라 `unstable_` 접두사가 붙어 있답니다.

예시 코드가 주어졌는데, 쉽게 설명해볼게요.

```js
// Child 컴포넌트에서
import { unstable_cacheLife as cacheLife } from "next/cache";

cacheLife("hours");
return <div>Child Content</div>;

// 이 컴포넌트의 캐시는 'hours' 프로필 기준으로 관리됩니다.
```

여기서 `cacheLife('hours')`라고 설정하면, 이 컴포넌트는 상대적으로 짧은 시간 단위인 'hours' 기준으로 캐시가 유지돼요. 즉, 더 빨리 캐시가 만료된다는 뜻이죠.

### 참고할 점!

- `cacheLife` 안에 들어갈 수 있는 프로필은 보통 'seconds', 'minutes', 'hours', 'days' 등이 있어요.
- 이 기능은 아직 실험 단계라 문서가 계속 업데이트되고, API도 바뀔 수 있어요.
- 캐시 수명을 적절히 조정하면 사용자 경험을 향상시키면서 서버 부하도 줄일 수 있다는 점에서 유용해요!

### 마치며

Next.js에서 캐시를 더 세밀하게 제어하고 싶을 때 `unstable_cacheLife`를 써볼 수 있지만, 아직 완전 안정적이지 않은 점을 기억하세요. 프로덕션에서는 주의해서 사용하고, 공식 문서 업데이트를 꾸준히 확인하는 걸 추천드립니다!

도움이 되셨다면 좋겠네요. 다음에도 유용한 팁으로 찾아올게요! 😊
