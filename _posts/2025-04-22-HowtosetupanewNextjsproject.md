---
title: "Next.js 14 2025년 최신 프로젝트 세팅 방법 완벽 가이드"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 00:32
ogImage:
  url: /assets/nextjs.png
tag: Tech
originalTitle: "How to set up a new Next.js project"
link: "https://nextjs.org/docs/app/getting-started/installation"
isUpdated: false
---

# 새로운 Next.js 프로젝트 설정 방법

## 시스템 요구 사항

시작하기 전에, 시스템이 다음 요구 사항을 충족하는지 확인하세요:

| 요구 사항 | 설명                                 |
| --------- | ------------------------------------ |
| Node.js   | 18.18 버전 이상                      |
| 운영 체제 | macOS, Windows(WSL 포함), 또는 Linux |

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

## 자동 설치

새로운 Next.js 앱을 가장 빠르게 만드는 방법은 create-next-app을 사용하는 것입니다. 이 도구가 모든 설정을 자동으로 해줍니다. 프로젝트를 생성하려면 다음 명령어를 실행하세요:

```js
npx create-next-app@latest
```

설치 중에 다음과 같은 프롬프트가 나타납니다:

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

| 질문                                                             | 선택지   |
| ---------------------------------------------------------------- | -------- |
| What is your project named?                                      | my-app   |
| Would you like to use TypeScript?                                | No / Yes |
| Would you like to use ESLint?                                    | No / Yes |
| Would you like to use Tailwind CSS?                              | No / Yes |
| Would you like your code inside a `src/` directory?              | No / Yes |
| Would you like to use App Router? (recommended)                  | No / Yes |
| Would you like to use Turbopack for `next dev`?                  | No / Yes |
| Would you like to customize the import alias (`@/*` by default)? | No / Yes |
| What import alias would you like configured?                     | @/\*     |

프로젝트 생성 후, create-next-app이 프로젝트 이름으로 된 폴더를 만들고 필요한 의존성들을 설치해 줍니다.

## 수동 설치

Next.js 앱을 수동으로 새로 만들고 싶다면, 필요한 패키지를 설치하세요:

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

| 스크립트명 | 설명                                      |
| ---------- | ----------------------------------------- |
| dev        | 개발 서버를 실행합니다.                   |
| build      | 프로덕션용으로 애플리케이션을 빌드합니다. |
| start      | 빌드된 애플리케이션을 실행합니다.         |
| lint       | 코드 린팅을 실행합니다.                   |

위 명령어들은 애플리케이션 개발의 각 단계를 위한 스크립트들이에요. `npm install`로 최신 버전의 Next.js와 React를 설치한 뒤, `package.json`에 이 스크립트들을 추가하면 다양한 개발 작업을 쉽게 실행할 수 있답니다!

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

- next dev: 개발 서버를 시작합니다.
- next build: 애플리케이션을 프로덕션용으로 빌드합니다.
- next start: 프로덕션 서버를 시작합니다.
- next lint: ESLint를 실행합니다.

### 앱 디렉토리 만들기

Next.js는 파일 시스템 라우팅을 사용해, 애플리케이션의 라우트가 파일 구조에 따라 결정됩니다.

`app` 폴더를 만드세요. 그리고 `app` 폴더 안에 `layout.tsx` 파일을 만드세요. 이 파일이 루트 레이아웃이고 꼭 필요하며, `html`과 `body` 태그를 포함해야 합니다.

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
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

홈 페이지 app/page.tsx를 다음과 같이 초기 콘텐츠와 함께 만들어 주세요:

```js
export default function Page() {
  return <h1>Hello, Next.js!</h1>;
}
```

사용자가 애플리케이션의 루트 경로 (/)를 방문하면 layout.tsx와 page.tsx 두 파일이 모두 렌더링됩니다.

| 파일명     | 역할                                       |
| ---------- | ------------------------------------------ |
| layout.tsx | HTML 구조와 공통 레이아웃 정의             |
| page.tsx   | 루트 경로에 표시할 초기 페이지 콘텐츠 제공 |

친절하게 궁금한 점 있으면 언제든지 물어보세요!

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

<img src="/TIL/assets/img/2025-04-22-HowtosetupanewNextjsproject_0.png" />

> 알아두면 좋은 점:
> 루트 레이아웃(root layout) 파일을 만들지 않아도, next dev 명령어로 개발 서버를 실행할 때 Next.js가 자동으로 이 파일을 생성해 줍니다.  
> 프로젝트 루트에 src 디렉터리를 만들어 애플리케이션 코드를 구성 파일과 분리해서 관리할 수도 있습니다.

### public 폴더 생성하기 (선택 사항)

프로젝트 루트에 public 폴더를 만들어 이미지, 폰트 등 정적 자산을 저장해 보세요.  
public 폴더 안에 있는 파일들은 기본 URL(/)부터 참조할 수 있습니다.

| 폴더명 | 설명                                       |
| ------ | ------------------------------------------ |
| public | 정적 자산(이미지, 폰트 등)을 저장하는 폴더 |

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

자산은 루트 경로(/)를 사용해 참조할 수 있습니다. 예를 들어, public/profile.png 파일은 /profile.png로 참조할 수 있습니다:

```js
import Image from "next/image";

export default function Page() {
  // 여기에 컴포넌트 내용을 작성하세요
}
```

## 개발 서버 실행하기

- npm run dev 명령어로 개발 서버를 시작하세요.
- http://localhost:3000 에 접속해 애플리케이션을 확인하세요.
- app/page.tsx 파일을 수정하고 저장하면 브라우저에서 변경된 결과를 즉시 볼 수 있습니다.

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

## TypeScript 설정하기

> 최소 TypeScript 버전: v4.5.2

Next.js는 TypeScript를 기본적으로 지원합니다. 프로젝트에 TypeScript를 추가하려면 파일 이름을 `.ts` 또는 `.tsx`로 변경하고 `next dev`를 실행하세요. 그러면 Next.js가 필요한 종속성을 자동으로 설치하고, 권장 설정이 포함된 `tsconfig.json` 파일을 추가합니다.

### IDE 플러그인

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

Next.js는 VSCode와 다른 코드 편집기에서 고급 타입 검사와 자동 완성을 사용할 수 있도록 맞춤형 TypeScript 플러그인과 타입 체크 기능을 포함하고 있어요.

VS Code에서 플러그인을 활성화하려면 다음 단계를 따라 주세요:

- 명령 팔레트 열기 (Ctrl/⌘ + Shift + P)
- "TypeScript: Select TypeScript Version" 검색하기
- "Use Workspace Version" 선택하기

| 단계 | 설명                                         |
| ---- | -------------------------------------------- |
| 1    | 명령 팔레트 열기 (Ctrl/⌘ + Shift + P)        |
| 2    | "TypeScript: Select TypeScript Version" 검색 |
| 3    | "Use Workspace Version" 선택                 |

<img src="/TIL/assets/img/2025-04-22-HowtosetupanewNextjsproject_1.png" />

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

TypeScript 참고 페이지에서 더 많은 정보를 확인할 수 있어요.

## ESLint 설정하기

Next.js는 내장 ESLint를 제공합니다. create-next-app으로 새 프로젝트를 만들면 필요한 패키지를 자동으로 설치하고 적절한 설정을 구성해 줘요.

기존 프로젝트에 ESLint를 수동으로 추가하려면 package.json에 다음과 같이 next lint 스크립트를 추가하면 됩니다:

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
{
  "scripts": {
    "lint": "next lint"
  }
}
```

그런 다음, `npm run lint` 명령어를 실행하면 설치 및 설정 과정을 안내받을 수 있습니다.

```js
npm run lint
```

다음과 같은 프롬프트가 나타날 거예요:

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

> ? ESLint를 어떻게 설정하시겠어요?
> ❯ 엄격 모드 (권장)
> 기본 모드
> 취소

| 옵션      | 설명                                                                                                                                  |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| 엄격 모드 | Next.js의 기본 ESLint 설정에 더 엄격한 Core Web Vitals 규칙 세트가 포함됩니다. ESLint를 처음 설정하는 개발자에게 권장되는 설정입니다. |
| 기본 모드 | Next.js의 기본 ESLint 설정이 포함됩니다.                                                                                              |
| 취소      | 설정을 건너뜁니다. 직접 커스텀 ESLint 설정을 할 계획이라면 이 옵션을 선택하세요.                                                      |

엄격 모드나 기본 모드를 선택하면, Next.js가 자동으로 eslint와 eslint-config-next를 애플리케이션의 종속성으로 설치하고, 프로젝트 루트에 선택한 설정이 포함된 `.eslintrc.json` 파일을 생성합니다.

이제 `next lint` 명령어를 실행하여 ESLint로 오류를 점검할 수 있습니다. ESLint 설정이 완료되면, 빌드할 때마다 (`next build`) 자동으로 ESLint가 실행됩니다. 오류가 있을 경우 빌드가 실패하며, 경고는 빌드에 영향을 주지 않습니다.

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

ESLint 플러그인 페이지에서 더 많은 정보를 확인할 수 있어요.

## 절대 경로 및 모듈 경로 별칭 설정하기

Next.js는 tsconfig.json과 jsconfig.json 파일의 "paths"와 "baseUrl" 옵션을 기본적으로 지원해요.

이 옵션들을 통해 프로젝트 디렉터리를 절대 경로로 별칭(alias) 설정할 수 있어서, 모듈을 더 쉽고 깔끔하게 임포트할 수 있답니다. 예를 들어:

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

| Before                                                    | After                                              |
| --------------------------------------------------------- | -------------------------------------------------- |
| js<br>import { Button } from '../../../components/button' | js<br>import { Button } from '@/components/button' |

절대 경로(import)를 설정하려면 tsconfig.json 또는 jsconfig.json 파일에 baseUrl 설정 옵션을 추가하세요. 예를 들어:

```js
{
  "compilerOptions": {
    "baseUrl": "src/"
  }
}
```

baseUrl 경로를 설정하는 것 외에도 "paths" 옵션을 사용해 모듈 경로에 별칭(alias)을 지정할 수 있습니다.

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

예를 들어, 다음 설정은 @/components/*를 components/*에 매핑합니다:

```js
{
  "compilerOptions": {
    "baseUrl": "src/",
    "paths": {
      "@/styles/*": ["styles/*"],
      "@/components/*": ["components/*"]
    }
  }
}
```

"paths"에 있는 경로들은 모두 baseUrl 위치를 기준으로 한 상대 경로입니다.
