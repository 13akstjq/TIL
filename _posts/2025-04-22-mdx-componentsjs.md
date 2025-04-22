---
title: "Next.js 15에서 mdx-components.js로 커스텀 MDX 컴포넌트 만드는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:41
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "mdx-components.js"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/mdx-components"
isUpdated: false
---


# mdx-components.js (또는 .tsx) 파일 소개

Next.js에서 `@next/mdx`와 App Router를 함께 사용할 때, `mdx-components.js` (또는 `mdx-components.tsx`) 파일은 꼭 필요한 친구예요. 이 파일이 없으면 MDX를 제대로 쓸 수 없거든요. 게다가 이 파일을 활용하면 MDX 내에서 사용하는 컴포넌트들의 스타일이나 동작을 커스터마이징할 수도 있답니다!

---

## 위치는 어디에?

- 프로젝트 루트 바로 아래에 둬요.  
- 예를 들어, `pages` 또는 `app` 폴더와 같은 레벨에 (또는 `src` 폴더 안에) 두면 좋아요.

---

## 기본적인 작성법

```tsx
import type { MDXComponents } from 'mdx/types'

export function useMDXComponents(components: MDXComponents): MDXComponents {
  return {
    ...components,
  }
}
```

이 코드는 기본 설정으로 컴포넌트들을 그대로 반환하는 역할을 해요. 여기서 `components`에 우리가 직접 만든 커스텀 컴포넌트들을 덮어씌워서 스타일이나 기능을 내가 원하는 대로 바꿀 수 있죠.

---

## 실제로 커스텀 해보기

예를 들어, MDX 안에서 `<h1>` 태그의 스타일을 바꾸고 싶다면 이렇게 작성할 수 있어요:

```tsx
import type { MDXComponents } from 'mdx/types'

const MyH1 = (props) => <h1 style={{ color: 'tomato', fontSize: '2.5rem' }} {...props} />

export function useMDXComponents(components: MDXComponents): MDXComponents {
  return {
    h1: MyH1,
    ...components,
  }
}
```

이렇게 하면 MDX 파일 내 `<h1>` 태그가 모두 토마토색 큰 글씨로 바뀌죠.

---

## 참고! 왜 이걸 쓰는 걸까?

MDX 안에는 기본 HTML 태그뿐만 아니라, React 컴포넌트도 자유롭게 넣을 수 있어요. 예를 들어 버튼, 카드, 또는 특별한 레이아웃 컴포넌트 등이죠. 이걸 한 번에 관리하고 싶다면 이 `mdx-components` 파일을 활용해서 확장하거나 수정하는 게 가장 깔끔하답니다.

---

**정리하자면**, `mdx-components.js`(또는 `.tsx`)는 Next.js + MDX 환경에서 커스텀 컴포넌트를 선언하고 스타일을 조절하는 중요한 허브 역할을 하니 꼭 기억하세요! 프로젝트에서 MDX를 쓴다면 이 친구는 필수랍니다 :)

---

필요하면 나중에 커스텀 컴포넌트를 더 만드는 법도 알려드릴게요!

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

## Exports

### useMDXComponents 함수

이 파일은 하나의 함수만 내보내야 해요. 기본 내보내기(default export)로 하거나 `useMDXComponents`라는 이름으로 내보낼 수 있죠.

```js
import type { MDXComponents } from 'mdx/types'

export function useMDXComponents(components: MDXComponents): MDXComponents {
  return {
    ...components,
  }
}
```

> 여기서 `useMDXComponents` 함수는 MDX 컴포넌트를 커스터마이징할 때 유용해요. 예를 들어, 기본 제공되는 MDX 컴포넌트를 당신만의 스타일이나 새로운 컴포넌트로 대체하고 싶을 때 이 함수를 사용하죠.  
> 그리고 `components` 객체를 받아 그대로 돌려주고 있는데, 필요하면 여기에 새로운 컴포넌트를 추가하거나 기존 컴포넌트를 덮어씌우는 작업도 할 수 있어요.  
>
> 간단한 예를 들어볼게요:  
> js
> export function useMDXComponents(components) {
>   return {
>     h1: (props) => <h1 style={{ color: 'tomato' }} {...props} />,
>     ...components,
>   }
> }
> 
> 이렇게 하면 MDX 내의 모든 `h1` 태그는 토마토색 글씨로 표시되겠죠!   
>  
> MDX로 콘텐츠 작업할 때 아주 많이 쓰이는 패턴입니다.  
> 필요에 따라 잘 커스터마이징해보세요!

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

## Params

### components

MDX 컴포넌트를 정의할 때, export 함수는 단일 파라미터인 `components`를 받습니다. 이 `components`는 `MDXComponents` 타입의 인스턴스인데요.

- **키(key)**는 대체하고자 하는 HTML 태그 이름을 의미합니다.  
- **값(value)**은 해당 HTML 태그 대신 렌더링할 컴포넌트를 지정하는 거죠.

즉, 기본 HTML 요소를 내 마음대로 바꾸고 싶을 때 유용하게 쓸 수 있습니다. 예를 들어, `<h1>` 태그를 나만의 스타일이 입혀진 헤딩 컴포넌트로 교체할 수도 있고, `<a>` 태그를 커스텀 링크 컴포넌트로 바꿔서 새 창 열기 같은 기능을 쉽게 추가할 수도 있답니다.

이걸 활용하면 MDX 문서 내에서 일관된 UI를 손쉽게 유지할 수 있고, 스타일이나 동작도 프로젝트에 맞게 맞춤 설정할 수 있어요! 많이 써먹어보세요~

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

> 참고할 점: 오버라이드가 없는 다른 컴포넌트들도 모두 전달해주는 것(즉, ...components)을 잊지 마세요.

## 버전 히스토리

| Version     | Changes           |
|-------------|-------------------|
| `v13.1.2`   | MDX 컴포넌트 추가 | 

여기서 MDX 컴포넌트란, Markdown과 React 컴포넌트를 함께 쓸 수 있게 해주는 기능이에요. 프로젝트에서 문서나 블로그 작성할 때 훨씬 유용하게 쓰이니까 꼭 한 번 써보시길 추천합니다!