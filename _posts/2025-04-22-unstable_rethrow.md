---
title: "Next.js 15에서 unstable_rethrow 사용 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:49
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "unstable_rethrow"
link: "https://nextjs.org/docs/app/api-reference/functions/unstable_rethrow"
isUpdated: false
---


# unstable_rethrow

Next.js에서 앱 코드에서 발생한 에러를 처리하려고 할 때, 내부적으로 발생하는 Next.js 자체 에러를 잡지 않고 넘기고 싶다면 `unstable_rethrow`를 활용할 수 있어요.

예를 들어, `notFound` 함수를 호출하면 Next.js 내부 에러가 던져지고, 이때 자동으로 `not-found.js` 컴포넌트가 렌더링됩니다. 그런데 이걸 `try/catch` 블록 안에서 감싸버리면, 내부 에러까지 잡히면서 `not-found.js`가 렌더링되는 걸 막아버려서 원하는 동작이 안 될 수 있어요.

아래 코드를 보면,

```js
import { notFound } from 'next/navigation'

export default async function Page() {
  try {
    const post = await fetch('https://.../posts/1').then((res) => {
      if (res.status === 404) notFound()  // 여기가 내부 에러 발생 지점
      if (!res.ok) throw new Error(res.statusText)
      return res.json()
    })
  } catch (err) {
    console.error(err)  // 여기서 내부 Next.js 에러까지 잡혀버림
  }
}
```

`notFound()` 호출 시 던지는 내부 Next.js 에러가 `catch`에 잡혀버리기 때문에, 사실은 `notFound`가 의도한 “404 페이지 렌더링”이 되지 않는 거죠.

---

### `unstable_rethrow`는 어떻게 쓰나?

`unstable_rethrow`를 활용하면 이런 내부 에러를 다시 던져서 Next.js가 원래 의도한 대로 처리할 수 있도록 도와줍니다.

예시는 다음과 같아요:

```js
import { notFound, unstable_rethrow } from 'next/navigation'

export default async function Page() {
  try {
    const post = await fetch('https://.../posts/1').then((res) => {
      if (res.status === 404) notFound()
      if (!res.ok) throw new Error(res.statusText)
      return res.json()
    })
  } catch (err) {
    unstable_rethrow(err)  // 내부 Next.js 에러면 다시 던져서 처리하게 함
    console.error(err)
  }
}
```

- 여기서 `unstable_rethrow(err)`는 함수 내부에서 이 에러가 Next.js에서 던져진 내부 에러이면 그냥 놔두고, 아니면 `catch` 블록에서 자체적으로 처리할 수 있게 해줘요.

---

### 왜 이렇게 쓰나?

- 보통 외부 API 요청 실패나 특정 조건에서 `notFound()`, `redirect()` 등 내부 Next.js 함수들을 사용해 상태를 바꾸고자 하는데,
- `try/catch`로 감싸면 Next.js 내부 에러도 같이 묶여서, Next.js 쪽에서 기대하는 렌더링이나 라우팅이 안 일어나고 말죠.

그래서 `unstable_rethrow`가 중간에 역할을 하면서 “Next.js 내부 에러만 원래대로 던져주고, 그 외에는 내가 직접 처리할게” 라고 구분할 수 있게 해줍니다.

---

### 참고 팁

- `unstable_` 접두사가 붙은 함수들은 아직 실험적 API라는 뜻이에요. 따라서 앞으로 Next.js 버전업 시 사용법이나 역할이 바뀔 수도 있습니다.
- 하지만 에러 핸들링에서 내부 Next.js 에러를 잘 구분해서 다루고 싶다면 굉장히 유용한 함수입니다.
- 실제 운영 환경에서는 주의해서 사용하되, 공식 문서와 버전별 변경사항을 꾸준히 확인하는 게 좋습니다.

---

기본 개념만 잘 이해해두면, Next.js 앱에서 다양한 에러 케이스를 우아하게 처리할 수 있으니 참고하세요!

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

Next.js에서 `unstable_rethrow` API를 사용하면 내부적으로 발생한 에러를 다시 던져서(throw) Next.js가 알아서 처리하게 하면서, 코드의 예상 동작을 이어갈 수 있어요.

예를 들어, 어떤 게시글을 API에서 가져올 때 404 응답이 오면 `notFound()`를 호출해서 Next.js가 404 페이지를 띄우도록 하고, 그 외에 의도치 않은 에러가 발생하면 직접 에러를 던져줘야 하죠. 여기서 `unstable_rethrow`를 써보면:

```js
import { notFound, unstable_rethrow } from 'next/navigation'

export default async function Page() {
  try {
    const post = await fetch('https://.../posts/1').then((res) => {
      if (res.status === 404) notFound()       // 404일 땐 notFound 함수 호출
      if (!res.ok) throw new Error(res.statusText) // 그 외 에러면 강제로 에러 발생
      return res.json()
    })
  } catch (err) {
    unstable_rethrow(err)  // 에러 다시 던져서 Next.js가 처리하도록 위임
    console.error(err)     // 콘솔에도 에러 로깅
  }
}
```

여기서 중요한 점은 `notFound()`, `redirect()`, `permanentRedirect()` 같은 Next.js 내장 API들은 내부적으로 에러를 던지면서 라우팅을 조작합니다. 따라서 이런 API들을 사용할 때는 에러를 다시 던져주지 않으면 Next.js가 정상적으로 동작하지 않을 수 있어요.

| Next.js API          | 설명                                                         |
|----------------------|--------------------------------------------------------------|
| `notFound()`         | 404 페이지를 렌더링하도록 이동                                |
| `redirect()`         | 클라이언트 또는 서버에서 즉시 리다이렉트 수행                 |
| `permanentRedirect()`| 301 영구 리다이렉트를 수행                                     |

이 함수들이 에러를 던지는 이유는 Next.js가 라우팅 흐름을 제어하기 위해서인데요. 개발자가 이걸 잡아서 처리하려고 하면 오히려 흐름이 꼬일 수 있으니 `unstable_rethrow`를 이용해 원래에서 다시 던져주고 Next.js에게 맡기는 게 가장 깔끔합니다.

---

추가로, 이 `unstable_rethrow` API는 아직 실험적(unstable)이라서 Next.js 버전업에 따라 변경될 수 있으니 프로덕션 환경에서 쓸 때는 주의가 필요해요. 대신 이렇게 내부 오류를 재발생시켜서 Next.js가 상황에 맞는 UI(404 페이지나 리다이렉트 등)를 보여주게 하는 방식은 코드를 훨씬 깔끔하고 명료하게 만들어 줍니다.

아참, 에러를 잡고 바로 `unstable_rethrow`를 하면 아래 코드처럼 `console.error`가 무의미해질 수 있는데, 만약 로그를 남기고 싶다면 Next.js가 에러 처리 전에 로그를 찍을 수 있도록 주의하세요!

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

라우트 세그먼트가 '정적(static)'이 아니면 에러를 던지도록 설정되어 있을 때, 동적 API 호출 역시 에러를 발생시킵니다. 이 에러는 개발자가 따로 잡아서 처리하면 안 된다는 점, 꼭 기억하세요. 참고로 Partial Prerendering(PPR)도 이런 동작에 영향을 줍니다. 

에러를 던지는 API는 다음과 같습니다:

| API 종류                          |
|----------------------------------|
| cookies                          |
| headers                          |
| searchParams                    |
| fetch(..., { cache: 'no-store' })        |
| fetch(..., { next: { revalidate: 0 } })  |

---

### 알아두면 좋은 점
- 에러를 다시 던질(unable_rethrow) 때는 catch 블록 맨 위에서 호출하며, error 객체를 유일한 인자로 넘겨줘야 합니다.
- .catch 핸들러 내에서도 사용할 수 있어요.
- 이런 API 호출을 try/catch로 감싸지 않는다면 unstable_rethrow를 쓸 필요가 없습니다.
- 만약 타이머나 인터벌 정리 같은 리소스 정리가 필요하다면, unstable_rethrow 호출 전에 하거나 finally 블록에서 처리해야 합니다.

---

실제로 이런 기능을 잘 활용하면, Next.js 같은 프레임워크에서 미리 렌더링되는(Prerendered) 페이지와 동적요소가 섞여 있을 때 발생하는 복잡한 에러 처리 로직을 간소화할 수 있어요. 특히 일부 API가 정적 컨텍스트에서만 동작하도록 제한하기 때문에, 개발자가 예상치 못한 동작에 빠지지 않도록 명확히 해두는 셈이죠.

추가 팁을 드리자면, 이런 특성을 이해하고 에러 처리 패턴을 세우면 팀 내 코드 리뷰 때도 “왜 여기서 try/catch를 안 썼냐” 같은 혼란을 줄일 수 있습니다! 그리고 무엇보다, 결국 네트워크 요청(fetch) 같은 부분에서 불필요한 에러를 재처리하지 않고 효율적으로 관리하는 데 큰 도움이 될 거예요.