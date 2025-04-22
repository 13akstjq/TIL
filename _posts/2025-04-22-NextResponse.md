---
title: "Nextjs 15에서 응답 처리 쉽게 하는 방법(NextResponse)"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:42
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "NextResponse"
link: "https://nextjs.org/docs/app/api-reference/functions/next-response"
isUpdated: false
---


# NextResponse

NextResponse는 Web Response API를 확장한 것으로, 좀 더 편리하게 사용할 수 있는 메서드들이 추가된 Response 객체입니다. 기존의 Response 객체보다 훨씬 직관적이고 유용한 기능들을 제공해서, 특히 서버 사이드에서 많이 활용됩니다.

## cookies

NextResponse의 `cookies` 속성을 사용하면, 응답(Response)의 `Set-Cookie` 헤더를 읽거나 변경할 수 있어요. 즉, 클라이언트에게 보낼 쿠키를 쉽게 관리할 수 있다는 뜻이죠.

예를 들어, 서버에서 쿠키를 추가하거나 수정할 때 직접 헤더를 조작하는 것보다 훨씬 깔끔하게 쓸 수 있습니다.

```js
import { NextResponse } from 'next/server';

export function middleware(req) {
  const res = NextResponse.next();

  // 쿠키 읽기
  const token = req.cookies.get('token');

  // 쿠키 설정하기
  res.cookies.set('loggedIn', 'true', { path: '/', httpOnly: true });

  return res;
}
```

이처럼 NextResponse의 `cookies` API를 쓰면 쿠키를 딱 한 줄로 읽고 쓸 수 있어서, 복잡한 헤더 조작 없이도 쉽게 작업할 수 있답니다.

추가로, `cookies.set()` 메서드는 옵션으로 `httpOnly`, `secure`, `maxAge` 같은 여러 속성들을 받아서 보안이나 유효기간 설정도 간편해요.  
또한, Next.js 13부터는 미들웨어나 API 라우트 등 서버 환경에서 이런 NextResponse API를 적극 활용해서 더 깔끔한 코드를 작성할 수 있으니, 꼭 한 번 써보시길 추천합니다!

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

### set(name, value)

이 메서드는 이름(name)과 값(value)을 받아서 응답(response)에 쿠키를 설정해주는 역할을 해요.

예를 들어, 사용자가 어떤 배너를 다시 보지 않도록 설정하고 싶을 때 이렇게 활용할 수 있죠.

```js
// /home 경로로 들어오는 요청이 있을 때
let response = NextResponse.next()
// 'show-banner'라는 이름의 쿠키를 'false' 값으로 설정해서 배너 숨기기
response.cookies.set('show-banner', 'false')
// 이 응답에는 `Set-Cookie: show-banner=false; path=/home` 헤더가 포함되어 반환됩니다.
return response
```

여기서 중요한 점은 `response.cookies.set()`을 사용하면 쿠키가 응답 헤더에 `Set-Cookie` 형태로 추가된다는 거예요. 그리고 경로(path) 옵션은 기본적으로 요청된 경로와 동일하게 설정되기 때문에, 원하는 경로로 지정하려면 옵션을 명시해 주어야 해요.

```js
response.cookies.set('user-preference', 'dark-mode', { path: '/' })
```

이렇게 하면 쿠키가 모든 경로에서 유효하게 설정됩니다.

---

### get(name)

쿠키 값을 읽을 때는 `get(name)` 메서드를 사용해요. 예를 들어 사용자가 이전에 설정한 배너 표시 여부를 확인하고 싶을 때 사용할 수 있겠죠.

```js
const showBanner = request.cookies.get('show-banner')
```

이렇게 하면 `showBanner` 변수에 쿠키의 값이 담겨 오고, 없으면 `undefined`가 반환됩니다.

---

### 참고로!

- 쿠키는 클라이언트와 서버 사이에서 상태를 유지하는 간단한 방법이에요. 특히 사용자의 선호나 로그인 상태 같은 정보를 저장할 때 자주 사용되죠.
- Next.js 13부터는 `NextResponse` 객체를 통해 쿠키를 쉽게 다룰 수 있어서 편리해요.
- 보안 상 민감한 정보(예: 비밀번호, 토큰 등)는 절대 쿠키에 그냥 저장하면 안 되고 `HttpOnly`, `Secure` 옵션을 꼭 설정해야 해요.
- 또한 쿠키 용량 제한과 만료일 설정도 신경 써야 하고요.

필요하면 쿠키 설정에 `expires`, `maxAge`, `secure`, `httpOnly` 등의 옵션도 함께 활용해 보세요!

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

쿠키를 다룰 때 자주 쓰는 메서드인 `get()`과 `getAll()`에 대해 쉽게 설명해볼게요!

---

### `get()` 메서드

- **기능:** 쿠키 이름을 주면, 그 쿠키의 **값(value)** 을 가져와 줘요.
- **예외 상황:** 
  - 쿠키가 없다면 `undefined`를 반환해요.
  - 만약 한 이름으로 여러 개의 쿠키가 있으면, 맨 처음 쿠키의 값을 반환합니다.

예를 들어, `/home` 경로에서 들어오는 요청이 있다고 하고, 

```js
let response = NextResponse.next()
// 쿠키 중 { name: 'show-banner', value: 'false', Path: '/home' } 가 있다고 가정하면,
response.cookies.get('show-banner') // 'false' 반환
```

첫 번째 일치하는 쿠키 값을 리턴하니 정말 간편하죠?

---

### `getAll()` 메서드

- **기능:** 
  - 쿠키 이름을 주면, 이름에 해당하는 모든 쿠키들을 배열로 받아요.
  - 이름을 안 주면, **현재 응답(response)에 포함된 모든 쿠키**를 배열로 반환합니다.

이걸 활용하면 같은 이름으로 중복된 쿠키를 한꺼번에 다룰 수도 있고,
한번에 모든 쿠키를 살펴보기도 좋아요.

---

### 추가 꿀팁!

- 쿠키는 같은 이름이라도 경로나 도메인 범위가 다를 수 있다는 점 기억하세요! 그래서 중복된 이름의 쿠키가 있을 수 있거든요.
- `get()`는 첫 번째 쿠키만 가져오니까, 혹시 여러 개 있을 때 모두 확인하려면 `getAll()`을 쓰세요.
- 클라이언트에서 쿠키를 조작할 때는 보안 설정(예: HttpOnly, Secure)도 꼭 챙기는 게 좋아요.

꾸준히 쿠키를 잘 다루면 로그인, 알림 설정 등 중요한 기능들을 매끄럽게 만들 수 있답니다!

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

next.js에서 response.cookies 다룰 때, 쿠키를 삭제하는 방법에 대해 간단하게 알려줄게요.

보통 쿠키를 삭제할 땐 set-cookie 헤더를 통해 만료된 상태로 만들어 보내잖아요? next.js 13부터는 NextResponse 객체가 쿠키 API를 제공해서, `response.cookies.delete('쿠키명')` 이렇게 한 줄로 쉽게 삭제가 가능해요.

```js
// /home 경로로 들어온 요청 처리 중
let response = NextResponse.next()
// 'experiments'라는 이름의 쿠키가 있다면 삭제합니다.
const deleted = response.cookies.delete('experiments') 
console.log(deleted) // 쿠키를 삭제했으면 true, 없으면 false 반환
```

여기서 `delete` 메서드는 실제 쿠키가 있으면 삭제 처리 후 `true`를 반환하고, 없으면 `false`를 반환해서 잘 삭제되었는지 확인할 수 있어요. 그리고 삭제할 때는 해당 쿠키의 path(경로)도 맞춰줘야 삭제가 제대로 되는데, 기본 path가 맞으면 굳이 지정 안 해도 돼요.

---

추가로 알아두면 좋은 점!

- `response.cookies.getAll()`로 response에 담긴 모든 쿠키를 배열로 쉽게 조회할 수 있어요.
- 쿠키 이름이 같으면, 여러 개의 쿠키가 배열로 반환됩니다. (`[{name, value, Path}, ...]`)
- 쿠키를 삭제할 때 특정 path나 domain도 설정해야 할 경우가 있으니, 필요하면 옵션도 같이 넘겨줄 수 있어요.

---

조금 더 쉽게 이해되도록 요약표를 만들었으니 참고하세요!

| 메서드                  | 설명                           | 반환값                |
|----------------------|------------------------------|-------------------|
| `response.cookies.getAll('쿠키명')` | 특정 이름의 모든 쿠키 조회           | 쿠키 배열               |
| `response.cookies.getAll()`        | 모든 쿠키 조회                    | 쿠키 배열               |
| `response.cookies.delete('쿠키명')` | 쿠키 삭제                       | 삭제 성공 시: `true`, 실패 시: `false` |

---

사실 개발할 때 쿠키 관리가 깔끔해야 나중에 디버깅하기도 편한데, next.js의 `response.cookies` API가 이런 면에서 훨씬 편리하니 적극 활용해 보세요!

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

## json() 함수 사용하기

Next.js에서 API 응답을 JSON 형태로 쉽게 만들어주는 `NextResponse.json()` 함수에 대해 알아볼게요.

```js
import { NextResponse } from 'next/server'
 
export async function GET(request: Request) {
  return NextResponse.json({ error: 'Internal Server Error' }, { status: 500 })
}
```

위처럼 사용하면, 클라이언트에 `{ error: 'Internal Server Error' }`라는 JSON 데이터를 보내면서, HTTP 상태 코드를 500으로 설정할 수 있어요. 간단하죠?

참고로, `NextResponse.json()`은 내부적으로 `Content-Type` 헤더를 `application/json`으로 설정하니까, 따로 헤더 신경 쓸 필요가 없답니다.

---

## redirect() 함수 소개

`redirect()` 함수는 원하는 URL로 클라이언트를 리다이렉트할 때 사용해요. 서버에서 특정 조건이 충족되면 다른 페이지로 보내고 싶을 때 참 유용하죠.

예를 들어, 다음처럼 쓸 수 있습니다:

```js
import { NextResponse } from 'next/server'

export async function GET(request: Request) {
  // 로그인하지 않은 사용자라면, 로그인 페이지로 리다이렉트
  const isLoggedIn = false // 예시용 변수
  if (!isLoggedIn) {
    return NextResponse.redirect(new URL('/login', request.url))
  }
  
  return NextResponse.json({ message: 'Welcome back!' })
}
```

위 코드에서는 사용자가 로그인하지 않았다면 `/login` 경로로 리다이렉트시키고, 로그인되어 있으면 JSON 메시지를 보내줘요.

---

### 추가 팁: HTTP 상태 코드 지정하기

`redirect()`는 기본적으로 307 Temporary Redirect 상태 코드를 사용하는데, 필요에 따라 다른 상태 코드를 지정할 수도 있습니다.

```js
return NextResponse.redirect(new URL('/login', request.url), 302)
```

여기서 `302`는 임시 리다이렉트를 의미해요. 상황에 맞게 301 (영구 리다이렉트) 같은 코드를 넣어도 됩니다.

---

### 정리하자면…

| 기능          | 예시 코드                                      | 설명                                               |
|---------------|------------------------------------------------|----------------------------------------------------|
| JSON 응답     | `NextResponse.json({ key: 'value' }, { status: 200 })` | JSON 데이터와 상태 코드 함께 보내기                   |
| 리다이렉트    | `NextResponse.redirect(new URL('/path', request.url), 302)` | 클라이언트를 다른 URL로 리다이렉트하기                  |

이 두 함수만 잘 활용해도 Next.js API 라우트에서 훨씬 깔끔하고 명확한 응답 관리가 가능하니 꼭 익혀두세요!

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

Next.js에서 리다이렉션을 구현할 때는 `NextResponse.redirect()`를 사용하면 정말 편리해요. 간단히 말해, 원하는 URL로 사용자를 보내는 기능인데요, URL을 동적으로 만들거나 수정할 수도 있답니다.

예를 들어, 기본적인 리다이렉트는 이렇게 할 수 있어요:

```js
import { NextResponse } from 'next/server'

export function middleware(request) {
  return NextResponse.redirect(new URL('/new', request.url))
}
```

위 코드는 현재 요청된 URL을 기준으로 `/new` 경로로 사용자를 넘겨주는 동작이에요.

여기서 더 한 걸음 나아가면, 기존 URL 정보도 활용해서 쿼리 파라미터를 붙이는 것도 가능하답니다! 

```js
import { NextResponse } from 'next/server'

export function middleware(request) {
  // 현재 요청 URL을 기준으로 /login 페이지를 만든다
  const loginUrl = new URL('/login', request.url)
  // query parameter로 from=/현재경로 정보를 추가
  loginUrl.searchParams.set('from', request.nextUrl.pathname)
  // 그리고 리다이렉트!
  return NextResponse.redirect(loginUrl)
}
```

이렇게 하면 `/login?from=/현재경로` 형태의 URL로 보내서, 로그인 후 다시 원래 페이지로 돌아가게 하는 UX도 구현할 수 있죠.

---

참고로, `request.nextUrl`은 Next.js가 제공하는 편리한 속성으로, 요청 URL의 구성요소를 쉽게 읽고 수정할 수 있게 도와줘요. 그냥 `request.url`은 문자열인데, `nextUrl`은 URL 객체라서 더 편합니다.

또한, 미들웨어 안에서 리다이렉션을 사용할 때는 항상 올바른 상대경로나 절대경로를 사용해야 하며, 프로토콜이나 도메인이 바뀌는 리다이렉트는 미들웨어 환경에 따라 제한될 수 있으니 주의해주세요.

이런 방식으로 Next.js 미들웨어에서 동적인 리다이렉션을 만들면, 로그인 이동, 권한 체크, 리다이렉션 로직 등을 유연하게 처리할 수 있어요. 직접 써보면 생각보다 간단하니 한번 도전해 보세요!

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

## rewrite() 함수 살펴보기

Next.js에서 `rewrite()` 함수는 원래의 URL은 그대로 유지하면서, 서버 내에서 요청을 다른 경로로 내부 재요청(proxing)할 때 유용해요. 쉽게 말해, 브라우저 주소창에 주소가 그대로 보이는데, 실제로는 다른 URL에서 처리를 하고 싶을 때 사용하죠.

예를 들어, 아래처럼 코드를 작성했다고 가정해볼게요.

```js
import { NextResponse } from 'next/server'

// 사용자가 /about 경로로 요청하지만 내부적으로는 /proxy로 재작성
return NextResponse.rewrite(new URL('/proxy', request.url))
```

- **브라우저 주소창**: `/about` 그대로 유지
- **실제 서버 처리 경로**: `/proxy`

이렇게 하면 URL 구조를 깔끔하게 유지하면서 내부적으로 다른 핸들러나 API 라우트에 요청을 넘길 수 있어요.

---

## next()는 무엇인가요?

`next()`는 미들웨어 체인에서 현재 미들웨어를 종료하고 다음 미들웨어로 제어를 넘겨주는 함수입니다. 만약 경로를 바꿔주지 않고 단순히 현재 요청을 그대로 처리하고 싶다면 `NextResponse.next()`를 호출해요.

예를 들어,

```js
import { NextResponse } from 'next/server'

export function middleware(request) {
  // 특정 조건에서만 재작성하고, 그렇지 않으면 그냥 다음 미들웨어로
  if (request.nextUrl.pathname.startsWith('/proxy')) {
    return NextResponse.rewrite(new URL('/api/proxy', request.url))
  }
  return NextResponse.next()
}
```

위 코드는 `/proxy`로 시작하는 요청은 `/api/proxy`로 내부 요청을 재작성하지만, 그 외의 경로는 그냥 원래 경로대로 처리하라는 의미입니다.

---

### 정리하자면
| 함수            | 역할                                     | 브라우저 주소창 반영 여부       |
| --------------- | ---------------------------------------- | ----------------------- |
| `NextResponse.rewrite()` | 요청 경로를 서버 쪽에서 내부적으로 변경함       | 아니요 (원래 경로 유지)      |
| `NextResponse.next()`    | 다음 미들웨어 또는 원래 처리 로직으로 제어 넘김  | 해당 없음 (경로 변경 없음)   |

---

이 기능 덕분에 깔끔한 URL 설계도 가능하고, 복잡한 리버스 프록시(proxy) 역할도 손쉽게 만들 수 있어요. 특히 API 라우트와 페이지 라우트를 효율적으로 분리할 때 여간 편리한 게 아니랍니다!

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

next() 메서드는 미들웨어에서 특히 유용해요. 이 메서드를 이용하면 지금 처리하던 작업을 조기에 종료하고, 다음 라우팅 단계로 자연스럽게 넘어갈 수 있거든요.

```js
import { NextResponse } from 'next/server'

return NextResponse.next()
```

그리고 응답을 만들 때 헤더를 그대로 전달하거나, 새로운 헤더를 추가해서 넘길 수도 있어요. 예를 들어, 클라이언트 요청의 헤더를 받아서 수정하거나 필요한 정보를 추가하고 싶을 때 이렇게 하면 됩니다:

```js
import { NextResponse } from 'next/server'

// 들어온 요청을 기준으로 기존 헤더를 복사
const newHeaders = new Headers(request.headers)

// 새 헤더 추가
newHeaders.set('x-version', '123')

// 수정된 헤더를 포함한 다음 단계 응답 반환
return NextResponse.next({
  request: {
    headers: newHeaders,
  },
})
```

헤더를 이렇게 다룰 수 있으면, 인증 토큰이나 사용자 맞춤 정보 등 다양한 데이터를 미들웨어에서 편리하게 관리할 수 있어서 미들웨어 로직 작성이 훨씬 유연해져요!

혹시 next()만 쓰다가 특정 상황에서는 다른 Response 객체를 직접 반환해야 한다면, `NextResponse.rewrite()`나 `NextResponse.redirect()` 같은 메서드들도 함께 알아두면 좋습니다. 이렇게 다양한 Response 객체를 활용하면 미들웨어에서 요청 흐름을 자유롭게 제어할 수 있답니다!