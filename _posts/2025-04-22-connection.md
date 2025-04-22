---
title: "Next.js 15에서 Connection 관리하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:59
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "connection"
link: "https://nextjs.org/docs/app/api-reference/functions/connection"
isUpdated: false
---

# connection 함수 살펴보기

오늘은 Next.js에서 제공하는 `connection()` 함수에 대해 이야기해볼게요. 이 함수는 이름만 들으면 네트워크 연결처럼 느껴지지만, 실제로는 렌더링 시점을 제어하는 역할을 한답니다.

## connection() 함수가 뭐예요?

쉽게 말해, `connection()` 함수는 렌더링을 잠시 멈췄다가 **유저의 요청이 들어올 때까지 기다렸다가** 렌더링을 계속하도록 해줘요.

왜 이런 기능이 필요하냐고요? 보통 컴포넌트들은 프리렌더링(빌드 타임에 미리 화면을 만들어두는 것)을 하거든요. 그런데 우리가 원하는 건 프리렌더링이 아니라, **실시간으로 변화하는 값**을 화면에 보여주는 거라면 다르게 처리해야 해요.

예를 들면 `Math.random()`이나 `new Date()`처럼 매번 다른 결과를 내는 값들이 그러하죠. 이런 값들은 빌드할 때 계산해버리면, 그 결과가 계속 고정되어 버리잖아요? 그래서 유저가 접속할 때마다 새로 계산해서 보여줘야 하는 경우가 있어요.

이럴 때 `connection()` 함수를 쓰면, 빌드 타임에 렌더링하는 대신 유저 요청 시점에 렌더링하도록 하는 거죠.

## 간단한 예제 코드

```jsx
import { connection } from "next/server";

export default async function Page() {
  await connection();
  // 이 아래에서 작성한 코드는 빌드 시점이 아니라,
  // 실제 요청이 들어올 때 실행돼요!
  const rand = Math.random();
  return <span>{rand}</span>;
}
```

보시면 `await connection()`을 호출하는 순간부터 아래 코드는 빌드 타임에 무시되고, 유저가 페이지를 요청했을 때 실행된답니다.

## 이런 점도 알아두면 좋아요!

- `connection()`은 서버에서만 동작해요. 그래서 클라이언트 쪽 코드에서는 사용할 수 없고, 주로 서버 컴포넌트 안에서 써요.
- 프리렌더링을 완전히 안 하니까, 렌더링 속도는 약간 느릴 수 있어요. 그러니 꼭 필요할 때만 사용하는 게 좋아요.
- `connection()` 없이도 `force-dynamic` 같은 옵션으로도 비슷한 효과를 낼 수 있는데, 사용 의도에 따라 적절하게 선택하면 돼요.

## 마무리

정리하자면, `connection()`은 **빌드 타임이 아닌 요청 시점에 렌더링을 하도록 만들어주는 함수**에요. 그래서 동적인 데이터를 실시간에 가깝게 보여야 할 때 정말 유용하답니다.

Next.js를 쓸 때 이 함수가 왜 필요한지, 어떻게 쓰는지 이해하면 좀 더 유연한 서버 컴포넌트를 만들 수 있어요. 나중에 실시간 데이터 처리 혹은 매번 변하는 값을 렌더링해야 할 때 한 번 써보면 좋겠습니다!

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

## 참고 자료

### 함수 타입

```js
function connection(): Promise<void>
```

### 파라미터

이 함수는 딱히 파라미터를 받지 않아요. 그냥 연결을 시도하고, 연결이 성공하면 `Promise`가 해결되고, 실패하면 에러가 발생하는 구조입니다.

---

함수 이름인 `connection`으로 봤을 때, 아마도 데이터베이스 연결이나 네트워크 연결 같은 걸 담당하는 함수가 아닐까 싶은데요. 실제로 이런 함수는 비동기 작업이 많기 때문에 `Promise<void>`를 반환하는 게 일반적입니다.

`Promise<void>`라는 건, 작업이 성공적으로 끝나면 별다른 값을 반환하지 않는다는 뜻이에요. 그래서 이 함수를 사용할 때는 `await connection()` 처럼 쓰거나, `.then()`을 사용해서 연결이 끝난 후에 실행할 코드를 작성하면 됩니다.

```js
async function initApp() {
  try {
    await connection();
    console.log("연결 완료!");
  } catch (error) {
    console.error("연결 실패:", error);
  }
}
```

연결이 성공했는지 여부에 따라서 애플리케이션 로직이 달라질 수 있기 때문에, 꼭 `try-catch` 문이나 `.catch()`를 활용해 에러 처리를 해주시는 게 좋아요!

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

- 이 함수는 어떠한 매개변수도 받지 않습니다.

### 반환 값

- 함수는 void를 반환하는 Promise를 리턴합니다. 즉, 호출자가 결과 값을 활용하도록 설계된 함수가 아니에요.

## 알아두면 좋은 점

이런 타입의 함수는 보통 내부에서 비동기 작업을 처리하지만, 결과를 직접적으로 반환하거나 전달하지 않고 그냥 처리만 하고 끝나는 경우에 많이 쓰여요. 예를 들어 로그를 저장하거나, 캐시를 비우는 작업처럼 "결과가 있으면 좋지만 없어도 되는" 경우에요.

만약 함수의 처리가 실패할 수 있다면, Promise를 리턴하니 async/await나 then/catch로 에러를 잡아주는 걸 잊지 마세요! 그래야 앱이 예기치 않게 멈추는 걸 막을 수 있습니다.

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

- connection이 unstable_noStore를 대체해서, 앞으로 Next.js의 방향에 더 잘 맞게 업데이트되었어요.
- 이 함수는 동적 렌더링이 꼭 필요할 때만 사용하면 되며, 일반적인 Dynamic API들을 사용할 경우에는 필요하지 않아요.

### 버전 히스토리

| 버전         | 변경사항                 |
| ------------ | ------------------------ |
| `v15.0.0`    | `connection`가 안정화됨. |
| `v15.0.0-RC` | `connection` 도입됨.     |

참고로, Next.js가 점점 더 SSR(서버 사이드 렌더링)과 SSG(정적 사이트 생성)을 효율적으로 처리할 수 있도록 많은 개선을 하고 있어서, 이런 함수들이 안정화되는 게 개발자 입장에서 꽤 반가운 소식이에요. 특히 동적 콘텐츠를 다룰 때 성능과 안정성을 높이는 데 도움이 된답니다!
