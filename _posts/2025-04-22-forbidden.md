---
title: "Next.js 15에서 Forbidden 에러 해결하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 03:03
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "forbidden"
link: "https://nextjs.org/docs/app/api-reference/functions/forbidden"
isUpdated: false
---


# forbidden 함수 사용법과 설정 가이드

안녕하세요! 오늘은 Next.js에서 권한 오류를 처리할 때 아주 유용한 `forbidden` 함수에 대해 알아볼게요. 이 함수는 권한이 없을 때 403 에러 페이지를 띄워주는데, 기본 제공 UI를 `forbidden.js` 파일로 직접 커스터마이징할 수도 있어서 유용하답니다.

## forbidden 함수란?

`forbidden` 함수는 사용자가 접근 권한이 없을 때 호출하면, Next.js가 자동으로 403 에러 페이지를 렌더링해 줍니다. 보통 로그인 상태나 권한 상태를 체크할 때 유용하게 쓸 수 있어요.

## 설정 방법

`forbidden` 함수를 쓰려면 먼저 `next.config.js`에서 실험적 기능인 `authInterrupts` 옵션을 활성화해야 합니다. 아래처럼 설정해 주세요:

```js
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  experimental: {
    authInterrupts: true,
  },
}

export default nextConfig
```

이 설정을 하면 `forbidden` 함수를 호출했을 때 Next.js가 자동으로 권한 부족 화면을 보여주게 됩니다.

## forbidden.js로 UI 커스터마이징하기

기본 403 화면 대신 회사나 프로젝트에 맞는 디자인으로 꾸미고 싶다면, 앱 루트에 `forbidden.js` 파일을 만들어서 커스텀 UI를 구현할 수 있어요.

예를 들어:

```jsx
export default function Forbidden() {
  return (
    <div style={{ textAlign: 'center', padding: '2rem' }}>
      <h1>403 - 접근 금지</h1>
      <p>죄송합니다만, 이 페이지에 접근할 권한이 없습니다.</p>
      <button onClick={() => window.location.href = '/login'}>
        로그인 페이지로 가기
      </button>
    </div>
  )
}
```

이런 식으로 친근한 UI를 만들어 놓으면 사용자 경험이 더 좋아질 거예요.

## 팁: 권한 체크 로직과 연동하기

실제로는 서버나 클라이언트에서 인증 정보를 확인한 후, 권한이 없으면 `forbidden()`을 호출하는 식으로 동작합니다. 예를 들어:

```js
import { forbidden } from 'next/dist/server/api-utils'

export async function getServerSideProps(context) {
  const user = await getUserFromSession(context.req)

  if (!user || !user.hasAccess) {
    throw forbidden()
  }

  return { props: { user } }
}
```

이렇게 하면 권한이 없는 사용자에겐 자동으로 403 페이지가 렌더링됩니다.

---

Next.js에서 권한 관련 에러 처리를 깔끔히 하고 싶다면 `forbidden` 함수와 `authInterrupts` 설정을 적극 활용해 보세요. 기본 UI는 귀찮으면 그대로 써도 좋고, 필요하면 `forbidden.js`로 멋지게 꾸며 보시고요!

필요한 내용 있으면 언제든 질문 남겨 주세요~ :)

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

next.js에서 `forbidden` 함수는 Server Components, Server Actions, 그리고 Route Handlers 안에서만 호출할 수 있어요. 이 함수는 권한이 없는 사용자가 특정 페이지에 접근하려 할 때 간단하게 403 상태 코드를 반환하도록 도와주는데요, 예를 들어 관리자 페이지 같은 경우에 유용하게 사용할 수 있죠.

아래 예시를 한번 보세요:

```jsx
import { verifySession } from '@/app/lib/dal'
import { forbidden } from 'next/navigation'
 
export default async function AdminPage() {
  const session = await verifySession()
 
  // 사용자가 'admin' 역할인지 확인
  if (session.role !== 'admin') {
    forbidden() // 권한 없으면 403 에러를 발생시켜요.
  }
 
  // 권한이 있으면 관리자 페이지 렌더링
  return <></>
}
```

여기서 `verifySession()`은 사용자 세션을 확인하고, 세션에 포함된 `role` 정보를 바탕으로 권한 검사를 하게 됩니다. 만약 역할이 'admin'이 아니라면 `forbidden()` 함수를 호출해서 접근을 차단하는 거죠.

### 알고 있으면 좋은 점
- `forbidden` 함수는 루트 레이아웃(root layout)에서는 호출할 수 없어요. 루트 레이아웃에서 호출하면 에러가 발생하니 주의하세요.
- `forbidden()` 이 호출되면 HTTP 상태 코드 403이 자동으로 반환되고, 페이지는 더 이상 렌더링되지 않습니다.
- 이 방법은 클라이언트 사이드가 아닌 서버 사이드에서 권한 체크가 진행되기 때문에 보안적으로 훨씬 안전합니다.

요약하면, Next.js에서 권한 제어를 할 때 서버 컴포넌트나 서버 액션, 라우트 핸들러에서 `forbidden()`을 이용해 간편하고 안전하게 접근을 제한할 수 있다는 점 기억하세요!

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

## 예제

### 역할(role) 기반 라우트 보호

`forbidden` 함수를 사용하면 유저의 역할에 따라 특정 페이지 접근을 제한할 수 있어요. 인증은 되었지만 권한이 없는 사용자가 중요한 페이지에 들어오는 걸 막는 데 아주 유용하답니다.

```js
import { verifySession } from '@/app/lib/dal'
import { forbidden } from 'next/navigation'

export default async function AdminPage() {
  const session = await verifySession()

  // 유저의 역할이 'admin'인지 확인하기
  if (session.role !== 'admin') {
    forbidden()  // 권한 없으면 접근 금지 처리
  }

  // 권한이 확인된 사용자에게 관리자 페이지 렌더링
  return (
    <main>
      <h1>Admin Dashboard</h1>
      <p>Welcome, {session.user.name}!</p>
    </main>
  )
}
```

---

### 조금 더 설명을 덧붙이자면

- `verifySession()` 같은 함수는 보통 로그인 상태와 사용자 정보를 서버에서 확인하는 역할을 해요.
- `forbidden()`이 호출되면 Next.js가 자동으로 403 상태 코드를 반환하면서 페이지 렌더링을 막아요.
- 이 구조를 응용하면 관리자뿐만 아니라 일반 회원, VIP 등 다양한 권한 기반 페이지 구성이 깔끔하게 가능하답니다.
- 클라이언트 쪽이 아니라 서버 사이드에서 권한 검사를 하기에 보안도 한층 강해지죠.

이런 식으로 `forbidden()`을 적절히 써서 중요한 페이지에 사용자 권한을 확실히 적용할 수 있답니다!

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

### Server Actions에서의 Mutation 사용하기

서버 액션(Server Actions)으로 뮤테이션(mutation)을 구현할 때, 민감한 데이터를 업데이트하는 작업은 꼭 특정 권한이 있는 사용자만 할 수 있도록 제한하는 게 중요해요. 예를 들어, 관리자(admin)만 역할(role)을 변경할 수 있게 막는 방법을 살펴볼게요.

```js
'use server'

import { verifySession } from '@/app/lib/dal'
import { forbidden } from 'next/navigation'
import db from '@/app/lib/db'

export async function updateRole(formData: FormData) {
  const session = await verifySession()

  // 관리자만 역할 변경 권한을 부여
  if (session.role !== 'admin') {
    forbidden()
  }

  // 권한 확인 후 역할 업데이트 로직 수행
  // ...
}
```

여기서 중요한 건 `verifySession()` 함수를 통해 현재 사용자의 세션 정보를 받아와서, 역할(role)이 'admin'인지 확인하는 부분이에요. 만약 관리자가 아니라면, `forbidden()` 함수를 호출해서 접근을 막아요. 이 함수는 Next.js의 내비게이션에서 권한을 제한할 때 쓰이는 함수로, 권한이 없으면 클라이언트에 적절한 에러 페이지를 보여줍니다.

---

### 덧붙여서!

- **세션 검증은 꼭 서버에서!**  
  세션이나 권한 체크 등 보안과 관련된 로직은 클라이언트가 아니라 서버에서 처리해야 안전합니다.

- **forbidden() 외 에러 처리**  
  상황에 따라서는 `forbidden()` 대신 직접 403 응답을 커스텀할 수도 있지만, Next.js 내장 함수 사용이 더 간편하고 안전하죠.

- **코드 구조 고민하기**  
  `updateRole` 같은 함수는 재사용성, 테스트 용이성 측면에서 최대한 작게, 명확하게 작성하는 게 좋아요.

- **db 연동**  
  주석 처리된 부분에는 실제 데이터베이스 업데이트 로직을 넣으면 되는데, 예를 들어 Prisma 같은 ORM을 사용한다면, `db.user.update()` 같은 함수를 호출해서 처리할 수 있어요.

서버 액션에서 뮤테이션을 처리할 때 권한 체크를 깔끔하게 하는 방법, 참고가 되었으면 좋겠습니다!

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

아래는 위의 HTML table 태그를 Markdown 형식으로 바꾼 내용입니다.

| Version   | Changes                   |
|-----------|---------------------------|
| `v15.1.0` | `forbidden` introduced.   |

여기서 `forbidden`이라는 새 기능이 v15.1.0 버전에 추가됐다고 하는데요, 보통 이런 변화는 특정 상황에서 접근을 제한하거나 권한을 관리할 때 활용되곤 합니다. 만약 여러분이 API나 설정에서 권한 관련 처리를 한다면, `forbidden` 옵션이 어떤 역할을 하는지 한번 체크해보는 것도 좋겠네요!