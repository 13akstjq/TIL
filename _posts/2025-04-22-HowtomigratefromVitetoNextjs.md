---
title: "Vite에서 Next.js 15로 마이그레이션 하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:30
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to migrate from Vite to Next.js"
link: "https://nextjs.org/docs/app/guides/migrating/from-vite"
isUpdated: false
---


# Vite에서 Next.js로 마이그레이션하는 방법

안녕하세요! 오늘은 Vite로 만들어진 기존 프로젝트를 Next.js로 옮기는 방법에 대해 이야기해볼게요. 요새 React 생태계에서 Next.js가 워낙 대세라서요, 프로젝트 확장성이나 SEO 같은 부분 때문에 넘어가는 분들이 많더라고요. 

## 왜 굳이 Next.js로 갈아타야 할까?

사실 Vite도 빠르고 사용하기 편한 툴인데, Next.js로 갈아타는 몇 가지 이유가 있거든요.

| 이유 | 설명 |
|---|---|
| **서버 사이드 렌더링(SSR)** | Next.js는 기본적으로 SSR을 지원해서 SEO에 훨씬 유리해요. Vite는 클라이언트 렌더링에 초점이 맞춰져 있죠. |
| **파일 기반 라우팅** | Next.js는 폴더 구조만 잘 맞추면 라우팅 설정이 자동으로 되어 개발 속도가 빨라져요. Vite는 라우터 설정을 직접 해줘야 하죠. |
| **API 라우트 지원** | Next.js 내에서 API 엔드포인트를 쉽게 만들 수 있어 백엔드 & 프론트엔드 통합이 편리해요. Vite는 보통 별도의 서버를 띄워야 해요. |
| **강력한 커뮤니티와 플러그인** | Next.js는 많은 플러그인과 라이브러리가 있어서 생태계가 풍부하고, 지원도 활발해요. |

사실 프로젝트 규모가 커지고, SEO가 중요해지고, 좀 더 안정적인 서버 사이드 렌더링이 필요하다면 Next.js는 거의 필수 선택지라고 할 수 있겠네요!

다음 포스팅에서는 실제로 Vite 프로젝트를 어떻게 Next.js 프로젝트로 옮길지, 단계별로 자세히 알려드릴게요. 기대해주세요!

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

만약 기본 Vite 플러그인으로 리액트 애플리케이션을 만들었다면, 여러분의 앱은 순수한 클라이언트 사이드 애플리케이션입니다. 이렇게 클라이언트 사이드에서만 앱을 실행하는 방식, 즉 흔히 싱글 페이지 애플리케이션(SPA)이라고 하죠. 이 방식은 보통 초기 페이지 로딩 속도가 느려지는 문제가 발생합니다. 왜 그런지 간단히 살펴볼게요.

- 브라우저가 여러분의 리액트 코드와 전체 애플리케이션 번들을 내려받고 실행하기 전까지는 데이터 요청조차 시작할 수 없습니다.
- 시간이 지나면서 기능이 추가되고 의존성도 늘어나면서 애플리케이션 코드가 점점 커져서 더 오래 걸리게 됩니다.

즉, 초기 로딩에 필요한 코드가 한 번에 너무 많아서 브라우저가 버거워하는 상황인 거죠.

### 자동 코드 분할이 없다면?

자동 코드 분할이란 쉽게 말해, 필요한 코드만 쪼개서 나중에 불러오는 걸 의미해요. 하지만 기본 설정으로 만든 Vite + React 프로젝트엔 이 기능이 적용되어 있지 않습니다. 그래서 앱 전체 코드를 한 번에 내려받아야 하죠.

자동 코드 분할이 없으면 다음과 같은 단점이 있어요.

| 문제점              | 설명                                                                                     |
|-----------------|----------------------------------------------------------------------------------------------|
| 초기 로딩 느림      | 모든 코드가 한 번에 내려오니까 페이지가 뜨기까지 기다려야 하는 시간이 길어짐                                       |
| 불필요한 코드 다운로드 | 현재 페이지에서 필요하지 않은 코드도 같이 받게 되어 네트워크 자원이 낭비됨                                                |
| 사용자 경험 저하    | 로딩 시간이 길어지면 방문자가 페이지 이탈할 확률이 높아짐                                                         |

자동 코드 분할을 도입하면 첫 페이지 로딩은 가볍게, 이후 필요한 기능은 필요한 순간에 불러올 수 있어 앱 속도가 체감상 빨라집니다.

---

### 추가 팁: 해결책으로 SSG/SSR 도입하기

초기 로딩 속도를 확실히 개선하고 싶다면, 서버 사이드 렌더링(SSR)이나 정적 사이트 생성(SSG)을 도입하는 것도 좋은 방법입니다. Next.js 같은 프레임워크는 기본적으로 이 기능들을 지원해요. 요청 시 서버에서 미리 렌더링된 HTML을 보내면 브라우저가 JS 실행을 기다리지 않아도 빠르게 페이지를 보여줄 수 있죠.

단, SSR/SSG는 설정이 좀 더 복잡할 수 있으니 상황과 요구에 맞게 도입하면 됩니다.

---

이처럼 리액트 앱 개발할 때 초기 로딩 속도 문제는 SPA의 흔한 고민이고, 자동 코드 분할 또는 SSR/SSG 같은 기술을 적절히 적용하면 훨씬 개선할 수 있어요. 앞으로 프로젝트에 적용해 보면서 체감 성능을 경험해보시길 바랍니다!

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

이전 글에서 언급한 느린 로딩 문제는 코드 스플리팅(code splitting)으로 어느 정도 해결할 수 있어요. 그런데 직접 수동으로 코드 스플리팅을 하려고 하면 오히려 성능이 더 안 좋아질 수도 있어요. 수동으로 코드를 나누다 보면 네트워크 워터폴(Network waterfalls, 요청이 순차적으로 쌓여서 느려지는 현상)이 쉽게 발생할 수 있거든요. 다행히 Next.js는 자체 라우터에 자동 코드 스플리팅 기능이 내장되어 있어서 이런 문제를 훨씬 간편하게 해결할 수 있어요.

### 네트워크 워터폴(Network waterfalls)이란?

네트워크 워터폴은 애플리케이션이 클라이언트와 서버 사이에서 순차적으로 여러 데이터를 요청할 때 발생하는 성능 저하 현상이에요. SPA(싱글 페이지 애플리케이션)에서 데이터를 받아올 때 주로, 처음에는 로딩용 자리 표시자(placeholder)를 렌더링하고, 컴포넌트가 마운트된 후에야 데이터를 받아오는 패턴이 많죠. 이 경우 하위 컴포넌트가 데이터를 요청하려면, 반드시 상위 컴포넌트가 자신의 데이터를 모두 받은 뒤여야 해서 대기시간이 길어질 수밖에 없습니다.

Next.js는 클라이언트에서 데이터를 받아오는 것도 지원하지만, 데이터 페칭을 서버 사이드에서 처리하게끔 선택할 수도 있어요. 서버에서 데이터를 미리 받아서 렌더링하면 이런 클라이언트-서버 워터폴 현상을 완전히 없앨 수 있답니다.

---

마치면서 한 가지 팁!  
서버 사이드 렌더링(SSR)이나 SSG(정적 사이트 생성)를 활용하면, 페이지 렌더링 전에 데이터를 미리 받아오기 때문에 사용자 입장에서 훨씬 빠른 초기 로딩을 경험할 수 있어요. Next.js의 `getServerSideProps`나 `getStaticProps` 같은 함수들이 그 역할을 하니, 꼭 활용해보세요!

---

| 개념          | 설명                                                                                  |
|--------------|-------------------------------------------------------------------------------------|
| 코드 스플리팅    | 자바스크립트 코드를 여러 덩어리로 나누어 필요한 부분만 불러와 초기 로딩 속도를 개선하는 방법                   |
| 네트워크 워터폴 | 클라이언트와 서버가 요청을 연속적으로 주고받아서 응답시간이 길어지는 현상                                        |
| Next.js 자동 코드 스플리팅 | Next.js의 라우터가 페이지별로 자동으로 코드 스플리팅을 해주어 성능 최적화를 돕는 기능                           |
| 서버 사이드 데이터 페칭 | Next.js가 페이지 렌더링 전에 서버에서 데이터를 미리 받아와 클라이언트-서버 간 요청 지연 문제를 해결하는 방식          |

필요하면 다음 글에서 Next.js의 데이터 페칭 방법과 활용법도 자세히 알려드릴게요!

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

React Suspense가 스트리밍을 기본적으로 지원하면서, 어떤 UI 부분을 먼저 로딩할지, 또 어떤 순서로 로딩할지 더 세심하게 설계할 수 있게 되었습니다. 덕분에 네트워크 워터폴(waterfall) 현상을 피할 수 있죠.

네트워크 워터폴이란, 한 요청이 끝나야 다음 요청이 시작되는 상황을 말하는데, 이게 계속 쌓이면 로딩 속도가 느려져 사용자 경험이 떨어집니다. Suspense를 사용하면 여러 컴포넌트를 병렬로 로딩하면서도 사용자에게 매끄럽게 보여줄 수 있어서, 페이지 로딩 속도도 빨라지고 레이아웃이 불안정하게 변하는 문제(layout shift)도 줄일 수 있습니다.

### 데이터 패칭(가져오기) 전략 선택하기

내가 배운 바로는, 데이터 패칭 전략을 잘 선택하는 게 매우 중요해요. 예를 들어, 모든 데이터를 한 번에 가져오느냐, 필요한 데이터만 나눠서 가져오느냐에 따라 성능과 사용자 경험이 확 달라집니다.

아래 표는 다양한 데이터 패칭 전략과 그 특징을 간단히 정리한 거예요.

| 전략                   | 설명                                  | 장점                          | 단점                          |
|----------------------|-------------------------------------|-----------------------------|-----------------------------|
| 전체 데이터 한 번에 로드    | 페이지 로드 시 모든 데이터를 한꺼번에 요청           | 초기 로딩 후 빠른 탐색 가능           | 초기 로딩 시간 길어짐               |
| 필요한 데이터만 나눠서 로드 | 필요한 부분만 요청, 점진적 로딩 지원                  | 빠른 초기 로딩, 네트워크 부하 분산       | 구현 복잡도 증가                    |
| 캐싱 활용               | 요청한 데이터를 캐싱하여 재사용                        | 불필요한 중복 요청 방지                 | 데이터 최신성 관리 필요              |
| 스트리밍                 | React Suspense와 함께 스트리밍 데이터 로딩 활용          | 사용자에게 빠르게 부분 렌더링 가능       | 서버 및 클라이언트 세팅 복잡           |

특히 React Suspense와 같은 최신 기능들을 활용하면, 스트리밍 방식으로 데이터를 점진적으로 받아오면서 사용자에게 빠른 피드백을 줄 수 있어요. 개발자로서 이런 점들을 염두에 두면, 퍼포먼스 좋은 앱을 만드는 데 큰 도움이 됩니다.

---

궁금한 점 있으면 언제든 댓글로 남겨주세요! 앞으로도 이런 실용적인 개발 팁들을 계속 공유할게요 :)

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

Next.js는 상황에 맞게 페이지나 컴포넌트 단위로 데이터 가져오기 전략을 선택할 수 있도록 유연성을 제공합니다. 예를 들어, 빌드 시점에 데이터를 가져와서 CMS에 있는 블로그 포스트를 렌더링하고, 이를 CDN에 캐시해서 빠르게 서비스할 수 있어요. 또는 서버 요청 시점이나 클라이언트에서 데이터를 받아서 동적으로 처리할 수도 있죠. 이렇게 상황에 따라 가장 적합한 방식을 선택할 수 있다는 게 Next.js의 큰 장점 중 하나랍니다.

### 미들웨어 (Middleware)

Next.js의 미들웨어는 서버에서 요청이 완료되기 전에 코드를 실행할 수 있도록 해줍니다. 예를 들어, 로그인해야 볼 수 있는 페이지에 사용자가 접근했을 때, 잠깐 인증이 안 된 페이지가 보이는 ‘깜빡임(flash)’ 현상을 막고 바로 로그인 페이지로 리다이렉트할 수 있어요. 이 외에도 새로운 기능을 실험해보거나, 다국어 지원 같은 국제화 작업에 아주 유용하죠. 미들웨어가 있다 보니 이런 작업들이 좀 더 자연스럽고 효율적으로 처리됩니다.

### 내장 최적화 기능

Next.js는 개발자가 따로 신경 쓰지 않아도 자동으로 여러 최적화를 수행합니다. 예를 들어, 이미지 최적화, 번들 크기 최소화, 코드 스플리팅, 그리고 사전 렌더링 같은 기능들이 모두 내장되어 있어요. 특히 이미지 최적화는 웹 페이지 로딩 속도를 크게 높여주니, 사용자 경험 개선에 큰 효과가 있답니다. 덕분에 복잡한 설정 없이도 빠르고 효율적인 웹사이트를 만들 수 있어요.

---

Next.js를 처음 접하는 분들은 이런 데이터 페칭과 미들웨어, 그리고 최적화 기능들이 어떻게 연결되는지 이해하는 게 중요해요. 다양한 상황에 맞게 전략을 골라 사용하면 사이트 성능을 최대로 끌어올릴 수 있으니까요! 만약 이 부분들이 더 궁금하다면 다음 포스트에서 각 전략별 예제와 함께 자세히 설명해볼게요 :)

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

이미지, 폰트, 그리고 서드파티 스크립트는 애플리케이션 성능에 꽤 큰 영향을 줘요. 다행히 Next.js에는 이런 요소들을 자동으로 최적화해주는 내장 컴포넌트들이 있어서 성능 관리를 훨씬 쉽게 해줍니다.

---

## 마이그레이션 단계

이번 마이그레이션 목표는 최대한 빠르게 Next.js 기반의 작동하는 애플리케이션을 만드는 거예요. 그 후에 필요에 따라 Next.js의 여러 기능을 차근차근 적용해 나가면 돼요. 처음에는 기존 라우터를 그대로 두고, 순수 클라이언트 사이드 애플리케이션(SPA)으로만 유지하는 방식으로 진행할 거예요. 이렇게 하면 마이그레이션 중에 발생할 수 있는 문제나 병합 충돌을 최소화할 수 있습니다.

---

### 1단계: Next.js 의존성 설치하기

마이그레이션의 첫걸음은 Next.js 패키지를 프로젝트에 추가하는 것부터 시작합니다. 이 부분은 기본 중의 기본이니까 꼼꼼하게 설치해 주세요! 다음 명령어를 터미널에 입력하면 됩니다:

```bash
npm install next react react-dom
```

또는 yarn을 사용한다면,

```bash
yarn add next react react-dom
```

이렇게 하면 Next.js가 필요로 하는 핵심 라이브러리들이 프로젝트에 추가됩니다.

> 참고로, Next.js는 React 기반 프레임워크라 React와 ReactDOM도 함께 설치해야 해요. 이미 설치되어 있다면 중복 설치가 되지 않으니 걱정하지 않아도 됩니다!

설치가 완료됐으면, 다음 단계로 넘어가서 실제로 Next.js 프로젝트 구조에 맞게 파일들을 조금씩 정리해보도록 할게요.

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

먼저, Next.js를 사용하기 위해서는 프로젝트에 Next를 의존성으로 설치해야 해요:

```bash
npm install next@latest
```

### 2단계: Next.js 설정 파일 만들기

프로젝트 루트에 `next.config.mjs` 파일을 만들어주세요. 이 파일은 Next.js의 다양한 설정 옵션들을 담고 있어요.

> 참고로 이 설정 파일은 JavaScript나 TypeScript 파일처럼 동작하지만, `.mjs` 확장자를 사용하면 ES 모듈 방식을 명확히 하게 돼서 최근 Next.js 프로젝트에서 권장되고 있답니다.  

필요한 설정들을 여기서 자유롭게 추가해서 프로젝트 요구사항에 맞게 조절해보세요!

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

자, 이번에는 Next.js 설정 파일을 조금 바꿔볼게요! 위 예제 코드는 `next.config.js` 파일에 들어갈 내용인데요. 

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: 'export',    // 이 설정은 Next.js를 SPA(단일 페이지 애플리케이션)로 내보내도록 해줘요.
  distDir: './dist',   // 빌드 결과물을 기본 폴더가 아닌 './dist' 폴더에 저장해요.
}

export default nextConfig
```

> 참고로, Next.js 설정 파일은 `.js` 뿐만 아니라 `.mjs` 확장자를 써도 무방해요. 그래서 프로젝트 스타일에 맞게 골라 쓰시면 됩니다.

그리고 이제 TypeScript를 쓰는 분들은 tsconfig.json 파일을 수정해줘야 해요! Next.js와 호환되게끔 일부 설정을 바꿔줘야 하거든요. 만약 TypeScript를 쓸 필요가 없다면 이 부분은 건너뛰셔도 되니까 부담 갖지 마세요.

---

### TypeScript 사용자라면 tsconfig.json 수정하는 팁!

Next.js는 TypeScript를 공식 지원하지만, 기본 설정이 Next.js 빌드 시스템에 맞게끔 커스텀 필요가 있어요. 보통은 다음과 같이 `compilerOptions`와 같이 설정합니다:

| 옵션               | 설명                                        |
|--------------------|---------------------------------------------|
| `jsx`              | React JSX 변환 설정 (예: `"react-jsx"`)      |
| `module`           | 모듈 시스템 (주로 `"esnext"` 사용)            |
| `target`           | 컴파일 타겟팅 자바스크립트 버전                |
| `strict`           | 엄격한 타입 체크 설정                         |
| `baseUrl`           | 절대 경로 기준 디렉토리                          |
| `paths`            | 모듈 경로 별칭 설정                           |

예를 들어, tsconfig.json에 다음처럼 넣으면 더 편리하답니다:

```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "react-jsx"
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx"],
  "exclude": ["node_modules"]
}
```

이렇게 하면 Next.js에서 TypeScript를 더 원활하게 사용할 수 있답니다. 더 궁금하면 언제든 물어봐 주세요!

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

TypeScript 설정을 하면서 다음과 같은 변경사항을 적용해봤어요.

- `tsconfig.node.json` 프로젝트 참조 제거
- `include` 배열에 `./dist/types/**/*.ts` 와 `./next-env.d.ts` 추가
- `exclude` 배열에 `./node_modules` 추가
- `compilerOptions`의 `plugins` 배열에 `"name": "next"` 추가
- `esModuleInterop`을 `true`로 설정
- `jsx`를 `"preserve"`로 설정
- `allowJs`를 `true`로 설정
- `forceConsistentCasingInFileNames`를 `true`로 설정
- `incremental`을 `true`로 설정

아래는 이렇게 수정된 `tsconfig.json` 예시입니다:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "esModuleInterop": true,
    "skipLibCheck": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "preserve",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "allowJs": true,
    "forceConsistentCasingInFileNames": true,
    "incremental": true,
    "plugins": [{ "name": "next" }]
  },
  "include": ["./src", "./dist/types/**/*.ts", "./next-env.d.ts"],
  "exclude": ["./node_modules"]
}
```

---

조금 더 풀어 이야기하자면,

- `esModuleInterop: true`는 CommonJS 모듈과 ES 모듈 간의 호환성을 좋게 해줍니다. 그래서 `import` 구문을 쓸 때 오류를 줄여줘요.
- `jsx: "preserve"`는 JSX를 컴파일하지 않고 그대로 두라는 뜻인데, Next.js가 자체적으로 처리하기 때문에 이렇게 해주는게 일반적이에요.
- `allowJs: true`를 켜면 자바스크립트 파일도 TS 빌드 대상에 포함할 수 있어서, 점진적으로 타입스크립트로 이전하려는 프로젝트에서 유용합니다.
- `forceConsistentCasingInFileNames: true` 설정은 대소문자 차이로 생기는 파일 경로 문제를 예방해줍니다. 특히 OS마다 대소문자 구분이 다른 경우에 꼭 필요해요.
- `incremental: true`는 빌드 속도를 향상시키기 위해 이전 빌드 정보를 캐싱해주는 기능입니다. 개발할 때 시간 절약에 큰 도움이 돼요.
- 플러그인에 `"name": "next"`를 넣으면 Next.js에서 권장하는 타입스크립트 플러그인을 사용하게 됩니니다.

Next.js 공식 문서에도 TypeScript 구성에 대한 좋은 안내가 있으니, 필요하면 꼭 참고해보세요.  
https://nextjs.org/docs/basic-features/typescript

이 설정을 기반으로 프로젝트를 구성하면 Next.js + TypeScript 환경에서 좀 더 편리하고 안정적으로 개발하실 수 있습니다!

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

### 4단계: 루트 레이아웃 만들기

Next.js의 App Router를 사용할 때는 루트 레이아웃(root layout) 파일이 꼭 필요해요. 이 파일은 React Server Component로, 여러분의 앱 안에 있는 모든 페이지를 감싸는 역할을 하죠. 그리고 이 파일은 `app` 디렉토리의 최상단에 위치해야 합니다.

비슷한 개념을 Vite 앱에서 찾자면, 바로 `index.html` 파일이에요. 이 파일이 여러분의 HTML, head, body 태그를 포함하고 있어서, Next.js에서 말하는 루트 레이아웃과 가장 비슷한 역할을 하죠.

이번 단계에서는 여러분의 `index.html` 파일을 Next.js의 루트 레이아웃 파일로 변환해볼 거예요. 

---

여기서 꼭 짚고 넘어갈 점은 Next.js에서는 이 루트 레이아웃이 React 컴포넌트라는 거예요. 단순히 HTML 파일이 아니라 리액트 컴포넌트로 작성되니, 필요한 경우 React의 훅이나 다른 컴포넌트로 기능 확장이 훨씬 쉬워요. 즉, 더 동적이고 유연한 레이아웃 구성이 가능하다는 거죠.

그럼 이제 `index.html`에서 어떤 부분을 어떻게 바꿔주면 될지 살펴볼게요!

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

이번에는 Next.js 프로젝트에서 새로운 앱 디렉토리를 만들고, 레이아웃 설정을 하는 방법을 알려드릴게요. 특히 Next.js 13 버전에서 도입된 '새로운 app 디렉토리 구조'를 활용하는 예제인데요, 코드 설명도 곁들여서 천천히 같이 보시죠!

---

### 1. `src` 안에 `app` 디렉토리 만들기

먼저, 프로젝트 폴더 안 `src` 디렉토리 안에 `app` 폴더를 새로 만들어줘요. 여기서부터 우리 앱의 페이지, 레이아웃 등이 관리될 거예요.


src/
  └── app/


---

### 2. `RootLayout` 컴포넌트 만들기 (`layout.tsx`)

`app` 폴더 안에 `layout.tsx` 파일을 하나 새로 만들어요. 이 파일은 페이지들의 공통 레이아웃을 담당합니다.

```tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return '...'
}
```

- `children`은 이 레이아웃 안에 렌더링될 페이지나 하위 컴포넌트들을 의미해요.
- 중요! `layout.tsx` 같은 레이아웃 파일 확장자는 `.js`, `.jsx`, `.tsx` 중 원하시는 걸로 써도 된답니다.

---

### 3. 기존 `index.html` 내용을 `RootLayout` 안에 넣기

보통 React 프로젝트에는 `public/index.html` 파일이 있는데요, 여기에는 `<body><div id="root"></div></body>` 같은 구조가 있어요. 이것을 Next.js의 레이아웃 컴포넌트에 맞게 바꿔줘야 해요.

- `body` 안의 `<div id="root"></div>` 대신에 JSX 문법을 사용해서 `<div id="root">{children}</div>` 식으로 바꿔줍니다.
- `<script>` 태그나 직접적으로 body 바로 아래에 있는 태그들은 Next.js의 자동 렌더링 및 페이지 컴포넌트 구조에서 관리하니 제거하거나 옮겨줘야 해요.

아래처럼 작성할 수 있겠죠?

```tsx
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html>
      <head>
        {/* 여기에 메타 태그, 폰트 링크 등 head 요소를 넣어줄 수 있어요 */}
      </head>
      <body>
        <div id="root">{children}</div>
      </body>
    </html>
  )
}
```

---

### 잠깐, 여기서 팁!

- Next.js 13부터는 `app` 디렉토리 내에서 레이아웃 또는 페이지가 기본적으로 서버 컴포넌트(Server Component)로 동작해요.
- 따라서 클라이언트 사이드 상태 관리 등이 필요하면 `'use client'`를 꼭 넣어줍니다.
- 그리고 기존에 `<div id="root">`를 React 엔트리포인트로만 사용했던 것과 달리, Next.js에서는 `app` 구조가 페이지마다 분리돼서 더 깔끔하고 모듈화된 방식으로 관리돼요.

---

요약하자면:  
`src/app/layout.tsx`를 만들고, 기존 HTML 구조를 이 안에 JSX로 옮기면서 `<div id="root">{children}</div>` 형태로 바꿔주면 이제 Next.js 13 스타일의 앱 레이아웃 설정이 끝! 앞으로 페이지 컴포넌트도 이 안에서 자연스럽게 렌더링 될 거예요.

궁금한 점 있으면 편하게 물어봐 주세요!

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

Next.js를 사용하다 보면, 기본적으로 `<head>`에 들어가는 `meta charset`이나 `meta viewport` 태그가 자동으로 포함되어 있다는 점을 알게 될 거예요. 그래서 직접 이런 태그들을 작성할 필요가 없답니다! 조금 더 깔끔하고 관리하기 편하게 아래처럼 수정할 수 있어요.

```js
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <head>
        <link rel="icon" type="image/svg+xml" href="/icon.svg" />
        <title>My App</title>
        <meta name="description" content="My App is a..." />
      </head>
      <body>
        <div id="root">{children}</div>
      </body>
    </html>
  )
}
```

여기서 조금 더 팁을 드리자면, favicon.ico, icon.png, robots.txt 같은 메타데이터 파일들이 있다면, 이걸 직접 `<head>`에 `link` 태그로 명시하지 않아도 돼요. Next.js가 여러분의 앱 디렉토리 최상단에 이런 파일들이 있는지 자동으로 체크해서 `<head>`에 넣어주거든요.

즉, 이런 작업 구조가 가능해요:

| 위치                  | 역할                                          |
|---------------------|---------------------------------------------|
| `app/favicon.ico`    | 파비콘 자동 등록                              |
| `app/robots.txt`    | 검색 엔진 크롤러에서 참고하는 robots 파일 자동 포함  |
| `app/icon.svg`       | SVG 아이콘 자동 등록                            |

따라서, 이런 파일들만 잘 관리해두면 `link` 태그를 따로 추가하거나 지저분하게 관리할 필요가 없답니다.

---

> 🚀 **한가지 알아두면 좋은 점!**  
> Next.js의 이런 자동화 덕분에 앱 구조가 훨씬 깔끔하고 유지보수가 편해져요. 그리고 혹시 이런 메타데이터를 동적으로 변경하고 싶다면 Next.js 13부터 도입된 `metadata` API도 활용해보세요! 훨씬 더 편리하게 SEO 최적화가 가능합니다.

궁금한 게 있으면 언제든지 물어봐 주세요! 😊

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

Next.js에서 head 태그를 관리하는 새로운 방법, Metadata API에 대해 이야기해볼게요. 원래는 아래처럼 직접 `<head>` 태그 안에 title이나 meta를 적었었죠.

```jsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <head>
        <title>My App</title>
        <meta name="description" content="My App is a..." />
      </head>
      <body>
        <div id="root">{children}</div>
      </body>
    </html>
  )
}
```

그런데 Next.js는 이제 Metadata API라는 깔끔한 방식을 제공합니다. 이걸 쓰면 메타데이터를 컴포넌트 바깥에 `export const metadata`로 분리할 수 있어요. 코드가 더 간결해지고, SEO 최적화나 공유할 때 이점도 커지죠:

```tsx
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'My App',
  description: 'My App is a...',
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

이렇게 바꾸면 `head` 태그 자체는 선언하지 않고도 메타데이터가 Next.js에 의해 자동으로 관리되니까 훨씬 깔끔하고 유지보수도 쉽습니다. 그리고 나중에 동적으로 제목이나 설명을 바꾸고 싶을 때도 이 Metadata API가 더 편리해요.

참고로, 이 방식은 `/app` 디렉토리를 사용하는 Next.js 13 이상에서 권장되는 최신 방법이라는 점 꼭 기억하세요! 만약 아직 `pages` 디렉토리를 쓰고 있다면 다음 업그레이드 때 도입해 보시면 좋을 것 같아요.

요약하자면, Metadata API는 다음과 같은 장점이 있어요:

| 장점            | 설명                                                         |
|-----------------|--------------------------------------------------------------|
| 코드 분리       | 메타데이터를 컴포넌트 코드에서 분리해 관리가 편리해짐          |
| 자동 관리       | Next.js가 head 태그를 자동으로 처리해 실수를 줄임             |
| SEO & 공유 최적화 | SEO 관련 메타 정보와 소셜 미디어 공유 정보까지 쉽게 설정 가능  |
| 타입 지원       | 타입스크립트 Metadata 타입 제공으로 안정성 증가               |

이렇게 공식 문서를 따라가면 Next.js 프로젝트가 한층 더 체계적이고 깔끔해질 거예요! 꼭 한번 써보세요 :)

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

### 5단계: 진입점 페이지 만들기

Next.js에서는 앱의 진입점을 만들 때 `page.tsx` 파일을 작성하는 식으로 선언해요. 이걸 Vite로 치환하면, 보통 `main.tsx` 파일이 진입점 역할을 하게 됩니다. 이번 단계에서는 여러분 앱의 진입점(첫 시작점)을 설정해볼 거예요.

- `app` 디렉토리 안에 `[[...slug]]`라는 이름의 폴더를 생성하세요.

여기서 잠깐! 우리가 이번 가이드에서 Next.js를 SPA(싱글 페이지 애플리케이션)처럼 사용하려고 하거든요. 그러려면 앱의 첫 페이지가 모든 가능한 경로를 받아서 처리해야 합니다. 그래서 `app` 디렉토리에 `[[...slug]]`라는 폴더를 만들어서, 어떤 경로든 이 폴더가 잡아내서 라우팅할 수 있도록 하는 거죠.

---

추가로 설명을 좀 하자면, Next.js에서는 `[[...slug]]`와 같은 경로 패턴이 "캐치올(catch-all)" 라우팅 기능을 의미해요. 쉽게 말해서, URL에 뭘 붙여도 다 여기서 받겠다는 뜻이죠. 그래서 SPA를 구현할 때 정말 유용합니다. Vite 같은 도구에서는 이런 라우팅을 직접 구현하거나 라이브러리를 붙여야 하는데, Next.js가 너무 잘해주는 부분이죠!

---

혹시 React Router를 써서 SPA를 만든다면, 이런 "모든 경로를 캐치하는" 룰을 `<Route path="*">` 같은 식으로 걸어줄 수 있다는 점도 기억해 두세요. SPA의 핵심은 결국 한 페이지에서 모든 라우팅을 내부에서 다 처리하는 거니까요.

---

정리하자면:

- `[[...slug]]` 폴더 생성 → Next.js의 캐치올 라우팅
- SPA처럼 모든 경로를 한 페이지에서 처리
- Vite는 `main.tsx`가 진입점

이렇게 세팅하면 여러분 앱이 다양한 경로에서 잘 반응하는 싱글 페이지 앱이 되겠죠? 다음 단계도 같이 기대해 주세요!

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

Next.js에서는 폴더 구조가 라우트를 정의하는 중요한 역할을 해요. 여기서 소개한 `app/[[...slug]]` 디렉토리는 '옵셔널 캐치-올(optional catch-all)' 라우트 세그먼트라고 불려요. 쉽게 말하면, 이 폴더 안에 있는 페이지가 애플리케이션의 모든 경로를 잡아주도록 설정하는 거죠.

### 직접 해보기
`app/[[...slug]]` 안에 `page.tsx` 파일을 새로 만들고, 아래 내용을 넣어보세요:

```tsx
import '../../index.css'

export function generateStaticParams() {
  return [{ slug: [''] }]
}

export default function Page() {
  return '...' // 나중에 업데이트할 예정이에요
}
```

- 여기서 `generateStaticParams` 함수는 빌드 시에 미리 정적 페이지를 생성하기 위한 파라미터를 반환해요. 예제에선 빈 slug 배열을 반환해서 기본 경로를 세팅하고 있어요.
- `Page` 컴포넌트는 현재 그냥 문자열을 반환하는 기본 상태인데, 이후에 이 부분을 다양한 경로에 맞게 확장할 수 있습니다.

> 참고로, Next.js에서는 Page 컴포넌트 파일 이름에 `.js`, `.jsx`, `.tsx` 어떤 확장자든 사용할 수 있어요. 여러분이 편한 걸로 쓰면 됩니다!

이렇게 설정하면, 어떤 URL로 접근하든 여기 있는 `page.tsx`가 먼저 실행되어서 복잡한 라우팅 처리를 중앙 집중화 할 때 유용해요. 예를 들어 블로그의 포스트 여러 개를 한 페이지 컴포넌트에서 처리한다든지 할 때 쓰이죠.

---

추가로 Tip!
- `[...slug]`는 캐치-올 라우트(segment all)인데 필수로 하나 이상의 값이 있어야 합니다.
- `[[...slug]]`는 옵셔널 캐치-올이라 slug가 없어도 이 라우트가 적용돼요.
- 이렇게 라우트를 잡으면 동적 경로도 쉽게 만들 수 있어요.

Next.js 라우팅 좀 더 편하게 하고 싶다면, 이런 옵셔널 캐치-올 라우트 구조를 꼭 알아두면 좋아요!

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

이 파일은 서버 컴포넌트(Server Component)예요. `next build` 명령어를 실행하면 이 파일은 미리 렌더링(prerendering)되어 정적인 자산(static asset)으로 만들어집니다. 즉, 동적 코드가 필요 없다는 뜻이죠.

이 파일은 글로벌 CSS를 불러오고, `generateStaticParams` 함수에서는 오직 인덱스 경로인 `/` 하나만 정적으로 생성할 것이라고 알려줘요.

그럼 이제 나머지 Vite 애플리케이션 부분을 클라이언트 전용(Client-only)으로 옮겨볼게요. 다음처럼 작성하면 Next.js에서 클라이언트 전용 컴포넌트를 쉽게 다룰 수 있어요.

```jsx
'use client'

import React from 'react'
import dynamic from 'next/dynamic'

const App = dynamic(() => import('../../App'), { ssr: false })

export function ClientOnly() {
  return <App />
}
```

여기서 핵심은 `dynamic` 함수를 쓰면서 `{ ssr: false }` 옵션을 넣는 부분이에요. 이걸 통해서 서버 사이드 렌더링(SSR)을 비활성화하고, 해당 컴포넌트를 클라이언트에서만 불러오도록 설정합니다. 보통 Vite처럼 클라이언트 전용 라이브러리를 사용할 때 이렇게 하면 에러 없이 잘 작동하죠.

또, `'use client'` 디렉티브를 최상단에 넣는 것도 잊지 마세요. 이건 Next.js 13 버전부터 도입된 문법인데, 해당 파일이 클라이언트 컴포넌트임을 명시해줍니다.

---

**TIP**: 클라이언트 컴포넌트를 이렇게 분리해서 관리하면 서버 컴포넌트와 클라이언트 컴포넌트의 역할 분담이 명확해져서 유지보수도 훨씬 쉬워져요. 또한, 초기 페이지 로딩 속도도 더 좋아질 수 있으니 적극 추천합니다!

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

이 파일은 `use client` 지시어로 정의된 클라이언트 컴포넌트입니다. 클라이언트 컴포넌트라도 Next.js에서는 처음에 서버에서 HTML로 프리렌더링한 후 클라이언트에 전달해요.

하지만 클라이언트에서만 동작하는 애플리케이션을 만들고 싶다면, App 컴포넌트부터 시작해서 전체적으로 프리렌더링을 비활성화할 수 있어요. 이렇게 하면 서버에서는 HTML을 만들지 않고, 클라이언트에서만 렌더링하게 됩니다.

```js
const App = dynamic(() => import('../../App'), { ssr: false })
```

위 코드에서 `dynamic`을 사용하여 App 컴포넌트를 불러오면서 `{ ssr: false }` 옵션을 줬는데요, 이 옵션이 서버 사이드 렌더링을 끄는 역할을 해요.

그 다음에는 진입점(entrypoint) 페이지를 이제 이 동적으로 불러온 `App` 컴포넌트로 교체해주면 됩니다!

> 참고로, 클라이언트 전용 애플리케이션을 만들 때는 서버에서 처리할 필요가 없으니 이렇게 `ssr: false`를 설정하는 게 퍼포먼스 향상에도 도움이 돼요. 하지만 SEO가 중요한 페이지라면 신중하게 사용해야겠죠.  

필요하면 아래처럼 진입점 페이지에서 바로 사용해보세요.

```js
import dynamic from 'next/dynamic'

const App = dynamic(() => import('../../App'), { ssr: false })

export default function Page() {
  return <App />
}
```

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

Next.js와 Vite에서 정적 이미지(import) 다루는 방법이 조금 달라요. Vite에서는 이미지를 import 하면, 프로덕션 빌드 시 이미지의 URL 문자열이 반환돼서 바로 img 태그의 src에 쓸 수 있죠.

```js
import image from './img.png' // 빌드 후에는 '/assets/img.2d8efhg.png' 같은 URL이 됨

export default function App() {
  return <img src={image} />
}
```

근데 Next.js는 이런 식으로 바로 이미지 URL을 가져오는 게 아니라, `next/image` 컴포넌트를 이용해서 이미지 최적화 기능을 활용하도록 권장해요. 만약 그냥 정적 파일처럼 사용하고 싶으면 Next.js의 `public` 폴더에 이미지를 넣고 URL로 접근하는 게 가장 깔끔하답니다.

예를 들어, `public/img.png`에 이미지를 넣었다면:

```jsx
export default function App() {
  return <img src="/img.png" alt="example" />
}
```

아니면 Next.js의 Image 컴포넌트를 써서 자동으로 크기 조정, 최적화 기능을 쓰는 것도 좋아요:

```jsx
import Image from 'next/image'
import img from '../public/img.png'

export default function App() {
  return <Image src={img} alt="example" />
}
```

이렇게 하면 이미지가 자동으로 최적화되고 레이지 로딩도 적용돼서 퍼포먼스에 유리해요.

---

> **Tip!**  
> Next.js에서 이미지를 정적으로 import 하려면 `next.config.js`에서 `images` 설정을 잘 해줘야 하는 경우도 있어요. 그리고 외부 URL의 이미지를 쓰려면 도메인을 허용해줘야 하죠.  
> 이미지 최적화나 로딩 속도가 신경 쓰인다면 `next/image` 컴포넌트를 적극 활용해보세요!

---

결론적으로 Vite처럼 이미지 import 해서 바로 URL 받는 방식은 Next.js에선 조금 다르게 다뤄진다고 생각하면 돼요. 상황에 따라 `public` 폴더에 넣거나, `next/image`를 쓰는 두 가지 방법 중 선택하면 됩니다!

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

Next.js에서 이미지 파일을 정적으로 import하면, 이미지가 객체(object)로 반환돼요. 이 객체는 Next.js의 `Image` 컴포넌트에서 바로 사용할 수도 있고, 기존에 쓰던 `img` 태그에서는 이 객체의 `src` 속성만 꺼내서 쓸 수도 있답니다.

`Image` 컴포넌트의 장점은 자동 이미지 최적화가 된다는 것! 그리고 이미지의 실제 크기를 기반으로 `width`, `height` 속성을 자동으로 넣어줘서 이미지가 로딩될 때 레이아웃이 갑자기 바뀌는 문제(layout shift)를 막아줘요. 다만 주의할 점은 이미지의 한 쪽 치수만 스타일링하고 다른 쪽은 auto로 두지 않으면, 그 치수가 `img` 태그에 자동으로 들어간 크기로 고정돼서 이미지가 왜곡될 수 있다는 거예요.

그래서 기존 `img` 태그를 유지하면 코드 변경량도 줄일 수 있고, 위 문제도 피할 수 있죠. 나중에 필요하면 `Image` 컴포넌트로 단계별 마이그레이션을 할 수 있어요. 이때 직접 로더(loader)를 설정하거나, Next.js 기본 서버를 사용하면 자동 이미지 최적화를 편리하게 활용할 수 있답니다.

끝으로 `/public` 폴더에서 절대경로로 이미지를 import했다면, 상대경로로 바꾸는 것도 잊지 마세요. 이렇게 하면 관리도 더 편해지고 빌드 과정에서 문제도 줄일 수 있어요!

---

### 정리

| 방법                         | 장점                             | 주의 사항                              |
|----------------------------|--------------------------------|-----------------------------------|
| `Image` 컴포넌트 사용          | 자동 이미지 최적화, 레이아웃 안정성 확보     | 한쪽 치수만 스타일링 시 이미지 왜곡 가능성         |
| 기존 `img` 태그 유지           | 코드 변경 최소화, 왜곡 문제 없음           | 이미지 최적화 기능은 직접 구현해야 함           |
| `/public`에서 이미지 경로 변경  | 경로 관리 용이, 빌드 시 문제 감소         | 절대경로 -> 상대경로 수정 필요               |

---

### 추가 팁: 이미지 최적화 옵션
Next.js의 `Image` 컴포넌트는 기본적으로 자동 최적화를 지원하지만, 필요하면 `loader`를 정의해 좀 더 세밀한 설정도 가능해요. 예를 들어 외부 이미지 호스팅 서비스를 함께 쓸 때 유용하죠. 혹은 next.config.js에서 domains 옵션에 외부 도메인을 추가해주면, 더 다양한 이미지를 `Image` 컴포넌트로 불러올 수 있습니다.

---

필요에 따라 `Image` 컴포넌트의 장점과 한계를 잘 고려해서 사용하면, 퍼포먼스도 챙기고 개발도 부드럽게 할 수 있어요! 궁금한 점 있으면 언제든 질문 주세요~

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

자바스크립트 코드에서 이미지 파일을 불러올 때 경로나 방식이 조금 바뀌면서 주의할 점들을 정리해봤어요.

---

### 1. 이미지 파일 import 경로 변경하기

```js
// 이전 방식
import logo from '/logo.png'

// 변경된 방식
import logo from '../public/logo.png'
```

이전에는 루트부터 바로 경로를 지정했다면, 이제는 public 폴더를 기준으로 상대 경로를 명확히 써주는 게 좋아요. 그렇지 않으면 이미지가 제대로 불러와지지 않을 수도 있어요.

---

### 2. `img` 태그에 `src` 속성 넘길 때 주의하기

```js
// 이전 방식
<img src={logo} />

// 변경된 방식
<img src={logo.src} />
```

import 한 이미지 객체 전체를 `src`에 넘기면 안 되고, 그 안에 실제 URL이 담긴 `.src` 값을 넘겨줘야 이미지가 화면에 제대로 뜹니다.

---

### 3. 대안: 직접 public 폴더 경로 지정하기

이미지 파일이 `public/logo.png`에 있다면, 그냥 `src="/logo.png"`로 바로 작성해도 됩니다.

```js
<img src="/logo.png" />
```

이렇게 작성하면 이미지가 `public` 폴더 안에서 찾아지게 되는데, 간단하게 이미지 경로를 관리할 수 있는 방법이기도 해요. 다만, 이 경우 import 없이 스트링 경로를 직접 넣는 형태라는 점 참고하세요.

---

### 추가 팁

- 만약 프로젝트가 Next.js 같은 프레임워크라면 `public` 폴더의 파일은 빌드 시 그대로 공개 경로로 복사돼서 `/이미지이름`으로 접근 가능합니다.
- 자주 사용하는 공용 이미지들은 `public` 폴더에 모아두는 게 관리도 편하고, 빌드에도 영향을 덜 주는 편이에요.
- 상대 경로 작성 시 `..` 여러 개를 써야 할 때 헷갈리기 쉬우니, VS Code 같은 편집기의 경로 자동완성기능을 적극 활용하면 좋아요.

---

이제 이미지를 불러올 때 경로 문제로 에러 뜨는 일은 조금 줄어들 거예요! 자~ 다음 프로젝트 때 적용해 보세요. :)

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

> 경고: 만약 TypeScript를 사용 중이라면 src 속성에 접근할 때 타입 에러가 발생할 수 있어요. 지금은 무시해도 괜찮고, 이 가이드를 마칠 즈음에는 모두 해결될 테니 걱정 마세요.

### 7단계: 환경 변수 마이그레이션하기

Next.js도 Vite처럼 `.env` 파일을 통해 환경 변수를 관리할 수 있어요. 그런데 크게 차이나는 부분이 하나 있는데, 바로 클라이언트 쪽에서 노출되는 환경 변수에 붙는 접두사(prefix)에요.

| Vite                  | Next.js              |
|-----------------------|----------------------|
| `VITE_` (접두사)       | `NEXT_PUBLIC_` (접두사) |

즉, Vite에서 `VITE_`로 시작하는 환경 변수들은 Next.js에서는 `NEXT_PUBLIC_`로 바꿔줘야 클라이언트 사이드 코드에서 접근할 수 있어요. 예를 들어:

```js
# Vite 환경 변수
VITE_API_URL=https://api.example.com
```

위 변수는 Next.js로 옮길 때 이렇게 바꿔줘야 해요:

```js
# Next.js 환경 변수
NEXT_PUBLIC_API_URL=https://api.example.com
```

이걸 꼭 지켜야 클라이언트에서 `process.env.NEXT_PUBLIC_API_URL`로 안전하게 접근할 수 있어요. 만약 접두사를 안 바꾸면 클라이언트 쪽에 변수가 노출되지 않아서 참조할 수 없게 돼요.

---

**추가 팁!**  
환경 변수를 사용할 때 중요한 건, 민감한 정보(ex: API 키, 비밀번호 등)는 절대 클라이언트에 노출시키면 안 돼요. `NEXT_PUBLIC_` 붙은 변수는 클라이언트에서 볼 수 있기 때문에, 민감 데이터를 담기엔 적합하지 않답니다. 서버 전용 변수는 접두사 없이 그냥 `process.env`에서 바로 사용하세요.

---

앞으로 계속 환경 변수를 변경하면서도, 변수 이름을 이렇게 맞춰주는 게 Next.js와 Vite를 함께 쓰거나 Next.js로 이전할 때 발생하는 흔한 실수 중 하나예요. 신경 써서 관리하면 빌드 문제나 런타임 오류를 줄일 수 있어요!

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

Vite에서는 특별한 import.meta.env 객체를 통해 몇 가지 내장 환경 변수를 제공하는데, Next.js는 이 부분을 지원하지 않아요. 그래서 Vite에서 쓰던 환경 변수들을 Next.js에 맞게 이렇게 바꿔줘야 해요.

| Vite (import.meta.env)       | Next.js (process.env)                  |
|-----------------------------|--------------------------------------|
| import.meta.env.MODE         | process.env.NODE_ENV                  |
| import.meta.env.PROD         | process.env.NODE_ENV === 'production'|
| import.meta.env.DEV          | process.env.NODE_ENV !== 'production'|
| import.meta.env.SSR          | typeof window === 'undefined'        |

여기서 살짝 팁을 드리자면, import.meta.env.SSR 은 서버 사이드 렌더링 여부를 체크하기 위한 변수인데, Next.js에서는 window 객체가 없으면 서버 환경으로 인식할 수 있으니 `typeof window === 'undefined'` 로 체크해주면 됩니다.

그리고 한 가지 더! Next.js는 기본적으로 Vite에 있는 BASE_URL 같은 환경 변수를 제공하지 않아요. 하지만 직접 환경 변수를 만들어서 쓸 수는 있답니다. 방법은 간단해요, 프로젝트 루트에 `.env` 파일을 만들고 아래처럼 추가해주시면 됩니다.

```js
NEXT_PUBLIC_BASE_URL=https://your-domain.com
```

여기서 NEXT_PUBLIC_ 접두어는 Next.js에서 브라우저 코드에서도 접근할 수 있는 환경 변수를 의미해요. 만약 서버에서만 쓸 거라면 접두어 없이 그냥 선언하면 되고요.

이렇게 하면 코드에서 `process.env.NEXT_PUBLIC_BASE_URL` 로 접근해서 사용할 수 있어요. 예를 들어 API 엔드포인트를 설정할 때 유용하답니다.

정리하자면, Vite에서 Next.js로 넘어올 때 환경 변수 관련 코드를 이렇게 바꿔주시고, 필요한 환경 변수들은 직접 `.env` 파일에 추가해서 관리하시면 됩니다!

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

Next.js에서 기본 경로(basePath)를 환경 변수로 설정하는 방법에 대해 공유할게요. 프로젝트를 하다 보면 `basePath`를 동적으로 설정해야 할 경우가 있는데, 예를 들어 CDN이나 특정 경로에 배포할 때 유용하죠.

### 1. `.env` 파일에 환경 변수 설정하기

```js
NEXT_PUBLIC_BASE_PATH="/some-base-path"
```

여기서 중요한 점! `NEXT_PUBLIC_` 접두어로 시작해야 클라이언트 쪽에서도 접근 가능해요.

### 2. `next.config.mjs` 파일에서 basePath 설정하기

다음과 같이 `next.config.mjs`에 `basePath`를 환경 변수로 불러와 세팅해 주세요.

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: 'export',      // SPA 형태로 빌드
  distDir: './dist',     // 빌드 결과물을 'dist' 폴더로 변경
  basePath: process.env.NEXT_PUBLIC_BASE_PATH,   // env 변수를 basePath로 설정!
}

export default nextConfig
```

> TIP: `next.config.mjs`에서 import.meta.env 대신 `process.env`를 사용해야 하는 점 잊지 마세요. Next.js는 빌드 타임에 `process.env`로 환경 변수값을 주입합니다.

### 3. 코드 내에서 `import.meta.env.BASE_URL` -> `process.env.NEXT_PUBLIC_BASE_PATH`로 변경

기존에 `import.meta.env.BASE_URL` 을 사용하던 부분이 있다면, `process.env.NEXT_PUBLIC_BASE_PATH`로 바꿔줘야 합니다. 이 부분은 Vite 등 다른 빌드 시스템과 차이가 있어요.

```js
const basePath = process.env.NEXT_PUBLIC_BASE_PATH || ''

// 사용 예
const fullUrl = `${basePath}/some-resource`
```

### 정리하자면

| 단계               | 설명                                 |
|-------------------|------------------------------------|
| 1. env 파일 만들기    | `NEXT_PUBLIC_BASE_PATH` 설정          |
| 2. next.config.mjs 수정 | `basePath`에 `process.env.NEXT_PUBLIC_BASE_PATH` 지정 |
| 3. 코드 수정           | `import.meta.env.BASE_URL` → `process.env.NEXT_PUBLIC_BASE_PATH` |

---

이렇게 하면 빌드할 때마다 환경 변수로 기본 경로를 바꾸는 게 가능해져서 다양한 배포 환경에 유연하게 대응할 수 있습니다. Next.js 프로젝트 하면서 경로 문제에 한 번쯤 고민해보셨다면 참고하세요~! 필요하면 basePath 외에도 `assetPrefix`도 같이 고려하는 게 좋아요. 그럼 즐코딩~! 🚀

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

### 8단계: package.json 스크립트 업데이트하기

이제 Next.js로 마이그레이션을 잘 했는지 확인하기 위해 애플리케이션을 실행해볼 수 있어요. 그런데 그 전에 package.json 안의 스크립트를 Next.js에 맞게 살짝 바꿔줘야 합니다. 

기존에 있던 start나 build 명령어 대신 Next.js 명령어를 넣어주면 되는데요:

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  }
}
```

- `"dev"`: 개발 모드로 로컬 서버를 띄울 때 사용해요 (hot reload도 지원해서 개발할 때 편리해요)
- `"build"`: 배포 전, 프로덕션용으로 최적화된 빌드를 생성할 때 쓰입니다.
- `"start"`: 빌드된 결과물을 실제 서비스 환경에서 실행할 때 사용하죠.

그리고 Git 관리할 때 필요 없는 파일들을 깃에서 제외하기 위하여 `.gitignore` 파일에도 다음 내용을 추가해주세요:


.next
next-env.d.ts
dist


- `.next` 폴더는 Next.js가 빌드하면서 생성하는 임시파일들이라 보통 Git에 올리지 않아요.
- `next-env.d.ts`는 Next.js 타입 지원을 위한 선언 파일로, 자동 생성되므로 따로 관리하지 않습니다.
- `dist`는 프로젝트마다 다르지만, 빌드 후 결과를 담고 있는 폴더라면 역시 빼는 게 좋습니다.

참고로, `.gitignore`에 이런 빌드 결과 파일들을 미리 넣어두면 협업할 때 불필요한 충돌도 줄일 수 있어요. 다음 단계에서는 이렇게 설정한 상태에서 실제로 `npm run dev` 명령어로 개발 서버를 실행해보면서 잘 작동하는지 확인해보세요!

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

이제 터미널에서 `npm run dev` 명령어를 실행하고, http://localhost:3000 에 접속해 보세요. 그러면 당신의 Next.js 애플리케이션이 잘 실행되고 있는 모습을 확인할 수 있을 거예요.

> 참고로, Vite로 만들어진 애플리케이션을 Next.js로 마이그레이션한 실제 예제를 보고 싶다면 이 [Pull Request](https://github.com/vercel/next.js/pull/####)를 확인해 보세요. (원문에 링크가 있다면 여기에 넣어주세요.)

### 9단계: 정리하기

이제 Vite 관련 파일들이나 설정들은 모두 정리할 차례입니다. 프로젝트에서 Vite와 관련된 불필요한 것들을 깔끔하게 제거해서 코드베이스를 정돈해 주세요!

예를 들어, 다음과 같은 것들을 확인하시면 좋아요:

| 제거 대상                     | 설명                              |
|------------------------------|----------------------------------|
| `vite.config.js`              | Vite 설정 파일                   |
| `index.html`                  | Vite에서 사용했던 진입 HTML 파일 |
| `package.json`의 Vite 관련 의존성 | `vite`, `@vitejs/plugin-react` 등 |
| 기타 Vite 전용 스크립트       | 빌드 스크립트나 개발 서버 관련 설정 |

이렇게 정리하고 나면 Next.js만 사용하는 깔끔한 환경을 만들 수 있습니다.

---

만약 다음 단계가 궁금하다면, Next.js의 배포나 최적화 방법 등을 함께 알아보는 것도 추천합니다. Next.js는 기본적으로 SSR(서버 사이드 렌더링)과 SSG(정적 사이트 생성), 그리고 CSR(클라이언트 사이드 렌더링)을 유연하게 사용할 수 있어서 애플리케이션을 더 빠르고 SEO 친화적으로 만들어 주거든요!

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

- main.tsx 삭제  
- index.html 삭제  
- vite-env.d.ts 삭제  
- tsconfig.node.json 삭제  
- vite.config.ts 삭제  
- Vite 관련 의존성 제거  

## 다음 단계

만약 지금까지 작업이 잘 됐다면, 이제 Next.js를 기반으로 동작하는 싱글 페이지 애플리케이션을 갖게 된 거예요. 하지만 아직 Next.js가 제공하는 많은 장점들을 완전히 활용하고 있진 않은 상태입니다. 이제부터 점진적으로 개선하면서 Next.js의 진가를 맛보실 수 있는데요, 다음 단계로 고려해볼 만한 사항들을 소개할게요:

| 다음 단계 | 설명 |
| --- | --- |
| React Router에서 Next.js App Router로 마이그레이션 | - 자동 코드 분할로 초기 로딩 속도 개선<br>- 스트리밍 서버 사이드 렌더링 지원<br>- React 서버 컴포넌트 활용 가능 |
| 이미지 최적화 | Next.js `Image` 컴포넌트로 자동 최적화된 이미지 제공 |
| 폰트 최적화 | `next/font`를 활용해 폰트 로딩 속도 및 성능 개선 가능 |
| 서드파티 스크립트 최적화 | Next.js `Script` 컴포넌트로 외부 스크립트 로딩 최적화 |
| ESLint 설정 업데이트 | Next.js 규칙을 지원하도록 ESLint를 구성하여 코드 품질 유지 |

---

여기서 한 가지 팁을 추가하자면, Next.js의 App Router는 전통적인 React Router보다 많은 부분에서 성능과 개발 경험을 향상시키는데요. 특히 서버 컴포넌트(Server Components)를 적극 활용하면 클라이언트에서 불필요한 자바스크립트가 줄어들어 사용자 경험이 좋아집니다. 그리고 이미지나 폰트 최적화는 SEO와 사용자 경험에 직접적인 영향을 주기 때문에 꼭 챙겨보시길 추천드립니다!

조금씩 바꾸면서 Next.js 애플리케이션의 진정한 잠재력을 느껴보세요. 궁금한 점이 있으면 언제든 질문해 주세요!