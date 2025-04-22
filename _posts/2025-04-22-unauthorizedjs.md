---
title: "Next.js 15에서 Unauthorized.js로 권한 없는 접근 처리하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:47
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "unauthorized.js"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/unauthorized"
isUpdated: false
---


# unauthorized.js

`unauthorized.js` 파일은 인증 과정에서 `unauthorized` 함수가 호출될 때 보여줄 UI를 렌더링하는 역할을 해요. 이 파일을 통해 UI를 원하는 대로 꾸밀 수 있을 뿐만 아니라, Next.js가 자동으로 401 상태 코드를 반환해 준답니다.

```js
import Login from '@/app/components/Login'

export default function Unauthorized() {
  return (
    <main>
      <h1>401 - Unauthorized</h1>
      <p>Please log in to access this page.</p>
      <Login />
    </main>
  )
}
```

위 코드를 보면, 기본적으로 "401 - Unauthorized"라는 제목과 함께 로그인을 요청하는 문구가 보이고, `Login` 컴포넌트를 렌더링해 실제 로그인 화면도 함께 표시하고 있어요.

### 조금 더 알아보기

- **401 상태 코드란?**  
  401은 '인증이 필요함'을 나타내는 HTTP 상태 코드로, 사용자가 인증되지 않은 상태에서 접근 권한이 필요한 페이지에 접근할 때 서버가 보내요.

- **Next.js에서의 활용 팁**  
  이 파일을 커스터마이징하면 에러 페이지를 여러분의 서비스 스타일에 맞게 꾸밀 수 있답니다. 예를 들어, 로그인 버튼 대신 소셜 로그인이나 회원가입으로 유도하는 링크도 넣어볼 수 있겠죠?

- **추가로 하면 좋은 것**  
  사용자가 왜 인증이 필요한지 좀 더 자세한 안내나, 인증 절차에 도움이 될 만한 FAQ 링크를 같이 넣으면 UX가 더 좋아질 수 있어요.

필요하면 한 번 직접 만들어보고, 여러분만의 인증 오류 페이지도 멋지게 만들어 보세요!

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

### Props

`unauthorized.js` 컴포넌트는 어떤 props도 받지 않습니다.

## 예시

### 로그인하지 않은 사용자에게 로그인 UI 보여주기

로그인하지 않은 사용자를 대상으로 로그인 화면을 보여주고 싶을 때, `unauthorized.js` 컴포넌트를 사용할 수 있어요. 참고로 이 컴포넌트는 추가 설정을 위해 props를 받지 않으니, 간단하게 기본 UI를 렌더링하는 용도로 쓴다는 점 기억하세요.

만약 내 서비스에서 로그인 여부에 따라 다른 화면을 보여줘야 한다면, 조건문으로 이 컴포넌트를 넣어주면 됩니다. 예를 들어:

```jsx
{ !userIsLoggedIn && <Unauthorized /> }
```

이렇게 하면 인증되지 않은 사용자에게만 로그인 페이지가 표시되고, 이미 로그인한 사용자에겐 다른 화면이 보여지겠죠?

추가로, `unauthorized.js` 컴포넌트 내부를 수정해 나만의 로그인 UI로 커스터마이징해도 좋습니다. 그럴 땐 props 대신 내부 상태나 리덕스 등 상태관리 라이브러리를 이용해서 유연하게 UX를 조절해보세요!

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

여러분 안녕하세요! 오늘은 Next.js 앱에서 인증이 필요한 페이지에 대해 'unauthorized' 함수를 이용해 인증되지 않은 사용자에게 로그인 UI를 보여주는 방법을 쉽게 설명해드릴게요.

먼저, `DashboardPage` 컴포넌트를 살펴봅시다. 여기서는 `verifySession()` 함수를 통해 사용자의 세션 정보를 확인하는데요, 이 함수는 비동기 함수라 `await`를 붙여 호출해주고 있어요.

```jsx
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

- `verifySession()` 함수가 null 또는 undefined를 반환한다면 세션이 없는 것으로 간주하고, `unauthorized()` 함수를 호출해버립니다.
- 이 `unauthorized()` 함수는 내부적으로 Next.js에 내장된 기능으로, 렌더링을 멈추고 `unauthorized.js` 파일을 호출해서 401 페이지를 보여주게 해줍니다.

그럼 그 `unauthorized.js` 파일에 어떤 컴포넌트가 있냐면,

```jsx
import Login from '@/app/components/Login'
 
export default function UnauthorizedPage() {
  return (
    <main>
      <h1>401 - Unauthorized</h1>
      <p>Please log in to access this page.</p>
      <Login />
    </main>
  )
}
```

이렇게 간단하게 로그인 UI를 포함한 페이지를 만들고 있어요. 화면에 “401 - Unauthorized” 메시지와 로그인 폼이 나타나게 되는 거죠.

---

### 한 번에 쉽게 인증 처리하기

사실 여기서 중요한 점은, 이런 패턴을 쓰면 페이지 보호 로직이 깔끔해진다는 거예요. 개발자 입장에서 보면, 사용자가 로그인을 하지 않은 상태면 바로 401 페이지로 분기시키고, 그 화면에서 자연스럽게 로그인하도록 유도할 수 있죠.

- 기존의 리다이렉트 방식이나, 클라이언트 사이드에서 상태를 확인하는 것보다 서버에서 바로 인증 상태를 확인하는 게 보안상 더 안전합니다.
- 물론 세션 검증 함수(`verifySession`)는 직접 구현해야 해요. 예를 들어 쿠키에 포함된 토큰을 해석하거나, 데이터베이스를 조회해서 유효성을 검사하게 됩니다.

---

### 이해를 돕는 간단 다이어그램

| 동작 단계             | 설명                                    |
|-------------------|---------------------------------------|
| 1. 세션 확인          | `verifySession` 호출해서 로그인 여부 확인          |
| 2. 미인증 사용자 감지     | 세션이 없으면 `unauthorized()` 호출해서 401 페이지 렌더링   |
| 3. 401 페이지 렌더링      | `unauthorized.js`에 정의된 로그인 화면을 보여줌            |
| 4. 로그인 후 세션 생성     | 로그인 컴포넌트에서 정상 로그인이 되면 세션이 생성됨         |
| 5. 접근 허용           | 이후 대시보드 페이지 접속 가능                           |

---

### 마치며

Next.js 앱에서 인증이 필요한 페이지를 다룰 때, ‘unauthorized’ 함수를 잘 활용하면 간단하고 체계적인 접근 제어를 구현할 수 있습니다. 보안도 높이고 UX도 좋으니 여러분께서도 꼭 한 번 써보시길 추천드려요.

추가로, 로그인 컴포넌트나 세션 검증 로직에 대해 궁금하신 점 있으면 댓글로 알려주세요. 다음에는 그 부분도 자세히 다뤄보겠습니다!

오늘도 즐거운 개발 되시길! 🚀

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
| `v15.1.0` | `unauthorized.js` 도입됨 |

이번에 `v15.1.0` 버전에서 `unauthorized.js`라는 파일이 새롭게 추가됐어요. 보통 이런 파일 이름은 '허가되지 않은 접근'과 관련된 기능을 처리할 때 쓰이는데요, 예를 들어 로그인이 필요한 페이지에 비로그인 사용자가 접근하려 할 때 보여주는 에러 처리나 리다이렉트 등을 담당할 수도 있답니다.

이렇게 특정 상황에 맞는 자바스크립트 파일을 분리해서 관리하면 코드 유지보수도 편해지고, 필요한 경우에만 로드해서 성능 최적화도 할 수 있어요. 나중에 관련 기능을 더 자세히 살펴볼 기회가 있으면, `unauthorized.js`가 실제로 어떤 역할을 하는지 한번 분석해보는 것도 재밌겠네요!