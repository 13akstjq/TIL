---
title: "2025년 ReactJS와 Next.js에서 CSS를 가장 쉽게 사용하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:08
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to use CSS in your application"
link: "https://nextjs.org/docs/app/getting-started/css"
isUpdated: false
---


# 애플리케이션에서 CSS 사용법 알아보기

Next.js에서는 CSS를 사용하는 여러 가지 방법을 제공하는데요, 대표적으로 다음과 같은 방식들이 있어요:

- CSS 모듈(CSS Modules)
- 글로벌 CSS(Global CSS)
- Tailwind CSS
- Sass
- CSS-in-JS
- 외부 스타일시트(External Stylesheets)

이 글에서는 각각의 방법을 어떻게 사용하는지 차근차근 알려드릴게요. 앞으로 여러분 프로젝트에 어떤 방식이 가장 잘 맞을지 선택하는 데 도움이 되길 바랍니다!  

다양한 스타일링 옵션을 활용하면 코드 관리도 쉽고, 유지보수도 편해지니까 꼭 익혀두세요.

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

## CSS Modules

CSS Modules는 CSS 클래스 이름을 고유하게 만들어서, 같은 이름의 클래스를 여러 파일에서 마음껏 사용할 수 있게 도와주는 방법이에요. 덕분에 클래스 이름 충돌 걱정 없이 스타일을 관리할 수 있죠.

사용 방법도 간단해요! 스타일 파일을 `.module.css` 확장자로 만들고, 이걸 필요한 컴포넌트에서 import하기만 하면 돼요. 예를 들어:

```css
/* blog.module.css */
.blog {
  padding: 24px;
}
```

그리고 컴포넌트에서는 이렇게 불러와서 사용하면 돼요:

```jsx
import styles from './blog.module.css';

function Blog() {
  return <div className={styles.blog}>안녕하세요, 블로그입니다!</div>;
}
```

여기서 중요한 점! 일반 CSS 파일과는 달리, CSS Modules에서는 클래스를 사용할 때 항상 import한 `styles` 객체를 통해 접근해 주어야 해요. 이렇게 하면 자동으로 클래스 이름이 고유한 문자열로 변환돼서, 다른 컴포넌트의 같은 클래스 이름과 충돌하지 않도록 막아줍니다.

그리고 혹시 CSS Modules가 조금 낯설게 느껴질 수도 있지만, 최근에는 React를 비롯한 여러 프론트엔드 프레임워크에서 기본 지원하거나 쉽게 설정할 수 있어서 많이 쓰이는 추세에요. 특히 프로젝트가 커질수록 스타일 관리가 훨씬 편해지니, 개인 프로젝트나 회사 프로젝트 모두에서 강력 추천합니다!

덧붙여서, CSS Modules는 전역 네임스페이스 오염 없이 컴포넌트 단위 스타일링이 가능해서, 스타일을 쓰면서 발생하는 버그도 줄여준답니다. 다른 방법으로는 styled-components 같은 CSS-in-JS도 있으니, 용도와 선호도에 따라 선택하면 좋아요!

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
import styles from './styles.module.css'

export default function Page({ children }: { children: React.ReactNode }) {
  return <main className={styles.blog}>{children}</main>
}
```

## 글로벌 CSS(Global CSS)

애플리케이션 전체에 스타일을 적용하고 싶을 때는 글로벌 CSS를 사용하면 편리해요.

예를 들어, `app/global.css` 파일을 만들어서 스타일을 작성하고, 이 파일을 루트 레이아웃(root layout) 파일에 import 하면 애플리케이션의 모든 경로(route)에 글로벌 스타일이 적용돼요.

```css
/* app/global.css 예시 */
body {
  margin: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f9f9f9;
  color: #333;
}
```

그리고 루트 레이아웃 파일에서는 이렇게 import 해주세요.

```js
import './global.css';

export default function RootLayout({ children }) {
  return (
    <html>
      <body>{children}</body>
    </html>
  );
}
```

> 참고로, 글로벌 CSS는 보통 전체적인 기본 스타일(Reset, 폰트, 배경 등)을 적용할 때 사용하고, 각 컴포넌트별 스타일은 CSS 모듈이나 styled-components 같은 방식을 병행해 쓰는 게 유지보수에 좋아요.

추가로 Next.js에서는 글로벌 CSS가 한 번만 import 되어야 하니, 루트 레이아웃이나 앱 컴포넌트에 넣는 걸 추천합니다!

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

이번에는 Next.js에서 글로벌 스타일과 Tailwind CSS에 대해 간단히 살펴볼게요.

우선, CSS에서 `body` 태그에 아래와 같은 스타일을 적용한 예시가 있어요:

```css
body {
  padding: 20px 20px 60px;
  max-width: 680px;
  margin: 0 auto;
}
```

이 스타일은 `body`에 패딩을 줘서 콘텐츠 주변에 공간을 만들고, 최대 너비를 680px로 제한해주고, 좌우 여백은 자동으로 가운데 정렬하는 역할을 하죠.

그리고 Next.js 앱에서는 이렇게 글로벌 CSS를 가져와서 모든 페이지에 적용할 수 있어요:

```tsx
// 이 파일은 루트 레이아웃 컴포넌트입니다.
// 전체 앱에 글로벌 스타일이 적용되도록 설정해요.
import './global.css'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

여기서 `import './global.css'` 부분이 바로 앱 전체에 영향을 주는 글로벌 스타일을 불러오는 부분입니다.

### 알아두면 좋은 점
Next.js는 React의 스타일 시트 지원 기능과 Suspense를 활용해서 스타일 관리를 하는데요, 문제가 되는 부분은 "라우트 간 이동 시 스타일 시트가 자동으로 제거되지 않아 충돌(conflicts)이 날 수 있다"는 거예요.

그래서 권장하는 방법은,

- 정말 앱 전체에 적용되어야 하는 스타일만 글로벌 CSS로 작성하세요.
- 페이지별, 컴포넌트별로 독립된 스타일이 필요하다면 CSS 모듈(CSS Modules)을 사용하세요.

이렇게 하면 스타일 충돌을 막을 수 있고 관리도 더 쉬워집니다.

---

## Tailwind CSS 이야기

그럼 Tailwind CSS는 어떻게 Next.js와 쓰일까요? Tailwind CSS는 유틸리티 클래스 기반 CSS 프레임워크로, 복잡한 CSS를 손쉽게 작성할 수 있게 도와줘요.

예를 들어, 위에서 했던 `body` 스타일을 Tailwind로 표현하고 싶다면, 다음과 같이 할 수 있죠:

```tsx
<body className="p-5 pb-15 max-w-xl mx-auto">
  {children}
</body>
```

- `p-5`는 padding 20px (Tailwind 기본 spacing scale에서 대략 1.25rem) 정도를 줍니다.
- `pb-15`은 padding-bottom 60px 정도를 줘요.
- `max-w-xl` 은 최대 너비 약 680px에 해당합니다.
- `mx-auto`는 좌우 마진 자동으로 가운데 정렬을 의미합니다.

Tailwind를 활용하면 CSS 파일을 따로 관리하지 않아도 되고, 클래스만 잘 조합해서 손쉽게 반응형 및 스타일 변화를 줄 수 있죠.

---

### 참고
Tailwind CSS도 Next.js에서 쉽게 통합할 수 있으니 관심 있으면 더 자세히 알려드릴게요!  
그리고 CSS Modules와 같이 쓰면 좀 더 컴포넌트 단위로 깔끔한 스타일링도 가능합니다.

---

요약하자면,

| 내용                      | 설명                                        |
|-------------------------|-------------------------------------------|
| 글로벌 CSS              | 앱 전체에 적용되는 기본 스타일, 충돌 주의 필요          |
| CSS Modules             | 컴포넌트별 국소 스타일링, 충돌 적음                   |
| Tailwind CSS            | 유틸리티 클래스 기반, 빠른 스타일링 가능               |

필요에 따라 적절한 방법을 골라서 쓰면 되겠습니다!

다음에는 Tailwind CSS 설정법과 활용 팁을 공유할게요. 질문 있으면 부담 없이 알려주세요!

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

Tailwind CSS는 Next.js와 찰떡같이 잘 어울리는 유틸리티 퍼스트(Utility-First) CSS 프레임워크에요. 쉽게 말해, 미리 만들어진 클래스를 조합해서 빠르고 효율적으로 스타일을 입힐 수 있죠.

### Tailwind 설치하기

Tailwind를 바로 써보고 싶다면, 아래 명령어로 필요한 패키지들을 설치하세요:

```bash
npm install tailwindcss @tailwindcss/postcss postcss
```

여기서 `tailwindcss`는 본체이고, `@tailwindcss/postcss`랑 `postcss`는 Tailwind 스타일을 빌드할 때 필요한 도구들이에요. Next.js 프로젝트에서 스타일링할 때 필수라고 생각하면 됩니다.

참고로, 요즘 Next.js 최신 버전에서는 위 패키지 설치 후에 `npx tailwindcss init`으로 설정 파일을 생성해서 Tailwind 설정을 좀 더 커스터마이징할 수 있어요. 스타일을 내가 원하는 대로 바꾸고 싶다면 꼭 해보세요!

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

### Tailwind 설정하기

프로젝트 루트에 `postcss.config.mjs` 파일을 만들어서 PostCSS 설정에 `@tailwindcss/postcss` 플러그인을 추가해보세요! 간단한 설정 하나로 Tailwind CSS의 모든 유틸리티 클래스를 마음껏 사용할 수 있게 됩니다.

예를 들면, `postcss.config.mjs` 파일에 아래처럼 설정을 추가할 수 있어요:

```js
/** @type {import('tailwindcss').Config} */
export default {
  plugins: {
    '@tailwindcss/postcss': {},
  },
}
```

> 참고로 Tailwind 설정 파일은 `tailwind.config.js` 혹은 `tailwind.config.mjs`로 따로 만들어져 있어야 합니다. 만약 아직 없다면 기본 설정부터 만들어 놓는 걸 추천드려요. 잘 설정해두면 테마, 색상, 폰트 등 자유롭게 커스터마이징하기도 훨씬 수월합니다.

### Tailwind 사용하기

Tailwind를 프로젝트에 제대로 설정했다면, HTML이나 JSX 파일에서 바로 클래스명을 쓰기만 하면 돼요!

```html
<button class="bg-blue-500 text-white font-bold py-2 px-4 rounded">
  클릭해봐요!
</button>
```

이렇게 쓰면 버튼에 파란 배경, 하얀 글자, 굵은 글꼴, 약간의 패딩과 둥근 모서리가 한 번에 적용됩니다. 기존 CSS 작성하느라 헷갈릴 필요 없이, 클래스 이름으로 직관적인 스타일링 가능!

> 팁 하나 더! 개발 중에 변경사항이 잘 반영되지 않으면, PostCSS 빌드 프로세스를 재시작하거나 캐시를 지우는 것도 한번 시도해보세요. Tailwind는 JIT(Just-In-Time) 컴파일 기능 덕분에 빠르게 스타일을 적용할 수 있지만 간혹 환경마다 캐시 문제로 반영이 늦을 수 있거든요.

필요한 만큼 쉽고 빠르게 스타일을 적용할 수 있으니 꼭 한번 써보세요! Tailwind는 커뮤니티도 활발해서 다양한 플러그인과 자료들이 많답니다. 앞으로도 더 재미있는 Tailwind 팁들 공유할게요!

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

Tailwind CSS를 Next.js 프로젝트에 적용하려면, 먼저 글로벌 스타일시트에 Tailwind 지시어를 추가해야 해요. 이렇게 하면 Tailwind의 기본 스타일과 유틸리티 클래스들이 전역에서 적용됩니다.

```css
@import 'tailwindcss';
```

위 코드를 `globals.css` 같은 전역 스타일 파일에 넣어주세요.

그리고 Next.js 프로젝트의 최상위 레이아웃 파일에서 이 스타일시트를 불러와야 해요. 보통 `app/layout.tsx`나 `app/layout.js`에 아래처럼 `globals.css`를 임포트합니다:

```tsx
import type { Metadata } from 'next';
// 이 스타일들은 애플리케이션의 모든 라우트에 적용됩니다.
import './globals.css';

export const metadata: Metadata = {
  title: 'Create Next App',
  description: 'Generated by create next app',
};

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

---

### 조금 더 알아보기
- 이 `@import 'tailwindcss';`는 Tailwind가 필요한 기본 스타일과 유틸리티 클래스를 불러오는 역할을 해요.
- 만약 더 세부적으로 `@tailwind base;`, `@tailwind components;`, `@tailwind utilities;` 같은 지시어를 커스텀 스타일시트에 넣는 경우도 있는데, 기본적으로 `@import 'tailwindcss';` 하나로 필요한 부분을 다 가져와요.
- 그리고 글로벌 스타일을 한 번 적용하면, 각 페이지 컴포넌트에서는 따로 스타일을 임포트할 필요가 없어서 편리하답니다.
- Tailwind를 제대로 적용했는지 확인하려면, 애플리케이션을 실행 후 개발자 도구에서 클래스들이 잘 붙는지 보고, Tailwind 유틸리티 클래스를 적용해 스타일이 변하는지 테스트해보세요!

이제 Tailwind 스타일이 Next.js 프로젝트 전체에 적용됐으니, 마음껏 유틸리티 클래스를 활용해서 빠르게 스타일링해 보세요!

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

그럼 이제부터 여러분이 개발하는 애플리케이션에 Tailwind의 유틸리티 클래스를 본격적으로 적용할 수 있어요.

```js
export default function Page() {
  return <h1 className="text-3xl font-bold underline">Hello, Next.js!</h1>
}
```

위 예제처럼 클래스 이름만 쭉 써주면 스타일이 깔끔하게 적용되니 정말 편리하답니다.

---

## Sass 사용하기

Next.js는 Sass와 아주 잘 통합돼 있어서 `.scss`, `.sass` 확장자 둘 다 자유롭게 사용할 수 있어요. 

Sass는 CSS를 더 효율적이고 구조적으로 관리할 수 있게 해주는 CSS 전처리기인데요, 변수, 중첩, 믹스인 같은 강력한 기능 덕분에 코드 작성이 훨씬 편해진답니다.

Next.js 프로젝트에서 Sass를 사용하려면 일단 `sass` 패키지를 설치해주고 (`npm install sass`), 그다음에 `.scss`나 `.sass` 파일을 임포트해서 바로 스타일을 적용할 수 있어요.

예를 들어,

```scss
// styles.module.scss
$primary-color: #4f46e5;

.title {
  color: $primary-color;
  font-weight: bold;
}
```

```js
import styles from './styles.module.scss';

export default function Page() {
  return <h1 className={styles.title}>Hello, Next.js with Sass!</h1>;
}
```

이렇게 작성하면 Sass가 제공하는 변수 기능도 자연스럽게 쓸 수 있답니다.

---

> 참고로, Sass를 사용하면 CSS 유지보수가 더 쉬워지고, 규모가 큰 프로젝트에서는 특히 효과적이에요. 만약 Tailwind와 Sass 둘 다 같이 쓴다면, Tailwind로 기본 스타일을 빠르게 구성하고, Sass로 좀 더 복잡한 커스텀 스타일을 관리하는 식으로 활용할 수도 있겠죠!

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

Sass를 컴포넌트 단위로도 사용할 수 있어요. CSS Modules와 함께 `.module.scss`나 `.module.sass` 확장자를 사용하면 되죠. 덕분에 스타일의 범위를 컴포넌트 내부로 한정할 수 있어서, 다른 부분에 영향 주지 않고 스타일을 관리하기 정말 편해집니다.

### Sass 설치 방법

Sass를 프로젝트에서 바로 사용하려면 `sass` 패키지를 설치해줘야 해요. 터미널에 아래 명령어만 입력하면 끝!

```bash
npm install --save-dev sass
```

이렇게 하면 `node-sass` 대신 더 빠르고 최신 버전인 `sass`가 설치됩니다. 요즘은 대부분 이 패키지를 추천하고 있으니 꼭 참고하세요!

그리고 Sass를 쓰다 보면 `.scss`와 `.sass` 확장자의 차이가 궁금할 수도 있는데, 간단히 말하면 `.scss`는 CSS 문법과 거의 비슷해서 배우기 쉽고, `.sass`는 들여쓰기를 기반으로 한 문법이에요. 보통 `.scss`를 더 많이 사용한답니다.

만약 Create React App 같은 환경에선 위 설치만 해도 바로 Sass를 쓸 준비가 된 거예요! 컴포넌트별 Sass를 적용하려면 스타일 파일 이름을 `ComponentName.module.scss`처럼 지으면 되고, 이렇게 하면 CSS가 자동으로 모듈화돼서 충돌 없이 안정적입니다.

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

### Sass 옵션 커스터마이징하기

Next.js에서 Sass 옵션을 설정하고 싶을 때는 `next.config.js` 파일 내에 `sassOptions`를 사용하면 돼요. 예를 들어, 모든 Sass 파일에 공통으로 사용할 변수를 선언하고 싶다면 `additionalData` 옵션을 활용할 수 있어요.

```js
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  sassOptions: {
    additionalData: `$var: red;`,
  },
}

export default nextConfig
```

위 예제에서는 Sass 변수 `$var`에 빨간색(`red`) 값을 기본으로 넣어두었어요. 이렇게 설정하면 모든 Sass 파일에서 별도의 선언 없이 `$var` 변수를 바로 사용할 수 있답니다. 덕분에 공통 변수나 믹스인 등을 간편하게 적용할 수 있어요.

> 팁!  
> `additionalData`는 파일마다 자동으로 추가되는 코드라서, 여러 변수나 스타일 믹스인들을 등록하면 훨씬 효율적으로 Sass를 관리할 수 있어요.  
> 예를 들어, 공통 색상 팔레트나 폰트 스타일을 미리 정의해두는 식이죠.

---

## CSS-in-JS

(여기서부터 CSS-in-JS에 대해 다룰 예정인데, 추가로 궁금한 부분이 있으면 말씀해 주세요!)

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

React Server Components(RSC)를 사용하다 보면 한 가지 주의할 점이 있어요. 바로 **런타임 시 자바스크립트가 필요한 CSS-in-JS 라이브러리들은 현재 React Server Components에서 지원되지 않는다**는 거죠.

### 왜 이런 제한이 생길까?
React Server Components는 서버에서 미리 렌더링하고 클라이언트에서는 최소한의 자바스크립트만 실행하는 방식을 취하기 때문에,
런타임에 자바스크립트가 꼭 필요한 스타일링 라이브러리들은 당장 호환되기 힘든 상황이에요.

### 그렇다면 어떤 CSS-in-JS 라이브러리는 지금 앱 디렉토리의 Client Components에서 쓸 수 있을까?
아래는 현재 지원되는 라이브러리 목록입니다. (알파벳순)

| 지원 라이브러리              |
|-----------------------|
| ant-design            |
| chakra-ui             |
| @fluentui/react-components |
| kuma-ui               |
| @mui/material         |
| @mui/joy              |
| pandacss              |
| styled-jsx            |
| styled-components     |
| stylex                |
| tamagui               |
| tss-react             |
| vanilla-extract       |

### 지원 관련 고민 중인 라이브러리도 있다!
현재 React 최신 버전에 맞춰 Server Components와 Streaming 지원을 준비 중인 라이브러리들도 있으니, 앞으로 더 확대될 가능성이 높습니다.

### 추가 팁
- 만약 CSS-in-JS를 꼭 써야 한다면, Server Component가 아닌 Client Component에서만 해당 라이브러리를 사용하도록 분리하는 방법을 추천해요.
- 스타일링을 아예 CSS 모듈 또는 vanilla CSS로 관리하거나, `styled-jsx`같이 별도 런타임이 필요 없는 라이트한 스타일러를 고려하는 것도 좋아요.

---

React 개발하면서 최신 기능들을 써보고 싶은데 스타일링 라이브러리 때문에 고민이라면,
이 리스트와 팁을 참고해서 내 프로젝트에 맞게 잘 조합해보시길 바라요!

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

- emotion

서버 컴포넌트(Server Components)에 스타일을 입히고 싶다면, CSS 파일을 생성하는 CSS Modules나 Tailwind CSS 같은 솔루션을 사용하는 것을 추천해요. 이 방법들이 서버 컴포넌트와 잘 맞고, 성능 측면에서도 유리하답니다.

### CSS-in-JS 설정하기

만약 CSS-in-JS를 사용하고 싶다면, 다음과 같은 설정이 필요해요: 

(여기서는 emotion 같은 CSS-in-JS 라이브러리를 예로 들 수 있겠죠.)

추가로, CSS-in-JS를 사용할 때는 클라이언트 컴포넌트 내에서 스타일 적용이 이뤄지는 경우가 많아서, 서버 컴포넌트에서는 권장되지 않는다는 점을 기억하세요. 상황에 따라 적절한 방식을 선택하는 게 중요해요!

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

style-jsx를 Next.js 같은 React 환경에서 사용할 때 스타일을 깔끔하게 적용하고 수집하는 방법에 대해 알려드릴게요. 보통 SSR(서버 사이드 렌더링)을 하게 되면 스타일이 제대로 적용되는 시점이 중요하거든요. 그래서 스타일을 한 군데 모아 관리하고, 렌더 전에 HTML에 스타일을 주입하는 코드를 작성합니다.

아래는 주요 포인트와 예시 코드입니다.

---

### 스타일 레지스트리 만들기

- `createStyleRegistry()`를 이용해 스타일을 모으는 레지스트리를 만듭니다.
- 컴포넌트가 처음 렌더될 때 한 번만 생성되도록 `useState`의 lazy initializer 패턴을 씁니다.

### 스타일을 HTML에 주입하기

- `useServerInsertedHTML` 훅을 사용해서 서버에서 렌더링 직전에 스타일 태그를 HTML에 넣어줍니다.
- 이러면 나중에 페이지가 로드될 때 스타일이 깜빡이거나 지연 없이 바로 적용돼요.

### 클라이언트 컴포넌트에서 래핑하기

- 앱의 최상위 컴포넌트를 스타일 레지스트리로 감싸서 SSR 초기 렌더링할 때 스타일 관리를 집중시킵니다.

---

### Styled-jsx 적용 예시

```jsx
'use client'

import React, { useState } from 'react'
import { useServerInsertedHTML } from 'next/navigation'
import { StyleRegistry, createStyleRegistry } from 'styled-jsx'

export default function StyledJsxRegistry({
  children,
}: {
  children: React.ReactNode
}) {
  // 스타일시트를 한 번만 생성 (lazy init)
  const [jsxStyleRegistry] = useState(() => createStyleRegistry())

  // 서버 렌더링 시 스타일 태그를 먼저 삽입
  useServerInsertedHTML(() => {
    const styles = jsxStyleRegistry.styles()
    jsxStyleRegistry.flush()
    return <>{styles}</>
  })

  // 스타일 등록기를 통해 자식 요소들 감싸기
  return <StyleRegistry registry={jsxStyleRegistry}>{children}</StyleRegistry>
}
```

---

### 부가 팁!

- `StyleRegistry`는 스타일을 수집하고 렌더링하는 역할을 해주기 때문에, 이 컴포넌트로 앱 전체를 감싼다면 스타일 충돌도 관리할 수 있어요.
- `flush()` 메서드는 수집한 스타일을 비워서 중복 삽입을 막아줍니다.
- 만약 styled-jsx 외에 Emotion, Styled-components 등 다른 CSS-in-JS 라이브러리를 쓴다면 각각의 서버 스타일 주입 방법이 있으니 참고하세요.

이렇게 하면 서버에서 스타일이 빠르게 적용되어 사용자 경험이 훨씬 좋아집니다! 한 번 적용해 보시면 렌더링 과정에서 스타일이 정확하고 깔끔하게 적용되는 걸 눈으로 확인하실 수 있을 거예요.

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

자, 이제 root layout을 registry로 감싸는 방법을 알려드릴게요.

```js
import StyledJsxRegistry from './registry'
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html>
      <body>
        <StyledJsxRegistry>{children}</StyledJsxRegistry>
      </body>
    </html>
  )
}
```

이렇게 하면 스타일 관련 문제를 좀 더 깔끔하게 관리할 수 있어요. 특히 여러 컴포넌트에서 스타일이 꼬일 때 유용하죠.

---

### styled-components 사용법

만약 Next.js 프로젝트에서 **styled-components**를 사용하려면, `next.config.js` 파일에서 별도로 활성화해야 해요. 다음과 같이 설정해 주세요:

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  // 기타 설정들...

  compiler: {
    styledComponents: true,
  },
}

module.exports = nextConfig
```

이 설정을 하면 styled-components가 Next.js 컴파일러와 잘 연동되어 SSR (서버사이드 렌더링) 시 스타일이 깨지지 않도록 도와줘요.

---

#### 참고 팁!

- `styled-components` 쓸 때는 Babel 설정도 필요할 수 있는데, Next.js 12 이상이면 위 `compiler` 옵션만으로 대부분 해결돼요.
- `StyledJsxRegistry` 같은 커스텀 레지스트리를 사용하는 이유는 스타일 충돌을 방지하고, 렌더링 최적화를 위해서입니다.
- 스타일 관리는 React+Next.js에서 꽤 복잡할 수 있지만, 이런 설정들을 적용하면 훨씬 안정적인 UI를 만들 수 있으니 꼭 적용해 보세요!

궁금한 점 있으면 언제든 물어보세요~

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

Next.js에서 `styled-components`를 잘 사용하려면, 기본 설정과 함께 서버 사이드에서 스타일을 제대로 수집하고 HTML `<head>`에 주입하는 작업이 필요해요. 간단히 말해서, SSR 환경에서 스타일이 누락되지 않도록 조치를 취하는 거죠.

### 1. `next.config.js` 설정

일단 Next.js 설정 파일에서 `styledComponents` 플래그를 `true`로 켜줘야 해요. 그렇지 않으면 컴파일러가 스타일드 컴포넌트 문법을 제대로 처리하지 못해요.

```js
import type { NextConfig } from 'next';

const nextConfig: NextConfig = {
  compiler: {
    styledComponents: true,
  },
};

export default nextConfig;
```

이 작업은 CSS 클래스 이름 충돌을 방지하고, 개발 편의성을 높여줘요!

---

### 2. 서버에서 스타일 수집하기: `StyledComponentsRegistry` 컴포넌트

서버 측 렌더링(SSR) 시 모든 스타일 규칙을 모아서 나중에 `<head>` 태그에 삽입해야 해요. 이걸 위해 `ServerStyleSheet`를 사용해서 스타일을 수집하고, `useServerInsertedHTML` 훅으로 클라이언트에 주입합니다.

```js
'use client';

import React, { useState } from 'react';
import { useServerInsertedHTML } from 'next/navigation';
import { ServerStyleSheet, StyleSheetManager } from 'styled-components';

export default function StyledComponentsRegistry({
  children,
}: {
  children: React.ReactNode;
}) {
  // 스타일시트는 한 번만 생성하도록 lazy 초기화 사용
  const [styledComponentsStyleSheet] = useState(() => new ServerStyleSheet());

  useServerInsertedHTML(() => {
    const styles = styledComponentsStyleSheet.getStyleElement();
    styledComponentsStyleSheet.instance.clearTag(); // 기존 태그 초기화
    return <>{styles}</>;
  });

  // 클라이언트에서는 그냥 children만 리턴
  if (typeof window !== 'undefined') return <>{children}</>;

  // 서버에서는 StyleSheetManager로 감싸서 스타일 수집
  return (
    <StyleSheetManager sheet={styledComponentsStyleSheet.instance}>
      {children}
    </StyleSheetManager>
  );
}
```

#### 핵심 포인트

- `ServerStyleSheet`로 렌더링된 스타일을 묶어서 관리해요.
- `useServerInsertedHTML` 훅은 서버 호출 시 스타일 태그를 `<head>` 안에 넣어 줍니다.
- 클라이언트에서는 스타일 수집할 필요가 없으니 그냥 children을 렌더링하죠.
- 서버에서는 `StyleSheetManager`로 감싸서 스타일링을 추적합니다.

이 구조 덕분에 FOUC(Flash of Unstyled Content, 스타일링 없는 깜빡임)를 없앨 수 있고, SEO와 UX 품질도 깔끔해져요!

---

### 3. 루트 레이아웃에 적용하기

마지막으로 `app/layout.tsx` 같은 루트 레이아웃 파일에서, 기존 children을 `StyledComponentsRegistry`로 감싸주세요.

```tsx
import StyledComponentsRegistry from './StyledComponentsRegistry';

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="ko">
      <head>{/* 여기에 다른 head 요소들 */}</head>
      <body>
        <StyledComponentsRegistry>
          {children}
        </StyledComponentsRegistry>
      </body>
    </html>
  );
}
```

---

## 정리

| 단계                   | 설명                                                                                |
|------------------------|-----------------------------------------------------------------------------------|
| 1. next.config.js 설정  | `compiler.styledComponents: true`를 켜서 styled-components 컴파일러를 활성화            |
| 2. StyledComponentsRegistry 작성 | 서버 스타일 수집용 컴포넌트 작성. `ServerStyleSheet`, `useServerInsertedHTML` 활용  |
| 3. 루트 레이아웃 감싸기  | 레이아웃에서 모든 children을 위 컴포넌트로 감싸서 SSR 시 스타일 누락 방지                |

---

### 참고 팁

- `styled-components` 이전 버전에서는 `_document.js`에서 SSR 설정을 하곤 했는데, Next.js 13의 App Router에서는 `useServerInsertedHTML` 훅으로 훨씬 더 간편하게 적용할 수 있어요.
- `ServerStyleSheet` 인스턴스는 꼭 한 번만 생성하고 재사용해야 렌더링 시 메모리 누수를 방지합니다.
- 개발 중에 styles가 이상하게 보이면 캐시를 삭제하고 다시 빌드해 보는 것도 좋아요.

---

이 방법으로 프로젝트에서 `styled-components`를 깔끔하게 SSR 환경에 맞게 쓸 수 있답니다! 혹시 더 궁금한 점 있으면 언제든 물어봐 주세요~ 🚀

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

이번 글에서는 React 컴포넌트에서 외부 스타일시트를 다루는 방법과 styled-components를 사용하는 루트 레이아웃 구성을 소개할게요.

먼저, styled-components를 활용한 RootLayout 컴포넌트 모습부터 살펴봅시다.

```jsx
import StyledComponentsRegistry from './lib/registry'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html>
      <body>
        <StyledComponentsRegistry>{children}</StyledComponentsRegistry>
      </body>
    </html>
  )
}
```

여기서 StyledComponentsRegistry는 styled-components를 SSR(서버 사이드 렌더링)에서 제대로 작동하게 하도록 도와주는 역할을 해요. 만약 여러분이 Next.js 같은 프레임워크를 사용한다면, SSR 시 스타일 섞임 현상이나 스타일 누락 문제를 줄이는데 아주 유용하죠.

그 다음, 외부 패키지에서 제공하는 CSS 파일을 불러오는 방법입니다. 예시로 Bootstrap CSS를 활용해 볼게요.

```jsx
import 'bootstrap/dist/css/bootstrap.css'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body className="container">{children}</body>
    </html>
  )
}
```

이렇게 레이아웃 최상위 루트에 Bootstrap을 임포트해서 전체 앱에 적용할 수 있어요. 이렇게 하면 별도로 각 컴포넌트마다 스타일을 임포트하지 않아도 되니 편리합니다.

추가 팁을 드리자면:

- 외부 CSS는 글로벌하게 적용되기 때문에 기본값 덮어쓰기나 클래스 충돌을 염두에 두고 사용하세요.
- styled-components 같이 CSS-in-JS 방식을 쓰면서 외부 스타일시트를 혼합하는 경우, 스타일 우선순위나 특정성 specificity를 고려해야 합니다.
- 종종 styled-components로 스타일을 작성하면서 Bootstrap처럼 미리 만들어진 UI 라이브러리 CSS를 함께 쓰면, 코드 재사용성과 개발 생산성을 높일 수 있으니 두 방식을 적절히 섞는 걸 추천합니다!

요약하자면, styled-components SSR 설정을 통해 안정적인 스타일 렌더링을 확보하고, 외부 CSS는 전역에 한번만 불러오는 방식을 취하면 여러분 프로젝트의 스타일 관리가 훨씬 깔끔해질 거예요. 필요한 부분에서는 컴포넌트 단위로 스타일을 나누고, 글로벌 스타일이나 라이브러리는 루트 레이아웃에서 한꺼번에 관리하는 게 좋아요.

이제 여러분도 Next.js 프로젝트나 React 앱에서 스타일시트 관리가 좀 더 수월해지길 바랍니다! 궁금한 점 있으면 언제든 댓글로 알려주세요. :)

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

외부 스타일시트는 꼭 npm 패키지에서 직접 가져오거나, 파일을 다운받아서 내 프로젝트 코드와 함께 두어야 해요. 그러니까 `<link rel="stylesheet" />` 태그처럼 외부 URL을 그냥 연결해서 쓰는 방식은 사용할 수 없다는 뜻이죠.

왜 그런 규칙이 있냐면, 보통 이렇게 하면 스타일이 내 프로젝트 안에 완전히 포함되기 때문에 빌드하거나 배포할 때 의존성이 깔끔해지고, 네트워크 지연 없이 더 빠르게 스타일을 적용할 수 있으니까요.

실제로 CSS 가져오는 방법은 크게 두 가지예요:

1. **npm 패키지에서 직접 import**  
   예를 들어, `import 'package-name/dist/style.css';` 이렇게 사용하면 스타일시트가 내 프로젝트 안에서 관리되니까 편리해요.

2. **파일 다운로드 후 프로젝트에 포함**  
   필요한 CSS 파일을 직접 다운받아서 `/styles` 같은 디렉터리에 넣고,  
   `import './styles/custom.css';` 이런식으로 불러옵니다.

이때 주의할 점은, 외부에서 그냥 `<link>` 태그로 불러오면 리액트 같은 프레임워크 환경에서 스타일 관리가 까다로워지고, 로드 타이밍 문제도 생길 수 있으니 가급적 권장하지 않는다는 거예요.

개발할 때 이런 점 참고해서, 스타일 관리도 깔끔하게 하는 습관 들이면 좋아요! 필요하면 이후에 CSS-in-JS 같은 방법도 한번 알아보시는 걸 추천드립니다. 스타일시트와 자바스크립트를 한 곳에서 관리하니까 훨씬 효율적이거든요.