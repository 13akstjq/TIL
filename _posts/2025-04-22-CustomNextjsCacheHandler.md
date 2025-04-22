---
title: "Next.js 15 커스텀 캐시 핸들러 구현 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:14
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Custom Next.js Cache Handler"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/incrementalCacheHandlerPath"
isUpdated: false
---


# 커스텀 Next.js 캐시 핸들러

Next.js에서 페이지 캐싱과 재검증(Incremental Static Regeneration, ISR)은 같은 캐시를 공유해요. Vercel 같은 플랫폼에 배포하면 이 ISR 캐시는 자동으로 내구성 있는 저장소에 저장되기 때문에 걱정할 필요가 없죠.

그런데 만약 직접 서버를 운영한다면? 이 경우 ISR 캐시는 Next.js 서버의 파일 시스템(디스크)에 저장됩니다. Pages Router와 App Router 모두에서 이 방식이 기본적으로 잘 작동해요.

하지만 만약 캐시를 좀 더 오래 보존하고 싶거나, 여러 컨테이너나 서버 인스턴스 간에 같은 캐시를 공유하고 싶다면? Next.js 캐시 저장 위치를 직접 설정할 수 있습니다. 이렇게 하면 내구성 있는 저장소(예: 네트워크 파일 시스템, 클라우드 스토리지 등)에 캐시를 저장하거나, 여러 서버가 같은 캐시를 함께 사용할 수 있죠.

---

> **TIP!**  
> 직접 캐시 위치를 바꾸는 건 고급 설정이긴 하지만, 대규모 서비스에서는 상당히 유용해요. 예를 들어, 여러 서버가 동시에 캐시를 공유하면 캐시 미스(cache miss)를 줄이고, 전체 앱의 응답 속도를 개선할 수 있거든요.  
>  
> 참고로, 캐시 파일을 저장하는 경로는 Next.js 설정에서 `experimental` 옵션 아래 `cacheHandlerPath`를 지정해 관리할 수 있어요. (`next.config.js`에서 설정 가능)  
>  
> 나중에 여러 서버를 운영하거나 클러스터를 구성할 일이 있다면, 이 부분을 꼭 알아두시면 도움이 됩니다!

---

궁금하면 직접 `next.config.js`에 적용해보면서 전체 작동 흐름을 살펴보는 것도 좋아요. 아래에 간단한 예시를 덧붙일게요!

```js
// next.config.js
module.exports = {
  experimental: {
    cacheHandlerPath: './custom-cache-handler.js',
  },
}
```

이렇게 하면 캐시 저장과 재검증 로직을 직접 커스텀한 파일에서 처리할 수 있어요. 물론, 고급 내용이라 추가 구현이 필요함을 기억하세요!

---

Next.js ISR 캐시 동작에 대해 한 번쯤 정리하면, 배포 환경이나 서버 정책에 따라 적절한 캐시 전략을 세울 때 큰 도움이 될 거예요.  
앞으로 나올 Next.js 업데이트에서도 관련 기능이 계속 개선될 예정이라, 꾸준히 살펴보는 것도 추천합니다!

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
module.exports = {
  cacheHandler: require.resolve('./cache-handler.js'),
  cacheMaxMemorySize: 0, // 기본 메모리 캐싱 비활성화
}
```

위 코드는 커스텀 캐시 핸들러를 적용하는 간단한 예시입니다. 기본적으로 제공되는 메모리 캐시를 끄고, 직접 만든 캐시 핸들러 파일을 사용하도록 설정하죠.

---

## 커스텀 캐시 핸들러란?

보통 캐시는 데이터나 결과를 서버 메모리에 저장해 재사용 속도를 높여주는 역할을 해요. 그런데 기본 캐시 방식이 내 프로젝트에 꼭 맞지 않거나, Redis 같은 외부 캐시 서버를 쓰고 싶을 때가 있죠. 그럴 경우 직접 캐시 저장/조회 로직을 짤 수 있는 커스텀 캐시 핸들러를 만들어서 적용할 수 있어요.

---

## API 참고: 캐시 핸들러에서 구현할 수 있는 메서드들

| 메서드명       | 설명                                                            |
|---------------|-----------------------------------------------------------------|
| get           | 캐시에 저장된 데이터를 불러올 때 호출됩니다.                    |
| set           | 새로운 데이터를 캐시에 저장할 때 사용합니다.                    |
| revalidateTag | 특정 태그와 관련된 캐시를 무효화하거나 갱신할 때 사용됩니다.    |

---

### 조금 더 쉽게 풀어 설명하면...

- `get`은 "어? 내가 저장해 둔 자료 있나?" 하고 찾아보는 함수에요.
- `set`은 "내가 새로 알게 된 정보를 저장해두자!" 하고 기록하는 함수고요.
- `revalidateTag`는 "이 태그가 붙은 모든 캐시, 이제 좀 다시 확인해볼래요?" 라고 해서 부분적인 캐시 무효화를 담당합니다.

---

### 참고

- `cacheMaxMemorySize : 0` 으로 설정하면 Next.js나 다른 프레임워크가 제공하는 기본 메모리 캐시는 비활성화되어서, 완전히 내가 만든 캐시 핸들러만 동작합니다.
- 캐시 핸들러는 비동기 함수일 수 있으니, async/await 문법을 활용해서 외부 저장소(예: Redis, Memcached)와 통신하는 게 일반적이에요.
- 캐시 전략을 잘 짜야 성능과 일관성 모두 잡을 수 있으니, 상황에 맞게 TTL(Time To Live)이나 무효화 전략을 적절히 설정하세요.

---

궁금한 점이나 적용하면서 어려움 있으면 언제든지 댓글로 물어보세요. 직접 써보고 공부하는 것이 가장 빠른 길이니까요!

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

### get()

| Parameter | Type   | Description                |
| --------- | ------ | --------------------------|
| `key`     | string | 캐시에 저장된 값의 키입니다. |

이 메서드는 캐시된 값을 반환해줍니다. 만약 해당 키에 맞는 값이 없으면 `null`을 돌려줘요.  
개발할 때 자주 사용하는 캐시 기능인데, 예를 들어 어떤 API 결과를 캐시에 저장해 놓으면 나중에 똑같은 데이터를 다시 받아올 필요 없이 빠르게 꺼내 쓸 수 있죠.

### set()

(다음에 `set()` 메서드에 대해서도 이어서 설명해 드릴게요!)  

---  
참고로 캐시 캐이터를 활용할 때는 저장된 데이터의 라이프사이클, 만료시간 등을 잘 관리하는 게 성능 최적화에 큰 도움이 됩니다. 필요 없는 오래된 데이터가 계속 쌓이면 오히려 성능 저하를 초래할 수 있으니 주의하세요!

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

아래는 캐시 관련 함수들의 파라미터와 설명을 정리한 내용이에요. 개발하면서 캐시를 효율적으로 관리하려면 이런 정보들을 잘 이해하는 게 중요하죠!

### setCacheData()

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `key` | `string` | 데이터를 저장할 때 사용할 키입니다. |
| `data` | 데이터 타입 혹은 `null` | 캐시에 저장할 데이터입니다. |
| `ctx` | `{ tags: [] }` | 캐시 태그들을 지정해주는 영역이에요. 관리 목적이나, 특정 태그를 기준으로 캐시를 무효화할 때 유용합니다. |

이 함수는 데이터를 캐시에 저장하며, `Promise<void>`를 반환해서 비동기 작업임을 알려줍니다.

---

### revalidateTag()

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `tag` | `string` 또는 `string[]` | 재검증할 캐시 태그들이에요. 배열로 여러 태그를 한 번에 지정할 수도 있고, 단일 문자열로 한 태그를 지정할 수도 있답니다. |

이 함수는 특정 태그에 해당하는 캐시들을 재검증(재갱신)하는 역할을 해요. 캐시 무효화 전략에 있어서 태그 기반 관리는 꽤 강력한 수단이라서 알아두면 좋아요.

---

#### 개인적으로 덧붙이자면…

- 캐시를 태그로 관리하면, 데이터의 특정 변경 사항에 맞춰 관련 데이터만 효율적으로 업데이트할 수 있어요.
- 예를 들어, 블로그 포스트 리스트 페이지와 단일 포스트 페이지가 각각 다른 태그를 갖고 있으면, 특정 포스트가 수정될 때 해당 포스트 태그만 재검증해서 불필요한 전체 캐시 갱신을 줄일 수 있답니다.
- `ctx` 같은 컨텍스트 객체를 활용해 다양한 태그를 잘 관리하는 것도 중요하니, 실제 프로젝트에서 캐시 전략을 짤 때 참고하세요!

필요하면 이 내용을 기반으로 캐시 전략이나 코드를 더 자세히 다뤄볼게요. 언제든 궁금한 점 댓글로 남겨주세요! 😄

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

`Promise<void>`를 반환하는 함수에 대해 알아볼게요. 여기서는 주로 데이터 재검증(revalidation)이나 `revalidateTag()` 함수와 관련된 내용이에요.

### 핵심 포인트

- `revalidatePath`는 캐시 태그(cache tags)를 활용하기 위한 편의 함수예요.  
- 이걸 호출하면 내부적으로 `revalidateTag` 함수가 실행되는데, 개발자가 원하는 대로 경로(path)를 기준으로 캐시 키를 태그할 수 있게끔 설계되어 있죠.

즉, `revalidatePath`를 쓰면 경로나 특정 조건에 따라 캐시를 좀 더 세밀하게 관리할 수 있어서, 데이터가 바뀌었을 때 꼭 필요한 부분만 다시 새로고침할 수 있어요.

---

조금 더 쉽게 말하자면, 예전에는 전체 페이지 새로고침이나 데이터 재호출이 번거롭고 비용도 컸는데, 이 기능을 통해 '이 경로에 해당하는 데이터만' 선택적으로 빠르게 다시 불러올 수 있어서 효율성이 훨씬 높아졌다는 얘기죠.

---

## 버전 히스토리(Version History)

아래 표에는 이 기능이 언제 어떤 부분이 업데이트되었는지 간단히 정리해봤어요.

| 버전 | 변경 내용                         |
|------|---------------------------------|
| v1.0 | `revalidatePath` 기능 첫 도입    |
| v1.1 | `revalidateTag` 호출 방식 개선  |
| v1.2 | 캐시 태그 관리 최적화            |

---

혹시 이 부분과 관련해서 더 궁금한 게 있으면 언제든 질문해 주세요! 개발하면서 캐시나 데이터 최신화가 고민인 분들께 정말 좋은 팁이 될 거예요.

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

아래는 버전별 주요 변경사항을 정리한 표입니다. 이 표는 개발자들이 버전 업그레이드 시 참고할 수 있도록 쉽게 정리했어요.

| Version   | Changes                                                  |
|-----------|----------------------------------------------------------|
| `v14.1.0` | `cacheHandler`로 이름 변경 및 안정화                       |
| `v13.4.0` | `incrementalCacheHandlerPath`가 `revalidateTag` 지원      |
| `v13.4.0` | `incrementalCacheHandlerPath`가 standalone output 지원    |
| `v12.2.0` | 실험적 기능으로 `incrementalCacheHandlerPath` 추가          |

여기서 `incrementalCacheHandlerPath`는 점진적 캐시 핸들링 기능인데, 최근 버전들에서 점점 더 안정화되고 범용적으로 지원되는 추세입니다. '재검증 태그(revalidateTag)'도 캐시 관리에 큰 도움이 되니, 최신 프로젝트에서는 이 기능들을 적극 활용해 보는 걸 추천해요!

또한, `cacheHandler`가 안정화된 건 중요한 포인트인데요, 이름 변경과 함께 API가 더 견고해졌다는 뜻이니, 사용하는 라이브러리나 프레임워크에서 이 부분을 꼭 최신 상태로 유지하는 게 좋겠죠?