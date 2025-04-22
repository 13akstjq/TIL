---
title: "Next.js 15에서 useParams로 현재 URL 파라미터 쉽게 가져오는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:50
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "useParams"
link: "https://nextjs.org/docs/app/api-reference/functions/use-params"
isUpdated: false
---


# useParams 훅 쉽게 이해하기

Next.js에서 클라이언트 컴포넌트에서 현재 URL에 포함된 동적 라우트 파라미터를 읽고 싶을 때 `useParams` 훅을 사용해요. 예를 들어, 페이지 경로가 `/shop/[tag]/[item]` 같은 동적 경로일 때, URL에 들어오는 값들을 아주 간단하게 가져올 수 있답니다.

```js
'use client'

import { useParams } from 'next/navigation'

export default function ExampleClientComponent() {
  const params = useParams<{ tag: string; item: string }>()

  // 예를 들면,
  // Route: /shop/[tag]/[item]
  // URL:   /shop/shoes/nike-air-max-97
  // params: { tag: 'shoes', item: 'nike-air-max-97' }
  console.log(params)

  return '...'
}
```

위 예시에서 보듯이, URL에 적힌 `shoes`와 `nike-air-max-97`이 `params` 객체로 딱 받아와집니다. 타입스크립트를 쓴다면 타입을 이렇게 명시해주는 게 타입 안정성에도 좋아요.

---

## useParams 훅으로 알아둬야 할 파라미터

| 파라미터명 | 설명                        | 타입          | 비고                       |
|------------|-----------------------------|---------------|----------------------------|
| —          | 현재 URL의 동적 라우트 파라미터를 객체로 리턴 | `Record<string, string \| undefined>` | 기본적으로 `string` 타입의 값을 가지며, 없으면 `undefined` |

- `useParams()`는 경로에서 동적으로 지정된 부분들을 모두 객체 형태로 반환해요.
- 파라미터가 URL에 없다면 해당 키값에는 `undefined`가 들어옵니다.

---

### 추가 팁!

- 만약 페이지 안에서 여러 동적 경로를 분기처리하거나, 현재 URL 상태에 따라 다른 UI를 보여줘야 한다면 이 `useParams` 훅이 굉장히 유용하답니다.
- 다음과 같이 조건문을 통해서도 동작할 수 있어요:

```js
if (params.tag === 'shoes') {
  // shoes 관련 컴포넌트 렌더링
} else if (params.tag === 'clothing') {
  // clothing 관련 컴포넌트 렌더링
}
```

이렇게 간단하게 URL 경로에 따라 유저에게 보여질 화면을 조절할 수 있답니다!

---

`useParams` 훅은 Next.js 13의 App Router에서 클라이언트 단에서 라우트 파라미터를 다룰 때 필수라고 할 수 있어요. 앞으로 동적 라우트와 클라이언트 상태관리를 함께 하려면 꼭 기억해두세요! :)

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
const params = useParams()
```

`useParams`는 파라미터를 받지 않는 훅(Hook)이에요.

## 반환값

`useParams`는 현재 라우트에서 동적으로 채워진 파라미터들을 담고 있는 객체를 반환합니다.

예를 들면, 경로가 `/user/:id` 라고 했을 때, URL이 `/user/123` 이라면 `params` 객체는 `{ id: '123' }` 가 되는 거죠.

---

### 좀 더 알아보기

`useParams`는 리액트 라우터에서 동적 라우팅을 할 때 매우 유용해요. URL 경로에 따라 컴포넌트가 다르게 동작해야 하는 경우 이 훅을 사용해서 URL의 특정 부분값을 쉽게 읽어올 수 있으니까요.

예를 들어, 블로그 글 상세 페이지에서 포스트 아이디를 URL에서 꺼내와서 API 요청에 사용하거나, 특정 사용자 페이지에서 사용자 아이디에 해당하는 데이터를 불러오는 작업에 딱 맞아요!

```js
import { useParams } from 'react-router-dom';

function UserProfile() {
  const params = useParams();
  console.log(params.id); // 여기서 id는 URL에서 받은 파라미터 값

  // 받아온 id로 API 호출 등 로직 작성 가능
}
```

요거 한번 잘 활용해보세요! 동적 라우팅이 필요한 프로젝트라면 정말 많으니까요.

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

useParams에 대해 쉽게 정리해볼게요!

---

### useParams란?

`useParams`는 URL 경로에서 동적 세그먼트(dynamic segment)를 쉽게 가져올 수 있게 도와주는 훅이에요. 여기서 '동적 세그먼트'란 URL 중 변하는 부분을 말해요.

---

### 핵심 정리

- 객체 내 속성 각각은 하나의 동적 세그먼트입니다.
- 속성 이름은 그 세그먼트의 이름이고, 값은 실제 채워진 값이에요.
- 값은 문자열 하나일 수도 있고, 여러 개가 배열로 들어있을 수도 있어요. (동적 세그먼트 타입에 따라 달라짐)
- 만약 경로에 동적 파라미터가 없으면 `useParams`는 빈 객체를 리턴합니다.
- Pages Router에서 사용하면 초기 렌더링 시 `useParams`가 `null`이고, 라우터가 준비되면 위 규칙에 맞게 업데이트됩니다.

---

### 예제로 이해하기

| Route                       | URL         | `useParams()`               |
|-----------------------------|-------------|-----------------------------|
| `app/shop/page.js`          | `/shop`     | `{}`                        |
| `app/shop/[slug]/page.js`   | `/shop/1`   | `{ slug: '1' }`             |
| `app/shop/[tag]/[item]/page.js` | `/shop/1/2` | `{ tag: '1', item: '2' }` |
| `app/shop/[...slug]/page.js` | `/shop/1/2` | `{ slug: ['1', '2'] }`      |

---

### 살짝 덧붙이자면…

- `useParams`는 Next.js 앱 디렉터리 구조에 맞춘 편리한 훅이에요.
- 동적 라우팅 패턴을 복잡하게 만들수록 `useParams`를 잘 이용하면 URL 파싱이 훨씬 편해져요.
- 예를 들면, `[...slug]`와 같이 rest 파라미터로 여러 경로 세그먼트를 배열로 받을 수 있어 유연하게 쓸 수 있답니다.
- 단, `useParams`는 클라이언트 사이드 훅이기 때문에 서버사이드 렌더 상황에 따라 초기 값이 다를 수 있다는 점도 고려하세요.

이해를 돕기 위해 다른 예제가 더 궁금하면 언제든 물어봐주세요! 😄

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

아래는 위의 HTML 테이블을 마크다운 형식으로 바꾼 내용이에요. 

| Version   | Changes              |
|-----------|----------------------|
| `v13.3.0` | `useParams` 도입됨.  |

---

`useParams`는 React Router 같은 라우팅 라이브러리에서 URL 파라미터를 쉽게 가져올 수 있게 도와주는 훅(hook)이에요. 보통 동적인 라우팅을 할 때 매우 유용하죠. 예를 들어 `/user/:id` 같은 경로에서 id 값을 가져올 때요.

다음엔 `useParams`를 실제로 어떻게 쓰는지 간단한 예시도 같이 다루도록 할게요. 개발하면서 자주 쓰게 될 기능이라 미리 익혀두면 좋아요!