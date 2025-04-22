---
title: "Next.js 15에서 Middleware.js로 서버 데이터 처리하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:41
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "middleware.js"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/middleware"
isUpdated: false
---


# middleware.js란?

middleware.js(또는 middleware.ts)는 서버에서 요청이 완료되기 전에 중간에 끼어들어 특정 코드를 실행할 수 있도록 도와주는 파일이에요. 즉, 클라이언트가 서버에 요청을 보내면, 해당 요청을 처리하기 전에 이 미들웨어가 먼저 동작하게 되죠.

이걸 쓰면 어떻게 좋냐고요? 예를 들어, 들어오는 요청에 따라 응답을 바꾸거나 리다이렉트 시키거나, 요청이나 응답 헤더를 수정하는 일이 가능해요. 심지어는 바로 응답을 보내버릴 수도 있답니다!

미들웨어는 라우트가 렌더링되기 전에 작동하기 때문에, 인증(auth) 처리, 로깅(logging), 그리고 복잡한 리다이렉트 같은 서버 사이드 로직을 구현할 때 아주 유용하게 쓰여요.

보통 프로젝트 루트에 `middleware.ts` 또는 `middleware.js` 파일을 만들어서 정의합니다. 이 위치는 `app`이나 `pages` 폴더와 같은 수준이거나, `src` 폴더 안일 수도 있어요.

---

### 참고로!

- 미들웨어에서는 **응답을 직접 반환**하면 그 뒤에 라우트 렌더링은 안 되고, 그냥 그 응답이 바로 전송돼요.
- 요청을 **리라이트(rewrite)** 하면 내부 경로 변경이, **리다이렉트(redirect)** 하면 클라이언트에게 다른 주소로 이동하라고 지시하는 의미예요.
- 요청이나 응답 헤더를 자유롭게 조작할 수 있으니, 예를 들어 사용자 에이전트(user-agent)를 검사하거나 쿠키 작업도 가능합니다.

이러면 서버가 좀 더 똑똑해지고, 클라이언트에 맞춤형 응답을 줄 수 있어요!

---

필요하시다면 다음 글에서는 실제 코드 예제와 함께 미들웨어의 구체적인 사용법도 다뤄볼게요~!

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

Next.js에서 middleware 작성할 때 기본적으로 export하는 함수는 단 하나여야 한다는 점, 알고 계셨나요? 오늘은 그 부분을 중심으로 간단하게 설명해 보려고 해요.

```js
import { NextResponse, NextRequest } from 'next/server'
 
// 이 함수는 요청을 가로채서 /about 경로 하위에 있는 모든 경로 요청을 /home 으로 리다이렉트 해줘요.
export function middleware(request: NextRequest) {
  return NextResponse.redirect(new URL('/home', request.url))
}
 
export const config = {
  matcher: '/about/:path*',
}
```

위 코드에서 핵심은 `middleware` 함수 하나만 export 하고 있다는 거예요. Next.js는 한 파일에서 여러 미들웨어 함수를 내보내는 것을 지원하지 않아요. 따라서 두 개 이상의 미들웨어가 필요한 경우엔 각각 따로 파일을 만들어야 하죠.

그리고, `config` 객체의 `matcher` 속성은 어떤 경로에 이 미들웨어를 적용할지 정해주는 역할을 해요. 여기서는 `/about/` 경로 이하 모든 경로를 지정했죠. 참고로 `:path*` 부분은 와일드카드 같은 역할을 해서 하위 경로 전체를 포괄할 수 있어요.

### 정리하면

| 항목                 | 설명                                                  |
|--------------------|-----------------------------------------------------|
| 미들웨어 함수          | 꼭 하나만 export 해야 한다                               |
| `matcher` 설정        | 어떤 URL 패턴에 대해 미들웨어를 실행할지 지정               |
| 리다이렉션 처리         | `NextResponse.redirect` 로 요청을 원하는 경로로 보낼 수 있다  |

이런 룰들은 Next.js 내부에서 미들웨어 처리 방식을 단순화하고, 성능 최적화를 위한 부분이에요. 프로젝트 설계 시 이 점 꼭 기억해 두시면 쓸데없는 에러를 예방할 수 있답니다.

또한, 만약 복잡한 미들웨어 로직이 필요하다면 여러 파일로 분리하거나, 환경변수에 따라 미들웨어를 다르게 실행하도록 코드를 짜는 것도 좋은 전략이에요. 

다음에는 미들웨어에서 자주 쓰이는 다른 기능들도 한번 같이 살펴봐요!

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
// 기본 export 예제
export default function middleware(request) {
  // 미들웨어 로직 작성
}
```

### config 객체 (선택 사항)

미들웨어 함수와 함께 config 객체를 export할 수도 있어요. 이 객체 안에는 `matcher`가 들어가는데, 이 matcher를 통해 미들웨어가 적용될 경로를 지정할 수 있습니다.

#### matcher

matcher는 미들웨어가 실행될 URL 경로나 패턴을 지정하는 역할을 해요. 예를 들어, 특정 경로나 API 엔드포인트에만 미들웨어가 동작하도록 설정할 수 있죠.

```js
export const config = {
  matcher: ['/about/:path*', '/dashboard/:path*'],
};
```

위 예시는 `/about`과 `/dashboard`로 시작하는 모든 하위 경로에 미들웨어가 적용된다는 뜻이에요.

참고로, matcher에 지정할 수 있는 패턴은 Next.js의 라우팅 규칙과 비슷해서 익숙하시다면 편하게 쓸 수 있을 거예요.

만약 더 세밀한 조건이 필요하다면, 직접 미들웨어 안에서 request 객체를 검사하는 방법도 있어요. 예를 들어, 쿠키나 헤더에 따라 동작을 조절할 수도 있죠.

미들웨어 설정 시 알아두면 좋은 팁!
- `matcher`를 구체적으로 지정하면 불필요한 미들웨어 실행을 줄여서 퍼포먼스를 개선할 수 있어요.
- 여러 경로를 배열로 전달할 수 있으니 한꺼번에 관리하기 편합니다.
- 잘못된 matcher 패턴은 미들웨어가 의도치 않게 작동하지 않을 수 있으니, 꼭 테스트해보세요!

다음에는 미들웨어 내부에서 실제 request 요청을 다루는 법과 응답 처리 방법에 대해 이야기해볼게요. 그럼 같이 공부해봐요!

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

`matcher` 옵션은 Middleware가 특정 경로에서만 실행되도록 설정할 때 사용해요. 쉽게 말해, 이 옵션을 통해 ‘어떤 경로에 Middleware를 적용할지’를 정할 수 있다는 뜻이죠.

그 방법은 크게 세 가지예요:

- **단일 경로 지정:**  
  그냥 문자열로 경로를 적으면 돼요. 예를 들어, `/about`이면 `/about` 경로에서만 Middleware가 실행됩니다.

- **여러 경로 지정:**  
  배열을 사용해서 여러 경로를 한 번에 지정할 수 있어요.  
  예시) `matcher: ['/about', '/contact']` 이렇게 하면 `/about`과 `/contact` 두 경로 모두 적용됩니다.

- **정규 표현식(Regex) 사용:**  
  더 정교한 경로 필터링이 필요할 땐 정규식도 사용할 수 있어요.  
  예를 들면,  
  js
  matcher: ['/((?!api|_next/static|_next/image|.*\\.png$).*)']
  
  이렇게 하면 `api`, `_next/static`, `_next/image` 폴더나 `.png` 파일 경로는 제외하고 나머지 경로에만 Middleware를 적용해요.  
  정규 표현식의 부정형 전방탐색(negative lookahead)을 활용해 특정 경로나 파일 형식을 꼼꼼히 걸러낼 수 있답니다.

그리고 `matcher` 옵션에는 배열 안에 객체 형식으로 경로를 더 세분화해서 지정할 수도 있는데, 이 객체에는 다음과 같은 키들이 있어요:

| 키 이름      | 설명                                               |
|--------------|----------------------------------------------------|
| `src`        | 적용할 경로를 정의 (문자열 또는 정규식)             |
| `methods`    | 적용할 HTTP 메소드 배열 (예: `['GET', 'POST']`)     |
| `has`        | 요청에 특정 헤더, 쿠키, 쿼리 파라미터가 있는지 조건 지정 |
| `missing`    | 요청에 특정 헤더, 쿠키, 쿼리 파라미터가 없는지 조건 지정 |

이런 식으로 복잡한 조건을 걸 수 있어서, 예를 들어 `POST` 요청이고 특정 쿠키가 있을 때만 Middleware를 적용하는 것도 가능하답니다.

---

### 추가 팁  
Router 미들웨어를 효과적으로 사용하려면, 불필요하게 모든 경로에 Middleware가 실행되지 않도록 `matcher`를 잘 설정하는 게 좋아요. 안 그러면 성능에 영향이 있을 수 있거든요! 또한, Next.js에서 자주 쓰는 `_next/static` 같은 내부 리소스 경로들은 보통 Middleware 대상에서 제외하는 게 일반적이에요.

이 정도만 잘 활용해도 경로별로 딱 맞는 Middleware 적용이 훨씬 수월해질 거예요!

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

요즘 웹 개발할 때, 특정 요청 경로에 맞춰서 로직을 처리하거나 미들웨어를 적용하는 경우가 많죠? 이번에 소개할 내용은 이런 요청 경로 매칭을 세밀하게 조정할 수 있는 설정 방법이에요.

---

### 주요 옵션 설명

| 옵션명     | 설명                                                                                           |
|------------|------------------------------------------------------------------------------------------------|
| source     | 요청 경로를 매칭하는 경로 또는 패턴이에요. 문자열로 직접 지정하거나 복잡한 패턴을 지정할 수 있어요.             |
| regexp     | 정규표현식으로 source를 세밀하게 조절할 수 있어요. 포함하거나 제외할 경로를 더욱 정확하게 조절할 때 쓸 수 있죠. |
| locale     | 불리언 값인데, false로 하면 로케일(언어 설정 등)을 매칭에서 무시해요. 기본값은 true로 다루는 경우가 많습니다.  |
| has        | 요청 헤더, 쿼리 파라미터, 쿠키 등의 특정 요소가 있을 때 조건을 걸고 싶을 때 사용해요.                            |
| missing    | 반대로, 헤더나 쿠키 같은 특정 요소가 없을 때만 동작하도록 조건을 걸고 싶을 때 쓰면 좋아요.                         |

---

### 실제 예제

```js
export const config = {
  matcher: [
    {
      source: '/api/*',
      regexp: '^/api/(.*)',
      locale: false,
      has: [
        { type: 'header', key: 'Authorization', value: 'Bearer Token' },
        { type: 'query', key: 'userId', value: '123' },
      ],
      missing: [{ type: 'cookie', key: 'session', value: 'active' }],
    },
  ],
}
```

- `/api/*` 경로에 대해 매칭합니다.
- `regexp`로 좀 더 정교하게 `/api/` 뒤에 어떤 값이 오든 매칭하도록 했어요.
- 로케일 기반 매칭은 무시(`locale: false`)하고요.
- 요청 헤더에 `Authorization`이 `'Bearer Token'` 이고, 쿼리 파라미터에 `userId=123` 이 있어야 합니다.
- 그리고 `session` 쿠키가 없을 때만 이 설정이 적용되는 거죠.

---

### 참고로!

- `has`와 `missing` 조건은 다중으로 걸 수 있으니, 복합 조건 처리도 편리하게 구성할 수 있습니다.
- `regexp` 쓰면 경로를 한 층 더 정교하게 제어할 수 있어서, 간단한 경로나 특수 문자 포함 경로도 문제없이 처리 가능해요.
- `locale` 설정은 멀티랭귀지 사이트 만들 때 유용하고, 단일 언어 사이트는 보통 false로 두고 쓰기도 합니다.

이렇게 매칭 설정을 잘 사용하면, Next.js나 비슷한 프레임워크에서 요청 경로 조건에 맞는 미들웨어나 특정 API 처리 로직을 깔끔하게 관리할 수 있답니다.

---

다음 글에서는 이 설정을 실제 미들웨어에 어떻게 적용할 수 있는지, 그리고 성능 최적화 팁도 함께 다뤄볼게요! 궁금한 점 있으면 댓글로 남겨주세요~

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

Middleware를 정의할 때 기본 내보내기 함수는 하나의 매개변수인 request를 받습니다. 이 request는 NextRequest의 인스턴스로, 들어온 HTTP 요청을 나타내죠.

```js
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  // 여기에 Middleware 로직을 작성하세요
}
```

> 알아두면 좋은 점:
NextRequest는 Next.js Middleware에서 들어오는 HTTP 요청을 나타내는 타입이에요. 반면에 NextResponse는 HTTP 응답을 조작하고 돌려보낼 때 사용하는 클래스로 구분되어 있습니다.

## NextResponse

NextResponse는 Middleware 안에서 주로 클라이언트에게 반환할 응답을 조작할 때 사용합니다. 예를 들어, 요청을 리다이렉트하거나 쿠키를 설정하는 등의 작업을 할 수 있죠.

간단한 예시를 보면:

```js
import { NextResponse } from 'next/server'

export function middleware(request) {
  // 특정 경로로 리다이렉트하기
  if (request.nextUrl.pathname === '/old-path') {
    return NextResponse.redirect(new URL('/new-path', request.url))
  }
  return NextResponse.next()
}
```

이렇게 요청된 경로가 `/old-path`라면 `/new-path`로 리다이렉트 시키고, 그렇지 않으면 다음 미들웨어나 라우터로 요청을 넘기게 됩니다.

또한 NextResponse를 이용해 쿠키를 설정할 수도 있어요. 예를 들어:

```js
import { NextResponse } from 'next/server'

export function middleware(request) {
  const response = NextResponse.next()
  response.cookies.set('my-cookie', 'my-value', { path: '/', maxAge: 60 * 60 * 24 }) // 1일간 유지되는 쿠키
  return response
}
```

위 예시에서는 응답에 'my-cookie'라는 이름의 쿠키를 추가하는 거죠.

Middleware에서 NextRequest와 NextResponse의 역할을 잘 구분해두면 효과적인 요청 처리 및 응답 제어가 가능하니 꼭 기억해두세요!

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

미들웨어(Middleware)는 NextResponse 객체를 사용할 수 있는데, 이 객체는 Web Response API를 확장한 거예요. NextResponse 객체를 반환하면 쿠키를 직접 조작하거나, 헤더를 설정하고, 리다이렉트를 구현하거나, 경로를 다시 쓸 수 있어서 정말 유용하답니다.

> 참고로 리다이렉트를 구현할 땐 NextResponse.redirect 대신 Response.redirect를 써도 된다는 점! 상황에 맞게 골라 쓰시면 돼요.

## 실행 환경(Runtime)

미들웨어는 Edge 런타임에서만 동작해요. 그래서 Node.js 런타임에서는 사용할 수 없다는 점, 꼭 기억해주세요.

---

### 미들웨어에서 NextResponse를 쓰면 좋은 점

- 쿠키 설정/삭제가 편리해요.
- 헤더를 자유자재로 조작할 수 있어서 보안이나 캐싱 정책 등을 설정할 때 유리하죠.
- 경로 재작성(Rewrite) 기능으로 SEO 최적화나 사용자 맞춤 라우팅이 쉬워져요.

Edge 런타임이라는 건, 쉽게 말해 요청이 사용자와 가까운 서버(Edge)에서 빠르게 처리된다는 뜻인데요. 그래서 미들웨어가 빠른 응답 속도에 도움을 줘서 좋은 사용자 경험을 만들 수 있답니다.

필요할 때 미들웨어와 NextResponse를 적절히 활용해보세요! 개발할 때 꽤 큰 힘이 되어줄 거예요.

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

## 버전 히스토리 (Version History)

| 버전       | 변경 사항                                                                                         |
|------------|--------------------------------------------------------------------------------------------------|
| `v13.1.0`  | 고급 미들웨어 플래그(Advanced Middleware flags) 추가                                           |
| `v13.0.0`  | 미들웨어가 요청 헤더, 응답 헤더를 수정할 수 있고, 응답도 직접 보낼 수 있게 개선                  |
| `v12.2.0`  | 미들웨어가 안정화됨. 자세한 업그레이드 가이드는 [여기](https://nextjs.org/docs/messages/middleware-upgrade-guide) 참고 |
| `v12.0.9`  | Edge Runtime에서 절대 URL 강제 적용 ([관련 PR](https://github.com/vercel/next.js/pull/33410))                  |
| `v12.0.0`  | 미들웨어(Beta) 기능 추가                                                                         |

---

*잠깐!*

여기서 말하는 '미들웨어'는 Next.js에서 API 요청과 응답 사이에 원하는 로직을 끼워 넣을 수 있는 기능이에요. 예를 들어 로그인 검증, A/B 테스트, 사용자 맞춤 리디렉션 등을 처리할 때 아주 유용하답니다.

그리고 `v13.0.0`부터는 요청과 응답 헤더를 수정하거나 직접 응답을 보내는 등 훨씬 더 강력한 제어가 가능해져서, 미들웨어를 활용하는 범위가 정말 넓어졌어요.

버전이 올라가면서 미들웨어가 안정화되고 기능도 점점 확장되고 있으니, 현재 개발 중인 프로젝트에 맞춰 적절히 선택해서 쓰시면 좋겠네요!