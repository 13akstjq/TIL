---
title: "Create React App에서 Next.js 15로 마이그레이션하는 과정 정리"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:26
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to migrate from Create React App to Next.js"
link: "https://nextjs.org/docs/app/guides/migrating/from-create-react-app"
isUpdated: false
---


# Create React App에서 Next.js로 마이그레이션하는 방법

안녕하세요! 오늘은 여러분이 기존에 만들었던 Create React App(CRA) 프로젝트를 Next.js로 옮기는 과정을 쉽게 도와드리려고 해요. React로 만든 프로젝트를 좀 더 강력하고 효율적으로 운영하고 싶다면 Next.js를 고려해볼 만한데요, 그 이유부터 살펴볼게요.

## 왜 Next.js로 갈아탈까?

Create React App도 충분히 좋은 툴이지만, Next.js가 제공하는 몇 가지 장점 때문에 많은 분들이 이동하고 있어요:

| 이유 | 설명 |
|---|---|
| **서버 사이드 렌더링(SSR)** | Next.js는 서버에서 미리 페이지를 렌더링해서 SEO 최적화와 초기 로딩 속도를 개선해줍니다. 단순히 SPA보다 빠르고 검색 엔진 친화적이에요. |
| **파일 기반 라우팅** | 별도의 라우팅 설정 없이 `pages` 폴더 내 파일 이름만으로 라우팅이 자동 설정돼서 개발이 훨씬 수월해요. |
| **정적 사이트 생성(SSG)** | 빌드 시 HTML 페이지를 미리 생성해두고 빠르게 서빙할 수 있어요. 블로그나 홍보 사이트에 딱이죠. |
| **API 라우트 내장** | 백엔드 API도 Next.js 안에서 바로 만들 수 있어, 풀스택 개발까지 한 번에! |
| **이미지 최적화** | Next.js 에서 제공하는 `<Image />` 컴포넌트로 자동 이미지 최적화 기능을 쓸 수 있어요. |

이 외에도 커뮤니티 지원, 최신 React 기능과 완벽 연동 등 다양한 이유가 있지만, 대략 이대로만 알아도 충분히 넘어갈 동기부여가 되죠?

다음에는 본격적으로 CRA 프로젝트를 Next.js로 옮기는 구체적인 단계를 천천히 살펴볼게요. 

궁금한 점 있으면 댓글로 알려주세요!

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

### 느린 초기 페이지 로딩 시간

Create React App은 순수하게 클라이언트 사이드 React를 사용해요. 이렇게 클라이언트 사이드에서만 동작하는 앱, 즉 싱글 페이지 애플리케이션(SPA)은 초기 페이지 로딩이 느려질 때가 많답니다. 왜 그런지 간단히 살펴볼게요.

- 브라우저가 React 코드와 여러분의 애플리케이션 전체 번들을 다 다운로드하고 실행해야만, 데이터 요청을 보낼 수 있어요.
- 그리고 새로운 기능이나 라이브러리를 추가할수록 애플리케이션 코드가 점점 커지고요.

이런 이유들 때문에 처음 앱을 켤 때 로딩 속도가 느려질 수 있죠.

---

### 자동 코드 분할(Automatic Code Splitting)이 없어요

Create React App은 기본적으로 자동 코드 분할 기능을 제공하지 않아요. 자동 코드 분할이란, 사용자가 필요로 하는 코드만 그때그때 불러오는 기술을 말하는데요, 이렇게 하면 초기 다운로드 크기를 줄여서 빠르게 화면을 보여줄 수 있습니다.

하지만 Create React App에서는 이런 기능이 자동으로 적용되지 않다 보니, 모든 코드가 한꺼번에 번들링되어 초기 로딩 시간이 길어지는 문제가 생기죠.

---

#### 추가 팁! 빠른 로딩을 위한 대안

- **React.lazy와 Suspense 활용하기:** 수동으로 코드 분할을 적용해 볼 수 있어요. 페이지나 컴포넌트를 동적으로 불러와 초기 로딩 시간을 줄일 수 있답니다.
- **서버 사이드 렌더링(SSR) 도입:** Next.js 같은 프레임워크를 사용하면 서버에서 미리 HTML을 만들어 보내주기 때문에, 사용자는 빠르게 콘텐츠를 볼 수 있어요.
- **번들 사이즈 최적화:** 사용하지 않는 라이브러리를 제거하거나, 필요한 부분만 가져오는 트리 쉐이킹(tree shaking)을 신경 써서 코드 크기를 줄여보세요.

---

이렇게 SPA에서 초기 로딩 시간이 느린 원인과 해결 방안을 알아봤는데, 여러분의 프로젝트 상황에 맞게 적절한 방법을 골라 적용해 보세요. 웹 앱 사용성 개선에 큰 도움이 될 거예요!

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

이전 글에서 언급한 느린 로딩 문제는 코드 분할(code splitting)로 어느 정도 해결할 수 있어요. 하지만 직접 수동으로 코드 분할을 하려고 하면 오히려 네트워크 워터폴(network waterfall) 같은 비효율적인 요청 흐름이 생길 수 있습니다. 다행히 Next.js는 라우터와 빌드 파이프라인에 자동 코드 분할과 트리 쉐이킹(tree shaking)을 기본으로 지원해서 이런 문제를 덜어줍니다.

### 네트워크 워터폴(Network waterfalls)이란?

네트워크 워터폴은 앱이 클라이언트와 서버 사이에 연속적으로 요청을 보내면서 성능이 저하되는 상황을 말해요. SPA에서 흔히 볼 수 있는 패턴 중 하나는, 컴포넌트가 먼저 플레이스홀더(자리 표시자)를 렌더링하고, 그 후에 데이터를 가져오는 식이죠. 그런데 문제는, 자식 컴포넌트가 데이터를 가져오기 시작하려면 부모 컴포넌트가 자기 데이터를 다 받아야 하니까 여러 요청이 순차적으로 이어지면서 딜레이가 예상보다 커지는 거에요. 이게 바로 '워터폴'이에요.

Next.js는 클라이언트 사이드에서 데이터 패칭을 지원하지만, 더욱 중요한 점은 서버 사이드로 데이터 패칭을 옮길 수 있게 해 준다는 겁니다. 이렇게 서버에서 미리 데이터를 받아오면, 클라이언트-서버 간의 여러 번의 연속 요청이 아예 없어지거나 크게 줄어들어 워터폴 문제가 대부분 해결되죠.

덧붙여서, Next.js의 getServerSideProps나 getStaticProps 같은 서버 데이터 페칭 메서드를 이용하면 SEO에도 유리하고, 사용자 입장에서도 초기 페이지 로딩 속도가 확실히 좋아지는 걸 체감할 수 있으니 꼭 활용해보시길 추천해요!

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

### 빠르고 의도적인 로딩 상태 관리

React Suspense 덕분에 스트리밍 기능이 내장되어 있어서, UI의 어떤 부분을 먼저 로드할지 그리고 어떤 순서로 로드할지 직접 정할 수 있어요. 덕분에 네트워크 병목 현상을 줄이고, 페이지가 훨씬 빠르게 로딩되도록 만들 수 있죠.

또한 이렇게 하면 화면이 갑자기 바뀌는 레이아웃 시프트(layout shift)를 최소화해서 사용자 경험을 한층 더 부드럽게 만들 수 있답니다.

### 데이터 가져오기 전략 선택하기

여러분이 데이터를 어떻게 가져올지, 즉 데이터 패칭 전략(data fetching strategy)을 선택하는 것도 중요한 부분이에요. API 호출을 어디서, 어떻게 할지 결정하면 성능과 사용자 경험 모두에 큰 영향을 끼치거든요.

예를 들어, 서버 사이드 렌더링(SSR), 정적 사이트 생성(SSG), 클라이언트 사이드 렌더링(CSR) 같은 다양한 패턴 중 상황에 맞는 방법을 선택해서 사용하는 것이죠.

---

만약 React Suspense와 데이터 패칭 방법에 대해 더 궁금하시다면, 이 두 가지를 조합해서 어떻게 더욱 빠르고 안정적인 웹 앱을 만드는지 사례와 함께 설명해 드릴게요!

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

Next.js는 필요에 따라 페이지나 컴포넌트 단위로 데이터 가져오기 방식을 선택할 수 있게 해줘요. 예를 들어, CMS에서 데이터를 가져와 빌드 시점에 블로그 글을 렌더링하는 방식을 사용할 수 있는데, 이걸 SSG(Static Site Generation)라고 부르고요. 이렇게 하면 페이지 로딩 속도가 훨씬 빨라집니다. 반면, 사용자 요청 시점에 데이터를 가져오는 SSR(Server Side Rendering)을 써야 할 경우도 있죠. 이건 예를 들어, 사용자마다 다른 데이터를 보여줘야 할 때 유용해요.

### 미들웨어 (Middleware)

Next.js의 미들웨어는 서버에서 요청이 완성되기 전에 코드를 실행할 수 있게 해줘요. 예를 들어보면, 로그인한 사용자만 볼 수 있는 페이지에서 비로그인 상태의 사용자가 접근하려 할 때, 미들웨어에서 로그인 페이지로 바로 리다이렉트 시켜 ‘비인증 콘텐츠가 잠깐 보이는 현상(플래시)’을 막을 수 있죠. 이외에도 A/B 테스트, 실험 기능, 다국어 지원 등에도 활용할 수 있어요.

### 내장 최적화 기능 (Built-in Optimizations)

Next.js는 기본적으로 다양한 최적화 기능들이 내장돼 있어요. 예를 들어, 이미지를 자동으로 최적화하거나, 코드 분할을 통해 불필요한 코드가 로드되지 않게 하죠. 이런 최적화 덕분에 개발자는 성능에 크게 신경 쓰지 않아도 훌륭한 사용자 경험을 제공할 수 있습니다.

> 개인적으로는 미들웨어를 적극적으로 활용하는 걸 추천해요. 미들웨어 덕분에 서버 단에서 많은 제어가 가능해지고, 보안이나 UX 향상에 큰 도움이 되니까요.

추가로 궁금한 점 있으면 언제든 질문해 주세요!

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

이미지, 폰트, 그리고 서드파티(3rd-party) 스크립트는 애플리케이션 성능에 큰 영향을 미칠 수 있어요. 다행히 Next.js에는 이런 리소스들을 자동으로 최적화해주는 특화된 컴포넌트와 API들이 내장되어 있답니다.

## 마이그레이션 단계

우리 목표는 최대한 빠르게 동작하는 Next.js 애플리케이션을 만드는 거예요. 그렇게 하면 Next.js의 여러 기능을 한꺼번에 도입하는 대신, 단계적으로 천천히 적용해 나갈 수 있답니다. 처음에는 기존 라우터를 바로 바꾸지 않고, 순수 클라이언트 사이드 애플리케이션(SPA)처럼 다루면서 시작할 거예요. 이렇게 하면 복잡함도 줄어들고, 코드 머지할 때 충돌도 최소화할 수 있죠.

> 참고: 만약 CRA(Create React App)에서 package.json의 커스텀 홈페이지 설정, 서비스 워커, 또는 Babel/webpack 세부 조정 같은 고급 설정을 사용 중이라면, 이 가이드 맨 마지막 ‘추가 고려사항’ 섹션을 꼭 확인해 보세요. Next.js로 넘어갈 때 이런 설정들을 어떻게 비슷하게 구현하거나 다르게 적용할지 도움이 될 거예요.

---

그리고 한 가지 덧붙이자면, Next.js의 이미지 최적화 컴포넌트(next/image)는 기본적으로 레이지 로딩(lazy loading)을 지원해서 초기 로드 시간을 줄여주고, 폰트 최적화(next/font)는 폰트 로딩 방식도 개선해 줘서 사용자 경험 향상에도 한 몫 한답니다. 그러니 마이그레이션하면서 자연스럽게 퍼포먼스도 같이 챙길 수 있다는 점, 꼭 기억하세요!

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

### Step 1: Next.js 설치하기

기존 프로젝트에 Next.js를 설치하려면 터미널에 아래 명령어를 입력하세요:

```bash
npm install next@latest
```

여기서 `next@latest`는 최신 버전의 Next.js를 설치한다는 의미예요. Next.js는 React 기반의 프레임워크로, 서버 사이드 렌더링(SSR)과 정적 사이트 생성(SSG)을 지원해서 빠르고 SEO 친화적인 웹사이트를 만들 때 유용하답니다.

### Step 2: Next.js 설정 파일 만들기

Next.js를 제대로 활용하려면 프로젝트 루트에 `next.config.js` 파일을 만들어서 필요한 설정을 추가해야 해요. 예를 들어, 커스텀 환경변수나 빌드 옵션을 설정할 때 이 파일을 사용하죠.

```js
// next.config.js
module.exports = {
  reactStrictMode: true, // React의 엄격 모드 활성화
  // 여기서 추가 설정을 해보세요!
};
```

처음에는 간단하게 `reactStrictMode`만 설정해도 충분해요. 개발 중에 React의 잠재적인 문제를 알려주기 때문에 디버깅할 때 도움이 많이 된답니다.

> 참고로, Next.js에서는 `pages` 폴더 안에 React 컴포넌트를 파일로 작성하면 자동으로 라우팅이 설정되니까, 라우팅 관련 설정은 따로 안 해도 돼요.

다음 단계에서는 페이지를 만들어서 직접 Next.js의 구조를 체험해보면 좋아요!

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

프로젝트 루트 폴더(보통 package.json 파일과 같은 위치)에 `next.config.ts` 파일을 만들어 주세요. 이 파일은 Next.js의 설정 옵션을 담는 곳이에요.

```ts
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  output: 'export', // SPA(Single-Page Application)로 결과물을 내보내겠다는 뜻이에요.
  distDir: 'build',   // 빌드 결과물이 저장되는 폴더를 기본 '.next' 대신 'build'로 바꿔줘요.
}

export default nextConfig
```

> 참고! 여기서 `output: 'export'`를 설정하면, 정적 사이트로 내보내는 거예요. 그래서 서버사이드 렌더링(SSR)이나 API 같은 서버 기능은 사용할 수 없답니다. 만약 이런 기능들이 필요하면 이 줄을 삭제해 주세요.

---

다음 단계로 루트 레이아웃 파일을 만드는 작업이 이어집니다! 필요하면 알려드릴게요 :)

---

추가 팁!

- `distDir` 옵션을 바꾸면 빌드 폴더 이름이 바뀌어서, 배포 시 원하는 구조로 폴더를 관리하기 좋습니다.
- Next.js에서 보통 SSR을 쓰냐 정적 export를 쓰냐는 프로젝트 성격에 따라 다르니, `output: 'export'` 설정 전에 어느 방식을 쓸지 고민해보시는 걸 추천해요.

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

Next.js 앱 라우터에서 꼭 필요한 게 바로 루트 레이아웃 파일이에요. 이 파일은 React Server Component 형태로 되어 있고, 앱의 모든 페이지를 감싸는 역할을 하죠.

만약 CRA(Create React App) 환경에서 작업해보셨다면, 이 루트 레이아웃과 가장 비슷한 게 `public/index.html` 파일이에요. 기본적인 `html`, `head`, `body` 태그를 포함하고 있어서 앱의 뼈대를 담당하죠.

그럼 Next.js에서 어떻게 만들면 될지 한번 볼게요!

1. 우선 프로젝트 루트(또는 `src` 폴더 안)에 `app` 이라는 새 디렉터리를 만들어 주세요.

2. 그 안에 `layout.tsx` 혹은 `layout.js` 파일을 만들고, 아래처럼 작성하면 돼요:

```tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="ko">
      <head>
        <title>내 Next.js 앱</title>
      </head>
      <body>
        {children}
      </body>
    </html>
  )
}
```

여기서 중요한 점은, 이 루트 레이아웃이 실제로 여러분의 페이지들을 감싸주는 최상위 레이아웃이라는 거예요. 그리고 `children`은 자식 컴포넌트, 즉 각 페이지들의 내용을 의미하죠.

추가로, Next.js 13 App Router에서는 이 레이아웃이 서버에서 렌더링되는 ‘서버 컴포넌트’ 형태라서 SEO 최적화나 초기 렌더링 성능에 유리하답니다.

**팁!**  
`<head>` 태그 안에는 메타태그, 폰트 로드, SEO 설정 등도 함께 넣을 수 있으니 필요에 따라 꼭 활용해 보세요!  
그리고 다국어 지원을 하려면 `<html lang="en">` 처럼 `lang` 속성에도 신경 써주시면 좋고요.

한마디로, Next.js 13 App Router의 `app/layout.tsx`는 옛날에 `index.html`이 맡았던 역할을 React 방식으로 훨씬 깔끔하고 유연하게 대체했다고 보면 됩니다.  
이제 뼈대부터 잘 잡고 멋진 Next.js 프로젝트 시작해봐요!

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

이제 예전의 index.html 내용을 이 `RootLayout` 컴포넌트로 옮겨보세요. 특히 body 태그 안에 있던 `div#root`와 `noscript` 부분은 다음처럼 `div id="root"` 안에 `children`을 넣는 형태로 바꾸면 됩니다.

```tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <head>
        <meta charSet="UTF-8" />
        <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <title>React App</title>
        <meta name="description" content="Web site created..." />
      </head>
      <body>
        <div id="root">{children}</div>
      </body>
    </html>
  )
}
```

> 참고: Next.js는 기본적으로 CRA(Create React App)의 `public/manifest.json`, 추가 아이콘들, 그리고 테스트 설정을 무시해요. 만약 이런 것들이 필요하다면, Next.js가 제공하는 Metadata API나 테스트 설정 기능을 활용해 설정할 수 있습니다.

---

### Step 4: Metadata

다음은 메타데이터 설정 단계입니다. Next.js에서는 메타데이터를 효과적으로 관리할 수 있도록 도와주는 API가 있으니, 이를 활용하면 SEO나 웹사이트 정보 관리가 더 편리해집니다.

> 추가 팁: `RootLayout` 같은 전역 레이아웃에서 메타태그를 관리하면 페이지마다 일일이 설정하지 않아도 되고, 일관성 유지에도 좋아요.

필요하면 제가 메타데이터 설정 방법도 따로 정리해 드릴게요!

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

Next.js를 사용하다 보면, `<head>` 태그 안에 `<meta charset="UTF-8" />`나 `<meta name="viewport" content="width=device-width, initial-scale=1" />` 같은 태그들을 직접 넣지 않아도 자동으로 포함된다는 점을 알게 될 거예요. 그래서 불필요하게 중복되니까, 이런 태그들은 제거해도 OK입니다.

예를 들어, 처음에는 이렇게 작성할 수도 있죠:

```jsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <head>
        <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
        <title>React App</title>
        <meta name="description" content="Web site created..." />
      </head>
      <body>
        <div id="root">{children}</div>
      </body>
    </html>
  )
}
```

근데 사실 favicon.ico나 icon.png, robots.txt 같은 메타데이터 파일들을 `app` 디렉토리 최상단에 넣어두면, Next.js가 자동으로 `<head>` 태그에 넣어줘요. 그러니 직접 `<link>` 태그로 선언할 필요가 없습니다.

실제로 앱 디렉토리 내에 이런 파일들을 위치시키고 나서는, `<link rel="icon" ... />` 부분도 지워서 훨씬 깔끔하게 만들 수 있죠:

```jsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <head>
        <title>React App</title>
        <meta name="description" content="Web site created..." />
      </head>
      <body>
        <div id="root">{children}</div>
      </body>
    </html>
  )
}
```

----

### 요점 정리

| 항목                            | 설명                                                         |
|-------------------------------|------------------------------------------------------------|
| charset, viewport 메타 태그     | Next.js가 자동으로 포함해 주므로 직접 넣지 않아도 된다.                  |
| favicon.ico, icon.png, robots.txt 등 | `app` 디렉토리 최상단에 넣어두면 자동으로 `<head>`에 추가됨. 직접 `<link>` 태그 불필요. |
| `title`, `meta description`   | 직접 넣어야 하며, 앱에 맞게 커스터마이징 가능하다.                        |

이런 자동화 덕분에 `<head>` 태그 관리가 더 쉬워지고 코드도 깔끔해지니, 꼭 활용해보세요! 😊

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

마지막으로, Next.js에서는 Metadata API를 활용해서 `head` 태그를 관리할 수 있어요. 즉, 최종 메타데이터 정보를 `export`된 `metadata` 객체로 옮기면 된답니다.

```js
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'React App',
  description: 'Web site created with Next.js.',
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>
        <div id="root">{children}</div>
      </body>
    </html>
  )
}
```

위 코드처럼, 예전에는 `index.html` 안에서 직접 모든 메타데이터를 선언했지만, 이제는 Next.js의 프레임워크 내장 기능인 **Metadata API**를 이용하는 방식으로 전환했어요. 이 방법 덕분에 SEO(검색 엔진 최적화)와 웹 페이지 공유 시 필요한 메타 정보 관리가 훨씬 간편해집니다.

> 참고로, Metadata API는 페이지 단위로 메타데이터를 설정할 수 있어서, 각 페이지마다 다른 title이나 description을 쉽게 적용할 수 있다는 점도 큰 장점이에요. 또 오픈 그래프(Open Graph) 같은 소셜 미디어 공유 정보를 추가하는 것도 수월해요!

---

### Step 5: Styles

이제 다음 단계는 스타일을 적용하는 것이에요. Next.js에서는 CSS 모듈, 글로벌 CSS, 혹은 Tailwind CSS 같은 유틸리티 퍼스트 프레임워크 등 다양한 스타일링 방법도 지원하니, 여러분 프로젝트에 맞게 골라서 적용하면 됩니다. 앞으로 스타일을 입히는 방법도 차근차근 소개할게요!

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

CRA(Create React App)처럼 Next.js도 CSS Modules를 기본적으로 지원해요. 그리고 전역으로 적용되는 CSS 파일도 쉽게 불러올 수 있답니다.

만약에 전역 스타일을 관리하는 CSS 파일이 있다면, 보통 `app/layout.tsx` 파일에 이렇게 불러와요:

```tsx
import '../index.css'

export const metadata = {
  title: 'React App',
  description: 'Web site created with Next.js.',
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>
        <div id="root">{children}</div>
      </body>
    </html>
  )
}
```

여기서 중요한 점은 Next.js는 CSS를 모듈 방식으로도, 전역 스타일로도 사용할 수 있어서 스타일 관리가 훨씬 자유롭다는 거예요. 예를 들어 컴포넌트 단위로 스타일을 캡슐화하고 싶으면 CSS Modules를, 전체 사이트에 공통되는 스타일은 전역 CSS 파일로 관리하면 되죠.

그리고 만약 Tailwind CSS를 쓰고 싶다면, Next.js 공식 문서에서 설치 방법이 자세히 안내되어 있으니 꼭 참고하세요. Tailwind와 Next.js는 사실 찰떡궁합이라서, 빠르게 스타일링을 시작하기에 아주 좋아요!

개발하면서 CSS가 헷갈릴 때는 언제든 이렇게 전역과 모듈 스타일을 적절히 섞어가며 사용해보시길 추천드려요. 그럼 스타일 관리가 훨씬 편해질 거예요!

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

### 6단계: 진입점 페이지 만들기

Create React App에서는 `src/index.tsx`(또는 `index.js`)가 진입점(entry point)이었죠. 근데 Next.js(App Router)에서는 `app` 폴더 안에 있는 각 폴더가 각각의 라우트(경로)가 되고, 그 폴더마다 `page.tsx` 파일이 꼭 있어야 해요.

여기서 우리 목표는 일단 앱을 SPA(싱글 페이지 애플리케이션)처럼 유지하면서 모든 라우트를 한 곳에서 가로채고 싶어요. 그래서 Next.js의 **optional catch-all route**를 사용할 거예요.

즉, `app` 폴더 안에 `[[...slug]]`라는 폴더를 새로 만들어주세요.

---

> **optional catch-all route** 쉽게 설명하자면?  
> URL 경로가 어떤 형태든 전부 이 곳으로 몰아넣겠다는 뜻이에요. 예를 들어 `/about`, `/posts/123` 같은 경로도 모두 이 `[[...slug]]` 폴더가 담당하게 되죠. 이렇게 하면 SPA처럼 라우팅 처리가 편해집니다.

추가로, Next.js에서 `app/[[...slug]]/page.tsx`를 만들면, 전체 경로를 하나로 묶어 관리하는 훌륭한 방법이 될 수 있답니다! 이 부분 잘 활용하면 복잡한 라우팅도 깔끔하게 처리할 수 있어요.

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

Next.js에서 `app` 디렉토리 구조를 이렇게 구성하고 있다고 가정해볼게요:


app
 ┣ [[...slug]]
 ┃ ┗ page.tsx
 ┣ layout.tsx


여기에서 `page.tsx`에 다음과 같은 내용을 추가하는 걸 보셨죠?

```js
export function generateStaticParams() {
  return [{ slug: [''] }]
}

export default function Page() {
  return '...' // 나중에 업데이트 예정
}
```

### 이 코드는 대체 무슨 뜻일까요?

- `generateStaticParams` 함수는 Next.js에게 어떤 경로들을 정적으로 생성할지 알려주는 역할을 해요.
- 여기서 `[{ slug: [''] }]`라는 배열을 반환해서 빈 문자열, 즉 루트 경로 `/`만 정적으로 생성한다는 뜻이죠.
- `[[...slug]]` 이건 *catch-all* 라우팅을 의미해요. 즉, `/`, `/about`, `/posts/1` 등 어떤 경로든 모두 이 페이지로 매핑됩니다.
- 하지만 이 상태에선 실제로 정적 경로는 빈 배열 `slug: ['']`만 있으니 루트 경로 하나만 빌드 타임에 정적으로 생성되고, 나머지는 동적으로 처리되는 구조입니다.

### 추가 팁!

- 이 파일(`page.tsx`)은 Server Component이므로, 빌드 타임에 HTML로 미리 렌더링 됩니다. 덕분에 SEO에 유리하고 초기 로딩 속도가 빠르죠.
- 만약 모든 경로를 정적으로 생성하려면 `generateStaticParams`에서 각각 필요한 `slug`를 다 반환해주면 됩니다. 하지만 그게 무척 많거나 동적이라면 이처럼 빈 경로로 제한하는 게 성능이나 빌드 시간에 유리할 수 있어요.

### 간단 정리

| 함수명                | 역할                                 |
|--------------------|----------------------------------|
| `generateStaticParams`   | Next.js가 정적으로 생성할 경로 목록 반환          |
| `export default Page`    | 해당 경로들에 대해서 보여줄 페이지 컴포넌트           |
| `[[...slug]]`            | 모든 경로를 한 페이지로 처리하는 catch-all 라우팅   |

이 구조를 이해하고 나면 Next.js의 앱 라우팅과 정적 생성을 훨씬 잘 활용할 수 있답니다! 앞으로는 이 베이스 위에 실제 UI나 데이터를 넣어서 페이지를 만들어보세요.

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

### 7단계: 클라이언트 전용 엔트리포인트 추가하기

이번엔 CRA(Create React App)의 루트 App 컴포넌트를 Next.js의 클라이언트 컴포넌트 안에 넣어볼 거예요. 이렇게 하면 모든 로직이 클라이언트 사이드에서만 실행되도록 할 수 있답니다.

Next.js 경험이 처음이라면 참고할 점! 클라이언트 컴포넌트라고 해서 무조건 클라이언트에서만 렌더링되는 건 아니에요. 기본적으로 Next.js는 클라이언트 컴포넌트도 서버에서 미리 렌더링(prerendered)해둡니다. 다만, 클라이언트 컴포넌트는 서버-rendering에 추가해서 클라이언트 자바스크립트를 실행할 수 있는 특징이 있죠.

자, 그럼 `app/[[...slug]]/` 폴더 안에 `client.tsx` (또는 `.js`) 파일을 만들어주세요.

```tsx
'use client'

import dynamic from 'next/dynamic'

const App = dynamic(() => import('../../App'), { ssr: false })

export function ClientOnly() {
  return <App />
}
```

여기서 중요한 점!  
`dynamic`의 `ssr: false` 옵션을 줘서 서버 사이드 렌더링(SSR)을 완전히 끄고 클라이언트에서만 렌더링하도록 설정했어요. 이렇게 하면 서버에서는 아예 렌더링 과정이 스킵되니, 오직 브라우저에서만 `App`이 실행되겠죠.

이 방식은 특히 CRA에서 작성한 컴포넌트를 Next.js로 옮길 때 유용해요. CRA는 기본적으로 클라이언트 사이드 앱이라 SSR을 고려하지 않거든요. 이처럼 Next.js의 클라이언트 컴포넌트를 활용하면 기존 코드를 수정하지 않고도 Next.js 프로젝트에 자연스럽게 통합할 수 있습니다.

추가 팁!  
만약 클라이언트 전용 컴포넌트가 많아지면, 이런 클라이언트 전용 래퍼 컴포넌트를 만들어 재사용하는 것도 좋은 방법이에요. 그래야 중복 코드도 줄이고 유지보수도 편해지니까요.

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

- `use client` 지시어는 해당 파일을 클라이언트 컴포넌트로 만들어 줍니다.
- `dynamic` import에서 `ssr: false`를 설정하면 해당 컴포넌트는 서버 사이드 렌더링이 되지 않고 완전한 클라이언트 전용 컴포넌트(SPA)로 작동하게 됩니다.

이제 이 내용을 반영해서 `page.tsx` (또는 `page.js`)를 다음처럼 업데이트해보세요:

```js
import { ClientOnly } from './client'
 
export function generateStaticParams() {
  return [{ slug: [''] }]
}
 
export default function Page() {
  return <ClientOnly />
}
```

---

### 8단계: Static Image Import 업데이트하기

이제 정적 이미지 import를 업데이트할 차례입니다. Next.js에서는 이미지 최적화를 위해 `next/image` 컴포넌트를 많이 사용하는데요, 이 과정에서 이미지 import 방식을 다르게 해야 할 수도 있어요.

예를 들어, 기존에 이렇게 이미지 파일을 바로 import 했다면:

```js
import img from '../public/example.png'
```

Next.js에서는 이미지를 `next/image` 컴포넌트와 함께 쓰는 게 더 좋은데, 이렇게 바꿔볼 수 있답니다:

```js
import Image from 'next/image'

export default function MyComponent() {
  return (
    <Image
      src="/example.png"
      alt="예시 이미지"
      width={500}
      height={300}
    />
  )
}
```

이렇게 하면 기본적인 최적화(예: 레이지 로딩, 웹포맷 변환 등)를 자동으로 적용 받을 수 있어요.

---

추가 팁!  
이미지가 자주 변경되지 않는다면 `next.config.js`에서도 이미지 도메인 설정을 확인해보세요. 외부에서 이미지를 불러온다면 CORS 문제도 방지할 수 있답니다.

끝으로, 이미지 import와 관련해서 더 궁금한 게 있거나 실제 프로젝트에 적용해보고 싶다면 편하게 질문 주세요!

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

CRA(Create React App)와 Next.js에서 이미지 파일을 다루는 방식의 차이를 한번 살펴볼게요.

---

### CRA에서 이미지 import하기

CRA에서는 이미지 파일을 import 하면 이미지의 **공개 URL(문자열)**을 반환해요. 그래서 일반 `<img>` 태그의 `src` 속성에 바로 넣을 수 있죠.

```jsx
import image from './img.png';

export default function App() {
  return <img src={image} alt="example" />;
}
```

이 경우는 아주 직관적이고 사용하기 편해요. 이미지는 빌드 과정에서 public 경로로 복사되고, 이 URL이 변수에 저장됩니다.

---

### Next.js에서 이미지 import하기

반면 Next.js에서는 이미지 import 시 **객체**를 반환해요. 이 객체는 이미지의 여러 메타데이터(예: `src`, `height`, `width`)를 가지고 있죠. 그래서 Next.js의 `Image` 컴포넌트와 같이 쓸 수 있어요.

```jsx
import image from './img.png';
import Image from 'next/image';

export default function App() {
  return <Image src={image} alt="example" />;
}
```

만약 기존에 사용하던 `<img>` 태그를 그대로 쓰고 싶으면 `image.src`처럼 객체에서 `src` 속성만 꺼내서 쓸 수도 있어요.

```jsx
import image from './img.png';

export default function App() {
  return <img src={image.src} alt="example" />;
}
```

---

### Next.js `Image` 컴포넌트가 좋은 이유

- **자동 이미지 최적화**: 이미지를 자동으로 최적화해서 브라우저에 적합한 형태로 보여줘요.
- **너비와 높이 자동 설정**: 이미지 원본 크기 기반으로 `width`, `height` 속성을 자동으로 넣어줍니다. 그래서 이미지 로딩 시 레이아웃이 흔들리는 현상(Cumulative Layout Shift)을 줄여줘요.
- **반응형 및 로딩 지연 기능**: 화면 크기에 맞춰 크기를 조절하거나, 이미지 로딩을 지연시키는 기능도 내장되어 있습니다.

---

### 알아두면 좋은 팁: styled 이미지와 레이아웃 이슈

`Image` 컴포넌트가 자동으로 `width`와 `height` 속성을 넣어주는 덕분에 레이아웃 안정성이 좋아지지만, **이미지 스타일링에 주의해야 해요**.

- 예를 들어 너비만 고정하고(가로 크기만 지정), 높이는 CSS에서 자동(`auto`)으로 두지 않으면,
- 그 높이는 원본 이미지 크기가 아닌 `Image` 컴포넌트가 자동으로 지정한 높이 값으로 설정되면서,
- 이미지가 찌그러지거나 늘어져 보일 수 있어요.

그럴 때는 `height`도 `auto`로 설정하거나, 둘 다 명확한 값을 주도록 CSS를 조절하는 게 좋습니다.

---

### 간단 비교 표

| 구분                    | CRA                        | Next.js                       |
|-------------------------|----------------------------|-------------------------------|
| 이미지 import 결과      | 문자열(URL)                 | 객체({ src, width, height })  |
| 이미지 태그             | `<img src={image} />`       | `<Image src={image} />` or `<img src={image.src} />` |
| 이미지 최적화           | 별도 설정 필요               | 자동 제공                       |
| 너비/높이 속성 자동 지정 | X                          | O                              |
| 레이아웃 흔들림 방지      | 브라우저 기본 영향           | 자동 설정으로 방지 가능         |

---

요약하자면, Next.js는 이미지 최적화와 레이아웃 안정성에 초점을 맞춘 방식이에요. 기존 CRA 방식에 익숙해도 Next.js 방식으로 넘어오면 이런 점들을 기억하면 좋겠죠?  

그럼 다음에도 개발에 꿀팁들로 찾아올게요! 😊

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

이미지를 다룰 때 `img` 태그를 계속 사용하면 애플리케이션에 변경 사항을 최소화할 수 있어서 예상치 못한 문제를 줄일 수 있어요. 나중에 여유가 된다면 `Image` 컴포넌트로 천천히 옮겨가면서, 이미지 최적화를 위한 로더 설정을 하거나 Next.js 기본 서버에서 제공하는 자동 이미지 최적화 기능을 활용할 수 있답니다.

그리고 `/public` 폴더에서 이미지를 가져올 때 절대 경로 대신 상대 경로를 쓰는 게 좋아요. 이렇게 하면 경로 문제가 훨씬 덜 생기고, 프로젝트 구조가 좀 더 명확해지거든요:

```js
// Before
import logo from '/logo.png'

// After
import logo from '../public/logo.png'
```

또한, `img` 태그에 이미지를 넘겨줄 땐 이미지 객체 전체를 넘기지 말고, **`src` 속성에 이미지 경로만 넘기는 것**이 좋습니다. 이렇게 하면 불필요한 데이터가 전달되는 걸 막을 수 있거든요.

예를 들어:

```jsx
// Bad
<img {...logo} alt="logo" />

// Good
<img src={logo.src} alt="logo" />
```

추가로, Next.js의 `Image` 컴포넌트를 쓰면 이미지 로딩 최적화, 레이지 로딩, 적절한 크기 조절 등이 자동으로 처리돼서 퍼포먼스에 큰 도움이 돼요. 처음에는 조금 복잡해 보일 수 있지만, 한두 번 써보면 금방 익숙해질 거예요. 필요에 따라서 점진적으로 바꿔가면서 적용해보세요!

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

앞서 예제로 보여준 이미지 태그 변경 방식에 대해 조금 더 풀어서 설명해볼게요.

```jsx
// Before
<img src={logo} />

// After
<img src={logo.src} />
```

보통 Next.js나 modern 프레임워크에서 이미지를 다룰 때, 그냥 `logo`만 넘기면 문제가 생길 수 있어요. 그래서 실제 이미지 URL을 가리키는 `logo.src`를 명시해줘야 하죠.

또 다른 방법으로는, 이미지 자산을 `public` 폴더에 넣어서 사용하는 거예요. 예를 들어 `public/logo.png`파일을 두면, HTML에 `<img src="/logo.png" />`처럼 경로를 쓸 수 있고, 이 경우 코드에서는 `src="/logo.png"`라고 작성하면 돼요.

---

### 타입스크립트를 쓴다면 주의할 점!

이미지 객체에서 `.src`프로퍼티를 사용하면 타입 에러가 뜰 수 있어요. 이럴 땐 `tsconfig.json` 파일에 `next-env.d.ts`를 포함시켜줘야 하는데요,

```json
// tsconfig.json 일부 예시
{
  "include": [
    "next-env.d.ts",
    "**/*.ts",
    "**/*.tsx"
  ]
}
```

`next-env.d.ts` 파일은 Next.js가 자동으로 생성해주니까, 앱을 한번 실행하면 생성돼요.

---

### 참고로, Step 9: 환경변수 마이그레이션

이제 다음 단계인 9단계는 환경 변수 마이그레이션이에요. 환경 변수는 API 키나 시크릿 같이 숨겨야 하는 정보를 관리할 때 꼭 필요한데, 마이그레이션하면서 설정법이 약간 달라질 수 있으니 꼼꼼히 확인하세요.

---

### 팁!

- Next.js에서 `public` 폴더를 쓰면, 이 폴더에 있는 파일은 경로 그대로 접근할 수 있으니 간단하게 이미지나 favicon 같은 정적 자산을 관리하기 좋아요.
- 만약 이미지를 import해서 쓰고 싶으면, Next.js Image 컴포넌트(`<Image />`)를 사용하는 것도 성능 최적화에 도움이 됩니다.

이렇게 하면 쉽게 이미지 경로 문제와 타입스크립트 오류를 해결 할 수 있어요. 다음 포스팅에서는 환경변수 마이그레이션에 대해 차근차근 알려드릴게요!

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

Next.js에서 환경변수 사용법은 Create React App(CRA)과 비슷한데, 브라우저에서 노출하려는 변수에는 반드시 `NEXT_PUBLIC_` 접두사를 붙여야 해요.

여기서 가장 큰 차이는 클라이언트 쪽에 노출할 환경변수에 어떤 접두사를 쓰느냐입니다. CRA에서는 `REACT_APP_` 을 사용했지만, Next.js에서는 이걸 전부 `NEXT_PUBLIC_` 으로 바꿔줘야 해요.

---

### Step 10: package.json 스크립트 업데이트하기

`package.json` 안의 스크립트도 Next.js에 맞게 수정해줘야 합니다. 그리고 `.next` 폴더랑 `next-env.d.ts` 파일은 깃에 커밋할 필요가 없으니 `.gitignore`에 꼭 추가해 주세요.

---

아래는 예시로 바꿔줄 수 있는 문법입니다:

| 기존 (CRA)                            | 변경 후 (Next.js)                             |
|-------------------------------------|---------------------------------------------|
| `REACT_APP_API_URL=http://localhost`| `NEXT_PUBLIC_API_URL=http://localhost`      |
| `npm start`                         | `next start`                                |
| `npm run build`                    | `next build`                                |
| `npm run dev`                      | `next dev`                                  |

---

`.gitignore`에 다음 내용 추가하기:


.next/
next-env.d.ts


---

**참고로**  
Next.js는 서버 사이드 렌더링(SSR)도 지원하기 때문에, 서버에서만 사용할 환경변수와 클라이언트에서도 필요한 환경변수를 구분해서 관리하면 더 효율적입니다. 예를 들어, DB 비밀번호같은 민감한 정보는 `NEXT_PUBLIC_` 없이 관리해서 클라이언트로 노출되지 않도록 하세요.

그럼 다음 단계에서는 Next.js 프로젝트에서 환경변수를 어떻게 효율적으로 사용하는지, 그리고 SSR과의 연계 방법도 살펴보겠습니다!

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

안녕하세요! 오늘은 Next.js 프로젝트에서 자주 볼 수 있는 스크립트 설정과 관련된 내용을 간단히 정리해볼게요.

먼저 `package.json` 파일에 다음과 같은 스크립트가 있어요:

```json
{
  "scripts": {
    "dev": "next dev --turbopack",
    "build": "next build",
    "start": "npx serve@latest ./build"
  }
}
```

### 스크립트 설명
- `dev`: 개발 모드로 실행하는 스크립트예요. `next dev --turbopack` 명령어는 Next.js 개발 서버를 Turbopack 빌드러로 실행하게 해주는데, Turbopack은 Webpack보다 훨씬 빠른 빌드 속도를 자랑하는 차세대 번들러입니다. 아직 완전한 안정화 단계는 아니지만, 속도가 매우 빠르니 한 번 써보시는 걸 추천해요!
- `build`: 프로젝트를 빌드합니다. 실제 서비스용으로 준비할 때 사용해요.
- `start`: 빌드된 결과물을 실행하는 명령어인데, 여기선 `serve` 라는 간단한 HTTP 서버를 실행해서 `./build` 폴더 내부를 서비스하도록 되어 있어요. 이 부분은 간단한 정적 서버를 실행하고 싶을 때 유용해요.

---

그 다음, 프로젝트 폴더 안에는 다음과 같은 파일 or 폴더가 있을 텐데요:

```js
.next
next-env.d.ts
```

- `.next` 폴더는 Next.js가 빌드하면서 자동으로 생성하는 캐시 및 빌드 결과물 폴더입니다. 이 폴더는 직접 수정하지 않는 게 좋아요.
- `next-env.d.ts`는 TypeScript를 사용할 때 Next.js에서 자동으로 생성하는 환경 설정 파일이에요. typings 관련 설정이 들어있죠.

---

### 실행 방법
모든 셋팅이 끝나면, 터미널에서 아래 명령어로 개발 서버를 켤 수 있어요:

```bash
npm run dev
```

이 명령어가 실행되면, `localhost:3000`(기본값)에서 Next.js 애플리케이션을 확인할 수 있습니다!

---

### 추가 팁!

1. **Turbopack 사용 시 주의점**  
   아직 Turbopack은 실험 단계이므로 일부 패키지나 기능이 제대로 동작하지 않을 수 있어요. 안정적인 개발 환경을 원한다면 `next dev` 명령어를 그냥 쓰거나, 옵션 없이 실행하는 것도 고려해보세요.

2. **`start` 스크립트 변경하기**  
   `npx serve`는 설치없이 바로 사용할 수 있어 편리하지만, 실제 프로덕션 서버에서는 [PM2](https://pm2.keymetrics.io/) 같은 프로세스 매니저를 쓰거나, Next.js가 자체적으로 제공하는 `next start` 명령어(서버 사이드 렌더링 가능)를 사용하는 게 더 적합합니다.

3. **환경 설정**  
   Next.js를 본격적으로 다룰 때는 `.env.local` 같은 환경 변수 파일을 사용해 API 키나 민감한 정보를 관리하는 것도 중요해요.

오늘은 Next.js 프로젝트에서 자주 쓰이는 기본 스크립트와 환경을 정리해봤어요. 다음에 더 재밌는 내용으로 찾아올게요! :)

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

http://localhost:3000 에 접속해보세요. 그러면 Next.js로 실행 중인 여러분의 애플리케이션이 (SPA 모드로) 잘 돌아가는 걸 확인할 수 있을 거예요.

### 11단계: 정리 작업하기

이제 Create React App에서 사용하던 것들을 정리할 차례입니다. Next.js로 완전히 넘어왔으니 불필요한 파일과 설정들을 깔끔하게 치워주면 좋아요.

다음 항목들을 프로젝트에서 제거해 주세요:

| 제거해야 할 항목                        | 설명                                                 |
|-------------------------------------|----------------------------------------------------|
| `public/index.html`                  | CRA의 루트 HTML 파일로, Next.js에선 필요 없어요           |
| `src/index.tsx`                     | CRA의 진입점 파일, Next.js에선 `pages` 폴더가 담당해요 |
| `src/react-app-env.d.ts`             | CRA 사용 시 타입스크립트 환경설정용 파일                  |
| reportWebVitals 관련 설정               | 퍼포먼스 측정을 위한 CRA 전용 설정, 필요 없어요                |
| `react-scripts` 의존성 제거          | `package.json`에서 CRA를 위한 스크립트 의존성도 삭제하세요    |

이렇게 정리하면 프로젝트가 훨씬 깔끔해지고, Next.js에 맞는 환경으로 잘 정돈됩니다. 여기서 더 나아가, 만약 CRA에서 사용하던 스타일링이나 기타 라이브러리가 Next.js와 충돌난다면 버전이나 설정도 한 번 점검해 보세요.

정리 과정에서 궁금한 점 있으면 언제든 물어봐 주세요!

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

## 추가로 알아두면 좋은 점

### CRA에서 커스텀 홈페이지 경로 사용하기

만약 CRA(Create React App)에서 package.json의 `homepage` 필드를 이용해 앱을 특정 서브 경로(예: `/my-subpath`)에 배포했다면, Next.js에서도 비슷하게 설정할 수 있어요. Next.js는 `next.config.ts`에서 `basePath` 설정을 통해 이 기능을 제공한답니다!

예를 들어, 아래처럼 작성하면 `/my-subpath` 하위 경로에 앱을 서빙할 수 있어요:

```typescript
import { NextConfig } from 'next'

const nextConfig: NextConfig = {
  basePath: '/my-subpath',
  // 추가 설정들..
}

export default nextConfig
```

이렇게 설정하면 모든 페이지 경로가 `/my-subpath`를 기준으로 동작하게 돼요. 예를 들어, `/about` 페이지는 실제로 `/my-subpath/about`로 접근하는 식이죠.

> 참고로, `basePath`를 사용하면 정적 자원(css, 이미지 등) 경로도 함께 신경 써야 해요. 보통 Next.js가 알아서 처리하지만, 직접 경로를 지정할 때는 `basePath`를 붙여주는 걸 잊지 마세요!

---

### 추가 팁

- **환경별 경로 분기**: 개발 환경에선 `basePath`를 빈값으로 두고, 배포 환경에선 서브 경로를 지정하는 경우가 많아요. 이 경우 `next.config.ts`에서 환경변수로 분기 처리하는 걸 추천합니다.
- **Rewrite, Redirect와 함께 쓰기**: `basePath`는 URL 기본 경로를 바꾸는 설정이라, 리라이트나 리다이렉트를 할 때 함께 고려하면 UX를 향상시킬 수 있어요.
- **Custom 서버 없이 사용 가능**: CRA처럼 별도의 서버 설정 없이도 Next.js가 자체적으로 `basePath` 처리를 해주기 때문에 편리해요.

Next.js로 이전하면서 서브 경로 관련 설정 고민이라면, `basePath`를 꼭 살펴보세요!

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

### 커스텀 서비스 워커 다루기

만약에 Create React App(CRA)에서 제공하는 serviceWorker.js를 사용했다면, Next.js로 Progressive Web Application(PWA)을 만드는 방법도 알아두면 좋아요. CRA와 Next.js는 서비스 워커를 다루는 방식이 조금 다르지만, Next.js에서는 커스텀 서비스 워커를 직접 등록하고 관리할 수 있기 때문에 더 유연하게 사용할 수 있답니다.

예를 들어, CRA에서는 서비스 워커가 기본적으로 제공되면서 간단하게 PWA를 만들 수 있는 반면, Next.js에서는 `public` 폴더에 직접 서비스 워커 파일을 넣고, `_app.js` 혹은 커스텀된 `_document.js`에서 직접 등록 스크립트를 추가하는 형태로 작업해요. 그래서 더 맞춤형 접근이 가능하고 필요에 따라 기능을 확장하기 편하죠.

참고로, Next.js 공식 문서에도 PWA를 위한 별도 플러그인(next-pwa) 사용법이 소개되어 있는데, 이걸 활용하면 서비스 워커 등록과 캐시 관리가 더 쉬워져요. 관심 있다면 한번 살펴보시는 걸 추천드립니다!

---

### API 요청 프록시 처리하기

CRA에서 `package.json`에 `"proxy"` 필드를 넣어 API 요청을 백엔드 서버로 바로 보내는 방식 많이 써봤을 텐데요. Next.js에서는 이 역할을 `next.config.ts` 파일의 `rewrites`로 손쉽게 대체할 수 있어요.

아래는 예시인데요, 앞에 `/api`로 들어오는 요청을 실제 백엔드 API 서버 주소로 '재작성'해주는 역할을 합니다.

```ts
// next.config.ts
import { NextConfig } from 'next'

const nextConfig: NextConfig = {
  async rewrites() {
    return [
      {
        source: '/api/:path*', // Next.js 앱에서 /api/** 요청 감지
        destination: 'https://backend.example.com/api/:path*', // 실제 백엔드 서버 주소로 포워딩
      },
    ]
  },
}

export default nextConfig
```

이렇게 설정하면 클라이언트 쪽에서는 별다른 변경 없이 `/api/`로 요청을 보내면 되고, Next.js가 알아서 백엔드 서버랑 연결해 주니까 개발이 훨씬 편해져요.

> 그리고 한 가지 팁! 만약 배포 환경별로 백엔드 주소가 다르다면, 환경변수를 활용해 `destination` 값을 불러오는 방식을 추천해요. 이렇게 하면 개발/운영 환경을 쉽게 분리해서 관리할 수 있습니다.

---

이상으로 CRA에서 Next.js로 넘어오면서 서비스 워커와 API 프록시 처리를 어떻게 할 수 있는지 간단하게 정리해봤어요. 직접 해보면서 궁금한 점 있으면 언제든 물어봐 주세요!

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

Next.js에서 설정을 확장하거나 API 경로 리라이트를 적용하는 방법에 대해 알려드릴게요. CRA(Create React App)에서 custom webpack이나 Babel 설정을 하셨다면, Next.js에서는 next.config.ts 파일을 통해 비슷한 작업을 할 수 있습니다.

### API 요청 리라이트 설정하기

예를 들어, 프론트엔드에서 `/api/`로 시작하는 경로를 실제 백엔드 서버인 `https://your-backend.com`으로 리다이렉션하고 싶다면, 다음과 같이 `rewrites()` 함수를 사용하면 됩니다.

```ts
import { NextConfig } from 'next'

const nextConfig: NextConfig = {
  async rewrites() {
    return [
      {
        source: '/api/:path*',
        destination: 'https://your-backend.com/:path*',
      },
    ]
  },
}

export default nextConfig
```

**간단 설명:**

- `source`: 클라이언트에서 요청하는 경로 패턴입니다.
- `destination`: 실제 처리할 백엔드 서버 주소로, 여기서 `:path*`는 와일드카드 역할을 해서 `/api` 뒤의 경로가 그대로 유지됩니다.

이렇게 설정하면 서버와 클라이언트 도메인이 달라도 CORS 문제 없이 API 호출이 가능하답니다.

### 커스텀 Webpack 또는 Babel 설정 적용하기

CRA에서는 `webpack.config.js`나 `babel.config.js`를 직접 고치지만, Next.js에서 그런 설정을 건드리고 싶으면 `next.config.ts`에 `webpack` 설정을 추가하세요.

```ts
import { NextConfig } from 'next'

const nextConfig: NextConfig = {
  webpack: (config, { isServer }) => {
    // 여기에 커스텀 webpack 설정 작성하기
    // 예: 특정 플러그인 추가하거나 로더 수정 가능
    return config
  },
}

export default nextConfig
```

#### 추가 팁

- `config`는 기본 Next.js가 제공하는 webpack 설정 객체입니다.
- `isServer`는 해당 설정이 서버용인지 클라이언트용인지를 알려줘요.
- 필요에 따라 `config.plugins.push()`나 `config.module.rules.push()` 같은 방식으로 원하는 로더, 플러그인을 추가할 수 있습니다.

---

Next.js 설정은 이렇게 하면 되는데, 만약 Babel 설정도 손대고 싶다면 `babel.config.js` 혹은 `.babelrc` 파일을 루트에 만들어서 관리하는 게 좋아요. Next.js는 기본적으로 내장된 Babel 설정을 사용하지만, 커스텀하게 플러그인이나 프리셋을 추가하고 싶을 때는 별도로 설정 가능합니다.

Next.js로 마이그레이션하거나 커스텀 빌드 환경을 조절할 때, 이 부분만 잘 이해하면 성공적인 프로젝트 세팅에 많은 도움이 될 거예요! 혹시 더 궁금한 점 있으면 언제든 물어보세요. 🙂

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

> 참고: 이 작업을 하려면 dev 스크립트에서 --turbopack 옵션을 제거해서 Turbopack을 비활성화해야 합니다.

### TypeScript 설정하기

Next.js는 프로젝트에 tsconfig.json 파일이 있으면 자동으로 TypeScript 환경을 설정해줍니다. 이때 tsconfig.json 파일 내에서 `next-env.d.ts`가 반드시 include 배열에 포함되어야 하는데요, 이렇게 해줘야 Next.js가 타입 정보를 제대로 인식할 수 있어요. 아래처럼 설정하면 됩니다:

```json
{
  "include": ["next-env.d.ts", "app/**/*", "src/**/*"]
}
```

여기서 `next-env.d.ts` 파일은 Next.js가 자동 생성해주는 타입 선언 파일로, 꼭 포함되어 있어야 타입 체크가 정상적으로 작동해요. 만약 이 부분을 빼먹으면 간헐적으로 타입 에러가 발생할 수 있으니 꼭 확인하세요!

추가로, 앱 구조에 따라 `app/**/*`이나 `src/**/*` 대신 다른 폴더 경로를 넣어주셔도 되고, 프로젝트에 맞게 수정하시면 됩니다. 이처럼 include 배열 안에 TypeScript로 작성될 소스 파일 위치들을 모두 포함시켜야 빌드 시 누락 없이 타입 검사가 진행돼요.

TypeScript를 적용하면 코드 안정성도 높아지고, 자동완성 및 인텔리센스 지원도 좋아져서 개발이 훨씬 편해집니다. Next.js에서는 기본적으로 환경 세팅을 많이 도와주니 크게 신경 쓰지 않아도 되지만, 꼼꼼히 한 번 확인만 해주시면 되겠습니다!

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

## 번들러 호환성

Create React App과 Next.js는 기본적으로 웹팩(webpack)을 번들러로 사용해요. 다만, Next.js는 로컬 개발을 더 빠르게 할 수 있게 도와주는 새로운 번들러인 Turbopack도 지원하는데요, 이렇게 실행할 수 있어요:

```bash
next dev --turbopack
```

이게 바로 Next.js에서 Turbopack을 활성화하는 명령어입니다. Turbopack은 웹팩보다 훨씬 빠른 빌드 속도를 자랑해서, 특히 개발할 때 생산성을 높여줘요.

하지만 기존에 Create React App에서 쌓아온 웹팩 설정 같은 고급 설정을 그대로 옮기고 싶다면, Next.js에서도 커스텀 웹팩 설정을 적용할 수 있으니 걱정하지 마세요. 개발 환경에 따라 필요에 맞게 유연하게 쓸 수 있다는 점이 장점입니다!

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

## 다음 단계 (Next Steps)

잘 따라왔다면, 이제 Next.js 애플리케이션이 단일 페이지 애플리케이션(SPA)으로 제대로 동작하고 있을 거예요. 지금은 Next.js의 강력한 기능들, 예를 들어 서버 사이드 렌더링(SSR)이나 파일 기반 라우팅 같은 것을 활용하지 않은 상태인데요. 이제부터 천천히 하나씩 적용해나가면 됩니다!

### 앞으로 해볼 만한 것들

| 작업 | 설명 |
|---|---|
| React Router에서 Next.js App Router로 마이그레이션 | Next.js가 제공하는 **자동 코드 분할**, **스트리밍 서버 렌더링**, **React Server Components**를 활용할 수 있어요. |
| 이미지 최적화 | `Image` 컴포넌트를 사용하면 웹 성능 향상에 크게 도움이 됩니다. |
| 폰트 최적화 | `next/font`를 활용해 폰트를 최적화하면 페이지 로딩 속도가 더 빨라져요. |
| 서드파티 스크립트 최적화 | `Script` 컴포넌트를 사용해 외부 스크립트를 효율적으로 로딩하세요. |
| ESLint 활성화 | `npx next lint`를 실행해 Next.js 추천 규칙으로 ESLint를 설정하고, 프로젝트에 맞게 커스터마이징하면 코드 품질 유지에 좋습니다. |

### 참고 사항

> 지금 만약 `next.config.ts`에서 **출력 형식을 static export (output: `export`)**로 설정했다면, `useParams` 훅이나 다른 서버 사이드 기능들은 사용할 수 없어요. Next.js의 모든 기능을 활용하려면 `output: 'export'` 설정을 삭제하는 게 좋아요.

---

### 추가로 알려드리고 싶은 팁!

- **App Router와 Server Components**: Next.js 13부터 도입된 App Router와 React Server Components는 UI를 효율적으로 렌더링하고 데이터 페칭을 더 직관적으로 처리할 수 있게 해줍니다. 새 프로젝트라면 꼭 도입을 고려해보세요!
- **이미지 최적화**: 일반 `<img>` 태그 대신 `next/image`를 쓰면 이미지 사이즈에 맞춰 자동 최적화되고, Lazy loading도 기본으로 됩니다. 성능 향상에 크게 기여하죠.
- **ESLint 도입은 꼭 하자!** Next.js가 기본으로 권장하는 ESLint 규칙은 최신 React 및 Next.js 코딩 스타일에 맞춰져 있으니 꼭 설정해 두시길 권장해요. 초기부터 코드 품질을 잘 관리하면 나중에 유지보수도 한결 쉬워집니다.

이제 Next.js의 진짜 힘을 경험할 준비가 됐네요! 궁금한 점 있으면 언제든 찾아주세요. 같이 멋진 웹 앱 만들어봐요! 🚀