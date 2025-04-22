---
title: "Next.js 15에서 unstable_noStore 옵션 사용하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:48
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "unstable_noStore"
link: "https://nextjs.org/docs/app/api-reference/functions/unstable_noStore"
isUpdated: false
---


# unstable_noStore

Next.js 15 버전에서는 더 이상 `unstable_noStore`를 권장하지 않고, 대신 **`connection`**을 사용하길 추천해요.

`unstable_noStore`는 정적 렌더링(static rendering)을 명시적으로 제외하고, 특정 컴포넌트를 캐시하지 않도록 표시하는 데 사용됐어요.

아래 예제처럼 사용했었죠:

```js
import { unstable_noStore as noStore } from 'next/cache';

export default async function ServerComponent() {
  noStore();
  const result = await db.query(...);
  ...
}
```

---

## 조금 더 풀어서 설명하자면

- **`unstable_noStore`** 는 이름에서 알 수 있듯이 아직 완전히 안정화되지 않은 API예요. 이 기능을 사용하는 컴포넌트는 정적 HTML로 빌드할 때 캐싱에서 빠지게 되고, 항상 새로 데이터를 불러와야 하는 상황에서 유용했어요.
- 하지만 Next.js 15 버전부터는 새로운 접근법인 `connection` API가 등장하면서, 캐시 관련 처리를 더 명확하고 안전하게 할 수 있게 되었답니다.

## 요즘에는 어떻게 할까?

Next.js 앱을 작성하면서 특정 데이터 요청이 매번 새로워야 하거나, 캐시를 끄고 싶다면, 이제 `connection` 방식을 적극 활용해 보세요!

예를 들어, 서버 컴포넌트 안에서 데이터베이스의 연결을 관리한다면, `connection`을 이용해 상태를 안전하게 유지하며 최적화할 수 있답니다.

---

## 정리

| 기능                       | 이전 방식 (`unstable_noStore`)                 | 권장 방식 (`connection`)             |
|---------------------------|----------------------------------------------|----------------------------------|
| 캐시 사용 여부 제어         | `noStore()` 호출로 캐시 제외                     | `connection` API로 명확하게 연결 관리      |
| 안정성                     | 불안정한 API                                    | 안정화된 새 API                     |
| Next.js 버전 권장 사항        | 15 이전 버전에서 사용                            | Next.js 15 이상에서 사용 권장            |

---

Next.js의 캐시 처리 방식은 점점 발전하고 있으니, 공식 문서와 릴리즈 노트를 꾸준히 체크하면서 최신 방식을 따라가는 게 좋아요!  
궁금한 점이나 더 알고 싶은 부분 있으면 언제든 알려 주세요 :)

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

> 알아두면 좋은 점:  
unstable_noStore는 fetch의 cache: `no-store`와 동일한 기능을 합니다.  
unstable_noStore가 export const dynamic = `force-dynamic`보다 더 선호되는 이유는, 컴포넌트 단위로 더 세밀하게 적용 가능하기 때문입니다.

- 참고로, unstable_noStore를 unstable_cache 내부에서 사용한다고 해서 static generation에서 벗어나는 건 아닙니다. 대신 캐시 설정을 따라 결과를 캐싱할지 결정하게 됩니다.

## 사용법

fetch 함수에 cache: `no-store` 옵션이나 next: { revalidate: 0 }와 같은 추가 옵션을 전달하기 번거로울 때, 혹은 fetch가 지원되지 않는 상황에서는 noStore() 함수를 대신 사용할 수 있습니다. 이 함수가 위 옵션들의 대체재 역할을 해 준답니다.

---

### 추가 팁

Next.js에서 static generation과 dynamic rendering은 성능과 개발환경에 큰 영향을 끼치죠. 캐시 설정을 잘 이해하면 불필요한 데이터 호출을 줄여서 UX도 개선할 수 있어요.

`unstable_noStore`를 각 컴포넌트에 유연하게 적용하면, 페이지 전체가 아닌 특정 부분만 최신 상태로 유지하는 전략을 세우기 좋습니다.  
따라서, 데이터가 자주 바뀌는 UI 부분에는 `unstable_noStore`를, 정적인 부분에는 기본 캐싱을 적용하는 하이브리드 방식을 추천해요.

앞으로 Next.js를 쓰면서 API 호출 최적화를 고민할 때 한 번 참고해보세요!

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
import { unstable_noStore as noStore } from 'next/cache';

export default async function ServerComponent() {
  noStore();
  const result = await db.query(...);
  ...
}
```

## 버전 히스토리

| 버전        | 변경 내용                                  |
|-------------|-----------------------------------------|
| `v15.0.0`   | `unstable_noStore`가 더 이상 권장되지 않고 `connection`으로 대체됨. |
| `v14.0.0`   | `unstable_noStore`가 새롭게 도입됨.                |

---

위 코드는 Next.js에서 `unstable_noStore`라는 기능을 사용하는 예제입니다. 이 함수는 서버 컴포넌트에서 캐시 저장을 방지하는 역할을 해주는데요, 데이터베이스 쿼리처럼 매번 최신 데이터를 받아와야 할 때 유용합니다.

하지만 `v15.0.0` 버전부터는 `unstable_noStore` 대신 `connection` API로 대체되었으니, 최신 버전을 사용한다면 해당 부분을 업데이트 하는 것이 좋겠죠. `unstable_` 접두어가 붙은 함수들은 실험적 기능이라 버전 업그레이드 시 변경 가능성이 있으니, 사용 시 주의해야 합니다.

참고로, Next.js의 이런 캐시 관련 기능들은 서버 컴포넌트와 클라이언트 컴포넌트의 데이터 관리 방식을 효율적으로 하기 위한 것으로, 서버에서 데이터를 가져올 때 얼마나 캐시를 활용할지 세밀하게 조절할 수 있게 해줍니다. 특히 데이터 신선도가 중요한 서비스라면 이런 기능을 잘 활용해보시길 추천드려요!