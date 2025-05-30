---
title: "Next.js 15에서 클라이언트 사이드 렌더링 하는 방법 (use client)"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:13
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "use client"
link: "https://nextjs.org/docs/app/api-reference/directives/use-client"
isUpdated: false
---


# use client

React에서 `use client`는 컴포넌트를 클라이언트 사이드에서 렌더링하도록 지정하는 지시자입니다. 쉽게 말해, 사용자와 상호작용하는 UI, 예를 들면 상태 관리(state management), 이벤트 처리(event handling), 브라우저 API 접근 등이 필요할 때 이 지시자를 사용해요. 서버에서 렌더링되는 컴포넌트와 달리, 클라이언트 컴포넌트는 브라우저에서 직접 실행되기 때문에 좀 더 동적인 기능을 구현할 수 있죠.

## 사용법

컴포넌트를 클라이언트 컴포넌트로 만들고 싶다면, 파일 제일 상단에 `use client` 지시자를 추가하면 됩니다. 이 지시자는 모든 import문보다 먼저 위치해야 해요.

```jsx
"use client";

import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      클릭 수: {count}
    </button>
  );
}

export default Counter;
```

> 여기서 중요한 점은 `"use client"`가 문자열로서 파일 맨 위에 위치해야 한다는 거예요. 만약 위치가 바뀌면 제대로 작동하지 않을 수 있어요.

---

### 추가 팁!

- React 18부터 도입된 이 기능은 Next.js 13 이상의 App Router와 함께 자주 사용돼요.
- 클라이언트 컴포넌트에는 서버 전용 데이터 페칭 기능을 직접 사용할 수 없으니, 필요한 데이터는 부모 서버 컴포넌트에서 미리 받아서 props로 전달하는 방식으로 설계해야 합니다.
- 너무 많은 컴포넌트에 `use client`를 붙이면 클라이언트 번들 크기가 커져서 로딩 속도에 영향을 줄 수 있으니, 필요한 부분에 집중해서 사용하는 게 좋아요.

`use client`를 잘 활용하면 React의 서버-클라이언트 경계에서 효율적인 UI 구성이 가능하답니다!

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

```jsx
import { useState } from 'react'

export default function Counter() {
  const [count, setCount] = useState(0)

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  )
}
```

## 서버 컴포넌트 안에 클라이언트 컴포넌트 넣기

여러분, React 최신 트렌드인 서버 컴포넌트(Server Components)와 클라이언트 컴포넌트(Client Components)에 대해 들어보셨나요? 이 두 가지를 잘 섞으면 속도 빠르고 사용자와 상호작용이 뛰어난 앱을 만들 수 있답니다.

| 컴포넌트 종류      | 용도 및 특징                                         |
|------------------|---------------------------------------------------|
| 서버 컴포넌트       | 정적인 콘텐츠, 데이터 패칭, SEO 최적화에 적합          |
| 클라이언트 컴포넌트  | 상태 관리, 이벤트 핸들링, 브라우저 API 사용 등 동적 기능 담당 |
| 조합 방법           | 서버 컴포넌트 안에 필요한 UI에 클라이언트 컴포넌트를 넣어 사용 |

예를 들어, 서버에서 렌더링하는 부분은 정적으로 두고, 사용자가 버튼을 클릭해 카운터를 올리는 등 동적인 기능이 필요한 부분은 클라이언트 컴포넌트로 처리합니다.

이렇게 하면, 전체 앱의 초기 로딩 속도는 빠르면서도, 사용자와의 상호작용도 놓치지 않는 깔끔한 구조를 유지할 수 있어요.

추가로! 서버 컴포넌트는 기본적으로 클라이언트 상태나 이벤트를 직접 사용할 수 없기 때문에, 꼭 필요할 때만 클라이언트 컴포넌트를 분리해서 넣어주는 게 좋습니다. 이렇게 역할을 명확히 나누면, 코드 관리도 훨씬 수월해지고 최적화도 쉬워져요.

궁금하다면 직접 서버 컴포넌트에서 클라이언트 컴포넌트를 호출하는 예제를 찾아보면서 써보세요! React 18 이후부터 이런 방식이 점점 더 표준이 되고 있답니다.

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

다음 예시를 살펴볼게요:

- **Header**는 정적 콘텐츠를 처리하는 **서버 컴포넌트**입니다.
- **Counter**는 페이지 내에서 상호작용을 가능하게 하는 **클라이언트 컴포넌트**예요.

```js
import Header from './header'
 
export default function Page() {
  return (
    <div>
      <Header />
    </div>
  )
}
```

---

### 좀 더 쉽게 이해해볼까요?

서버 컴포넌트는 서버에서 렌더링되어 HTML 형태로 클라이언트로 전달돼요. 그래서 페이지를 빠르게 보여줄 수 있고, 초기 로드가 더 빨라지죠. 단, 서버 컴포넌트는 React 훅(useState, useEffect 등)을 사용할 수 없고, 사용자와 직접 상호작용하는 기능을 처리하는 데는 적합하지 않아요.

반대로 클라이언트 컴포넌트는 브라우저(클라이언트)에서 실행되므로, 사용자 입력에 반응하거나 실시간으로 변화하는 UI를 만들 수 있어요. 다만, 클라이언트 컴포넌트는 서버 컴포넌트보다 초기 로딩 시 무거울 수 있답니다.

---

### 간단 비교 테이블

| 구분         | 서버 컴포넌트          | 클라이언트 컴포넌트      |
| ------------ | --------------------- | ----------------------- |
| 실행 위치     | 서버                  | 브라우저(클라이언트)     |
| 상태 관리     | 불가능                | 가능                    |
| 렌더링 속도   | 빠름                  | 상대적으로 느림          |
| 사용자 상호작용 | 제한적                 | 자유롭게 가능            |
| 사용 예시     | 헤더, 정적 페이지 등   | 버튼, 폼, 카운터 등       |

---

이렇게 두 가지 컴포넌트의 역할을 잘 나누면, 성능과 개발 효율성을 모두 잡을 수 있습니다. 앞으로 Next.js나 React 최신 버전을 쓸 때 이 점 꼭 기억해 주세요! 필요하다면 서버 컴포넌트 안에서 클라이언트 컴포넌트를 불러 올 수도 있으니, 상황에 맞게 섞어서 써보는 것도 추천합니다.

필요한 자료가 더 있다면 언제든 알려주세요!

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

React 문서에서 "use client"에 대해 더 자세히 알아볼 수 있어요.

사실 "use client"는 React 18부터 도입된 개념이라, 클라이언트 사이드 렌더링을 명확히 구분하고자 할 때 많이 사용돼요. 예를 들어, Next.js 13처럼 React 18 기반 프레임워크에서는 서버 컴포넌트와 클라이언트 컴포넌트를 구분해서 작성하는 게 중요하거든요.

"‘use client’"라는 지시어를 컴포넌트 파일 상단에 적으면, 이 컴포넌트가 클라이언트 쪽에서 실행된다는 걸 명확히 알릴 수 있어요. 그럼 그 컴포넌트 안에 useState, useEffect 같은 React 훅들을 편하게 쓸 수 있답니다.

더 자세한 내용과 공식 가이드는 리액트 공식 문서에서 확인해 보세요! 여기에 간단 링크도 남겨 둘게요.

- [React 공식 문서 - Client Components](https://reactjs.org/docs/client-components.html) (영문)  
- Next.js에서 클라이언트 컴포넌트 사용하는 방법도 참고하면 좋아요

이외에 React 18을 쓰면서 새로운 컴포넌트 구분법에 대해 궁금한 점 있으면 언제든 물어봐 주세요!