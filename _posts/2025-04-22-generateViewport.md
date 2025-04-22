---
title: "Next.js 15에서 generateViewport 함수로 반응형 페이지 만드는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:39
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "generateViewport"
link: "https://nextjs.org/docs/app/api-reference/functions/generate-viewport"
isUpdated: false
---


# generateViewport

웹페이지의 초기 뷰포트(viewport)를 설정할 때, 정적인 `viewport` 객체나 동적인 `generateViewport` 함수를 사용할 수 있어요.

> 알아두면 좋은 점:
> - `viewport` 객체와 `generateViewport` 함수는 서버 컴포넌트(Server Components)에서만 지원돼요.
> - 하나의 라우트 세그먼트(route segment)에서 `viewport` 객체와 `generateViewport` 함수를 동시에 내보낼(export) 수 없답니다.
> - 만약 메타데이터(metadata) 내보내기(export) 방식을 이전 버전에서 마이그레이션하는 중이라면, `metadata-to-viewport-export` 코드를 자동 변환해주는 codemod를 활용하면 편리해요.

## viewport 객체

(여기서부터는 viewport 객체에 대한 설명이 이어질 텐데, 필요하면 추가로 알려드리겠습니다!)

---

### 조금 더 부연 설명

`generateViewport`는 화면 크기나 사용자 환경에 맞춰 화면 표시를 동적으로 조정하고 싶을 때 유용해요. 예를 들어, 데스크탑과 모바일을 구분해서 뷰포트 설정을 다르게 하고 싶을 때 사용할 수 있죠.

반대로, `viewport` 객체는 애초에 고정된 초기값을 정할 때 적합합니다. 보통 한 가지 뷰포트 설정으로 충분한 경우라면 이걸 사용하면 돼요.

서버 컴포넌트에서만 지원된다는 점도 기억해야 해요. 클라이언트 컴포넌트에서는 이 기능을 사용할 수 없으니, 뷰포트를 조절하려면 서버 사이드 렌더링 또는 서버 컴포넌트를 활용해야 한답니다.

다음에는 예제 코드와 함께 자세히 보여드릴게요!

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

뷰포트(viewport) 설정을 정의하려면, `layout.jsx` 또는 `page.jsx` 파일에서 뷰포트 객체를 export 하면 돼요.

```js
import type { Viewport } from 'next'

export const viewport: Viewport = {
  themeColor: 'black',
}

export default function Page() {}
```

여기서 `viewport` 객체에 여러 옵션들을 넣을 수 있는데, 예를 들어 `themeColor`를 지정해서 브라우저 테마 색상을 바꿀 수 있어요. 이렇게 하면 사용자의 환경에 맞게 더 맞춤화된 UI를 제공할 수 있답니다.

---

### generateViewport 함수

`generateViewport` 함수는 하나 이상의 뷰포트 필드를 포함하는 `Viewport` 객체를 반환해야 해요. 즉, 동적으로 뷰포트 옵션을 생성하고 싶을 때 이 함수를 사용하면 유용하죠.

예를 들어, 사용자의 설정이나 페이지 상태에 따라 뷰포트 메타 정보를 바꿔야 할 때 `generateViewport`를 작성할 수 있습니다.

```js
import type { Viewport } from 'next'

export function generateViewport(): Viewport {
  return {
    themeColor: 'blue',
    initialScale: 1,
    width: 'device-width',
  }
}
```

이렇게 하면서 `themeColor`, `initialScale`, `width` 같은 다양한 뷰포트 관련 설정을 추가할 수 있어요. 

---

### 뷰포트 옵션에 대해 조금 더!

뷰포트 설정에 들어가는 일반적인 필드를 몇 가지 소개할게요.

| 필드명         | 설명                                                      |
| -------------- | --------------------------------------------------------- |
| `width`        | 뷰포트의 너비를 지정해요. 예: 'device-width' 사용 가능.   |
| `initialScale` | 페이지 로드 시 초기 확대/축소 비율을 설정해요.             |
| `minimumScale` | 사용자가 최소 확대/축소할 수 있는 비율.                    |
| `maximumScale` | 사용자가 최대 확대/축소할 수 있는 비율.                    |
| `userScalable` | 사용자가 확대/축소 가능 여부 설정 ('yes' 또는 'no').       |
| `themeColor`   | 브라우저의 주소창 색상 등에서 사용할 테마 색상을 지정해요. |

이 필드들을 적절히 조합해서 모바일에서 최적의 UX를 제공할 수 있습니다!

---

요약하자면, Next.js에서 뷰포트를 설정할 땐 `export const viewport` 나 `generateViewport` 함수를 활용하는데, 동적 설정이 필요하다면 후자를, 고정 값만 필요하다면 전자를 쓰시면 돼요. 쉽죠? 필요한 옵션들을 잘 심어서 모바일 사용자에게 더 좋은 화면 경험을 선사해봅시다!

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

이 코드는 Next.js 같은 프레임워크에서 뷰포트(Viewport)를 동적으로 생성하는 함수예요.

```js
export function generateViewport({ params }) {
  return {
    themeColor: '...',
  }
}
```

여기서 `generateViewport` 함수는 뷰포트 설정을 runtime(실행 시점)에 맞춰서 동적으로 리턴해주는데요, 중요한 점은 다음과 같습니다.

> 참고!  
> 만약 뷰포트가 실행 시점과 상관없이 항상 똑같다면, 이 함수를 쓰지 말고 정적인(static) viewport 객체를 만들어서 쓰는 게 더 좋아요. 이렇게 하면 빌드 타임에 최적화가 가능하거든요.

---

## 뷰포트(Viewport) 필드 정리

### themeColor

- `themeColor`는 모바일 브라우저에서 사용자가 보는 브라우저 UI, 상태 바(status bar) 등의 색상을 정하는 필드입니다.
- 예를 들어, iOS Safari에서는 이 색상이 상단 상태 바 색상으로 적용되고, Android에서는 툴바 색상 등에 활용돼요.
- 색상을 지정하면 웹사이트의 아이덴티티를 좀 더 강하게 보여줄 수 있어서 사용자 경험(UX)을 향상시키는 데 도움 됩니다.

---

### 추가로 알려드리고 싶은 팁!

많은 개발자가 메타 태그(meta tag)를 통해 `theme-color`를 직접 넣어 관리하는데, Next.js 같은 최신 프레임워크에서는 이처럼 뷰포트 설정을 함수 또는 객체로 관리함으로써 좀 더 깔끔하고 일관된 방법으로 처리할 수 있어요.

또한, 다이나믹하게 `themeColor`를 바꿔야 한다면, URL 파라미터(`params`) 같은 runtime 데이터에 따라 조건을 걸어 다양한 색상을 반환하는 것도 가능하니 참고하세요!

---

필요하다면 나중에 `generateViewport` 이외의 다른 뷰포트 속성이나 관련 메타 태그 설정도 함께 정리해볼게요. 지금은 `themeColor`에 집중해서 말씀드렸고, 다음에는 `viewport`의 다른 필드들(예: width, initial-scale 등)도 다뤄보겠습니다!

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

theme-color에 대해 좀 더 쉽게 설명해볼게요.

---

## theme-color란?

웹사이트를 모바일 기기에서 열었을 때, 브라우저 상단 바(상태 표시줄)의 배경색을 설정할 수 있게 해주는 메타 태그예요. 즉, 사용자가 사이트를 더 깔끔하고 통일감 있게 느낄 수 있도록 도와주죠.

---

## 간단한 theme-color 사용법

1. Next.js 같은 프레임워크를 쓴다면, viewport 설정에 `themeColor`를 지정할 수 있어요.

```js
import type { Viewport } from 'next'

export const viewport: Viewport = {
  themeColor: 'black',
}
```

2. 혹은 HTML 문서에 메타 태그로 직접 넣어줄 수도 있습니다.

```html
<meta name="theme-color" content="black" />
```

---

## 알아두면 좋은 점

- 색상은 기본 CSS 색상 값이나 헥스 컬러코드(`#000000`) 모두 가능해요.
- 안드로이드 크롬에서 특히 잘 작동하며, iOS 사파리는 아직 완전 지원하지는 않습니다.
- `theme-color` 덕분에 사용자가 사이트를 북마크하거나 홈 화면에 추가했을 때도 바탕색이 설정된 느낌을 줄 수 있어요.

---

## 조금 더 활용해보기

브라우저나 다크 모드에 따라 다르게 색을 지정하고 싶다면, 미디어 쿼리를 사용해서 조건부로 `theme-color`를 설정할 수 있답니다.

예를 들어:

```html
<meta name="theme-color" content="#ffffff" media="(prefers-color-scheme: light)" />
<meta name="theme-color" content="#000000" media="(prefers-color-scheme: dark)" />
```

이렇게 하면 사용자의 다크 모드 여부에 따라 자동으로 테마 컬러가 변경돼서 훨씬 세련된 UX를 만들 수 있어요.

---

간단하지만 뜻밖에 사이트 인상에 큰 영향을 주는 `theme-color`! 놓치지 말고 꼭 활용해보세요 :)

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

이번에 살펴볼 내용은 Next.js에서 제공하는 `Viewport` 타입 예시인데요, 특히 테마 색상(theme-color)을 미디어 쿼리(media attribute)를 활용해서 조건부로 적용하는 방법이에요.

```js
import type { Viewport } from 'next'

export const viewport: Viewport = {
  themeColor: [
    { media: '(prefers-color-scheme: light)', color: 'cyan' },
    { media: '(prefers-color-scheme: dark)', color: 'black' },
  ],
}
```

이렇게 하면, 사용자가 밝은 모드(light mode)를 사용하면 테마 색상이 cyan, 어두운 모드(dark mode)를 사용하면 black으로 자동 설정됩니다.

실제 HTML에서는 이런 meta 태그로 변환돼요:

```html
<meta name="theme-color" media="(prefers-color-scheme: light)" content="cyan" />
<meta name="theme-color" media="(prefers-color-scheme: dark)" content="black" />
```

이 meta 태그의 `media` 속성은 브라우저의 미디어 쿼리 조건을 활용해서, 특정 조건일 때만 해당 테마 색상을 적용하게 해줍니다. 덕분에 사용자 환경에 따라 자동으로 색상이 바뀌어 UX가 좋아지죠.

---

### width, initialScale, maximumScale, userScalable

이번엔 뷰포트 설정에서 자주 쓰는 속성들에 대해 간단히 정리해볼게요.

| 속성명           | 설명                                                         |
|------------------|--------------------------------------------------------------|
| `width`          | 뷰포트의 너비를 설정합니다. 보통 `device-width`를 많이 씁니다.       |
| `initialScale`   | 초기 배율을 정합니다. 예: 1.0은 100% 크기라는 뜻이에요.           |
| `maximumScale`   | 사용자가 확대할 수 있는 최대 배율을 지정합니다.                   |
| `userScalable`   | 사용자가 확대/축소 가능 여부를 설정합니다. `yes` 또는 `no` 값 사용. |

예를 들면, 이렇게 쓸 수 있어요:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
```

- `width=device-width`: 뷰포트 너비를 디바이스 화면 너비에 맞춤
- `initial-scale=1.0`: 페이지 처음 로딩 시 확대/축소 안됨
- `maximum-scale=1.0`: 사용자가 확대할 수 있는 최대 크기를 1배로 제한
- `user-scalable=no`: 사용자가 화면 확대/축소를 못 하게 제한

요즘은 접근성 측면에서 `user-scalable=no`는 주의해서 써야 합니다. 모바일 사용자가 글자가 작다면 확대할 수 있는 게 좋거든요. 따라서 특별한 이유가 없다면 확대 제한을 두지 않는 걸 추천합니다.

---

이렇게 meta 태그에서 다양한 뷰포트 설정과 미디어 조건을 활용하면, 사용자 환경에 딱 맞는 페이지를 만드는 데 큰 도움이 돼요. Next.js 같은 modern 프레임워크에서도 타입으로 안전하게 관리할 수 있으니, 한번 적용해보시면 좋겠습니다!

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

알아두면 좋은 점: Next.js에서는 뷰포트(viewport) meta 태그가 자동으로 설정돼서 보통은 따로 만질 필요가 없어요. 기본 설정이 대부분의 경우에 딱 맞기 때문이죠. 그래도 완전한 정보를 위해 어떻게 설정할 수 있는지 예시를 알려드릴게요.

```js
import type { Viewport } from 'next'

export const viewport: Viewport = {
  width: 'device-width',
  initialScale: 1,
  maximumScale: 1,
  userScalable: false,
  // 좀 덜 자주 쓰이긴 하는데 아래 옵션도 지원돼요
  // interactiveWidget: 'resizes-visual',
}
```

그리고 HTML에서 직접 메타 태그로 작성하면 이렇게 생겼어요:

```html
<meta
  name="viewport"
  content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no"
/>
```

여기서 한 가지 팁을 더 드리자면, `user-scalable=no` 옵션은 사용자가 페이지를 핀치 줌(pinch zoom)으로 확대하는 것을 막아요. 이게 디자인을 고정하고 싶을 때는 좋은데, 접근성 측면에서는 주의가 필요해요. 사용자들이 글자 크기를 키워서 보는 걸 방해할 수 있으니까요.

---

### colorScheme

다음으로는 `colorScheme`에 대해 간단히 설명해볼게요. 웹사이트나 앱에서 다크 모드, 라이트 모드 같은 색상 테마를 지원할 때 사용하는 설정이에요. 예를 들어, 사용자의 OS 설정에 따라 자동으로 다크 모드가 적용되게 하거나, 특정 테마를 강제할 수 있죠.

Next.js나 React를 사용할 때는 보통 CSS 미디어쿼리 `prefers-color-scheme`을 활용하거나, 상태 관리로 테마를 바꾸는 방식으로 구현해요.

```css
@media (prefers-color-scheme: dark) {
  body {
    background-color: #121212;
    color: #ffffff;
  }
}

@media (prefers-color-scheme: light) {
  body {
    background-color: #ffffff;
    color: #000000;
  }
}
```

또한, Next.js 13부터는 내장된 `colorScheme` 타입이나 설정을 활용해 더 간편하게 테마를 관리하는 방법도 있답니다. 궁금하시면 더 알려드릴게요!

---

뷰포트 설정과 색상 테마, 요 두 가지는 요즘 프론트엔드 개발에서 사용자 경험을 좌우하는 중요한 부분이니 꼭 기억해두세요. 다른 궁금한 점 있으면 언제든 질문해 주세요!

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

color-scheme에 대해 더 알아보기

웹 개발하다 보면 사용자 기기의 테마(라이트 모드, 다크 모드)에 맞춰 UI를 자동으로 조정하고 싶을 때가 있죠? 이럴 때 유용한 게 바로 `color-scheme`입니다.

`color-scheme`는 브라우저나 OS가 현재 페이지의 색상 모드(라이트, 다크, 혹은 둘 다)를 알려주는 역할을 해요. 쉽게 말해, 당신의 사이트가 어떤 테마를 지원하는지 브라우저에게 알려주는 설정이라 볼 수 있어요.

예를 들어 Next.js 같은 프레임워크에서 `viewport` 타입 설정할 때 이렇게 사용할 수 있어요:

```js
import type { Viewport } from 'next'

export const viewport: Viewport = {
  colorScheme: 'dark',
}
```

그리고 HTML 메타태그로도 지정해줄 수 있답니다.

```html
<meta name="color-scheme" content="dark" />
```

이 메타 태그를 넣으면 브라우저가 다크 모드 환경에 맞춰 스크롤바나 기본 UI 요소의 색상을 조정해줍니다. 물론, 이건 사이트 내부 스타일에 영향을 주는 게 아니라 브라우저 레벨에서 UI를 맞춰주는 거라서 직접 CSS에서 따로 색상 조정을 해주는 것과는 차이가 있어요.

## color-scheme의 타입 종류

| 타입명      | 설명                                    |
|-------------|---------------------------------------|
| `normal`    | 기본적인 라이트 모드 (기본값)           |
| `dark`      | 다크 모드 지원                         |
| `light`     | 라이트 모드 지원                       |
| `only`      | 특정 모드만 사용 (예: 다크 모드만 허용) |

한 가지 팁! 다크 모드를 완벽히 지원하려면 `color-scheme` 설정뿐 아니라 `prefers-color-scheme` 미디어 쿼리도 같이 사용해서 사용자 테마에 따라 스타일을 다르게 적용해줘야 해요.

```css
@media (prefers-color-scheme: dark) {
  body {
    background-color: #121212;
    color: #ffffff;
  }
}
```

이렇게 하면 사용자의 OS 설정과 심리스하게 연동되는 사이트를 만들 수 있답니다. `color-scheme` 메타 태그는 주로 브라우저 기본 UI의 색상 조절을 돕는 역할이라고 생각하면 돼요!

그럼 오늘은 여기까지! 다음에 또 흥미로운 웹 개발 팁으로 찾아올게요 :)

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

이번 포스트에서는 Next.js에서 뷰포트(Viewport) 객체에 타입 안전성을 추가하는 방법에 대해 이야기해보려고 해요. 자바스크립트로 작성하면 타입에 대해 신경 쓰지 않아도 되지만, 타입스크립트를 쓴다면 명확하게 타입을 지정해주는 게 훨씬 안정적이고 실수도 줄일 수 있답니다.

### viewport 객체에 타입 지정하기

Next.js에서 뷰포트 객체를 만들 때 `Viewport` 타입을 사용하면, 해당 객체가 어떤 속성을 가져야 하는지 IDE가 바로 알려주기 때문에 실수를 미리 방지할 수 있어요. TypeScript를 지원하는 IDE를 쓴다면 타입을 명시하지 않아도 자동으로 감지해주지만, 명시적으로 적어두면 다른 개발자들이 코드를 이해할 때 더 쉽고 명확하답니다.

```ts
import type { Viewport } from 'next';

export const viewport: Viewport = {
  themeColor: 'black',
};
```

위 코드에서 `viewport` 객체는 `Viewport` 타입을 명시적으로 지정해줬어요. 이렇게 하면 `themeColor` 외에도 `Viewport` 타입에 정의되어 있는 다른 속성들을 자동 완성으로 안내받을 수 있답니다.

또한, 이렇게 타입을 선언하면 잘못된 타입을 넣었을 때 컴파일 타임에 바로 오류를 잡아줘서 버그를 줄이는 데 큰 도움이 돼요.

### 추가 팁: generateViewport 함수에 타입 적용하기

`Viewport` 타입은 뷰포트 정보를 다루는 함수, 예를 들어 `generateViewport` 같이 뷰포트 데이터를 생성하는 함수에도 적용할 수 있어요. 이렇게 하면 함수가 리턴하는 데이터가 항상 타입에 맞는지 보장할 수 있죠.

다음 포스트에서는 `generateViewport` 함수에서 어떻게 타입을 활용하는지 실제 예시를 들면서 말씀드릴게요~!

---

이처럼 Next.js와 TypeScript를 함께 쓰면, 뷰포트뿐만 아니라 다양한 객체에 타입을 명확하게 지정하여 안전한 코드를 작성할 수 있답니다. 앞으로도 더 많은 타입스크립트 활용법을 공유할 테니 기대해 주세요!

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

안녕하세요! 오늘은 `generateViewport`라는 함수를 살펴보면서, Next.js에서 Viewport를 어떻게 다루는지 간단히 알아볼게요. 코드도 같이 보면서 쉽게 이해해볼까요?

---

### 일반 함수 형식

먼저 가장 기본적인 형태부터 볼게요.

```js
import type { Viewport } from 'next'
 
export function generateViewport(): Viewport {
  return {
    themeColor: 'black',
  }
}
```

여기선 `generateViewport`라는 함수를 정의했고, Next.js에서 타입을 가져와 `Viewport` 타입으로 리턴을 명확히 해줬어요. 실제로 리턴되는 값은 `themeColor: 'black'` 하나뿐인데요, 이처럼 간단하게 뷰포트 관련 설정을 만들 수 있습니다.

---

### 세그먼트 Props를 받는 경우

이번엔 Next.js의 동적 라우팅에서 자주 보는 segment props를 매개변수로 사용하는 버전이에요.

```ts
import type { Viewport } from 'next'
 
type Props = {
  params: Promise<{ id: string }>
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>
}
 
export function generateViewport({ params, searchParams }: Props): Viewport {
  return {
    themeColor: 'black',
  }
}
 
export default function Page({ params, searchParams }: Props) {}
```

| 내용 | 설명 |
|-------|------------|
| `params` | URL 경로에 포함된 변수예요. 예를 들어 `/posts/123`에서 `id`가 `123`이 됩니다. |
| `searchParams` | URL의 쿼리스트링, 예를 들어 `?sort=asc`와 같은 부분을 의미해요. |

여기서 조금 신기한 점은 `params`와 `searchParams`가 `Promise` 타입이라는 거예요. 보통은 동기적으로 바로 값을 받는데, 비동기적으로 처리할 수도 있게 설계한 거죠. 미래에 API 호출이나 파일 읽기 같은 비동기 작업과 연동하기에 유용해요.

---

### 추가 팁!

- `generateViewport` 함수는 페이지 렌더링 시 뷰포트 설정을 동적으로 제어할 수 있게 해줘서, 다양한 테마를 적용하거나 사용자에 맞춘 UI 조절에 쓰기 좋아요.
- 만약 `searchParams`가 배열 형태일 수도 있기 때문에 타입에 그런 부분도 반영해두었는데, 실제 활용하면서 쿼리 파라미터에 따라 조건화를 할 때 유용합니다.
- Next.js 13버전 이상의 앱 디렉터리 구조를 사용한다면, 이런 타입과 함수 구조가 점점 더 중요해지고 있으니 익혀두면 좋아요.

이처럼 기본부터 비동기 props까지, `generateViewport` 함수가 어떻게 활용될 수 있는지 감이 잡히셨나요? 다음에 더 재밌는 주제로 또 찾아올게요! 🙌

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

#### JavaScript 프로젝트에서 JSDoc으로 타입 안정성 높이기

JavaScript로 프로젝트를 하다 보면, 타입 안정성이 아쉬울 때가 많죠. 이럴 때 TypeScript로 전환하는 것도 방법이지만, 규모가 크지 않거나 빠르게 작업할 때는 조금 부담스러울 수 있어요. 그래서 추천하는 것이 바로 **JSDoc**을 활용하는 방법입니다.

예를 들어, Next.js 프로젝트에서 특정 객체에 타입을 지정하고 싶은 경우, 이렇게 작성할 수 있어요:

```js
/** @type {import("next").Viewport} */
export const viewport = {
  themeColor: 'black',
}
```

여기서 `@type` 태그를 사용하여 `next` 패키지에서 제공하는 `Viewport` 타입을 참조한 거죠. 이걸 하면 에디터에서 자동완성도 되고, 타입에 맞지 않는 값을 넣으면 경고가 뜨기 때문에 실수 확률을 줄일 수 있어요.

### 이 방법의 장점과 팁

- **TypeScript 전환 없이도 타입 안정성 얻기**: 기존 자바스크립트 코드를 크게 바꾸지 않고도 타입 체크 기능을 누릴 수 있어요.
- **에디터 지원**: VSCode 같은 에디터에서 JSDoc 주석을 분석해 자동완성, 리팩토링 도움 기능을 제공합니다.
- **공식 타입 활용 가능**: 외부 라이브러리나 프레임워크의 타입 정의를 그대로 참고할 수 있어서, 타입 정의를 새로 만들 필요가 없어요.

만약 좀 더 큰 프로젝트라면, JSDoc과 함께 **TypeScript로 점진적 전환**을 고려해 보는 것도 좋아요. 처음엔 JSDoc으로 조금씩 타입을 적용하고, 필요에 따라 `.ts` 혹은 `.tsx` 파일을 만들어 나가는 전략입니다.

---

## 버전 히스토리 (Version History)

이 부분은 주로 프로젝트의 변경 이력을 기록하는 공간인데요, 아래와 같은 형식으로 정리해 두면 좋아요.

| 날짜         | 버전   | 변경 내용                             |
|--------------|--------|-------------------------------------|
| 2024-06-01   | 1.0.0  | 프로젝트 초기 버전 배포              |
| 2024-06-15   | 1.1.0  | JSDoc 타입 주석 추가 및 코드 정리   |

시간이 지나면서 어떤 기능이 추가됐고, 어떤 버전을 사용해야 하는지 명확해져서 협업에도 도움이 됩니다. 프로젝트 초반에 간단히라도 버전 히스토리 관리하는 습관, 꼭 추천드려요!

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

| Version   | Changes                                  |
|-----------|------------------------------------------|
| `v14.0.0` | `viewport`와 `generateViewport`가 추가됨 |

여기서 `viewport`와 `generateViewport`는 웹 개발할 때 화면 크기나 뷰포트 관련 작업을 할 때 엄청 유용한 기능이에요. 특히 반응형 디자인을 구현할 때 이 두가지를 잘 활용하면 더 깔끔하고 효율적으로 화면 크기를 조절할 수 있답니다. 다음번에 이 기능들에 대해 더 자세히 파헤쳐볼게요!