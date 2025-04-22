---
title: "Next.js 15 로딩 컴포넌트 만들기 (loading.js)"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:41
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "loading.js"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/loading"
isUpdated: false
---


# loading.js 소개

이번에는 React에서 사용할 수 있는 `loading.js` 파일에 대해 이야기해볼게요. 이 파일은 Suspense를 활용해서 **즉각적인 로딩 상태**를 만들어 주는 역할을 해요.

기본적으로는 서버 컴포넌트(Server Component)로 작동하지만, 필요에 따라 `"use client"` 지시어를 붙여서 클라이언트 컴포넌트(Client Component)로도 쓸 수 있답니다.

---

## 예시 코드

```js
export default function Loading() {
  // 또는 내가 만든 커스텀 로딩 스켈레톤 컴포넌트를 넣어줄 수도 있어요
  return <p>Loading...</p>;
}
```

### 좀 더 쉽게 설명하자면?

이 파일을 만들어 놓으면 페이지 혹은 컴포넌트가 로딩될 때, 사용자에게 “로딩 중입니다” 같은 상태를 바로 보여줄 수 있어 UX가 훨씬 좋아지죠. Suspense가 컴포넌트를 렌더링하는 동안 이 `Loading` 컴포넌트를 잠시 보여주는 식이에요.

### 꿀팁: 커스텀 로딩 UI 만들기

간단한 텍스트 대신에 애니메이션이 들어간 스켈레톤 UI나 뼈대 화면을 만들어서 넣으면 더 자연스럽고 프로답게 보일 수 있어요. React의 `react-loading-skeleton` 라이브러리를 사용해도 좋고, CSS 애니메이션으로 직접 꾸며줄 수도 있답니다.

---

그럼 다음에는 Suspense랑 함께 이 `loading.js`를 어떻게 더 멋지게 활용하는지도 같이 알아볼게요!

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

로딩 UI 컴포넌트는 어떤 파라미터도 받지 않는다는 점, 알아두면 좋아요.

> 참고 팁:
로딩 UI를 설계할 때 React Developer Tools를 이용하면 Suspense 경계(boundary)를 수동으로 토글해보면서 동작을 확인하는 데 큰 도움이 됩니다.

## 버전 히스토리

| 버전       | 변경 사항              |
|------------|-----------------------|
| `v13.0.0`  | `loading` 컴포넌트 도입 | 

추가로, 로딩 UI는 사용자 경험에서 정말 중요한 부분이에요. 너무 짧게 보여주면 깜빡이는 느낌이 들 수 있고, 너무 길면 지루한 느낌을 줄 수 있거든요. 그래서 React Suspense와 함께 사용할 때는 로딩 컴포넌트를 잘 활용해서 자연스러운 화면 전환 효과를 만드는 게 중요해요. React Developer Tools로 직접 토글해보면서 그런 느낌을 체험해 보세요!