---
title: "Next.js 15 서버 사이드 렌더링 사용법(use server)"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:14
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "use server"
link: "https://nextjs.org/docs/app/api-reference/directives/use-server"
isUpdated: false
---


# use server

'use server' 지시어는 특정 함수나 파일이 서버 측에서 실행되어야 함을 명시하는 역할을 해요. 파일 맨 위에 쓰면 그 파일 안의 모든 함수가 서버에서 실행된다는 뜻이고, 함수 바로 위에 inline으로 쓰면 그 함수만 서버 함수(Server Function)로 지정할 수 있답니다. 이건 React에서 제공하는 기능이에요.

## 파일 맨 위에 use server 쓰기

아래 예시는 파일 최상단에 `use server` 지시어를 넣은 경우에요. 이렇게 하면 그 안에 있는 모든 함수는 서버에서 실행됩니다.

```js
'use server';

export async function getServerData() {
  // 서버에서만 실행되는 로직
  const data = await fetch('https://api.example.com/data');
  return data.json();
}

export function helperFunction() {
  // 이 함수도 서버에서 실행돼요
}
```

### 추가 설명!

- `use server`를 쓰는 이유는 클라이언트에서 실행하면 안 되는 무거운 연산, 비밀 정보 처리, 데이터베이스 쿼리 등을 서버 쪽으로 감추기 위함이에요.
- React 컴포넌트 내에서 서버 함수 호출 시 클라이언트 코드와 서버 코드를 깔끔하게 분리할 수 있어서 보안과 성능 면에서 유리합니다.
- 만약 한 파일에 서버 함수와 클라이언트 함수가 혼재되어 있으면, 필요한 함수에만 inline으로 `use server`를 붙여서 관리하는 걸 추천해요.

서버 함수 잘 활용하면 앱 구조를 더 명확하게 하고, 유지보수도 쉬워지니 꼭 익혀두시면 좋아요!

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
// actions.ts
"use server"

import { db } from '@/lib/db'

export async function fetchUsers() {
  const users = await db.user.findMany()
  return users
}
```

이렇게 Server Functions는 `"use server"` 지시어를 파일 상단에 작성해줘야 해요. 그리고 만들고 나면 클라이언트 컴포넌트나 서버 컴포넌트 어디에서든 가져다 쓸 수 있게 됩니다.

예를 들어 클라이언트 컴포넌트에서 fetchUsers 함수를 사용하고 싶으면 이렇게 하면 돼요:

```tsx
"use client"

import { fetchUsers } from './actions'

export default function UserList() {
  async function loadUsers() {
    const users = await fetchUsers()
    console.log(users)
  }

  return <button onClick={() => loadUsers()}>Load Users</button>
}
```

여기서 포인트는 바로 클라이언트 컴포넌트에선 서버 함수가 비동기(Async) 함수라는 점이에요. 이걸 직접 호출하려면 이렇게 버튼 클릭 같은 이벤트 핸들러 안에서 호출하고 결과를 받아서 화면에 보여 주거나 로그를 찍는 식으로 사용합니다.

---

### Server Functions와 Database 연동 정리

| 설명              | 내용                                                         |
|-----------------|------------------------------------------------------------|
| Server Function 생성법    | `use server` 지시어를 파일 상단에 넣고 비동기 함수 작성                |
| Server 함수 위치          | 별도 파일(actions.ts 등)에 작성해 여러 컴포넌트에서 import 가능          |
| Client 컴포넌트에서 사용법 | async 함수 안에서 호출, 이벤트 핸들러에서 호출하는 식으로 비동기 처리        |
| 데이터베이스 연동          | 서버 함수 내에서 직접 `db` 클라이언트를 사용해 데이터 조회/수정 가능         |

---

### 참고 사항
- 아직 Next.js나 React에서 완전히 SSR과 Client 컴포넌트 경계가 명확하여 이 방식을 적용할 때 내가 직접 어떤 작업이 서버에서 일어나고 있는지 분명히 이해하고 사용해야 해요.
- 클라이언트에서 서버 함수를 호출할 때는 내부적으로 서버로 요청이 나가게 된다 생각하면 편해요. 그래서 고통 없는 데이터를 직접 다루는 것보다 네트워크 요청 비용이 있으니 꼭 필요한 경우에만 사용하세요.
- 또, 인증 처리 같은 민감한 로직은 항상 서버 함수에서 관리하는 게 안전합니다.

이 정도 핵심만 잘 이해해 두면 Server Functions를 효과적으로 쓸 수 있어요! 도움이 되었길 바랄게요 :)

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

이번 시간에는 Next.js의 Server Functions를 사용해서 데이터베이스에서 유저 데이터를 가져오는 예제를 살펴볼게요. 그리고 Fetch Users 함수를 클라이언트 컴포넌트에서 어떻게 호출할 수 있는지도 함께 알아봅니다.

---

### 1. Server Function 작성하기

아래 코드는 `fetchUsers`라는 Server Function입니다. 데이터베이스에서 모든 유저를 가져와서 반환해주죠.

```js
import { db } from '@/lib/db' // 데이터베이스 클라이언트 임포트

export async function fetchUsers() {
  const users = await db.user.findMany()
  return users
}
```

여기서 `db.user.findMany()`는 Prisma 같은 ORM을 사용한다고 가정하고, 유저들을 모두 조회하는 코드입니다.

---

### 2. 클라이언트 컴포넌트에서 Server Function 호출하기

이제 위에서 만든 `fetchUsers` 함수를 클라이언트 컴포넌트에서 호출해 보겠습니다.

```js
export default function MyButton() {
  return <button onClick={() => fetchUsers()}>Fetch Users</button>
}
```

버튼 클릭 시 `fetchUsers`가 실행됩니다.

---

### 참고

- 일반적으로 Server Function은 서버에서 실행하기 때문에, 클라이언트에서 직접 호출하려면 Next.js가 지원하는 방식으로 호출해야 합니다.
- 만약 바로 클라이언트에서 `fetchUsers`를 호출하면, 서버 측 환경 변수가 없거나 보안 이슈가 생길 수 있으니 주의하세요.
- Next.js 13 최신 기능 중 하나인 **Inline use server** 문법을 활용하면, 클라이언트 컴포넌트 안에서 서버 함수를 좀 더 쉽게 호출할 수 있습니다.

---

## Using `use server` inline

`use server`는 Server Action 혹은 Server Function을 클라이언트 컴포넌트 내에서 바로 선언하고 사용할 수 있는 문법입니다. 예를 들어,

```js
'use client'

import { useState } from 'react'

export default function MyButton() {
  async function fetchUsers() {
    'use server'  // 여기서 서버 함수 선언
    const users = await db.user.findMany()
    return users
  }

  return <button onClick={() => fetchUsers()}>Fetch Users</button>
}
```

이런 식으로 하면, `fetchUsers` 함수가 서버에서 실행되고 클라이언트에서 호출할 수 있습니다.

---

## 정리

| 개념                | 설명                                                         |
|---------------------|--------------------------------------------------------------|
| Server Function     | 서버에서만 실행되는 함수. 서버 자원에 직접 접근 가능          |
| 클라이언트에서 호출 | 클라이언트에서 호출 시 서버와 통신하거나, Next.js 방식 활용 필요 |
| `use server` inline | 클라이언트 컴포넌트 내에서 서버 함수를 선언하고 실행 가능     |

---

이제 여러분도 Next.js에서 Server Functions를 어떻게 만들고 클라이언트에서 호출하는지 감이 좀 오시죠? 요즘 프레임워크가 점점 더 편리해지고 있어서, 서버와 클라이언트 코드 구분이 한결 수월해지고 있어요. 필요할 때 서버 함수를 잘 활용하면 더 깔끔하고 안전한 앱을 만들 수 있답니다!

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

서버 함수(Server Function)를 표시할 때 `use server`를 함수 맨 위에 인라인으로 작성하는 방법에 대해 이야기해볼게요. 아래 예제를 보면, 데이터베이스에서 사용자 목록을 가져오는 함수가 있어요.

```js
import { db } from '@/lib/db' // 데이터베이스 클라이언트 불러오기

export default function UserList() {
  async function fetchUsers() {
    const users = await db.user.findMany()
    return users
  }

  return <button onClick={() => fetchUsers()}>Fetch Users</button>
}
```

이 예제에서는 `fetchUsers` 함수가 서버에서 실행되어야 하는데, 실제로 현재 함수 위에 `use server` 지시어가 빠져 있어요. 이걸 명시하면 Next.js 같은 프레임워크가 이 함수가 서버 전용임을 확실히 알 수 있답니다.

### 서버 함수에 `use server`를 추가할 때는 이렇게!

```js
import { db } from '@/lib/db' // 데이터베이스 클라이언트 불러오기

export default function UserList() {
  "use server"
  
  async function fetchUsers() {
    const users = await db.user.findMany()
    return users
  }

  return <button onClick={() => fetchUsers()}>Fetch Users</button>
}
```

혹은 함수 바로 앞에 붙이는 경우도 있어요.

```js
import { db } from '@/lib/db' // 데이터베이스 클라이언트 불러오기

export default function UserList() {
  const fetchUsers = async () => {
    "use server"
    const users = await db.user.findMany()
    return users
  }

  return <button onClick={() => fetchUsers()}>Fetch Users</button>
}
```

서버 함수는 클라이언트에 번들되지 않고, 서버에서만 실행되도록 보장해줘서 보안 측면에서 매우 중요해요.

---

## 보안 고려사항 (Security considerations)

`use server` 지시어를 사용해 서버 함수를 작성할 때는 보안을 꼭 염두에 두어야 해요. 서버에서 실행되는 만큼, 다음 사항들을 기억하세요:

| 체크포인트                    | 설명                                                                    |
|-----------------------------|------------------------------------------------------------------------|
| 민감한 데이터 보호하기          | 데이터베이스 연결 정보, API 키, 사용자 비밀정보 등은 절대 클라이언트에 노출되면 안 됩니다. 서버 함수 내부에서만 접근하도록 하세요. |
| 데이터 검증 및 인가 처리       | 클라이언트 요청 시 입력값을 반드시 검증하고, 인증과 권한 체크를 통해 불법 접근을 막으세요.          |
| 에러 처리 및 로그 관리          | 서버 함수에서 발생하는 예외 처리를 신경쓰고, 보안 관련 로그는 적절히 기록해 두세요.            |
| XSS 및 Injection 공격 방지    | 쿼리를 생성할 때 SQL 주입 또는 스크립트 주입 공격을 방지하도록 준비된 쿼리 사용과 데이터 정제 수행. |

서버 함수는 서버 전용 코드임을 명확하게 표시하고, 클라이언트에서는 최소한의 정보만 주고받도록 하는 게 핵심입니다. 이런 방법들을 지키면 더욱 안전한 웹 애플리케이션 개발에 한 걸음 더 다가갈 수 있어요.

---

더 자세한 내용이 궁금하면, 사용하는 프레임워크 문서에서 `Server Functions` 또는 `Server Components` 부분을 참고해 보세요. `use server` 지시어를 어떻게 활용하는지, 그리고 클라이언트와 서버 간 데이터 흐름을 어떻게 구성하는지 실무에 큰 도움이 될 거예요!

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

### 인증(Authentication)과 인가(Authorization)

서버 쪽에서 중요한 작업을 수행할 때는 항상 사용자가 누구인지 인증(Authentication)하고, 그 작업을 할 권한이 있는지를 확인하는 인가(Authorization)를 먼저 하셔야 해요.

아래 예제 코드를 살짝 바꿔가며 설명해볼게요.

```js
import { db } from '@/lib/db' // 데이터베이스 클라이언트
import { authenticate } from '@/lib/auth' // 인증 라이브러리

export async function createUser(data, request) {
  const user = await authenticate(request) // 요청에서 사용자 인증
  
  if (!user) {
    throw new Error('Unauthorized') // 인증 실패 시 에러 처리
  }
  
  // 인증된 사용자만 새 사용자 생성 가능
  const newUser = await db.user.create({ data })
  return newUser
}
```

위 코드에서 중요한 포인트는 `authenticate` 함수를 통해 사용자의 신원을 확인하는 과정을 거친다는 점이에요. 인증이 안 된 상태로 `createUser` 같은 주요 작업을 진행한다면 보안 이슈가 생길 수 있으니 꼭 확인해야 합니다.

### 인증과 인가, 이 둘의 차이

- **인증(Authentication)**: "너 누구야?" 라며 사용자 신원을 확인하는 과정  
- **인가(Authorization)**: "너 이 작업 해도 돼?" 라며 권한을 확인하는 과정

예를 들어, 로그인된 사용자가 있더라도 그 사람이 게시글 삭제 권한이 없다면 삭제할 수 없게 막는 게 바로 인가 절차가 됩니다.

### 덧붙여서

API 설계 시 보통 토큰(JWT 등)을 활용한 인증 방식을 많이 사용해요. 그리고 권한 관리는 역할(Role) 기반으로 하거나 특정 권한을 부여하는 식으로 구현하는 게 실무에서 흔하죠.

또한, 프론트엔드에서도 인증 상태를 관리하고, 서버에는 절대 신뢰할 수 없는 상태로 요청하지 않는 게 중요합니다! 서버가 항상 “내가 진짜 인증한 사용자와 이야기 중인지”를 검증해야 한다는 점, 꼭 기억하세요.

---

## 참고 문서

- [OAuth 2.0 공식 문서](https://oauth.net/2/)
- [JWT 소개와 활용법](https://jwt.io/introduction)
- [Node.js Authentication Best Practices](https://blog.nodejs.org/2021/10/05/node-js-authentication-best-practices/)

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

React 공식 문서에서 'use server'에 대한 자세한 내용을 확인할 수 있어요.  

'Use server'는 React 18부터 도입된 기능 중 하나로, 서버 컴포넌트를 사용할 때 주로 활용됩니다. 서버 컴포넌트를 통해 렌더링이 서버에서 이루어지기 때문에 클라이언트 사이드보다 초기 로딩 속도가 빠르고, SEO에도 좋다는 장점이 있어요.  

React 공식 문서에서는 'use server'가 어떻게 동작하는지, 그리고 서버 컴포넌트를 어떻게 잘 활용할 수 있는지 구체적으로 설명하고 있으니 직접 확인해보시면 많은 도움이 될 거예요.  

또한, 서버 컴포넌트는 데이터 요청(fetch) 같은 작업을 서버에서 직접 처리할 수 있어서 클라이언트의 부담을 줄여주는데, React의 'use server' 기능과 잘 결합하면 더욱 효과적이죠.  

궁금하다면 아래 React 공식 문서 링크에서 자세한 내용 읽어보세요!  
https://reactjs.org/docs/getting-started.html (React 공식 문서 메인 페이지입니다. 'Server Components'나 'use server'를 검색해보시면 도움됩니다.)  

필요하다면 관련 예제나 사용법도 같이 소개해드릴게요!