---
title: "ReactJS에서 useLinkStatus로 현재 링크 상태 쉽게 확인하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:49
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "useLinkStatus"
link: "https://nextjs.org/docs/app/api-reference/functions/use-link-status"
isUpdated: false
---


# useLinkStatus

`useLinkStatus` 훅은 `Link` 컴포넌트의 pending 상태(즉, 네비게이션이 진행 중인 상태)를 추적할 수 있게 해줍니다. 이걸 활용하면 페이지 이동이 완료될 때까지 스피너나 텍스트 깜빡임 같은 인라인 시각적 피드백을 사용자에게 보여줄 수 있어요.

### 언제 쓸까?

| 상황                     | 설명                                                          |
|-------------------------|-------------------------------------------------------------|
| Prefetching이 꺼져 있거나  | Prefetch가 비활성화되어 있거나 진행 중이면 네비게이션이 잠시 막힙니다.   |
| 동적 라우트인데 `loading.js`가 없는 경우 | 목적지 라우트가 동적이고, 즉각적인 네비게이션을 도와줄 `loading.js` 파일이 없을 때 |

이렇게 네비가 바로 이루어지지 않을 때, `useLinkStatus`를 사용하면 유저 경험을 좀 더 부드럽게 만들어 줄 수 있답니다.

### 덧붙이는 팁!

- 동적 라우트에서 로딩 컴포넌트를 별도로 관리하는게 귀찮다면, `useLinkStatus`로 간단하게 네비바에 로딩 표시만이라도 넣어주는 게 꽤 효과적이에요.
- 네비게이션 동안 UI가 멈춘 것처럼 보이는 건 사용자 경험에서 큰 마이너스 포인트니까, 이런 작은 피드백도 신경 쓰는 게 중요합니다.

필요하다면 다음 글에서 실제 사용 예제도 가져와서 보여드릴게요!

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

이번에 소개할 내용은 Next.js에서 링크가 눌렸을 때 로딩 상태를 쉽게 감지할 수 있는 `useLinkStatus` 훅에 대해 얘기해볼게요. 실제로 프로젝트에 적용하면 사용자 경험을 좀 더 부드럽게 만들어줄 수 있답니다.

### 코드 예제

먼저, 로딩 인디케이터를 보여주는 컴포넌트가 있어요. 링크가 클릭되고 나서 페이지가 로드 중일 때 동그란 스피너가 나타나도록 하는 간단한 코드입니다.

```js
'use client'

import { useLinkStatus } from 'next/link'

export default function LoadingIndicator() {
  const { pending } = useLinkStatus()
  return pending ? (
    <div role="status" aria-label="Loading" className="spinner" />
  ) : null
}
```

다음은 이 `LoadingIndicator` 컴포넌트를 활용해서 헤더 안에 링크와 함께 로딩 상태를 보여주는 예시입니다.

```js
import Link from 'next/link'
import LoadingIndicator from './loading-indicator'

export default function Header() {
  return (
    <header>
      <Link href="/dashboard" prefetch={false}>
        Dashboard <LoadingIndicator />
      </Link>
    </header>
  )
}
```

### 알아두면 좋은 팁

| 내용 | 설명 |
|-------|-------|
| `useLinkStatus`는 `Link` 컴포넌트 하위에 위치해야 사용 가능하다 | 훅이 제대로 상태 변화를 감지하려면 링크 컨텍스트 내부여야 해요 |
| `prefetch='false'`일 때 특히 유용 | 미리 페이지를 불러오면 로딩 상태가 안 보이기 때문에, 미리 불러오기 해제시 효과적이죠 |
| 이미 프리패치된 라우트라면 pending 상태가 건너뛰어진다 | 즉, 빠르게 로딩되는 경우엔 스피너가 안 띄워질 수 있어요 |
| 연속해서 여러 링크를 클릭하면 가장 마지막 클릭한 링크만 로딩 상태가 반영된다 | 사용자 입장에서 자연스럽고 깔끔하게 처리해주는 부분이죠 |
| 이 훅은 Pages Router에서는 지원하지 않는다 | 앱 라우터(app router) 전용 기능이라는 점 기억하세요 |

### 참고할 점

- `useLinkStatus` 훅은 사용자 클릭 후 네비게이션 상태를 실시간으로 감지할 수 있어서, ‘이 페이지 가려고 클릭했는데 지금 loading 중’이라는 걸 UI에서 바로 반영하기 좋아요.
- 동시에 여러 링크를 클릭하면서도 혼란 없이 마지막 링크의 로딩 상태만 보여주기 때문에, 사용자 경험이 매끄럽죠.
- 만약 Next.js 앱 라우터를 사용하는 중이고, 로딩 표시가 필요하다면 이 훅을 적극 활용해보세요.
- 반대로 기존의 Pages Router에서는 지원하지 않으니, 다른 방법을 찾아야 함을 잊지 마시고요!

사용할 때 간단히 `prefetch={false}` 옵션을 넣으면 그 링크가 클릭될 때만 실제 라우트를 불러와서 로딩 상태를 감지할 수 있어요. 미리 페이지를 불러오는 게 싫거나, 로딩 스피너를 정확히 보여주고 싶을 때 딱 좋답니다!

필요하다면 직접 CSS로 `spinner` 클래스를 만들어 돌리는 모양새를 구현해보는 것도 추천해요. 사용자에게 좋은 피드백을 주는 거니까요!

다른 궁금한 내용 있으면 언제든 질문 주세요~ 😊

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
const { pending } = useLinkStatus()
```

`useLinkStatus`는 파라미터를 받지 않는 훅입니다.

## 반환값

`useLinkStatus`는 하나의 프로퍼티를 가진 객체를 반환하는데요, 그 프로퍼티는 다음과 같습니다:

| 프로퍼티명 | 설명                         |
|------------|------------------------------|
| pending    | 현재 링크 상태가 "대기 중"인지 여부를 나타내는 불리언 값 |

이 `pending` 값이 `true`면, 링크가 아직 처리 중이라는 뜻이고, `false`면 처리가 완료됐다는 의미입니다.

---

사실, 이런 상태 값은 UI에서 로딩 인디케이터를 표시하거나, 사용자 입력을 잠시 막아야 할 때 굉장히 유용하답니다.  
예를 들어, 버튼 클릭 후 페이지 전환이 완료될 때까지 로딩 스피너를 보여주고 싶을 때 쓰면 좋아요!  
작게라도 이런 사용자 경험(UX) 개선이 앱을 더 프로답게 만들어줍니다 :)


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

아래는 `pending` 프로퍼티에 관한 내용을 좀 더 쉽게 설명한 내용입니다.

| Property | Type    | Description                         |
| -------- | ------- | --------------------------------- |
| pending  | boolean | 히스토리 업데이트 전에는 `true`, 업데이트 후에는 `false` |

`pending`은 주로 네비게이션이 아직 진행 중일 때 `true`가 되었다가, 네비게이션이 완료되면 `false`로 바뀌는 불리언 값이에요. 이걸 활용하면 사용자가 링크를 클릭했을 때, 네비게이션이 아직 끝나지 않았다는 시각적 피드백을 줄 수 있죠.

---

## 예제

### 인라인 로딩 인디케이터(loading indicator)

사용자가 어떤 링크를 클릭했을 때, 백그라운드에서 페이지가 로드되거나 데이터가 로딩되는 동안 아무런 표시가 없으면 좀 답답하잖아요? 그래서 `pending` 값을 확인해서 로딩 중임을 보여주는 인디케이터를 추가하는 게 UX 측면에서 좋아요.

예를 들어, React에서 `pending` 상태를 체크해서 텍스트 옆에 작은 로딩 스피너를 띄우는 식으로 활용할 수 있죠. 이렇게 하면 사용자는 클릭한 순간부터 변화가 느껴져서 기다림이 덜 지루해집니다.

---

실제로 이런 상태 관리와 사용자 피드백은 요즘 웹 개발에서 굉장히 중요한 부분이라서, 네비게이션 라이브러리나 데이터 페칭 라이브러리들에 `pending`, `loading` 같은 프로퍼티가 자주 등장해요. 여러분도 프로젝트에 적용할 때 꼭 고려해보세요!

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

이번에 소개할 내용은 Next.js에서 링크를 클릭했을 때 로딩 인디케이터를 자연스럽게 보여주는 방법이에요. 보통 페이지 이동이 느릴 때 로딩 스피너를 보여주면 좋은데, 너무 빨리 넘어가면 깜빡임처럼 느껴져서 오히려 UX가 떨어질 수 있거든요.

아래 코드를 보면, `useLinkStatus` 훅을 사용해서 현재 링크가 이동 중인지 (`pending`)를 체크해요. 링크가 이동 중일 때만 `<div role="status" aria-label="Loading" className="spinner" />`가 렌더링 돼서 로딩 인디케이터를 보여주죠.

```jsx
'use client'
 
import { useLinkStatus } from 'next/link'
 
export default function LoadingIndicator() {
  const { pending } = useLinkStatus()
  return pending ? (
    <div role="status" aria-label="Loading" className="spinner" />
  ) : null
}
```

그리고 메뉴바 컴포넌트에서는 각 링크 옆에 `LoadingIndicator`를 붙여서, 사용자가 클릭하면 이동 상태에 따라 스피너가 나타나요.

```jsx
import Link from 'next/link'
import LoadingIndicator from './components/loading-indicator'
 
const links = [
  { href: '/shop/electronics', label: 'Electronics' },
  { href: '/shop/clothing', label: 'Clothing' },
  { href: '/shop/books', label: 'Books' },
]
 
function Menubar() {
  return (
    <div>
      {links.map((link) => (
        <Link key={link.label} href={link.href}>
          {link.label} <LoadingIndicator />
        </Link>
      ))}
    </div>
  )
}
 
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <div>
      <Menubar />
      {children}
    </div>
  )
}
```

여기서 한 가지 팁! 페이지가 아주 빠르게 로딩되면 로딩 인디케이터가 잠깐 깜빡이는 것처럼 보여서 사용자가 거슬릴 수 있어요. 이런 문제를 해결하는 방법은 로딩 인디케이터에 '딜레이'를 주는 겁니다. 예를 들어 100ms 정도 딜레이를 주고, 처음에는 투명도 opacity를 0으로 시작해 서서히 보이도록 하면 훨씬 자연스러워져요.

간단한 예시 CSS를 보여드리면:

```css
.spinner {
  opacity: 0;
  transition: opacity 0.3s ease;
  animation-delay: 100ms; /* 딜레이 설정 */
  animation-fill-mode: forwards;
  /* spinner 스타일 추가 */
}
.spinner.visible {
  opacity: 1;
}
```

그리고 로딩 상태가 시작되고 딜레이가 지난 후에 `visible` 클래스를 붙여서 보이도록 조절하면 됩니다.

---

### 요약

| 문제점                   | 해결법                                                  |
|-----------------------|-----------------------------------------------------|
| 로딩 인디케이터가 너무 빨리 깜빡임 | 처음 보여주기 전에 100ms 정도 딜레이를 주고 opacity 0에서 시작하여 자연스럽게 나타나도록 함 |

사실 이런 작은 UX 개선이 사용자 경험에 큰 차이를 만든답니다. 여러분도 Next.js 앱에서 로딩 인디케이터를 쓸 땐 이런 딜레이 기법을 한 번 적용해보세요!

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

이 CSS 코드는 간단한 로딩 스피너 애니메이션을 만드는 예제예요. 

```css
.spinner {
  opacity: 0;
  animation:
    fadeIn 500ms 100ms forwards,
    rotate 1s linear infinite;
}
 
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
 
@keyframes rotate {
  to {
    transform: rotate(360deg);
  }
}
```

- `.spinner` 클래스는 처음에 투명도(opacity)가 0으로 설정되어 있어요.
- `fadeIn` 애니메이션은 500ms 동안 100ms 딜레이 후에 투명도가 0에서 1로 변하며 나타나게 해주고요.
- `rotate` 애니메이션은 1초 동안 회전을 계속 반복하도록 설정되어 있어, 스피너가 빙글빙글 도는 것처럼 보이게 합니다.

여기서 중요한 점은 두 애니메이션을 동시에 적용했기 때문에, 페이드인하고 나서 스피너가 계속 회전하는 동작이 자연스럽게 연결된다는 거예요.

---

간단하게 이 애니메이션을 HTML에 적용하는 법도 알려드릴게요.

```html
<div class="spinner">
  <!-- 여기에 스피너 아이콘이나 SVG, 이미지 등을 넣을 수 있어요 -->
</div>
```

그리고 스피너의 크기나 모양을 CSS로 더 커스터마이징하는 것도 가능하니, 필요에 따라 조절해 보세요.

---

또한 최신 브라우저들은 애니메이션 성능을 최적화해주지만, 너무 많은 애니메이션을 한꺼번에 적용하면 CPU 사용량이 늘 수 있으니 주의하세요!

---

이제 표 정보도 Markdown 형식으로 바꿔서 보여드릴게요:

| Version  | Changes                  |
|----------|--------------------------|
| v15.3.0  | `useLinkStatus` introduced. |

`useLinkStatus`가 뭔지 궁금하다면, 다음에 자세히 다뤄볼게요! 지금은 CSS 애니메이션과 함께 잠깐 살펴봤어요. 도움이 되셨길 바랍니다!