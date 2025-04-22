---
title: "Nextjs 15에서 unauthorized 함수 권한 처리하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:47
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "unauthorized"
link: "https://nextjs.org/docs/app/api-reference/functions/unauthorized"
isUpdated: false
---


# unauthorized 함수 사용법

Next.js에서 `unauthorized` 함수는 권한 문제가 발생했을 때 401 에러 페이지를 보여주는 역할을 해요. 즉, 사용자가 인증되지 않았을 경우에 적절히 오류를 처리할 수 있게 도와주는 거죠. 특히 인증 오류가 생겼을 때 기본으로 제공되는 UI 말고, 직접 `unauthorized.js` 파일을 만들어서 커스텀 디자인으로 바꿔줄 수도 있어요!

### 사용 준비: experimental 옵션 켜기

이 `unauthorized` 함수 기능을 사용하려면, `next.config.js` 파일에 실험적 기능인 `authInterrupts` 옵션을 활성화해줘야 해요. 설정 방법은 간단해요:

```js
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  experimental: {
    authInterrupts: true,
  },
}

export default nextConfig
```

이 옵션을 켜면 Next.js가 인증 관련 오류를 감지하고 자동으로 401 페이지를 렌더링해줘서, 훨씬 깔끔하게 권한 문제를 관리할 수 있습니다.

---

### 조금 더 알아보기

- `unauthorized.js` 파일을 통해 원하는 스타일이나 메시지로 401 페이지를 커스터마이징할 수 있으니, 사용자 경험에 맞게 꼭 만들어보세요!
- 이 기능은 Next.js 최신 버전에서 실험적으로 제공되고 있으니, 업데이트 내용과 안정성은 공식 문서를 계속 체크하는 게 좋아요.
- 만약 인증이 필수인 페이지에서 `unauthorized()`를 호출하면, 자동으로 401 페이지로 넘어가므로 별도의 라우팅 처리가 필요 없답니다.

이제 `unauthorized` 함수로 인증 이슈를 더 쉽게 처리할 수 있으니, 다음 프로젝트에 바로 적용해 보세요!

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

이번에는 Next.js에서 인증 처리를 할 때 유용한 `unauthorized` 함수 사용법에 대해 살펴볼게요.

---

```js
import { verifySession } from '@/app/lib/dal'
import { unauthorized } from 'next/navigation'
 
export default async function DashboardPage() {
  const session = await verifySession()
 
  if (!session) {
    unauthorized()
  }
 
  // 로그인 된 사용자에게 대시보드 보여주기
  return (
    <main>
      <h1>Welcome to the Dashboard</h1>
      <p>Hi, {session.user.name}.</p>
    </main>
  )
}
```

위 코드는 서버 컴포넌트(Server Component)에서 세션을 확인하고, 인증된 사용자가 아니라면 `unauthorized()` 함수를 호출해서 접근을 차단하는 예제입니다. `unauthorized()`가 호출되면 보통 401 Unauthorized 상태 코드가 리턴되면서 페이지가 차단돼요.

여기서 중요한 점은 `unauthorized` 함수가 **Server Components, Server Actions, Route Handlers에서만 호출 가능하다**는 거예요. 그래서 클라이언트 컴포넌트나 루트 레이아웃(root layout) 같은 데서는 사용할 수 없으니 참고하세요!

---

### 조금 더 알아보기

- `unauthorized()` 함수는 Next.js의 라우팅 시스템과 자연스럽게 연동돼서 인증되지 않은 사용자를 즉시 차단해줍니다.
- 서버쪽에서 인증 로직을 처리하면 클라이언트로 불필요한 데이터가 노출되는 걸 막을 수 있어서 보안이 강화돼요.
- 루트 레이아웃에서는 인증 처리 로직 대신, 페이지별로 인증 컴포넌트를 따로 구분해서 사용하는 게 일반적입니다.

---

이처럼 서버 컴포넌트에서 인증 제어를 하면서 사용자 경험도 자연스럽게 유지할 수 있으니, Next.js에서 인증 문제를 다룰 땐 이 방법을 꼭 활용해보세요!

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

## 예제들

### 로그인하지 않은 사용자에게 로그인 UI 보여주기

로그인하지 않은 사용자에게 로그인 UI를 보여주고 싶을 때는 `unauthorized` 함수를 활용할 수 있어요. 이 함수는 `unauthorized.js` 파일을 렌더링하는 역할을 하죠.

```js
import { verifySession } from '@/app/lib/dal'
import { unauthorized } from 'next/navigation'

export default async function DashboardPage() {
  const session = await verifySession()

  if (!session) {
    unauthorized()
  }

  return <div>Dashboard</div>
}
```

위 코드에서 `verifySession()`은 사용자의 세션을 확인하는 함수에요. 세션이 없으면(즉, 로그인하지 않은 상태면) `unauthorized()`를 호출해서 로그인 페이지 같은 별도의 UI를 보여줍니다.

---

조금 덧붙이자면, `unauthorized()`를 호출하면 보통 Next.js 앱에서 미리 정의해둔 로그인 화면으로 리다이렉트하거나 특정 컴포넌트를 렌더링하도록 설정해둔 경우가 많아요. 따라서 이 함수를 통해 인증되지 않은 사용자의 접근을 깔끔하게 차단하고, 친절한 로그인 유도 화면을 보여줄 수 있죠.

더불어, `verifySession` 같은 인증 관련 함수는 보통 서버 측에서 세션이나 토큰을 확인해서 로그인 상태를 판단하는데, Next.js의 서버 컴포넌트나 API 라우트에서 많이 사용됩니다. 만약 클라이언트 측에서 인증 상태를 확인해야 한다면, React의 상태 관리나 Context API와 같은 방법도 함께 사용하곤 합니다.

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

이번 글에서는 Next.js에서 인증되지 않은 사용자에게 401 Unauthorized 페이지를 보여주고, 서버 액션(Server Actions)에서 인증 검증을 하는 방법을 다뤄볼게요.

## 401 Unauthorized 페이지 만들기

먼저, 인증되지 않은 사용자가 접근했을 때 보여줄 페이지를 만들어볼게요. `UnauthorizedPage` 컴포넌트는 이렇게 생겼어요.

```jsx
import Login from '@/app/components/Login'

export default function UnauthorizedPage() {
  return (
    <main>
      <h1>401 - Unauthorized</h1>
      <p>이 페이지에 접근하려면 로그인이 필요해요.</p>
      <Login />
    </main>
  )
}
```

여기서 중요한 점은 `Login` 컴포넌트를 함께 렌더링해서, 사용자가 바로 로그인할 수 있는 UI를 제공하는 거예요. 사용자 경험이 훨씬 좋아지겠죠? 만약 로그인 UI가 별도로 없다면, 간단한 로그인 버튼이나 링크를 만들어도 괜찮아요.

## 서버 액션에서 인증 검사하기

이제 조금 더 심화된 내용으로, Next.js의 서버 액션(Server Actions)을 활용해 인증된 사용자만 특정 작업(예: 프로필 업데이트)을 할 수 있도록 제한하는 코드를 알아볼게요.

```js
'use server'

import { verifySession } from '@/app/lib/dal'
import { unauthorized } from 'next/navigation'
import db from '@/app/lib/db'

export async function updateProfile(data: FormData) {
  const session = await verifySession()

  // 인증되지 않은 경우 401 에러로 처리
  if (!session) {
    unauthorized()
  }

  // 인증된 경우 mutation 로직 진행
  // 실제 업데이트 로직 작성
}
```

### 여기서 주목할 점!

- `verifySession()` 함수는 사용자의 세션 정보를 확인해주는 커스텀 함수예요. 일반적으로 토큰이나 쿠키 정보를 검증해서 로그인 상태를 판단합니다.
- `unauthorized()` 함수는 Next.js 내장 네비게이션 함수로, 호출 시 401 Unauthorized 상태를 반환하고 해당 페이지로 리다이렉트해줍니다.
- 이렇게 하면 클라이언트 사이드에서 무작정 요청이 들어오는 걸 방지하고, 서버단에서 확실하게 검증할 수 있겠죠?

---

### 추가 팁

- 서버 액션은 React Server Components와 매우 잘 어울립니다. 서버에서 바로 데이터를 검증하고 변경할 수 있어 보안성을 높일 수 있어요.
- 만약 더 복잡한 인증 로직(예: 권한 레벨, 역할 기반 접근 제어 등)을 추가해야 한다면, `verifySession()` 함수 확장과 함께 별도의 미들웨어를 만들어 관리하는 걸 추천합니다.
- 에러 페이지 UI를 꾸밀 때는 사용자가 왜 접근이 제한됐는지 명확히 알 수 있도록 친절한 메시지를 넣는 게 좋아요.

---

이번 포스트를 통해 Next.js 13+에서 인증 관련 에러 핸들링과 서버 액션 내 인증 체크를 어떻게 구현하는지 감 잡으셨으면 좋겠네요! 실제로 적용해보면서 필요한 부분을 커스터마이즈해 보세요. 궁금한 점 있으면 댓글로 남겨주세요 :)

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

### Route Handlers에서 데이터 가져오기

Next.js에서 Route Handlers를 사용하면 인증된 사용자만 특정 API 엔드포인트에 접근하도록 쉽게 제한할 수 있어요. 특히, `unauthorized` 함수를 활용하면 인증이 안 된 사용자는 자동으로 401 에러 페이지를 보여주도록 할 수 있답니다.

```js
import { NextRequest, NextResponse } from 'next/server'
import { verifySession } from '@/app/lib/dal'
import { unauthorized } from 'next/navigation'
 
export async function GET(req: NextRequest): Promise<NextResponse> {
  // 사용자의 세션을 검증합니다.
  const session = await verifySession()
 
  // 세션이 없으면 401 상태 코드를 반환하면서 unauthorized.tsx 컴포넌트를 렌더링합니다.
  if (!session) {
    unauthorized()
  }
 
  // 필요한 데이터를 가져오는 로직을 작성하세요.
  // ...
}
```

이렇게 하면 인증이 되어야만 GET 요청에 접근할 수 있죠. `unauthorized()`를 호출하면 Next.js가 자동으로 401 상태와 함께 지정한 `unauthorized.tsx` 페이지를 보여주니까, 사용자 경험도 좋고 보안도 챙길 수 있어요.

추가로, `verifySession()` 같은 함수는 여러분 프로젝트 상황에 맞게 세션 검증 로직을 작성해야 하는데, 보통은 JWT 토큰 검사, 쿠키 확인, 데이터베이스 조회 같은 작업이 포함될 수 있어요. 인증 체크 로직이 서버 사이드에서 확실히 구동되니 클라이언트보다 훨씬 안전하답니다.

**참고:** `unauthorized()`는 라우트 핸들러에서 사용 가능한 함수로, 이 함수 호출 이후에는 응답을 작성하지 않아도 Next.js가 알아서 401 응답을 처리해 줘요.

---

다음에는 이 기능이 어떤 버전부터 지원되는지, 그리고 더 복잡한 인증 시나리오들에 대해 이야기해볼게요. 개발하면서 궁금한 점이 있으면 언제든 질문해 주세요!

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

| Version   | Changes                 |
|-----------|-------------------------|
| `v15.1.0` | `unauthorized` introduced. |

여기서 `unauthorized`라는 게 새로 추가됐다고 해요. 보통 웹 개발이나 API 쪽에서 `unauthorized`는 인증이 필요한 리소스에 대해 권한이 없을 때 발생하는 상태를 말하는데요. 이번 버전(`v15.1.0`)에 이 기능이나 상태가 새로 추가된 듯해요. API를 다루다 보면 401 Unauthorized 상태 코드를 다루는 일이 많으니, 이 부분을 참고해두면 좋아요! 만약 이 변화가 특정 라이브러리나 프레임워크에 관한 거라면, 공식 문서를 한 번 확인해서 구체적인 사용법도 알아두면 도움이 될 거예요.