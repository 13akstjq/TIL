---
title: "Next.js 15에서 not-found.js로 404 페이지 쉽게 만드는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:43
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "not-found.js"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/not-found"
isUpdated: false
---


# not-found.js 파일 사용법 - Next.js에서 404 페이지 만들기

안녕하세요! 오늘은 Next.js에서 사용자 정의 404 페이지를 만드는 방법을 소개할게요. `not-found.js` 파일을 사용하면, 특정 라우트(segment)에서 `notFound` 함수가 호출되었을 때 보여질 UI를 쉽게 꾸밀 수 있답니다.

## 기본 개념

- `not-found.js`는 라우트 내에서 `notFound()` 함수가 호출되는 경우, 그에 대한 UI를 렌더링해줍니다.
- 스트리밍 응답(response)인 경우에는 HTTP 상태 코드가 200으로 반환되지만, 스트리밍이 아닌 일반 응답은 404 상태 코드를 반환합니다.
- 즉, 사용자에게 맞춤형 404 페이지를 쉽게 제공하면서 백엔드의 응답 코드는 상황에 맞게 처리해줍니다.

## 예제 코드

아래는 아주 기본적인 `not-found.js` 파일 예제입니다.

```jsx
import Link from 'next/link'
 
export default function NotFound() {
  return (
    <div>
      <h2>Not Found</h2>
      <p>Could not find requested resource</p>
      <Link href="/">Return Home</Link>
    </div>
  )
}
```

- `<h2>` 태그로 큰 제목을, `<p>`로 간단한 안내 문구를 넣었고요.
- `next/link`의 `Link` 컴포넌트를 사용해서 홈으로 돌아갈 수 있는 링크도 만들었어요.

요렇게 하면, 사용자 경험을 높이는 맞춤형 404 페이지를 아주 쉽게 만들 수 있죠!

## 추가 팁!

- `not-found.js`는 라우트 폴더 바로 아래에 위치해야 작동해요. 예: `app/products/not-found.js`
- 더 멋진 UI를 위해 스타일링 라이브러리(Tailwind CSS 등)를 활용해도 좋습니다.
- 404 페이지에서 로그를 남기고 싶다면 컴포넌트 내에서 간단한 클라이언트 또는 서버 사이드 코드 작성도 가능해요.
- Next.js 13 버전 이상의 앱 라우터(App Router) 환경에서만 동작하니 참고하세요!

---

다음에 더 재미있고 유용한 Next.js 팁으로 찾아올게요! 필요하면 댓글로 질문 남겨주세요~ 🙌

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

not-found.js 컴포넌트는 아무런 props도 받지 않습니다.

> 알아두면 좋은 점: 예상되는 notFound() 오류를 잡아내는 것 외에도, 루트에 위치한 app/not-found.js 파일은 여러분 앱 전체에서 처리되지 않는 URL들도 다룹니다. 즉, 사용자가 여러분 앱 내에서 처리하지 않는 URL에 접속할 경우, app/not-found.js 파일에서 내보내는 UI가 보여지게 된다는 뜻이에요.

## 예시

(여기에는 실제 사용 예시나 코드가 들어가면 더 이해하기 좋겠죠? 만약 준비된 예시가 있다면 추가해보세요!) 

---

이 부분을 보면 Next.js 13의 새로운 파일 기반 라우팅에서 404 페이지를 처리하는 방식을 알 수 있어요. 예전에는 별도의 error handling 코드를 많이 짰는데, 이제는 이처럼 not-found.js 파일 하나로 깔끔하게 처리할 수 있습니다. 그리고 루트 디렉토리의 app/not-found.js는 전체 앱에서 처리하지 못하는 URL에 대해서도 자동으로 404 페이지 역할을 하니까, 개발하면서 404 페이지를 따로 신경 쓰지 않아도 된다는 점이 정말 편리해요.

더불어, 만약 특정 경로에서만 커스텀 404 페이지가 필요하다면 하위 폴더에 별도로 not-found.js를 만들어 적용할 수도 있다는 점, 알고 계시면 좋습니다!

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

### 데이터 가져오기 (Data Fetching)

기본적으로 `not-found` 컴포넌트는 서버 컴포넌트(Server Component)이에요. 하지만 이 컴포넌트를 `async`로 만들어 데이터를 받아오고 화면에 보여줄 수도 있답니다.

아래 예시를 보면, `headers()`를 통해 요청 헤더에서 도메인 정보를 가져오고, 그 도메인에 맞는 데이터를 비동기로 받아와서 화면에 출력하는 모습이에요.

```js
import Link from 'next/link'
import { headers } from 'next/headers'

export default async function NotFound() {
  const headersList = await headers()
  const domain = headersList.get('host')
  const data = await getSiteData(domain)
  return (
    <div>
      <h2>Not Found: {data.name}</h2>
      <p>Could not find requested resource</p>
      <p>
        View <Link href="/blog">all posts</Link>
      </p>
    </div>
  )
}
```

### 여기서 좀 더 알아두면 좋은 점!

- `headers()` 함수는 서버에서 실행될 때만 사용할 수 있어요. 클라이언트에서 실행하면 동작하지 않으니 주의하세요.
- 위 예시는 서버 컴포넌트에서 데이터를 가져오는 기본적인 패턴인데, 만약 `usePathname` 같은 클라이언트 훅을 써서 URL 경로에 따라 화면을 다르게 그리고 싶다면, 데이터를 클라이언트에서 가져와야 해요. 그러니까 클라이언트 컴포넌트로 만들어서 `useEffect` 같은 훅을 이용하는 거죠.
- 서버 컴포넌트와 클라이언트 컴포넌트를 혼용할 때는 데이터를 어디서, 어떻게 가져올지 상황에 맞게 설계하는 게 중요합니다.

즉, **비동기 데이터 페칭이 필요하다면 서버 컴포넌트에서 async 함수로 간단하게 처리할 수 있지만, 클라이언트 훅을 사용하고 싶다면 클라이언트 컴포넌트에서 처리하는 걸 기억하세요!**

이해하기 어려운 부분이나 직접 써보고 싶은 팁 있으면 언제든 물어보세요 :)

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

## 버전 히스토리

| 버전        | 변경 사항                                   |
|-------------|--------------------------------------------|
| `v13.3.0`   | 루트 `app/not-found`가 전역에서 매칭되지 않는 URL들을 처리하도록 변경됨. |
| `v13.0.0`   | `not-found` 기능이 도입됨.                   |

---

여기서 `not-found`는 사용자 경험을 위해 웹사이트에서 404 페이지 같은 '찾을 수 없음' 페이지를 쉽게 구현할 수 있게 도와주는 기능이에요. 특히 `v13.3.0`에서는 앱의 루트 경로에서 전역적으로 매칭되지 않는 URL을 처리하도록 확장돼서, 여러분이 앱 내에서 예상치 못한 경로로 접근할 때도 깔끔하게 안내 페이지를 띄울 수 있답니다.

웹 개발하면서 이런 '404페이지' 처리 중요하죠? 사용자가 잘못된 URL로 들어왔을 때 그냥 에러 메시지 띄우는 것보다 친절한 안내 페이지를 보여주는 게 훨씬 UX 측면에서 좋아요. Next.js 최신 버전들을 사용한다면 이런 기능들을 잘 활용해보세요!