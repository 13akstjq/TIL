---
title: "Nextjs 15에서 draftMode 기능 사용 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 03:02
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "draftMode"
link: "https://nextjs.org/docs/app/api-reference/functions/draft-mode"
isUpdated: false
---


# draftMode

이번에 소개할 기능은 Next.js에서 제공하는 `draftMode`라는 async 함수입니다. 이 함수는 서버 컴포넌트(Server Component) 내에서 Draft Mode를 켜고 끌 수 있게 해주고, 현재 Draft Mode가 활성화되어 있는지도 확인할 수 있어요.

```js
import { draftMode } from 'next/headers'

export default async function Page() {
  const { isEnabled } = await draftMode()
  // isEnabled가 true면 Draft Mode가 활성화된 상태,
  // false면 비활성화 상태입니다.
}
```

## Draft Mode란?

Next.js에서 Draft Mode는 관리자나 콘텐츠 작성자가 실제 공개된 페이지와는 별개로 콘텐츠를 미리보기하거나 테스트할 때 유용합니다. 보통 CMS(Content Management System)와 연동해서, 아직 퍼블리시되지 않은 글이나 변경사항을 미리 볼 때 많이 활용되죠.

## draftMode 함수 주요 기능

- **활성화**: Draft Mode 켜기
- **비활성화**: Draft Mode 끄기
- **상태확인**: 현재 Draft Mode가 켜져 있는지 확인

### 예를 들어 Draft Mode를 켜고 끄는 방법

아래처럼 `enable()`과 `disable()` 메서드를 쓸 수 있어요.

```js
import { draftMode } from 'next/headers'

export default async function Page() {
  const draft = await draftMode()

  // Draft Mode 활성화
  draft.enable()

  // Draft Mode 비활성화 (필요시)
  // draft.disable()

  const { isEnabled } = await draftMode()
  console.log('Draft Mode 활성화 여부:', isEnabled)
}
```

## 참고할 점

- `draftMode()`는 서버 컴포넌트에서만 쓸 수 있습니다. 클라이언트 컴포넌트에서는 동작하지 않아요.
- 주로 CMS와 함께 사용해, 콘텐츠 미리보기를 간단하게 구현할 때 유용합니다.
- Draft Mode가 켜져 있으면, 예를 들어 API 요청 시 비공개 상태인 콘텐츠도 가져오도록 설정할 수 있죠.

요즘 Next.js가 점점 서버 기능과 콘텐츠 관리 기능을 강화하는데, Draft Mode는 그 중에서도 굉장히 실용적인 기능이니 꼭 한번 써보시길 추천합니다!  

---

## Reference

- [Next.js Documentation - draftMode](https://nextjs.org/docs/app/building-your-application/configuration/draft-mode) (공식 문서 링크)

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

다음은 Draft Mode에서 사용할 수 있는 메서드와 프로퍼티들입니다:

| 메서드         | 설명                                                         |
| -------------- | ------------------------------------------------------------ |
| `isEnabled`    | Draft Mode가 활성화되어 있는지 여부를 나타내는 boolean 값입니다. |
| `enable()`     | Route Handler에서 Draft Mode를 활성화하며, 이때 `__prerender_bypass` 쿠키를 설정합니다. |
| `disable()`    | Route Handler에서 Draft Mode를 비활성화하며, 쿠키를 삭제합니다. |

---

## 알아두면 좋은 팁

- `draftMode` 함수는 비동기 함수로, Promise를 반환합니다. 따라서 `async/await`를 사용하거나 React의 `use` 함수를 사용해야 해요.  
  예전(Next.js 14버전 이하)에는 `draftMode`가 동기 함수였는데, 15버전에서는 하위 호환성을 위해 동기 접근도 가능하지만 곧 Deprecated될 예정입니다.

- `next build`를 할 때마다 새로운 우회 쿠키 값이 생성되기 때문에, 이 쿠키 값을 예측하기 어렵습니다. 보안 측면에서 좋은 변화죠!

- 로컬에서 Draft Mode를 HTTP로 테스트할 때는 브라우저 설정에서 **서드파티 쿠키 허용** 및 **로컬 스토리지 접근 허용**이 필요하니 참고하세요.

---

### 좀 더 자세한 설명!

`Draft Mode`는 쉽게 말해 "임시 저장" 모드 같은 개념인데요, 보통은 빌드된 정적 페이지를 보여주지만, 이 모드를 켜면 실시간으로 수정 중인 데이터를 확인할 수 있어 개발할 때 정말 유용합니다. 예를 들어, CMS에서 콘텐츠를 수정하며 바로바로 결과를 확인하고 싶을 때 많이 쓰이죠.

`enable()`을 호출하면 내부적으로 `__prerender_bypass`라는 쿠키가 설정되고, Next.js 서버는 이 쿠키를 보고 캐시된 정적 파일 대신 최신 서버 데이터를 반환합니다. 그리고 개발이 끝났으면 `disable()`을 호출해서 쿠키를 삭제해 다시 캐싱된 페이지를 보여주도록 해요.

마지막으로, 비동기 함수라는 점을 꼭 주의하세요. Next.js가 점점 비동기 패턴을 밀고 있기 때문에, 예전처럼 동기적으로 호출하면 버그가 날 수 있으니 async/await 사용을 권장합니다.

이 글이 Draft Mode를 이해하는 데 도움이 됐길 바라요! 개발하면서 궁금한 점 있으면 언제든 물어보세요~

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

## 예제

### Draft Mode 활성화하기

Draft Mode를 사용하려면 새로운 Route Handler를 만들고 `enable()` 메서드를 호출하면 돼요. 이렇게 하면 임시로 수정 중인 내용을 확인할 수 있답니다.

```js
import { draftMode } from 'next/headers'

export async function GET(request: Request) {
  const draft = await draftMode()
  draft.enable()
  return new Response('Draft mode is enabled')
}
```

---

### 조금 더 설명하자면?

`draftMode()` 함수는 현재 요청에서 Draft Mode의 상태를 제어할 수 있는 객체를 반환해줘요. 그 객체의 `enable()` 메서드를 호출하면 그 사용자의 세션에 Draft Mode가 활성화돼서, 보통은 비공개로 관리되는 수정 중인 콘텐츠들을 볼 수 있게 되죠.

예를 들어, 컨텐츠 수정 중인 페이지를 실시간으로 확인하고 싶을 때 엄청 편리해요. 반대로 `disable()` 메서드를 호출하면 Draft Mode가 해제됩니다.

Next.js를 사용 중이라면 이 기능을 활용해서 콘텐츠 검수나 개발 중인 기능 확인도 쉽게 할 수 있으니, 꼭 한 번 써보세요!

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

### Draft Mode 끄기

기본적으로 Draft Mode는 브라우저를 닫으면 세션이 종료돼요.

하지만 직접 Draft Mode를 끄고 싶다면, Route Handler 안에서 `disable()` 메서드를 사용하면 돼요. 예를 들어:

```js
import { draftMode } from 'next/headers'

export async function GET(request: Request) {
  const draft = await draftMode()
  draft.disable()
  return new Response('Draft mode is disabled')
}
```

이렇게 하면 현재 세션에서 Draft Mode가 비활성화돼서, 더 이상 임시 콘텐츠나 수정된 내용을 확인할 수 없게 돼요.

여기서 한 가지 팁! 보통 Draft Mode는 테스트나 미리보기용으로 많이 쓰이니, 실제 서비스에선 잘 꺼두는 게 좋아요. Draft Mode가 켜진 상태에서는 캐싱이나 일부 기능이 동작하지 않을 수 있으니까요.

참고로, Next.js의 `draftMode()`는 클라이언트-서버 환경 모두에서 사용 가능해, 서버에서 조건에 따라 쉽게 Draft Mode를 켜고 끌 수 있다는 점도 기억해두세요!

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

여러분, Next.js에서 Route Handler를 호출할 때 주의할 점이 있어요! 만약 `Link` 컴포넌트를 이용해서 라우트를 호출한다면, `prefetch='false'`를 꼭 넘겨줘야 해요. 이걸 안 하면 prefetch 과정에서 쿠키가 의도치않게 삭제될 수 있거든요. 

---

### Draft Mode가 켜져 있는지 확인하는 방법

서버 컴포넌트(Server Component)에서 Draft Mode가 활성화되어 있는지 확인하고 싶을 때는, `next/headers` 모듈의 `draftMode` 함수를 사용하면 돼요. 이 함수는 `isEnabled`라는 프로퍼티를 반환하는데, 이걸 통해 현재 Draft Mode 상태를 알 수 있죠.

아래 코드를 한 번 보세요:

```js
import { draftMode } from 'next/headers'

export default async function Page() {
  const { isEnabled } = await draftMode()
  return (
    <main>
      <h1>My Blog Post</h1>
      <p>Draft Mode is currently {isEnabled ? 'Enabled' : 'Disabled'}</p>
    </main>
  )
}
```

이렇게 하면 페이지를 렌더링할 때 Draft Mode가 켜져 있는지 바로 확인할 수 있어요. 

> 팁 하나 더! Draft Mode는 예를 들어 컨텐츠를 작성하거나 수정할 때 유용해요. 실제로 배포된 최종 버전에 반영되기 전에 미리 볼 수 있게 해주는 기능이라, 가끔 이 상태가 켜져 있는지 확인해 보는 것이 중요하답니다.

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

| 버전           | 변경사항                                                                                 |
| -------------- | ---------------------------------------------------------------------------------------- |
| `v15.0.0-RC`   | `draftMode`가 이제 비동기 함수(async function)로 변경되었습니다. 관련 [코드모드(codemod)](/docs/app/guides/upgrading/codemods#150)도 제공 중이에요. |
| `v13.4.0`      | `draftMode` 기능이 처음 도입되었습니다.                                                  |

---

여기서 잠깐! `draftMode`라는 기능이 뭔지 궁금하신 분들도 계실텐데요, 쉽게 말해서 Next.js에서 "초안 모드"처럼 작동해서 페이지가 캐시되지 않고 항상 최신 상태를 유지하도록 도와주는 기능이에요. 특히 서버사이드 렌더링하거나 ISR(Incremental Static Regeneration)을 사용할 때 실시간 업데이트가 필요할 때 유용하답니다.

그리고 `v15.0.0-RC`부터는 이 `draftMode` 함수가 async로 바뀌면서, 내부에서 비동기 작업이 가능해졌어요. 만약 이전 버전에서 사용 중이라면 코드 모드를 통해 자동으로 변환할 수 있으니 큰 걱정 하지 않아도 됩니다! 

Next.js 같은 프레임워크가 계속 발전하면서 이런 세세한 변경사항도 꼭 챙겨 두시는 게 좋아요. 개발할 때 버전별 차이로 헷갈리는 일이 줄어들 거예요!