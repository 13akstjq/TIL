---
title: "Next.js 15에서 permanentRedirect를 사용하는 방법 "
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:44
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "permanentRedirect"
link: "https://nextjs.org/docs/app/api-reference/functions/permanentRedirect"
isUpdated: false
---


# permanentRedirect 함수란?

permanentRedirect 함수는 사용자를 다른 URL로 영구적으로 이동시키고 싶을 때 사용하는 함수입니다. 이 함수는 Server Components, Client Components, Route Handlers, 그리고 Server Actions 등 다양한 환경에서 활용할 수 있어 아주 유용해요.

## 어떻게 동작할까?

- **스트리밍(streaming) 환경에서 사용하면?**  
  클라이언트 쪽에서 리다이렉트를 실행할 수 있도록 `<meta>` 태그를 삽입해줍니다. 즉, 브라우저가 이 태그를 보고 자동으로 원하는 URL로 이동하게 돼요.

- **서버 액션(Server Action)에서 사용하면?**  
  호출자에게 303 HTTP 리다이렉트 응답을 보내요. 303 코드는 "See Other"라는 의미로, 주로 POST 요청 후 GET 요청으로 리다이렉션할 때 씁니다.

- **그 외 경우에는?**  
  호출자에게 308 (Permanent Redirect) 응답을 보냅니다. 308은 영구적인 이동을 의미하므로 SEO나 캐싱에 좋은 영향을 줄 수 있답니다.

## 리다이렉트와 다르게 자원(리소스)이 없을 때는?

만약 요청한 페이지나 자원이 아예 없다면 permanentRedirect 대신 **notFound** 함수를 사용하세요. 이 함수는 404 페이지를 띄워주기 때문에 사용자 경험 측면에서 더 적절합니다.

---

이렇게 permanentRedirect 함수를 적절히 활용하면 사용자 흐름을 자연스럽게 제어할 수 있어요. 예를 들어, 회원 인증이 필요한 페이지에 인증되지 않은 사용자가 접근할 때 로그인 페이지로 영구 리다이렉트를 걸어줄 수도 있겠죠?

마지막으로, 리다이렉트를 할 때는 SEO(검색 엔진 최적화) 영향도 생각하면서 301(또는 Next.js에서는 308)으로 영구 리다이렉트할지, 302나 303으로 일시적 리다이렉트할지 결정하는 게 중요합니다.

혹시 더 궁금한 점 있으면 언제든 질문해 주세요!

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

> 참고할 만한 정보: 만약 308 (영구) 리다이렉트 대신 307 (임시) 리다이렉트를 사용하고 싶다면, `redirect` 함수를 사용하면 됩니다.

## 매개변수(Parameters)

`permanentRedirect` 함수는 두 개의 인자를 받습니다:

```js
permanentRedirect(path, type)
```

- `path`: 리다이렉트할 URL 경로를 지정해요.
- `type`: 리다이렉트의 종류를 나타내며, 보통 308이 기본값입니다.

---
여기서 잠깐!  
307과 308의 차이를 간단히 설명하자면, 307은 임시 리다이렉트로, 추후 경로가 원래대로 돌아올 가능성이 있을 때 쓰는 게 좋고요, 308은 영구 리다이렉트로 검색엔진 등에 "이 경로가 앞으로 쭉 이곳이다" 하고 명확하게 알려주는 용도로 좋아요.  
만약 서비스가 완전히 이전됐거나 URL 구조를 아예 바꾸었다면 308을, 잠깐 다른 페이지로 임시 이동시키는 경우에는 307을 선택하는 걸 추천합니다!

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

다음은 `permanentRedirect` 함수에서 사용하는 파라미터에 관한 설명입니다:

| Parameter | Type | Description |
| --- | --- | --- |
| `path` | `string` | 리다이렉트 할 URL입니다. 상대 경로나 절대 경로 모두 가능합니다. |
| `type` | `'replace'` (기본값) 또는 `'push'` (Server Actions에서는 기본값) | 리다이렉트 방식을 지정합니다. |

기본적으로, Server Actions에서는 `push` 방식을 사용해 브라우저 히스토리 스택에 새 항목을 추가하고, 그 외의 경우에는 `replace` 방식으로 현재 URL을 히스토리 스택에서 바꿉니다. 만약 이 동작 방식을 바꾸고 싶다면 `type` 파라미터를 통해 지정할 수 있어요.

단, Server Components에서는 `type` 파라미터가 아무런 영향을 미치지 않습니다.

---

이 부분에서 조금 더 설명을 덧붙이자면, 리다이렉트 방식을 `push`로 하면 사용자가 뒤로가기 버튼을 눌렀을 때 리다이렉트 이전의 페이지로 돌아갈 수 있어요. 반면에 `replace`는 현재 페이지를 히스토리에서 교체하기 때문에 뒤로가기가 이전 페이지가 아니라 그 전의 페이지로 연결되죠. 상황에 따라 적절한 방식을 선택하는 게 중요해요.

## Returns
(이 부분은 원문에 내용이 없는데, 보통 함수가 반환하는 값 등에 대해 설명할 때 쓰입니다. 이 함수가 반환하는 값이 있다면 여기에 그 내용을 추가해 주세요!)

---

만약 이 함수의 반환 값이나 사용 예제가 궁금하시다면, 언제든지 질문해 주세요!

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

permanentRedirect는 값을 반환하지 않습니다.

## 예시

permanentRedirect() 함수를 호출하면 NEXT_REDIRECT 에러를 발생시키고, 이 에러는 현재 라우트 세그먼트의 렌더링을 중단시킵니다.

```js
import { permanentRedirect } from 'next/navigation'

async function fetchTeam(id) {
  const res = await fetch('https://...')
  if (!res.ok) return undefined
  return res.json()
}

export default async function Profile({ params }) {
  const { id } = await params
  const team = await fetchTeam(id)
  if (!team) {
    permanentRedirect('/login')
  }

  // ...
}
```

---

### 좀 더 쉽게 설명하자면

`permanentRedirect` 함수는 단순히 "여기로 영구적으로 이동해!" 라는 신호를 보내는 역할을 해요. 그래서 실제로는 값을 돌려주지 않고, 호출하는 순간 렌더링이 멈추고 Next.js가 알아서 지정한 경로로 이동시키죠.

이 점을 헷갈리지 말아야 해요. 예를 들어 `return permanentRedirect('/login')` 처럼 값으로 사용하려고 하면 안되고, 그냥 `permanentRedirect('/login')`만 호출하면 됩니다.

### 유용한 팁

- `permanentRedirect`는 주로 서버 컴포넌트에서 사용하며, 클라이언트 사이드에서는 `useRouter` 훅의 `router.replace()` 같은 함수를 사용해요.
- 영구 리다이렉트라는 게, 브라우저에게 "이 URL은 앞으로도 이 주소로만 접속하세요" 라고 알려주는 301 상태 코드로 동작합니다. SEO에도 긍정적인 영향을 줘요.
- 만약 임시 리다이렉트가 필요하다면 `redirect()` 함수를 사용하고, 이건 302로 동작합니다.

Next.js 13부터 도입된 새로운 라우팅과 네비게이션 API, 조금 익숙해지면 서버와 클라이언트 모두에서 깔끔하고 안전한 네비게이션 처리가 가능하니 꼭 한번 사용해보세요!

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

알면 좋은 팁: permanentRedirect를 사용할 때 꼭 return permanentRedirect()처럼 반환할 필요가 없어요. 이 함수가 TypeScript의 never 타입을 사용하기 때문인데요.

여기서 never 타입은 함수가 절대 정상적으로 종료되지 않는다는 걸 의미해요. 그래서 permanentRedirect()를 호출하면 함수 실행이 멈추고 리다이렉트가 바로 발생하니까, 굳이 return을 붙여서 값을 반환할 필요가 없는 거예요.

예를 들어 이렇게 써도 전혀 문제없답니다:

```typescript
import { permanentRedirect } from 'next/navigation';

function handler() {
  // 어떤 조건에 따라 리다이렉트가 필요하면
  permanentRedirect('/new-path');
  // 사실상 여기에 도달하지 않아요.
}
```

개발할 때 이런 점을 알아두면 코드가 좀 더 깔끔해지고, 불필요한 return 문을 빼서 가독성도 좋아질 수 있답니다!