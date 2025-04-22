---
title: "Next.js 15에서 부분 프리렌더링(Partial Prerendering) 활용하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:03
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Partial Prerendering"
link: "https://nextjs.org/docs/app/building-your-application/rendering/partial-prerendering"
isUpdated: false
---

# 부분 프리렌더링 (Partial Prerendering)

> 참고: 부분 프리렌더링은 아직 실험적인 기능으로, Next.js의 캐나리 버전에서만 사용 가능하며 추후 변경될 수 있습니다. 아직 프로덕션용으로는 적합하지 않아요.

부분 프리렌더링은 한 경로(Route) 내에서 정적인 컴포넌트와 동적인 컴포넌트를 동시에 사용할 수 있게 해 주는 기능입니다.

어떻게 동작하냐면, 빌드 타임에 Next.js가 가능한 한 경로의 내용을 미리 렌더링해줍니다. 그런데 만약 동적 코드를 발견했다면(예를 들면, 들어오는 요청에서 데이터를 읽는다거나 할 때), 그 부분을 React Suspense 경계로 감싸줄 수 있어요. 이렇게 Suspense로 감싸진 컴포넌트는 빌드 시점에 실제 동적 데이터 대신 'fallback' UI가 미리 렌더링된 HTML로 포함됩니다.

즉, 정적인 부분은 그대로 미리 렌더링하고, 동적인 부분은 사용자가 페이지를 요청할 때 불러오는 방식을 섞어 쓸 수 있는 거죠.

---

### 조금 더 쉽게 이야기하면?

보통 Next.js의 정적 페이지는 모든 내용을 빌드할 때 미리 만들어 놓아요. 그런데 동적인 데이터가 있으면 빌드할 때 알 수 없으니 SSR(서버 사이드 렌더링) 방식으로 페이지를 채우죠. Partial Prerendering은 이 두 가지 방식을 한 페이지에서 섞어 쓸 수 있게 해 줍니다.

React Suspense를 잘 활용할 줄 안다면, 특정 컴포넌트에서 데이터를 읽는 부분만 동적으로 처리하고, 나머지는 미리 만들어진 HTML로 빠르게 보여줄 수 있다는 뜻입니다.

---

### 참고로 알아두면 좋은 점

- 현재는 실험적인 기능이니 코드에 적용할 때는 주의하세요.
- Suspense fallback UI가 어떻게 보일지 신경 써야 합니다. 너무 단순하면 사용자 경험이 떨어질 수 있어요.
- Next.js가 이 기능을 점점 개선할 예정이기 때문에 공식 문서나 릴리즈 노트를 꾸준히 확인하는 게 좋아요.

이런 기능 덕분에 더 빠르고 유연한 렌더링 경험을 제공하는 것이 앞으로의 목표입니다!

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

<img src="/TIL/assets/img/2025-04-22-PartialPrerendering_0.png" alt="Partial Prerendering illustration" />

> 🎥 영상: PPR이 뭔지, 어떻게 동작하는지 궁금하다면 → YouTube에서 10분짜리 동영상으로 확인해보세요!

---

## 배경 이야기

Partial Prerendering (부분 사전 렌더링, 이하 PPR)은 Next.js 서버가 미리 렌더링된 콘텐츠를 즉시 클라이언트에 전달할 수 있도록 도와주는 기술이에요.

사실, 웹 페이지를 렌더링할 때 서버가 완전히 렌더링을 마치지 않았는데도 이미 사용자에게 콘텐츠를 보여줄 수 있으면, 빠른 첫 화면 로딩을 경험할 수 있죠. PPR은 그걸 가능하게 합니다.

실제로 개발해보면, 전체 페이지를 미리 렌더링하는 것보다 특정 중요한 부분만 미리 렌더링하고, 나머지는 필요할 때 로드하게끔 하는 전략이 성능 최적화에 엄청 효과적이라는 걸 알 수 있어요.

필요한 부분만 빠르게 보여주고, 나머지는 나중에 채우는 이 방법! 여러분 프로젝트에도 적용해보시면 사용자 체감 속도가 확 올라간답니다. 다음 글에서는 PPR을 Next.js에 적용하는 구체적인 방법을 소개할게요!

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

클라이언트에서 서버로 이어지는 워터폴 문제를 막기 위해, 동적 컴포넌트들은 초기 프리렌더링과 함께 서버에서 병렬로 스트리밍되기 시작합니다. 이렇게 하면 클라이언트의 자바스크립트가 브라우저에 완전히 로드되기 전에 동적 컴포넌트가 렌더링을 시작할 수 있어 사용자 경험이 훨씬 부드러워집니다.

또한, 각각의 동적 컴포넌트마다 HTTP 요청을 여러 번 보내는 것을 막기 위해 부분 프리렌더링(Partial Prerendering, PPR)은 정적 프리렌더와 동적 컴포넌트를 하나의 HTTP 요청으로 합칠 수 있습니다. 이 덕분에 동적 컴포넌트마다 네트워크 왕복(roundtrip)이 발생하지 않고, 전체적인 로딩 속도가 크게 개선돼요.

## 부분 프리렌더링(Partial Prerendering) 사용법

### 점진적 도입 (버전 15 캐나리 버전 기준)

---

추가로, 이런 방식은 특히 대규모 애플리케이션에서 효과적이에요. 서버에서 먼저 준비된 콘텐츠를 빠르게 보여주면서도, 필요한 부분은 필요한 시점에만 동적으로 로딩하니 네트워크 자원도 효율적으로 사용할 수 있죠.

또한, 만약 여러분이 React나 Next.js 같은 프레임워크를 쓴다면, 이 Partial Prerendering 기능을 잘 활용하면 SEO 측면에서도 유리하고 초기 렌더링 퍼포먼스도 개선할 수 있어요. 초기 페이지 로딩 후 바로 상호작용할 수 있게 만드는 ‘경량화’ 전략이라고 생각하면 이해가 쉬울 거예요.

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

Next.js 15 최신 캐나리(canary) 버전에서 'Partial Prerendering(PPR)' 기능이 실험적으로 도입되었어요. 아쉽게도 아직 안정화 버전에는 포함되어 있지 않습니다. 혹시나 미리 사용해보고 싶다면 캐나리 버전을 설치해야 하는데요, 아래 명령어를 터미널에 입력하면 됩니다.

```bash
npm install next@canary
```

설치 후에는 프로젝트의 `next.config.js` 파일을 열어서 `ppr` 옵션을 `'incremental'`로 설정해 주세요. 그리고 다음과 같이 `experimental` 설정 안에 넣어주면 되는데요:

```js
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  experimental: {
    ppr: "incremental",
  },
};

export default nextConfig;
```

그리고 페이지나 레이아웃 파일 상단에서 `experimental_ppr` 라우트 구성 옵션을 내보내면 부분 프리렌더링을 점진적으로 적용할 수 있어요.

> 참고로, Partial Prerendering은 렌더링되지 않은 부분만 클라이언트에서 처리하면서도, 전체 페이지는 정적으로 일부 미리 렌더링하는 기술이에요. 그래서 초기 로딩은 빠르게 하면서도 일부 동적 변경이 가능한 유연한 페이지 구성이 가능하답니다.

다만, 아직 실험 단계기 때문에 사용하면서 생길 수 있는 문제에 대비해 꼭 테스트 환경에서 충분히 점검하는 걸 추천드려요. 안정 버전이 나오면 더 많은 업데이트와 공식 문서가 함께 제공될 테니 주목해 주세요!

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

리액트(Next.js)에서 실험적 기능인 `experimental_ppr`(Partial Prerendering)를 활용하는 코드와 관련하여 알아두면 좋은 내용들을 정리해볼게요. 조금 쉽게 설명해볼게요!

먼저, 제공된 코드를 보면 이렇게 생겼어요:

```js
import { Suspense } from "react";
import { StaticComponent, DynamicComponent, Fallback } from "@/app/ui";

export const experimental_ppr = true;

export default function Page() {
  return (
    <>
      <StaticComponent />
      <Suspense fallback={<Fallback />}>
        <DynamicComponent />
      </Suspense>
    </>
  );
}
```

---

## 핵심 포인트! `experimental_ppr`란?

- Next.js에서는 각 라우트별로 부분 프리렌더링을 할 수 있어요. (PPR)
- **PPR(Partial Prerendering)**를 활용하려면, 라우트 최상위에 `export const experimental_ppr = true`를 반드시 선언해야 해요.
- 만약 선언하지 않으면, 기본값은 `false`로 설정되어서 **프리렌더링이 되지 않아요**.
- 이 설정은 해당 라우트뿐만 아니라, 그 아래 자식 라우트나 레이아웃까지 적용돼요.
- 자식 라우트에서 PPR을 끄고 싶으면 자식 라우트에 `experimental_ppr = false`를 설정하면 됩니다.

즉, `experimental_ppr` 옵션은 라우트 트리에서 상위(루트) 영역에만 넣으면 자식까지 적용이 돼서 여러 파일에 일일이 선언할 필요 없어요.

---

## Suspense와 dynamic components (동적 컴포넌트) 이야기

PPR을 사용하는 라우트에서, Next.js 빌드 시에 반드시 동적 API 호출을 포함하는 컴포넌트는 `React.Suspense`로 감싸줘야 해요.

- `Suspense`가 없다면 자동 프리렌더링이 제대로 작동하지 않아요.
- `Suspense fallback`은 프리렌더된 페이지에서 **유저가 로딩 중일 때 보여질 UI**를 담당해줘요.
- 위 코드에서는 `<Fallback />` 컴포넌트를 로딩 중임을 보여주기 위해 넣었죠!

---

## 추가로 알아두면 좋은 것들

### 1. 왜 PPR을 쓰는 걸까?

전통적인 SSR은 모든 데이터를 서버에서 다 받아와서 렌더링해버려서, 느린 API나 복잡한 로직이 있으면 전체 페이지 로딩 시간이 느려질 수 있어요.

PPR은 정적 생성(Static Generation)과 SSR의 중간 느낌으로, "정적 프리렌더링 중 동적 컴포넌트만 나중에 클라이언트 쪽에서 로딩"하는 방식이에요.

덕분에:

- 페이지 첫 로딩은 빠르면서도,
- 자주 바뀌는 동적인 부분은 필요한 시점에만 로딩 가능해요.

### 2. `export const experimental_ppr = true`는 실험 기능!

현재는 아직 실험 단계라서 문서나 API가 조금씩 바뀔 수 있어요. 프로덕션 서비스라면 주의해서 사용해야 하죠.

---

## 요약해서 정리한 표!

| 내용                 | 설명                                                               |
| -------------------- | ------------------------------------------------------------------ |
| `experimental_ppr`   | 라우트(또는 세그먼트)에 프리렌더링을 활성화하는 플래그             |
| 기본값               | false                                                              |
| 적용범위             | 상위 세그먼트에만 선언하면 하위(자식 라우트, 레이아웃)에도 적용됨  |
| 동적 컴포넌트 감싸기 | 동적 로딩 컴포넌트는 React Suspense로 감싸야 하며 fallback UI 필요 |
| fallback UI          | 로딩 중 보여줄 UI. 보통 스피너나 텍스트 형태의 임시 UI를 넣음      |
| 사용 목적            | 빠른 페이지 로딩 & 프리랜더링과 동적 API 호출의 적절한 조화        |
| 주의점               | 아직 실험 기능이므로 버전 업에 따라 동작 변경 가능                 |

---

이렇게 `experimental_ppr`와 Suspense를 잘 활용하면 Next.js 앱에서 빠르고 유연한 프리렌더링을 경험할 수 있어요!

필요하면 더 자세한 예제나, 로딩 UI 디자인 팁도 공유할 수 있으니, 언제든 요청해 주세요. 😊

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

예를 들어 쿠키나 헤더 같은 함수들을 사용할 때는 이렇게 하게 돼요:

```js
import { cookies } from "next/headers";

export async function User() {
  const session = (await cookies()).get("session")?.value;
  return "...";
}
```

위 코드에서 `User` 컴포넌트는 들어오는 요청(request)을 보고 쿠키를 읽어야 해서, 이 컴포넌트를 PPR(Pre-Rendering with React)로 사용하려면 Suspense로 감싸줘야 합니다.

Vue에서는 Suspense가 없지만, React에선 비동기 처리를 하면서도 사용자 경험을 끊기지 않게 해주는 좋은 기능이에요. 예를 들면 이렇게요:

```js
import { Suspense } from "react";
import { User, AvatarSkeleton } from "./user";

export const experimental_ppr = true;

export default function Page() {
  return (
    <section>
      <h1>This will be prerendered</h1>
      <Suspense fallback={<AvatarSkeleton />}>
        <User />
      </Suspense>
    </section>
  );
}
```

여기서 핵심은 `Suspense`의 `fallback` 프로퍼티에 로딩 상태에서 보여줄 UI를 넣어준다는 점이에요. 예시에서는 `AvatarSkeleton`이 그 역할을 하죠. 즉, `User` 컴포넌트가 쿠키를 읽어 세션 정보를 가져올 때까지 사용자에게 깜빡이며 로딩 중임을 알려주고, 완료되면 실제 UI를 렌더링하는 거죠.

이렇게 하면 서버에서 미리 렌더링하는 부분과, 클라이언트에서 비동기적으로 받아와야 할 데이터가 함께 있어도 깔끔하게 처리할 수 있습니다. 특히 Next.js 최신 버전에서 SSR과 클라이언트 사이드 데이터 fetching을 조합할 때 정말 유용해요.

추가 팁을 드리자면:

- `cookies()` 같은 함수는 서버 측에서만 호출 가능해요. 클라이언트 컴포넌트에선 사용할 수 없으니 주의하세요.
- Suspense를 활용하면서 데이터 fetching이 비동기임을 명확히 해줘야 하므로, 해당 컴포넌트는 async 함수로 작성하고 반드시 프로미스를 반환해야 해요.
- PPR을 실험적으로 사용한다면, 나중에 정식 지원 상태를 꼼꼼히 확인하는 게 좋습니다. API가 변할 수도 있으니까요!

요약하자면, 서버에서 요청별로 쿠키를 읽어야 하면, 해당 컴포넌트를 Suspense로 감싸서 로딩 상태를 관리하며 안전하게 렌더링하는 패턴! Next.js에서 점점 더 퍼져나가는 최신 트렌드랍니다.

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

리액트 컴포넌트가 언제 '동적 렌더링(dynamic rendering)'을 하게 될까요? 바로 컴포넌트 내부에서 특정 값을 실제로 사용할 때입니다.

예를 들어, Next.js 페이지에서 `searchParams`를 받아 이를 자식 컴포넌트에 prop으로 넘긴 상황을 생각해볼게요:

```js
import { Table } from "./table";

export default function Page({ searchParams }: { searchParams: Promise<{ sort: string }> }) {
  return (
    <section>
      <h1>This will be prerendered</h1>
      <Table searchParams={searchParams} />
    </section>
  );
}
```

위 코드에서 `Page` 컴포넌트는 `searchParams`를 받아서 내부 `Table` 컴포넌트로 전달하고 있는데요. 중요한 점은, 이 `Page` 컴포넌트 자체는 프리렌더링(정적 생성)이 되지만, `Table` 컴포넌트 안에서 이 `searchParams` 값을 실제로 읽으면, `Table`만 동적 렌더링 상태로 전환됩니다.

즉, 부모는 정적으로 렌더링 되다가, 자식에서 비동기 값을 접근하는 순간 그 자식 컴포넌트는 실시간으로 렌더링되는 거죠. 이런 방식으로 불필요한 동적 렌더링을 방지하면서, 꼭 필요한 부분만 동적으로 처리할 수 있는 효율적인 구조가 완성됩니다.

---

추가로, 만약 `searchParams`를 여러 군데서 반복해서 사용한다면, 값을 미리 받아서 context에 저장하거나 캐싱하는 방법도 좋아요. 그래야 여러 컴포넌트에서 같은 값을 비동기로 반복 요청하는 부담을 줄일 수 있죠. 개발하면서 이런 패턴을 잘 활용하면 렌더링 성능과 사용자 경험이 꽤 좋아질 거예요!

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

안녕하세요! 오늘은 React 컴포넌트에서 `searchParams`를 비동기 함수로 받아올 때 주의할 점에 대해 같이 살펴볼게요.

아래 예제를 보실까요?

```js
export async function Table({ searchParams }: { searchParams: Promise<{ sort: string }> }) {
  const sort = (await searchParams).sort === "true";
  return "...";
}
```

여기서 `searchParams`가 `{ sort: string }` 객체를 반환하는 Promise네요. 그래서 컴포넌트 함수 앞에 `async` 키워드를 붙이고, `await`로 값을 받아옵니다.

하지만 React 컴포넌트에서 이렇게 `async` 함수를 직접 쓰는 게 늘 좋은 방식은 아니에요. 왜냐하면 컴포넌트 자체가 렌더링 시 동기적으로 작동하는 걸 기대하기 때문에, 여기에 비동기 처리를 넣으면 의도치 않은 동작이나 렌더링 지연이 발생할 수 있거든요.

그래서 보통은 이런 식으로:

- `getServerSideProps` (Next.js) 같은 서버사이드 데이터 로딩 함수에서 데이터를 먼저 받고,
- 데이터를 props로 내려받아 컴포넌트는 동기적으로 렌더링하는 게 깔끔해요.

만약 Next.js의 **App Router**를 사용중이라면, 페이지나 UI 컴포넌트에서 `searchParams`는 이미 동기적인 객체로 제공됩니다. 그래서 이렇게 쓸 수 있죠:

```tsx
type TableProps = {
  searchParams: { sort?: string };
};

export default function Table({ searchParams }: TableProps) {
  const sort = searchParams.sort === "true";
  return <div>정렬 상태: {sort ? "활성화" : "비활성화"}</div>;
}
```

마지막으로 `sort` 값을 단순히 문자열 `"true"`와 비교했는데, 혹시 `"true"`나 `"false"` 외에 다양한 값이 올 수도 있으니 `Boolean` 변환이나 다른 검증 로직도 상황에 맞게 활용해보세요!

---

### 짧은 요약

| 문제점                                           | 해결법                                                                  |
| ------------------------------------------------ | ----------------------------------------------------------------------- |
| 컴포넌트를 `async` 함수로 만들어 비동기 받아오기 | 서버사이드 또는 별도 데이터 fetching 함수로 데이터 받고, props로 넘기기 |
| `searchParams` Promise 형태 사용                 | Next.js App Router에서는 동기 객체로 바로 받아 사용 가능                |

이렇게 살짝만 더 신경 쓰면 깔끔하고 버그 없이 데이터를 다룰 수 있어서 추천드립니다! 필요하면 더 깊게 React와 Next.js의 데이터 흐름도 알려드릴게요~ 행복한 개발 되세요! 🚀
