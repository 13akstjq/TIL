---
title: "nextjs 15에서 데이터 패칭 최적화하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:05
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "dynamicIO"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/dynamicIO"
isUpdated: false
---


# dynamicIO

dynamicIO 플래그는 Next.js에서 실험적으로 제공되는 기능인데요, App Router에서 데이터 패칭(fetching) 작업을 미리 렌더링(pre-render)에서 제외시키는 역할을 해요. 단, 명시적으로 캐시가 설정된 경우는 예외입니다. 

이 기능이 왜 좋으냐면, 서버 컴포넌트에서 동적인 데이터를 다룰 때 성능 최적화에 도움이 되기 때문이에요. 예를 들어, 앱이 실행 중일 때마다 최신 데이터를 받아와야 하고, 미리 렌더된 캐시에 의존하고 싶지 않을 때 딱 맞습니다.

그러니 dynamicIO를 사용할 때는 보통 use cache와 함께 쓰는 게 좋아요. 기본적으로는 데이터 페칭이 런타임에 일어나도록 하면서, 특정 페이지나 함수, 컴포넌트 단위로 캐시를 적용하고 싶을 때 use cache를 활용하는 식이죠. 이렇게 하면 필요한 부분만 스마트하게 캐싱할 수 있어 성능과 최신 데이터 사이의 균형을 맞출 수 있습니다.

추가로, 아직 experimental(실험적) 상태이니 실제 운영 환경에 도입하기 전에는 충분히 테스트해보고, Next.js의 공식 문서와 업데이트 상황을 계속 체크하는 걸 추천드려요. 개발자 커뮤니티에서도 dynamicIO 관련 경험담이나 팁들이 올라오니 참고해 보시면 큰 도움이 될 거예요.

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

## 사용법

next.config.ts 파일의 `experimental` 섹션에 `dynamicIO` 플래그를 `true`로 설정하면 동적 IO 기능을 활성화할 수 있어요:

```js
import type { NextConfig } from 'next'
 
const nextConfig: NextConfig = {
  experimental: {
    dynamicIO: true,
  },
}
 
export default nextConfig
```

이렇게 `dynamicIO`를 활성화하면, 여러가지 캐시 관련 함수와 설정을 사용할 수 있게 됩니다.

> 참고로, experimental 옵션은 아직 완전히 안정화된 기능은 아니기 때문에, 실제 프로젝트에 적용할 때는 항상 주의가 필요해요. 특히 프로덕션에 바로 적용하기보다는 테스트 환경에서 충분히 검증해보는 걸 권장합니다.

추가로 dynamicIO는 서버 IO 작업을 더 유연하게 관리할 수 있도록 도와주는데, 이 덕분에 캐시 처리나 데이터 동기화 관련 작업이 훨씬 편리해지는 장점이 있어요. 다음에 실제 예제와 함께 더 자세히 설명해 드릴게요!

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

안녕하세요! 오늘은 웹 개발에서 성능 최적화와 관련해 자주 쓰이는 **캐시(cache)** 기능에 대해 살펴보려고 해요. 특히 자주 사용하는 세 가지 개념, 즉 **`use cache` 디렉티브**, **`cacheLife` 함수**, 그리고 **`cacheTag` 함수**에 대해 쉽게 설명해드릴게요.

---

## 1. `use cache` 디렉티브란?

`use cache`는 React나 Next.js 같은 프레임워크에서 데이터를 가져올 때 캐시를 활용하겠다는 의미예요. 데이터를 한 번 받아오면, 그 결과를 메모리에 저장해 두었다가 같은 요청이 들어오면 저장된 데이터를 바로 반환해서 빠르게 처리할 수 있죠.

이걸 적용하면 서버 사이드 렌더링(SSR)이나 클라이언트 렌더링 시 네트워크 지연 없이 깔끔한 사용자 경험을 만들 수 있답니다.

### 예시

```js
export async function fetchData() {
  'use cache';  // 이 함수에서 캐시 사용 지정
  // 데이터 요청 로직
}
```

⚠️ 단, 캐시는 데이터가 바뀔 가능성이 적거나 실시간 변화가 필요 없는 경우에 쓰는 게 좋아요. 실시간 데이터는 별도의 로직이 필요하겠죠!

---

## 2. `cacheLife` 함수로 캐시 수명 지정하기

캐시는 무조건 오래 유지하는 게 좋은 건 아니에요. 데이터 특성에 맞게 캐시가 살아있는 시간을 정해야 하죠. `cacheLife` 함수는 바로 이 캐시의 **수명(Time-To-Live, TTL)**을 지정해주는 역할을 해요.

예를 들어, 오늘 날씨 같은 정보는 10분 정도만 캐시하고, 그 이후엔 새로 데이터를 받아오길 원할 때 쓰면 편리하답니다.

### 예시

```js
cacheLife(600);  // 캐시 유지 시간을 600초(10분)으로 설정
```

이렇게 하면 10분 동안은 캐시된 데이터를 재사용하고, 그 이후엔 새로운 데이터를 fetch 하도록 하죠.

---

## 3. `cacheTag` 함수로 태그를 달아 캐시 관리 쉽게 하기

`cacheTag`는 캐시에 **태그**를 붙여주는 역할이에요. 태그를 이용하면 특정 그룹의 캐시만 한 번에 지우거나 갱신할 수 있어서 관리가 훨씬 편해지죠. 예를 들어, '공지사항' 태그를 붙여둔 캐시가 있다면 새로운 공지사항이 등록될 때 그 태그가 달린 캐시만 쉽게 삭제할 수 있답니다.

### 예시

```js
cacheTag('news');  // 이 캐시 데이터에 'news' 태그 부여
```

관리자가 태그별로 캐시를 조작하거나, 특정 그룹만 최신 상태로 유지하고 싶을 때 무척 유용해요!

---

## 마무리하면서...

정리하자면,

| 기능          | 역할                        | 장점                                  |
| ------------- | --------------------------- | ------------------------------------ |
| `use cache`   | 데이터 요청 시 캐시 사용 지정 | 네트워크 호출 줄이고 속도 향상       |
| `cacheLife`   | 캐시 데이터 수명 지정        | 데이터 최신성 유지와 효율적 캐시 관리 |
| `cacheTag`    | 캐시에 태그 붙이기           | 태그별 캐시 일괄 제어 가능           |

그리고 참고할 점! 

> **dynamicIO** 같은 동적 데이터 fetching 기법은 오히려 매번 새로운 데이터를 받아오기 때문에 **지연 시간이 늘어날 수 있어요**. 그래서 빠른 응답이 중요한 경우에는 캐시를 활용하는 게 더 좋답니다 😀

---

혹시 더 궁금한 점이나 캐시를 활용해 본 경험이 있다면 댓글로 공유해 주세요! 

내일은 이 내용을 바탕으로 Next.js에서 캐시를 실제로 어떻게 적용하는지도 다뤄볼게요. 그럼 즐거운 개발 시간 되세요! 🚀