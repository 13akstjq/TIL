---
title: "Next.js 15 unstable_cache로 캐싱 최적화하는 최신 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:47
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "unstable_cache"
link: "https://nextjs.org/docs/app/api-reference/functions/unstable_cache"
isUpdated: false
---


# unstable_cache 함수 소개

> 참고: 이 API는 아직 불안정 버전이며, 안정화되면 use cache라는 새로운 API로 대체될 예정이에요.

`unstable_cache`는 데이터베이스 쿼리처럼 비용이 많이 드는 작업 결과를 캐싱해서, 여러 요청 간에 그 결과를 재사용할 수 있게 도와주는 함수입니다. 덕분에 서버나 클라이언트에서 반복적인 작업을 줄여 성능을 개선할 수 있죠.

예를 들어, 사용자 정보를 데이터베이스에서 가져오는 작업이 있다고 할 때, `unstable_cache`로 감싸면 한 번 캐시된 결과를 재사용해서 같은 데이터를 다시 가져올 때 시간을 아낄 수 있어요.

아래 코드를 볼게요.

```js
import { getUser } from './data';
import { unstable_cache } from 'next/cache';

const getCachedUser = unstable_cache(
  async (id) => getUser(id),
  ['my-app-user']
);

export default async function Component({ userID }) {
  const user = await getCachedUser(userID);
  // ...
}
```

위 예제에서는 `getUser`라는 비싼 연산(사용자 정보 조회 함수)을 `unstable_cache`로 감싸서 `getCachedUser`라는 캐시된 함수를 만들었어요. 그 후 `Component` 컴포넌트 내에서 `getCachedUser`를 통해 빠르게 캐시된 사용자 정보를 받아올 수 있습니다.

---

## 조금 더 알아두면 좋은 점들

- `unstable_cache`의 두 번째 인자인 배열(`['my-app-user']`)은 캐시 키로 사용돼요. 이 키를 통해 캐시를 그룹화하거나 특정 조건에 따라 캐시를 재사용하거나 갱신할 수 있어요.
- 이 API는 아직 실험적이므로, 향후 Next.js 업데이트 시 API가 변경될 수 있다는 점 참고하세요.
- 캐시되는 데이터가 민감하거나 동적으로 자주 바뀌는 경우, 적절한 만료 전략과 무효화 로직이 꼭 필요해요.
- React 서버 컴포넌트에서 특히 유용하게 사용할 수 있는데, 서버 측에서 불필요하게 중복된 데이터 로드를 줄여 페이지 렌더링 속도를 높여줍니다.

이처럼 `unstable_cache`는 데이터 요청을 줄여 성능을 개선하는 좋은 수단이에요. 다만 안정적인 API가 나올 때까지는 테스트 환경에서 사용해보면서 차차 적용해보는 걸 추천드립니다!

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
캐시 범위 내에서 헤더나 쿠키 같은 동적인 데이터 소스에 접근하는 것은 지원되지 않습니다. 만약 캐시된 함수 안에서 이런 데이터가 필요하다면, 캐시 함수 외부에서 헤더를 받아서 필요한 동적 데이터를 함수 인자로 전달하는 방식을 사용하세요.
이 API는 Next.js의 내장 데이터 캐시 기능을 활용하여 요청이나 배포가 바뀌어도 결과를 유지합니다.

> 경고: 이 API는 아직 불안정한 상태이며 앞으로 변경될 수 있습니다. API가 안정화되면 마이그레이션 문서와 코드 변환 도구(codemods)를 제공할 예정입니다.

## 파라미터 설명

```js
const data = unstable_cache(fetchData, keyParts, options)()
```

위 코드는 `unstable_cache`라는 함수를 사용해 데이터를 캐싱하는 예시인데요, 여기서 각 인자가 무엇을 의미하는지 조금 더 자세히 알려드릴게요.

| 파라미터명  | 설명                                   |
|-------------|--------------------------------------|
| `fetchData` | 캐시할 데이터를 가져오는 함수          |
| `keyParts`  | 캐시 키를 구성하는 값 혹은 배열 (캐시 식별자 역할) |
| `options`   | 캐싱 동작을 제어하는 옵션 (예: 만료 시간 등)      |

이 함수를 호출하면 `fetchData` 함수가 반환하는 결과를 캐시에 저장해 두고, 다음부터는 같은 키로 요청할 때 캐시된 값을 재사용합니다. 서버 사이드 렌더링이나 API 라우트에서 자주 사용하는 패턴이죠.

> 팁!
동적 데이터를 다뤄야 할 때 헤더나 쿠키를 함수 외부에서 받아 인자로 넘기는 걸 꼭 기억하세요! 그래야 캐시가 잘 작동하면서도 필요한 동적 정보를 사용할 수 있습니다.

또한, `unstable_cache`는 아직 실험적인 기능이라 버전 업데이트 시 API가 달라질 수 있으니 프로젝트 적용 시 주의하세요! 필요하면 공식 문서나 업데이트 노트를 자주 확인하는 게 좋습니다.

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

fetchData: 비동기 함수로, 캐싱하려는 데이터를 가져와야 해요. 이 함수는 반드시 Promise를 반환해야 합니다.  
keyParts: 캐시 식별을 좀 더 세분화하기 위한 추가 키 배열이에요. 기본적으로 unstable_cache는 함수 인자와 함수의 문자열화된 버전을 캐시 키로 사용합니다. 대부분의 경우 선택 사항이지만, 매개변수로 전달하지 않는 외부 변수를 사용할 때 꼭 필요해요. 특히 클로저로 사용하는 변수는 여기 꼭 넣어줘야 합니다.  
options: 캐시 동작을 제어하는 객체인데, 다음과 같은 속성을 가질 수 있어요:
- tags: 캐시 무효화를 제어하는 데 사용하는 태그의 배열입니다. Next.js는 이 태그를 함수의 고유 식별자로 사용하지는 않아요.
- revalidate: 캐시를 다시 검증할 시간(초)입니다. 생략하거나 false를 전달하면 무기한 캐시하거나, revalidateTag() 또는 revalidatePath()가 호출될 때까지 캐시가 유지돼요.

---

### 반환값

unstable_cache는 호출 시 캐시된 데이터를 반환하는 함수(비동기)를 리턴합니다. 만약 캐시에 데이터가 없으면, 전달한 함수가 호출되고 결과가 캐시되어 반환됩니다.

---

## 예시

```javascript
import { unstable_cache } from 'next/cache';

const fetchUsers = unstable_cache(async (page) => {
  const res = await fetch(`/api/users?page=${page}`);
  if (!res.ok) throw new Error('Failed to fetch users');
  return res.json();
}, {
  revalidate: 60,  // 60초마다 캐시 재검증
  tags: ['users'],
});

export async function getData() {
  const users = await fetchUsers(1);
  return users;
}
```

---

### 추가 팁!  
- `keyParts`는 캐시 키 생성 시 핵심적인 역할을 하는데, 예를 들어 전역 상태나 설정 값을 참조할 때 꼭 넣어주면 캐시가 꼬이는 걸 방지할 수 있어요.  
- `revalidate`를 너무 짧게 설정하면 캐시 효율이 떨어지고, 너무 길게 하면 오래된 데이터를 쓰게 되니 상황에 맞게 적절히 조절해주시는 게 좋아요!  
- `tags`는 나중에 특정 태그에 해당하는 캐시를 한 번에 무효화할 때 유용합니다. 예를 들어 사용자 관련 데이터가 변했을 때 'users' 태그를 가진 모든 캐시를 리셋할 수 있죠.  

캐시를 잘 활용하면 서버 부담도 줄이고, 사용자 경험도 훨씬 좋아질 테니 꼭 한번 시도해보세요!

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

자, 오늘은 Next.js에서 `unstable_cache`라는 캐싱 함수가 어떻게 쓰이는지 간단하게 살펴볼게요. 코드 예제를 보면서 함께 이해해봐요!

```js
import { unstable_cache } from 'next/cache'
 
export default async function Page({
  params,
}: {
  params: Promise<{ userId: string }>
}) {
  const { userId } = await params
  const getCachedUser = unstable_cache(
    async () => {
      return { id: userId }
    },
    [userId], // userId를 캐시 키에 포함해서 특정 사용자 데이터를 캐싱
    {
      tags: ['users'], // 태그로 캐시 그룹 지정, 나중에 태그 기반으로 재검증 가능
      revalidate: 60,  // 60초 후 캐시 자동 무효화(재요청 시 다시 호출)
    }
  )

  //...
}
```

### 이 코드가 하는 일은?

- `unstable_cache` 함수는 Next.js에서 제공하는 비공식(unstable) 캐시 함수예요.
- 첫 번째 인자는 실제 실행할 비동기 함수 (이 경우 `userId`를 받아 간단한 객체를 반환).
- 두 번째 인자는 캐시 키를 결정하는 배열로, `userId`가 바뀌면 캐시도 새로 생성됩니다.
- 세 번째 옵션 객체에는 `tags`와 `revalidate` 같은 설정들이 있어요.
  - `tags`는 특정 그룹으로 묶어, 같은 태그를 가진 캐시들을 한 번에 무효화 할 때 사용 가능.
  - `revalidate`는 위 캐시가 자동으로 재갱신되는 시간(초) 설정입니다.

### 제가 더 알려드리고 싶은 점!

- `unstable_cache`가 "unstable"인 이유는 아직 공식 API가 완전히 안정화된 게 아니기 때문이에요. 그러니 프로덕션용으로 쓸 때는 주의가 필요하겠죠.
- 하지만 Next.js 14부터 도입된 이 기능은 서버 컴포넌트나 API 라우트에서 데이터 중복 호출을 줄이고, 성능 최적화에 꽤 도움을 줄 수 있어요.
- 또, 태그 기반 캐싱 덕분에 관련된 여러 데이터들을 한 번에 무효화하는 전략을 쉽게 구현할 수 있답니다.
- 캐싱 정책을 잘 설계하면 페이지 렌더링 속도가 확실히 빨라지니, 꼭 자기 프로젝트에 맞춰 실험해 보세요!

---

## 버전 히스토리

| Version   | Changes                  |
|-----------|--------------------------|
| v14.0.0   | `unstable_cache` 도입   |

Next.js의 최신 기능들을 잘 활용해서 여러분 프로젝트도 한층 더 빠르고 똑똑하게 만들 수 있길 바랄게요!