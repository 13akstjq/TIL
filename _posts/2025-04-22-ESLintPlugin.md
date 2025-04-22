---
title: "Next.js와 React 프로젝트에 ESLint Plugin 추가하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:36
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "ESLint Plugin"
link: "https://nextjs.org/docs/app/api-reference/config/eslint"
isUpdated: false
---


# ESLint 플러그인

Next.js에는 이미 기본 설정에 포함되어 있는 `eslint-plugin-next`라는 ESLint 플러그인이 있어요. 이 플러그인은 Next.js 애플리케이션을 개발할 때 자주 발생하는 문제나 실수를 미리 잡아내도록 도와줍니다. 덕분에 더 깔끔하고 버그 없는 코드를 작성할 수 있죠.

## 참고 자료

`eslint-config-next` 안에는 아래에 소개하는 여러 ESLint 플러그인들을 추천 규칙 세트로 포함하고 있어요. 그러니까 Next.js를 쓰면 별도의 복잡한 설정 없이도 좋은 코드 품질을 유지할 수 있다는 뜻이죠.

| ESLint Plugin           | 설명                                         |
|------------------------|--------------------------------------------|
| eslint-plugin-react    | React 코드에서 발생할 수 있는 문제를 잡아냄         |
| eslint-plugin-jsx-a11y | 웹 접근성 관련 규칙을 적용해 좀 더 접근성 좋은 UI 작성 지원 |
| eslint-plugin-import   | 모듈 임포트 관련 문제를 방지                      |

Next.js 개발할 때는 이런 기본 설정들이 이미 잘 갖춰져 있으니, 너무 ESLint 설정에만 신경 쓰지 말고 개발에 집중해도 되겠죠? 물론 필요하다면 플러그인이나 규칙을 추가로 조정해서 내 스타일에 맞게 커스터마이징하는 것도 가능해요.

추가로, ESLint와 prettier 같은 코드 스타일링 도구를 함께 사용하면 코드 품질 유지와 팀 협업이 훨씬 수월해진다는 점도 기억해두면 좋겠네요!

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

- eslint-plugin-react  
- eslint-plugin-react-hooks  
- eslint-plugin-next  

이 설정들은 next.config.js에 있는 설정보다 우선 적용됩니다.

### 규칙 (Rules)

전체 규칙 목록은 다음과 같습니다:  

| 규칙 이름                | 설명                                          |
|-------------------------|---------------------------------------------|
| eslint-plugin-react      | React 관련 코딩 스타일과 규칙을 관리하는 플러그인  |
| eslint-plugin-react-hooks| React Hooks 관련 규칙을 강화해주는 플러그인         |
| eslint-plugin-next      | Next.js 특화 규칙을 포함하는 플러그인               |

ESLint 플러그인들은 프로젝트의 코드 품질과 일관성을 유지하는 데 정말 유용해요. Next.js 프로젝트를 할 때 이런 플러그인들을 쓰면 Hooks를 잘못 쓰거나, React 컴포넌트에서 흔히 발생하는 실수를 미리 잡아낼 수 있어서 개발할 때 시간도 절약되고 오류도 줄일 수 있답니다.  

특히, `eslint-plugin-next`는 Next.js를 사용할 때만 적용되는 특수 규칙들(예: 이미지 최적화, 링크 태그 사용 등)을 포함하고 있어서 Next.js 프로젝트에 최적화된 코드를 작성하는 데 도움을 줍니다. 그런데 `next.config.js`의 설정보다 이 플러그인 설정이 우선한다는 점, 꼭 기억하세요!  

이 아래에 구체적인 각 규칙 셋을 보시면, 어떤 규칙들이 있고 각각 어떻게 동작하는지 더 자세히 알 수 있을 거예요. 궁금한 점 있으면 언제든 물어보세요!

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

아래는 Next.js에서 권장하는 ESLint 규칙들 목록입니다. 나열된 규칙들은 기본 권장 설정에 활성화되어 있고, 각각 어떤 역할을 하는지 간단히 설명되어 있어요. 참고하면 코드 작성 시 품질을 높이는데 큰 도움이 됩니다!

| 권장 설정에 활성화됨 | 규칙 | 설명 |
|:------------------:|:----------------------------|:---------------------------------------------------------|
| ✅ | [@next/next/google-font-display](https://nextjs.org/docs/messages/google-font-display) | Google Fonts 사용 시 `font-display` 속성 강제 적용 |
| ✅ | [@next/next/google-font-preconnect](https://nextjs.org/docs/messages/google-font-preconnect) | Google Fonts에 대해 `preconnect` 태그 사용 권장 |
| ✅ | [@next/next/inline-script-id](https://nextjs.org/docs/messages/inline-script-id) | 인라인 스크립트가 포함된 `next/script` 컴포넌트에 `id` 속성 필요 |
| ✅ | [@next/next/next-script-for-ga](https://nextjs.org/docs/messages/next-script-for-ga) | 구글 애널리틱스 인라인 스크립트 사용 시 `next/script` 컴포넌트를 선호 |
| ✅ | [@next/next/no-assign-module-variable](https://nextjs.org/docs/messages/no-assign-module-variable) | `module` 변수에 할당하는 행위 방지 |
| ✅ | [@next/next/no-async-client-component](https://nextjs.org/docs/messages/no-async-client-component) | 클라이언트 컴포넌트를 async 함수로 작성하는 것을 방지 |
| ✅ | [@next/next/no-before-interactive-script-outside-document](https://nextjs.org/docs/messages/no-before-interactive-script-outside-document) | `next/script`의 `beforeInteractive` 옵션은 `pages/_document.js`에서만 허용 |
| ✅ | [@next/next/no-css-tags](https://nextjs.org/docs/messages/no-css-tags) | 수동 스타일 시트 태그 방지 |
| ✅ | [@next/next/no-document-import-in-page](https://nextjs.org/docs/messages/no-document-import-in-page) | `next/document`를 `pages/_document.js` 외부에서 임포트 금지 |
| ✅ | [@next/next/no-duplicate-head](https://nextjs.org/docs/messages/no-duplicate-head) | `pages/_document.js` 내에서 `<Head>` 중복 사용 방지 |
| ✅ | [@next/next/no-head-element](https://nextjs.org/docs/messages/no-head-element) | `<head>` 요소 사용 방지 (Next.js에서는 `<Head>` 컴포넌트 사용 권장) |
| ✅ | [@next/next/no-head-import-in-document](https://nextjs.org/docs/messages/no-head-import-in-document) | `pages/_document.js`에서 `next/head` 임포트 금지 |
| ✅ | [@next/next/no-html-link-for-pages](https://nextjs.org/docs/messages/no-html-link-for-pages) | 내부 페이지 이동 시 `<a>` 대신 Next.js의 `<Link>` 컴포넌트 사용 권장 |
| ✅ | [@next/next/no-img-element](https://nextjs.org/docs/messages/no-img-element) | LCP (Largest Contentful Paint) 성능 저하와 높은 대역폭 문제 때문에 `<img>` 대신 Next.js `Image` 컴포넌트 사용 권장 |
| ✅ | [@next/next/no-page-custom-font](https://nextjs.org/docs/messages/no-page-custom-font) | 페이지 단독 사용자 지정 폰트 사용 방지 (글로벌로 적용 권장) |
| ✅ | [@next/next/no-script-component-in-head](https://nextjs.org/docs/messages/no-script-component-in-head) | `next/head` 컴포넌트에 `next/script` 사용 금지 |
| ✅ | [@next/next/no-styled-jsx-in-document](https://nextjs.org/docs/messages/no-styled-jsx-in-document) | `pages/_document.js` 내 styled-jsx 사용 금지 |
| ✅ | [@next/next/no-sync-scripts](https://nextjs.org/docs/messages/no-sync-scripts) | 동기 방식 스크립트 사용 방지 |
| ✅ | [@next/next/no-title-in-document-head](https://nextjs.org/docs/messages/no-title-in-document-head) | `next/document`에서 `Head` 컴포넌트 안에 `<title>` 사용 금지 |
| ✅ | @next/next/no-typos | Next.js 데이터 패칭 함수에서 흔히 하는 오타 방지 (`getStaticProps` 등) |
| ✅ | [@next/next/no-unwanted-polyfillio](https://nextjs.org/docs/messages/no-unwanted-polyfillio) | Polyfill.io의 중복 폴리필 방지 |

---

### 추가 팁!

- 위 리스트의 많은 규칙들은 Next.js에서 권장하는 최적화 및 구조적 일관성을 지키도록 도와줍니다.
- 예를 들어, 내부 페이지 이동 시 `<Link>` 컴포넌트를 쓰는 이유는 클라이언트 사이드 네비게이션 활성화와 빠른 페이지 전환을 가능하게 하기 때문입니다.
- 그리고 이미지 최적화를 위해 Next.js의 `Image` 컴포넌트를 사용하는 것이 페이지 로딩 속도 향상과 대역폭 절약에 크게 유리해요.
- `next/script` 컴포넌트는 스크립트 로딩 시점 및 우선순위를 조절하여 성능 최적화를 돕는데, 특히 Google Analytics 같은 외부 스크립트는 꼭 규칙대로 적용하는 게 좋습니다.

---

### 에디터에서 경고와 오류를 바로 확인하려면?

이런 ESLint 규칙들은 혼자 일일이 체크하기 어려운데요, VSCode 같은 편집기에서 Next.js ESLint 플러그인을 사용하면 개발 중 곧바로 문제점을 확인할 수 있어서 훨씬 편리합니다!

---

## 예시: 커스텀 디렉토리 및 파일에 Lint 적용하기

```bash
npx next lint --dir=app,components --files=**/*.ts,**/*.tsx
```

이렇게 하면 기본 `pages` 디렉토리 외에 `app`, `components` 폴더도 lint 검사를 할 수 있어요. 실제 프로젝트에 맞게 잘 활용해 보세요!

---

Next.js 개발하면서 꼭 알아두면 좋은 필수 ESLint 규칙들!

꾸준히 규칙을 지키면서 개발하면 유지보수가 훨씬 쉬워지고, 코드 품질도 자연스럽게 올라가니까 꼭 적용해 보세요 :)

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

Next.js는 기본적으로 pages/, app/, components/, lib/, src/ 디렉터리 안에 있는 모든 파일들에 대해 ESLint를 실행해줘요. 근데 프로덕션 빌드할 때는 어떤 디렉터리만 검사할지 next.config.js 파일 안에 eslint 옵션의 dirs 배열로 지정할 수도 있답니다.

```js
module.exports = {
  eslint: {
    dirs: ['pages', 'utils'], // next build 시에 'pages'와 'utils' 폴더만 ESLint 검사 대상이 됩니다.
  },
}
```

그리고 커맨드 라인에서 직접 ESLint를 실행할 때는 `next lint` 다음에 `--dir`이나 `--file` 플래그를 붙여 특정 디렉터리나 파일만 검사할 수도 있어요.

```bash
next lint --dir pages --dir utils --file bar.js
```

이렇게 하면 pages와 utils 폴더, 그리고 bar.js 파일만 ESLint 검사를 하게 돼서, 검사 범위를 좁혀서 속도를 개선하거나 원하는 부분만 빠르게 체크할 때 유용해요.

추가로 팁을 드리자면, 대규모 프로젝트에서는 불필요한 파일까지 ESLint가 검사하지 않도록 dirs 옵션을 잘 설정해두는 게 빌드 시간을 줄이는 데 꽤 도움이 돼요. 프로젝트가 커질수록 이런 세세한 설정이 실제 개발 효율에도 큰 차이를 만들어낸답니다!

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

### 모노레포(monorepo)에서 루트 디렉터리 지정하기

안녕하세요! 오늘은 Next.js 프로젝트를 모노레포 구조로 관리할 때, `eslint-plugin-next`가 Next.js 애플리케이션이 어디 있는지 잘 찾게 만드는 방법에 대해 이야기해볼게요.

보통 Next.js 프로젝트를 루트 디렉터리에 둔다면 별다른 설정 없이 잘 작동하는데요, 모노레포처럼 여러 패키지가 서브디렉터리에 있을 경우엔 `eslint-plugin-next`가 어디서 Next.js 일을 처리해야 할지 모를 수 있어요. 그래서 `.eslintrc` 파일(또는 설정 파일)에서 `settings` 속성 안에 `next.rootDir` 옵션을 지정해주는 거죠.

즉, ESLint 설정을 조금 손봐서 Next.js 앱 경로를 정확히 알려주는 방법을 사용할 수 있습니다. 예제를 볼게요:

```js
import { FlatCompat } from '@eslint/eslintrc'

const compat = new FlatCompat({
  // import.meta.dirname는 Node.js v20.11.0 이상에서 사용 가능해요
  baseDirectory: import.meta.dirname,
})

const eslintConfig = [
  ...compat.config({
    extends: ['next'],
    settings: {
      next: {
        rootDir: 'packages/my-app/',  // 여기에 Next.js 앱 폴더 경로를 넣어주세요
      },
    },
  }),
]

export default eslintConfig
```

여기서 좀 더 보충설명을 하자면,

| 설정 | 설명 |
|-------|------|
| `rootDir` | Next.js가 위치한 경로 (상대경로나 절대경로 모두 가능) |
| glob 패턴 | 여러 애플리케이션이 있는 경우 `"packages/*/"` 같은 glob 패턴도 쓸 수 있어요 |
| 배열 사용 | 여러 개 경로(또는 glob)를 배열로 묶어서 지정하는 것도 가능합니다 |

예를 들어, 여러 앱이 있을 때는 이렇게 할 수도 있죠:

```js
settings: {
  next: {
    rootDir: ['packages/app1/', 'packages/app2/', 'packages/*/'],
  },
}
```

이 설정 덕분에 ESLint가 Next.js 프로젝트 내 규칙을 제대로 이해하고 검사할 수 있어, 잘못된 코드 패턴이나 Next.js 특화된 규칙 위반을 정확히 잡아줍니다.

---

**참고로** `import.meta.dirname`는 Node.js v20.11.0부터 지원하니, 이 버전 이하를 사용 중이라면 다른 방법(예: `__dirname` 또는 경로 모듈 활용)을 써야 해요.

---

### 정리하자면

- 모노레포에서 각 Next.js 앱 위치를 알려줘야 ESLint가 제대로 동작한다.
- `.eslintrc` 혹은 ESLint 설정 파일의 `settings.next.rootDir`에 앱 경로 또는 glob 패턴을 지정하면 된다.
- 여러 앱이 있으면 배열로 여러 경로를 지정할 수도 있다.

monorepo 나눠서 관리하는 분들은 이렇게 한번 설정해두면 ESLint도 Next.js도 모두 스트레스 없이 사용할 수 있으니 꼭 참고해보세요! :)

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

### 캐시 비활성화하기

ESLint는 기본적으로 처리한 파일들의 정보를 캐시해서 성능을 높여줘요. 이 캐시는 보통 `.next/cache` 폴더나 여러분이 설정한 빌드 디렉토리에 저장됩니다. 

그런데 만약 ESLint 규칙 중에 한 소스 파일의 내용뿐만 아니라 다른 상태나 파일에 의존하는 규칙을 사용할 때는 캐시가 문제를 일으킬 수 있거든요. 이런 경우 캐시를 끄고 싶다면 `next lint` 명령어에 `--no-cache` 플래그를 붙여주면 돼요.

```bash
next lint --no-cache
```

이렇게 하면, ESLint가 이전에 저장해둔 캐시를 무시하고 항상 새로 검사하기 때문에 캐시로 인한 이상 동작을 방지할 수 있습니다.

---

### ESLint 규칙 비활성화하기

ESLint 룰을 상황에 따라 꺼야 할 때가 있어요. 프로젝트 전체에서 특정 룰을 끌 수도 있고, 특정 파일이나 코드 일부에만 룰을 무시하도록 설정할 수 있죠. 다음에는 ESLint 규칙 비활성화 방법에 대해서도 차근차근 알려드릴게요!

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

안녕하세요, 개발자 여러분! 오늘은 Next.js, React, 그리고 React Hooks 플러그인에서 제공하는 ESLint 규칙들을 어떻게 수정하거나 비활성화할 수 있는지 알려드리려고 해요. 특히 `.eslintrc` 설정 파일에서 `rules` 프로퍼티를 직접 변경하는 방법에 대해 다뤄볼게요.

먼저, Next.js에서는 ESLint 설정 방식이 조금 바뀌었는데요, 새로운 `@eslint/eslintrc` 패키지를 활용해서 호환성을 유지하고 설정을 쉽게 변경할 수 있어요. 아래 예시 코드를 통해 이해해볼게요.

```js
import { FlatCompat } from '@eslint/eslintrc'

const compat = new FlatCompat({
  // import.meta.dirname은 Node.js v20.11.0 이상부터 사용 가능합니다.
  baseDirectory: import.meta.dirname,
})

const eslintConfig = [
  ...compat.config({
    extends: ['next'],
    rules: {
      // react 컴포넌트 내에서 escape 처리되지 않은 엔티티 사용 경고를 끕니다.
      'react/no-unescaped-entities': 'off',
      // Next.js 페이지에서 custom font 사용을 금지하는 규칙을 끕니다.
      '@next/next/no-page-custom-font': 'off',
    },
  }),
]

export default eslintConfig
```

### 설명을 조금 더 살짝 덧붙여볼게요

- `FlatCompat`는 Next.js 최신 ESLint 설정이 기본적으로 `flat` config 방식을 사용하면서 발생하는 호환성 문제를 해결해 줍니다.
- `baseDirectory`는 현재 설정 파일의 경로 기준으로 확장과 규칙을 찾도록 해 줍니다.
- `extends`에 `'next'`를 명시하면 기본적으로 Next.js에서 추천하는 ESLint 규칙 세트를 사용하겠다는 뜻입니다.
- 여기서 `rules` 안에 원하는 규칙을 `off`나 다른 심각도 수준(`warn`, `error`)으로 직접 조정할 수 있어요.

---

### Core Web Vitals 관련 팁!

Next.js에서 `next lint` 명령어를 처음 실행할 때, `strict` 옵션을 선택하면 `next/core-web-vitals`라는 별도의 규칙 세트가 자동으로 활성화돼요. 이 규칙들은 웹 성능과 사용자 경험에 큰 영향을 미치는 Core Web Vitals 지표 개선에 도움을 준답니다. 따라서 엄격한 품질 관리가 필요한 프로젝트에서는 이 옵션을 적극 권장드려요.

---

요즘 ESLint 설정 방법이 조금 바뀌었지만 위 방법처럼 하면 이전보다 훨씬 유연하고 깔끔하게 원하는 규칙을 커스터마이징할 수 있습니다. 혹시 더 궁금한 점 있으면 편하게 알려주세요! 개발자 여러분, 행복한 코딩 하세요! 🚀

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

이번 글에서는 Next.js 프로젝트에서 ESLint 설정을 할 때 자주 쓰이는 `next/core-web-vitals` 확장 설정을 Flat ESLint 설정 방식과 함께 사용하는 방법을 소개할게요. 실무에서 꼭 알아두면 좋은 내용이니, 재미있게 읽어주세요!

---

## `next/core-web-vitals`란?

Next.js에서 제공하는 `next/core-web-vitals`는 Core Web Vitals(사용자 경험과 성능지표에 중요한 요소들)를 기준으로 ESLint 규칙들을 강화한 설정입니다. 기본적으로 경고(warning)로 표시되던 규칙 중 사용자 경험에 치명적인 부분에 대해서는 에러(error)로 올려줍니다. 이 덕분에 웹 앱 성능과 접근성을 더 신경 쓸 수 있죠.

### 유용한 점
- Create Next App으로 새 프로젝트 만들면 자동으로 포함돼 있어요.
- Core Web Vitals에 영향을 주는 코드에 대한 검사 강화
- 런타임 성능과 SEO, 사용자 경험 개선에 도움

---

## Flat ESLint 설정에서는 이렇게 써요

ESLint 8버전부터는 구성 파일이 Flat Config라는 새로운 방식도 지원합니다. 기존 `.eslintrc.js` 대신 `eslint.config.js` 같은 파일에 설정을 배열 형태로 쓴다고 생각하면 쉬워요.

`@eslint/eslintrc` 패키지의 `FlatCompat` 도구를 이용하면 이전 `.eslintrc` 형식을 Flat Config으로 변환해 쓸 수 있어서, 그 점을 활용한 코드가 바로 아래 예시입니다.

```js
import { FlatCompat } from '@eslint/eslintrc'

const compat = new FlatCompat({
  // import.meta.dirname은 Node.js v20.11.0 이후 버전에서 사용 가능해요.
  baseDirectory: import.meta.dirname,
})

const eslintConfig = [
  ...compat.config({
    extends: ['next/core-web-vitals'],
  }),
]

export default eslintConfig
```

- `FlatCompat`에 프로젝트 경로를 넘겨준 후,
- `compat.config()` 메서드에서 `extends` 배열에 `next/core-web-vitals`를 추가해줘요.
- 결과물을 그대로 export하면 ESLint가 Flat Config 방식으로 인식합니다.

---

## TypeScript 프로젝트에서 쓸 땐?

TypeScript에서도 마찬가지로 위 방식을 사용할 수 있는데요, 보통 `@typescript-eslint` 플러그인도 같이 사용하므로 아래처럼 작성합니다.

```js
import { FlatCompat } from '@eslint/eslintrc'

const compat = new FlatCompat({
  baseDirectory: import.meta.dirname,
})

const eslintConfig = [
  ...compat.config({
    extends: [
      'next/core-web-vitals',
      'plugin:@typescript-eslint/recommended', // TypeScript 권장 설정
    ],
  }),
]

export default eslintConfig
```

TypeScript와 Next.js 둘다 쓰는 프로젝트라면, 이렇게 병합해서 설정해주시면 끝!

---

## 마지막으로 팁 하나!

Next.js 최신 버전은 기본적으로 `next/core-web-vitals`를 포함하는 경우가 많지만, 기존 프로젝트나 수동으로 구성하는 경우 유용해요. ESLint 오류가 너무 많아 부담된다면 한두 가지 규칙만 선택적으로 켜서 점진적으로 적용해보길 추천합니다.

또, Node.js 버전이 `import.meta.dirname`을 지원하지 않는다면, `path` 모듈과 `import.meta.url`을 활용해서 경로를 추출하는 코드도 함께 작성해야 해요.

---

오늘은 Next.js 프로젝트에서 Core Web Vitals에 맞춘 ESLint 설정 방법과 Flat Config 적용법에 대해 간단히 다뤄봤어요. 다음에도 실무에 바로 써먹는 팁으로 돌아올게요!

궁금한 점 있으면 댓글로 물어봐 주세요 :)

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

Next.js를 사용하면서 ESLint 설정할 때, create-next-app --typescript 옵션을 쓰면 기본적으로 Next.js ESLint 규칙 뿐만 아니라 TypeScript에 특화된 lint 규칙도 함께 추가해줘요. next/typescript라는 규칙셋을 통해서요.

아래 코드를 보면 FlatCompat라는 도구를 써서 'next/core-web-vitals'와 'next/typescript' 두 규칙셋을 extends 하고 있죠.

```js
import { FlatCompat } from '@eslint/eslintrc'

const compat = new FlatCompat({
  // import.meta.dirname는 Node.js v20.11.0 이상에서 사용 가능해요
  baseDirectory: import.meta.dirname,
})

const eslintConfig = [
  ...compat.config({
    extends: ['next/core-web-vitals', 'next/typescript'],
  }),
]

export default eslintConfig
```

여기서 next/typescript 규칙들은 사실 `plugin:@typescript-eslint/recommended` 기반이에요. 즉, TypeScript 코드를 더 엄격하고 깔끔하게 만들어주는 다양한 룰들을 포함하고 있다는 뜻이죠. 더 자세한 내용은 [typescript-eslint 공식 문서](https://typescript-eslint.io/linting/configs/)를 참고하면 좋아요.

### Prettier와 함께 쓰기

ESLint와 함께 Prettier를 같이 사용하고 싶다면, 서로 충돌하는 규칙들을 조정해 줘야 해요. 특히 코드 스타일 관련 룰들은 Prettier가 담당하고, ESLint는 코드 품질과 오류 확인 위주로 역할 분담하는 게 일반적이죠.

이럴 땐 `eslint-config-prettier`를 추가하고, extends에 `prettier`를 마지막에 넣어줘서 Prettier와 충돌하는 ESLint 룰을 끄는 방식을 씁니다. 예를 들면 이렇게:

```js
...compat.config({
  extends: ['next/core-web-vitals', 'next/typescript', 'prettier'],
}),
```

그리고 Prettier 설정 파일(.prettierrc)을 잘 만들어 두면, VS Code 같은 편집기도 자동으로 스타일을 맞춰줘서 코딩하기 훨씬 편해져요!

---

**한마디 정리**  
create-next-app --typescript를 통해 만들어진 ESLint 설정은 Next.js 공식 규칙 + TypeScript 추천 룰들의 조합이에요. 여기에 Prettier를 더해서 코딩 스타일도 자동으로 맞춰주면, 팀 프로젝트나 개인 개발 모두 훨씬 깔끔하고 효율적이랍니다!

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

ESLint에는 코드 포맷팅 규칙도 포함되어 있어서, 기존에 사용 중인 Prettier 설정과 충돌이 발생할 수 있어요. 그래서 ESLint와 Prettier가 서로 잘 협업할 수 있도록 `eslint-config-prettier`라는 설정을 추가하는 걸 추천해드립니다.

먼저, `eslint-config-prettier` 패키지를 설치해야 해요. 사용하는 패키지 매니저에 따라 아래 명령어 중 하나를 실행하면 됩니다:

```bash
npm install --save-dev eslint-config-prettier

yarn add --dev eslint-config-prettier

pnpm add --save-dev eslint-config-prettier

bun add --dev eslint-config-prettier
```

설치가 끝났으면, 이제 ESLint 설정 파일(`.eslintrc.js`나 `.eslintrc.json` 등)에 `prettier`를 extends에 추가해줘야 해요. 예를 들어 `.eslintrc.js` 파일이라면 이렇게 작성할 수 있습니다:

```js
module.exports = {
  extends: [
    // 기존 확장 설정들
    "eslint:recommended",
    "plugin:react/recommended",
    // prettier 관련 설정은 항상 가장 마지막에 추가하는 게 좋아요
    "prettier"
  ],
  // 그 외 다른 ESLint 설정들
};
```

`eslint-config-prettier`는 ESLint의 포맷팅 관련 규칙을 끄면서, Prettier가 원하는 대로 포맷팅을 할 수 있게 도와줘요. 이렇게 해 놓으면 ESLint가 포맷팅 문제로 에러를 내는 경우가 줄어들고, 두 도구가 충돌 없이 함께 동작합니다.

참고로, 만약 `eslint-plugin-prettier`도 사용하고 있다면, `plugin:prettier/recommended`를 extends에 추가하는 방식도 고려해볼 수 있어요. 이 방법을 사용하면 Prettier가 제안하는 포맷팅 규칙을 ESLint가 직접 체크해서 에러로 보여주게 되니, 편리합니다.

사용자마다 설정 환경이 다를 수 있으니, 본인 프로젝트에 맞게 조절하면서 써보세요!

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

JS로 작성된 ESLint 설정과 함께, lint-staged와 Next.js lint를 함께 사용하는 방법에 대해 살펴볼게요.

```js
import { FlatCompat } from '@eslint/eslintrc'

const compat = new FlatCompat({
  // import.meta.dirname은 Node.js v20.11.0 이후부터 사용 가능해요
  baseDirectory: import.meta.dirname,
})

const eslintConfig = [
  ...compat.config({
    extends: ['next', 'prettier'], // next.js 기본 규칙과 prettier 규칙을 확장해서 사용
  }),
]

export default eslintConfig
```

여기서 `FlatCompat`은 ESLint가 새롭게 Flat Config 방식을 지원하면서, 기존의 extends 규칙을 쉽게 호환시켜 주는 도구예요. Node.js 환경에서 `import.meta.dirname`을 쓰려면 최소 버전이 20.11.0 이상이어야 하니 버전 확인 꼭 해보세요!

---

### 스테이지 된 파일만 린트 돌리기 (lint-staged)

git에 커밋 전에 변경된 파일들만 린트 돌리면서 자동 수정까지 하고 싶다면 `lint-staged`를 쓸 수 있어요. 그런데 Next.js의 `next lint` 명령어는 기본적으로 특정 파일만 지정해서 검사하려면 `--file` 플래그를 써야 하거든요.

그래서 `.lintstagedrc.js`에 아래처럼 설정해 주면,

```js
const path = require('path')

const buildEslintCommand = (filenames) =>
  `next lint --fix --file ${filenames
    .map((f) => path.relative(process.cwd(), f))
    .join(' --file ')}`

module.exports = {
  '*.{js,jsx,ts,tsx}': [buildEslintCommand],
}
```

- `filenames`에 있는 파일들 각각을 상대 경로로 바꿔서 `--file` 플래그와 함께 넘겨줘요.
- `--fix` 옵션 덕분에 린트 에러가 안 잡히는 부분은 자동으로 고쳐줘서 편리하답니다.

---

### 덧붙임: next lint와 lint-staged 활용 팁

- **commit hook에 연결**: 보통 `husky` 같은 git 훅 관리 도구와 함께 써서 커밋 전에 자동으로 린트가 실행되도록 많이 설정해요.
- **속도 개선**: 어떤 프로젝트는 린트 검사 시간이 오래 걸릴 수 있는데, lint-staged는 변경된 파일만 검사하니까 훨씬 빠릅니다.
- **eslint 설정 최신화**: `FlatCompat`을 쓰지 않고 공식 Flat Config 방식으로 마이그레이션을 고민중이라면, eslint 공식 문서도 꾸준히 참고하세요.

실제 프로젝트에 위 설정을 적용해서 깔끔한 코드 유지에 도움이 되길 바라요!

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

## 프로덕션 빌드 시 린트(lint) 비활성화하기

Next.js에서 프로덕션 빌드를 할 때 ESLint가 실행되는 걸 원하지 않는다면, `next.config.js` 파일에 `eslint.ignoreDuringBuilds` 옵션을 `true`로 설정하면 됩니다.

```js
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  eslint: {
    // 주의! 이 설정을 사용하면 ESLint 에러가 있어도 프로덕션 빌드가 정상적으로 완료됩니다.
    ignoreDuringBuilds: true,
  },
}

export default nextConfig
```

이 옵션을 활성화하면, 빌드 과정에서 ESLint 에러가 있어도 빌드가 중단되지 않기 때문에 빠르게 배포할 수 있다는 장점이 있지만, 반대로 코드 품질 문제를 간과할 위험도 있으니 주의하는 게 좋아요.

### 기존 설정 마이그레이션하기

만약 기존에 ESLint 설정을 다른 방식(예: 스크립트 또는 별도 플러그인 등)으로 관리하고 있었다면, 이제는 Next.js의 공식적인 설정 방식에 맞춰 `next.config.js` 내부에 위처럼 `eslint.ignoreDuringBuilds` 옵션만 추가해주면 됩니다.

간단히 말해:

- 기존에 빌드 시 `eslint` 검사를 별도로 돌리고 있었다면 이 옵션을 켜면 빌드 속도가 빨라짐
- 하지만 배포 전에 꼭 별도로 린팅하거나 CI/CD 파이프라인에서 린트를 수행하는 게 중요!

---

개발할 때는 린트를 꼭 켜두고, 프로덕션 빌드시간을 빠르게 하고 싶을 때만 이 옵션을 사용하는 게 가장 스마트한 방법입니다. 린트는 코드 품질 유지에 아주 중요한 역할을 하니까요!

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

이미 애플리케이션에 ESLint가 설정되어 있다면, 특별한 몇 가지 조건이 아니면 eslint-config-next를 포함하기보다는 이 플러그인을 직접 확장하는 걸 추천해요.

#### 추천 플러그인 룰셋

다음 조건들이 모두 맞으면 이 방법이 좋아요:

- 아래 플러그인 중 하나 이상을 이미 설치했을 때 (별도로 설치했거나 airbnb, react-app 같은 다른 설정을 이용해서 포함된 경우):
  - react
  - react-hooks
  - jsx-a11y
  - import
- 그리고 Next.js 내에서 Babel이 기본 설정된 방식과 다른 parserOptions를 직접 정의한 경우 (이건 보통 권장되지 않는 방법이에요. Babel 설정을 커스터마이징한 경우에만 해당돼요)
- eslint-plugin-import를 사용 중이고, Node.js 및/또는 TypeScript resolver가 import를 제대로 처리하도록 설정한 경우

---

요약하자면, 이미 React 관련 플러그인들이 잘 구성되어 있거나, Babel과 import 처리 방식에 변화를 줬을 때는 eslint-config-next를 포함하기보단 플러그인을 직접 확장하는 게 더 깔끔하고 충돌도 적답니다.

참고로, Next.js는 자체적으로 ESLint를 지원해주지만 프로젝트 상황에 따라 세밀하게 조정해야 할 때가 있으니 이런 점을 미리 체크해보는 게 좋아요!

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

Next.js 프로젝트에서 ESLint 설정을 할 때, 직접 설정을 추가하기보다는 Next.js에서 권장하는 ESLint 플러그인을 사용하는 게 훨씬 편하고 관리하기 좋습니다. 그래서 기존에 직접 설정해둔 부분이 있다면, 그것을 없애거나 Next.js가 제공하는 플러그인으로 바로 확장하는 걸 추천해요.

설정은 이렇게 간단합니다:

```js
module.exports = {
  extends: [
    // 기존 설정들...
    'plugin:@next/next/recommended',
  ],
}
```

이렇게 하면 Next.js에서 권장하는 ESLint 규칙들이 자동으로 적용돼서, 코드 품질도 높아지고 Next.js의 최신 가이드라인도 쉽게 따라갈 수 있죠.

그리고 이 플러그인은 프로젝트에 별도로 설치해줘야 하는데, `next lint` 명령어를 굳이 실행하지 않아도 됩니다. 설치는 각각의 패키지 매니저에 맞게 아래 명령어 중 하나를 사용하면 돼요:

```bash
npm install --save-dev @next/eslint-plugin-next
```

```bash
yarn add --dev @next/eslint-plugin-next
```

```bash
pnpm add --save-dev @next/eslint-plugin-next
```

```bash
bun add --dev @next/eslint-plugin-next
```

---

### 추가 팁!

- 이 플러그인은 Next.js의 최적화 코드 스타일, 성능, 접근성 관련 규칙도 포함하고 있어서 프로젝트 전반의 품질 관리에 큰 도움이 됩니다.
- ESLint 설정 변경 후에는 꼭 에디터를 재시작하거나 ESLint 프로세스를 재시작해서 변경 사항이 잘 반영되는지 확인해 보세요.
- 가끔 규칙이 너무 엄격하거나 불필요하다고 느껴지면, `.eslintrc.js`에서 개별 규칙을 오버라이드해서 조정할 수도 있어요.

이제 Next.js 공식 방식을 활용해서 더 깔끔하고 효율적인 개발 환경을 만들어보세요!

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

겹치는 플러그인이나 파서를 여러 설정 파일에서 불러와 생길 수 있는 충돌이나 오류 걱정 없이 안전하게 사용할 수 있는 방법이에요.

### 추가 설정 방법

만약 이미 따로 ESLint 설정을 사용 중인데, 거기에 `eslint-config-next`를 포함시키고 싶다면, 다른 설정들 뒤에 마지막으로 `next` 설정이 확장되도록 해야 합니다. 예를 들면 아래처럼요:

```js
import js from '@eslint/js'
import { FlatCompat } from '@eslint/eslintrc'

const compat = new FlatCompat({
  // import.meta.dirname는 Node.js v20.11.0 이후부터 사용할 수 있어요
  baseDirectory: import.meta.dirname,
  recommendedConfig: js.configs.recommended,
})

const eslintConfig = [
  ...compat.config({
    extends: ['eslint:recommended', 'next'],
  }),
]

export default eslintConfig
```

여기서 핵심은 `FlatCompat`를 사용해 기존의 ESLint 설정(예: `eslint:recommended`)과 `next` 설정을 안전하게 병합하는 거예요. 이렇게 하면 `next`의 설정이 나중에 적용돼서 우선순위도 제대로 조절되고, 설정 충돌을 미연에 방지할 수 있죠.

참고로, `import.meta.dirname`은 상대 경로 같은 디렉토리 정보를 제공하는데, 이 기능을 사용하려면 Node.js가 v20.11.0 이상이어야 한다는 점 기억해 주세요!

이 방법을 활용하면 Next.js 프로젝트나 여러 ESLint 설정을 동시에 관리하는 환경에서 훨씬 깔끔하고 효율적으로 ESLint 구성을 유지할 수 있습니다.

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

다음 설정(next configuration)은 parser, plugins, settings 속성에 대한 기본값을 이미 잘 처리해주고 있어요. 그러니 특별히 다른 설정이 필요하지 않은 한, 이 속성들을 다시 선언할 필요는 없답니다.

만약 다른 공유 가능한 설정(shareable configuration)을 포함한다면, 이 속성들이 덮어쓰거나 수정되지 않도록 잘 관리해줘야 해요. 그렇지 않으면, 기본적으로 next 설정과 겹치는 부분이 있을 수 있어서 혼란이 생길 수 있거든요.

간단하게 정리하면, 가능하면 next 설정과 중복되는 다른 설정들은 빼고, 필요하다면 Next.js ESLint 플러그인에서 직접 확장하는 게 가장 깔끔한 방법이에요.

개발하다 보면 설정이 꼬일 때가 있는데, 이렇게 기본값을 잘 활용하면 깔끔하고 유지보수하기 편한 ESLint 설정을 만들 수 있으니 참고하세요!