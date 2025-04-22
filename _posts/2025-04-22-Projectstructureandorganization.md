---
title: "Next.js 15 프로젝트 폴더 설계 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 00:55
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Project structure and organization"
link: "https://nextjs.org/docs/app/getting-started/project-structure"
isUpdated: false
---

# 프로젝트 구조와 조직 방법

이번 글에서는 Next.js에서 사용하는 폴더와 파일 규칙에 대해 한눈에 정리해 드리고, 프로젝트를 어떻게 잘 구성할지에 대한 팁도 함께 알려드릴게요.

## 폴더와 파일 규칙

### 최상위 폴더

최상위 폴더는 애플리케이션 코드와 정적 자산(이미지, 폰트 등)을 구분해서 관리할 때 사용해요.

![프로젝트 구조 이미지](/TIL/assets/img/2025-04-22-Projectstructureandorganization_0.png)

| 폴더 이름                                                                                  | 설명                                                 |
| ------------------------------------------------------------------------------------------ | ---------------------------------------------------- |
| [`app`](https://nextjs.org/docs/app/building-your-application/routing)                     | App Router (Next.js의 새로운 라우팅 방식)            |
| [`pages`](https://nextjs.org/docs/pages/building-your-application/routing)                 | Pages Router (기존 라우팅 방식)                      |
| [`public`](https://nextjs.org/docs/app/building-your-application/optimizing/static-assets) | 외부에 서비스할 정적 자산 모음                       |
| [`src`](https://nextjs.org/docs/app/building-your-application/configuring/src-directory)   | 옵션! 소스코드를 별도 관리하고 싶을 때 사용하는 폴더 |

### 최상위 파일

(여기서부터 다음 내용이 이어집니다.)

---

폴더 구조를 깔끔하게 정리하면 프로젝트 유지보수가 훨씬 수월해져요. 너무 복잡하게 만들지 말고, 자신의 프로젝트에 맞게 심플하게 가져가는 게 가장 좋아요!

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

Next.js에서 최상위(Top-level) 파일들은 여러분의 애플리케이션을 설정하고, 의존성을 관리하며, 미들웨어를 실행하고, 모니터링 도구를 통합하고, 환경 변수를 정의하는 데 사용돼요. 그러니까 프로젝트의 중요한 설정들이 여기서 모여있다고 보면 됩니다.

다음은 각 파일들이 어떤 역할을 하는지 간단히 정리한 표에요:

| 파일명                                                                                                        | 설명                            |
| ------------------------------------------------------------------------------------------------------------- | ------------------------------- |
| [`next.config.js`](https://nextjs.org/docs/app/api-reference/config/next-config-js)                           | Next.js 설정 파일               |
| [`package.json`](https://nextjs.org/docs/app/getting-started/installation#manual-installation)                | 프로젝트 의존성과 스크립트 관리 |
| [`instrumentation.ts`](https://nextjs.org/docs/app/building-your-application/optimizing/instrumentation)      | OpenTelemetry 및 계측 관련 파일 |
| [`middleware.ts`](https://nextjs.org/docs/app/building-your-application/routing/middleware)                   | Next.js 요청 미들웨어 파일      |
| [`.env`](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables)             | 환경 변수 설정 파일             |
| [`.env.local`](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables)       | 로컬 환경 변수 설정             |
| [`.env.production`](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables)  | 프로덕션 환경 변수 설정         |
| [`.env.development`](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables) | 개발 환경 변수 설정             |
| [`.eslintrc.json`](https://nextjs.org/docs/app/api-reference/config/eslint)                                   | ESLint 설정 파일                |
| `.gitignore`                                                                                                  | Git에서 무시할 파일/폴더 목록   |
| `next-env.d.ts`                                                                                               | Next.js용 TypeScript 선언 파일  |
| `tsconfig.json`                                                                                               | TypeScript 설정 파일            |
| `jsconfig.json`                                                                                               | JavaScript 설정 파일            |

---

### 라우팅 관련 파일

Next.js에서 앱 라우팅을 구성하는 데 사용되는 파일들을 모아봤어요:

| 파일명                                                                                          | 확장자            | 설명                          |
| ----------------------------------------------------------------------------------------------- | ----------------- | ----------------------------- |
| [`layout`](https://nextjs.org/docs/app/api-reference/file-conventions/layout)                   | .js / .jsx / .tsx | 레이아웃 컴포넌트             |
| [`page`](https://nextjs.org/docs/app/api-reference/file-conventions/page)                       | .js / .jsx / .tsx | 페이지 컴포넌트               |
| [`loading`](https://nextjs.org/docs/app/api-reference/file-conventions/loading)                 | .js / .jsx / .tsx | 로딩중 UI 컴포넌트            |
| [`not-found`](https://nextjs.org/docs/app/api-reference/file-conventions/not-found)             | .js / .jsx / .tsx | 404 페이지 UI                 |
| [`error`](https://nextjs.org/docs/app/api-reference/file-conventions/error)                     | .js / .jsx / .tsx | 에러 UI                       |
| [`global-error`](https://nextjs.org/docs/app/api-reference/file-conventions/error#global-error) | .js / .jsx / .tsx | 글로벌 에러 UI                |
| [`route`](https://nextjs.org/docs/app/api-reference/file-conventions/route)                     | .js / .ts         | API 엔드포인트                |
| [`template`](https://nextjs.org/docs/app/api-reference/file-conventions/template)               | .js / .jsx / .tsx | 다시 렌더링되는 레이아웃      |
| [`default`](https://nextjs.org/docs/app/api-reference/file-conventions/default)                 | .js / .jsx / .tsx | 병렬 라우트용 fallback 페이지 |

---

### 중첩 라우트 (Nested routes)

Next.js에서는 폴더 구조를 통해 중첩된 라우트를 쉽게 표현할 수 있어요.

| 폴더 구조       | 설명                   |
| --------------- | ---------------------- |
| `folder`        | 하나의 라우트 세그먼트 |
| `folder/folder` | 중첩된 라우트 세그먼트 |

---

### 동적 라우트 (Dynamic routes)

동적 라우팅은 경로에 가변 요소가 포함될 때 사용돼요.

| 경로 표현법                                                                                                                 | 설명                                         |
| --------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| [`[folder]`](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes#convention)                       | 동적 라우트 세그먼트                         |
| [`[...folder]`](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes#catch-all-segments)            | 모든 하위 경로를 잡아내는 catch-all 세그먼트 |
| [`[[...folder]]`](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes#optional-catch-all-segments) | 선택적인 catch-all 세그먼트                  |

---

블로그를 보시는 여러분도 Next.js를 쓸 때 이 파일들과 폴더 구조들을 잘 알고 나면, 프로젝트를 더 효율적이고 이해하기 쉽게 관리할 수 있습니다! 필요할 때마다 이 표를 참고하세요~

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

### Route Groups와 private 폴더

| 구분       | 설명                                             |
| ---------- | ------------------------------------------------ |
| `(folder)` | 라우팅에 영향을 주지 않고 라우트를 그룹화        |
| `_folder`  | 해당 폴더와 모든 하위 세그먼트를 라우팅에서 제외 |

### 병렬(Parallel) 및 인터셉트(Intercepted) 라우트

| 구분             | 설명                            |
| ---------------- | ------------------------------- |
| `@folder`        | 네임드 슬롯(named slot)         |
| `(.)folder`      | 같은 레벨을 인터셉트(intercept) |
| `(..)folder`     | 한 단계 위 레벨을 인터셉트      |
| `(..)(..)folder` | 두 단계 위 레벨을 인터셉트      |
| `(...)folder`    | 루트에서부터 인터셉트           |

### 메타데이터 파일 규칙

#### 앱 아이콘(App icons)

| 이름         | 확장자                                  | 설명                         |
| ------------ | --------------------------------------- | ---------------------------- |
| `favicon`    | `.ico`                                  | 파비콘(favicon) 파일         |
| `icon`       | `.ico`, `.jpg`, `.jpeg`, `.png`, `.svg` | 앱 아이콘 파일               |
| `icon`       | `.js`, `.ts`, `.tsx`                    | 코드로 생성된 앱 아이콘      |
| `apple-icon` | `.jpg`, `.jpeg`, `.png`                 | 애플 앱 아이콘 파일          |
| `apple-icon` | `.js`, `.ts`, `.tsx`                    | 코드로 생성된 애플 앱 아이콘 |

---

조금 어렵게 느껴질 수 있는 routing 영역에서 폴더명을 활용하는 여러 가지 문법과 파일명 규칙을 정리해봤어요.  
특히 (folder) 같이 라우팅에는 영향을 주지 않으면서 내부 조직화용으로 많이 쓰이고요, `_folder`는 아예 routing 대상에서 제외하는 용도로 쓴답니다.  
Parallel과 Intercept 라우트는 고급 기능인데, 슬래시 하나, 두 개 등에 따라 라우팅 경로를 제어할 수 있어서 원하는 맞춤 페이지 구성에 도움을 줘요.  
마지막으로 메타데이터 쪽은 favicon이나 app icon 관련 파일명과 확장자를 정해놓은 내용이니 프로젝트 셋업할 때 참고하면 좋습니다!

필요하시면 더 자세한 설명도 알려드릴게요!

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

아래는 Open Graph 이미지, Twitter 이미지, 그리고 SEO 관련 파일 종류와 확장자, 그리고 용도에 대한 표입니다. Markdown 형식으로 정리해 봤어요!

### Open Graph & Twitter 이미지 파일

| 이름                                                                                                                                        | 확장자                          | 설명                              |
| ------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------- | --------------------------------- |
| [opengraph-image](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#opengraph-image)                      | `.jpg`, `.jpeg`, `.png`, `.gif` | Open Graph 이미지 파일            |
| [opengraph-image](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#generate-images-using-code-js-ts-tsx) | `.js`, `.ts`, `.tsx`            | 코드로 생성하는 Open Graph 이미지 |
| [twitter-image](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#twitter-image)                          | `.jpg`, `.jpeg`, `.png`, `.gif` | Twitter에 보여줄 이미지 파일      |
| [twitter-image](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image#generate-images-using-code-js-ts-tsx)   | `.js`, `.ts`, `.tsx`            | 코드로 생성하는 Twitter 이미지    |

### SEO 관련 파일

| 이름                                                                                                                         | 확장자       | 설명                        |
| ---------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------------------- |
| [sitemap](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap#sitemap-files-xml)                     | `.xml`       | 사이트맵 파일               |
| [sitemap](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap#generating-a-sitemap-using-code-js-ts) | `.js`, `.ts` | 코드로 생성하는 사이트맵    |
| [robots](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots#static-robotstxt)                        | `.txt`       | robots.txt 파일             |
| [robots](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/robots#generate-a-robots-file)                  | `.js`, `.ts` | 코드로 생성하는 robots 파일 |

---

## 프로젝트 구성하기

Next.js에서는 프로젝트 파일을 어떻게 배치할지는 자유롭게 선택할 수 있어요. 딱 정해진 규칙이 있는 건 아니지만, 다음과 같은 기능들이 있어서 편리하게 구성할 수 있답니다.

### 컴포넌트 계층 구조

Next.js 내에서 특별한 이름의 파일들은 아래와 같은 계층 구조를 따라 렌더링됩니다:

- `layout.js` — 페이지 레이아웃 정의
- `template.js` — 페이지 템플릿
- `error.js` — React 오류 경계(Error Boundary) 처리
- `loading.js` — React Suspense 경계로 로딩 UI
- `not-found.js` — 404 페이지용 에러 경계
- `page.js` 혹은 nested `layout.js` — 실제 페이지 컴포넌트

이 계층 덕분에 페이지 단위 컴포넌트가 어떻게 조합되고 재사용되는지 명확하게 구조화할 수 있어요. 이를 활용해서 여러분만의 깔끔한 프로젝트 구조를 만들어 보세요!

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

여러분, 오늘은 Next.js 앱 디렉토리 구조와 라우트 구성에 대해 알아볼게요. 복잡해 보일 수 있지만, 하나씩 차근차근 이해하면 실제로 프로젝트를 더 효율적으로 관리할 수 있답니다.

---

### 라우트 컴포넌트는 계층적으로 렌더링된다

우리가 만든 컴포넌트들은 중첩된 라우트(nested routes)에서 재귀적으로 렌더링돼요. 쉽게 말해, 특정 라우트의 컴포넌트는 그 부모 라우트 컴포넌트 안에 중첩되는 구조입니다.

예를 들어, `/dashboard/settings` 같은 경로가 있다면 `dashboard` 컴포넌트가 먼저 렌더링되고, 그 안에 `settings` 컴포넌트가 들어가는 식이에요.

---

### 라우트는 폴더 구조로 정의한다 — 콜로케이션(colocation)

Next.js 앱 디렉토리 안에서, **중첩된 폴더가 라우트 구조를 뜻해요**. 폴더 하나가 하나의 라우트 세그먼트(route segment)를 나타내고, 이 세그먼트가 URL 경로의 일부로 매핑됩니다.

하지만 중요한 점! 폴더만 있다고 해서 그 경로가 바로 “공개적으로” 접근 가능한 페이지가 되진 않아요. 그 폴더 안에 `page.js` 혹은 `route.js` 파일이 있어야 비로소 해당 라우트가 활성화되고, 방문할 수 있답니다.

---

### 실제 클라이언트에 전송되는 것은?

라우트가 공개되어 있어도, 클라이언트에 보내지는 건 그 라우트의 `page.js`나 `route.js`에서 반환하는 콘텐츠뿐이에요. 부모 라우트 컴포넌트가 중첩되어 렌더링되더라도, 클라이언트에게는 각 세그먼트의 `page.js` 결과물만 전달되니까 참고하세요!

---

정리하면,

- 라우트 구조는 폴더 중첩으로 표현한다.
- 해당 폴더에 `page.js`나 `route.js` 파일이 있어야 라우트가 활성화된다.
- 중첩된 라우트는 부모-자식 구조로 렌더링된다.
- 클라이언트에는 각 세그먼트 라우트 파일에서 반환한 콘텐츠만 전달된다.

이런 구조 덕분에 프로젝트 내 파일과 라우트가 자연스럽게 연관되니, 관리도 쉽고 개발 속도도 빨라진답니다. 여러분도 프로젝트에 적용해 보세요!

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

앱 디렉터리 내에서 라우트 구간(route segments) 안에 프로젝트 파일들을 안전하게 함께 둘 수 있다는 뜻이에요. 이렇게 하면 파일들이 실수로 라우트로 잡히는 일을 방지할 수 있답니다.

---

> 참고할 점: 프로젝트 파일들을 꼭 app 폴더 안에 넣어야 하는 건 아니에요. 원한다면 app 디렉터리 밖에 보관해도 무방합니다.

---

### Private 폴더 만들기

폴더 이름 앞에 언더스코어(\_)를 붙여서 private 폴더를 만들 수 있어요. 예를 들면 `_folderName` 같은 식이죠.

이렇게 하면 라우팅시스템이 이 폴더와 그 하위 폴더들을 무시하니까, 라우트로 작동하지 않는 구현 세부 사항용 폴더로 쓸 수 있어요.

---

관련 그림 첨부해 뒀으니 한 번 참고해 보세요!

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

앱 디렉터리 내 파일은 기본적으로 안전하게 한 곳에 모아둘 수 있어서 private 폴더가 꼭 필요한 건 아니에요. 하지만 private 폴더가 있으면 다음과 같은 점에서 도움이 됩니다:

- UI 로직과 라우팅 로직을 분리할 수 있어요.
- 프로젝트 내 파일 구성과 Next.js 생태계 전반에서 내부 파일을 일관성 있게 정리할 수 있죠.
- 코드 에디터에서 파일을 분류하고 그룹화하기 쉬워요.
- 앞으로 나올 Next.js 파일 네이밍 규칙과 겹칠 수 있는 이름 충돌을 방지할 수 있습니다.

> 참고!
> 비록 Next.js 공식 프레임워크 규칙은 아니지만, private 폴더 외부에 있는 파일들도 언더스코어(\_) 패턴을 사용해서 "private" 임을 표시하는 방식을 고민해볼 수 있어요.  
> URL 세그먼트가 언더스코어로 시작해야 한다면 URL 인코딩된 형태인 `%5F`를 앞에 붙이면 됩니다. 예를 들어 `%5FfolderName`처럼요.  
> 만약 private 폴더를 사용하지 않는다면, 예상치 못한 이름 충돌을 막기 위해 Next.js의 특별한 파일 네이밍 규칙을 잘 숙지하는 게 좋겠죠.

### 라우트 그룹(Route groups)

라우트 그룹은 폴더 이름을 괄호로 감싸서 만듭니다: `(folderName)`

이렇게 하면 이 폴더는 단순히 파일 정리를 위한 용도이고, 실제 라우트 URL 경로에는 포함되지 않아요.

![라우트 그룹 예시 이미지](/TIL/assets/img/2025-04-22-Projectstructureandorganization_7.png)

라우트 그룹이 유용한 점은 다음과 같습니다:

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

- 사이트 섹션, 의도, 혹은 팀 단위로 라우트를 정리하기 (예: 마케팅 페이지, 관리자 페이지 등)
- 같은 라우트 세그먼트 레벨에서 중첩 레이아웃 활성화하기:

  - 같은 세그먼트 내에 여러 개의 중첩 레이아웃 만들기 (여러 개의 루트 레이아웃 포함)
  - 공통 세그먼트 내 일부 라우트에만 레이아웃 추가하기

- 같은 세그먼트 내에 여러 개의 중첩 레이아웃 만들기 (여러 개의 루트 레이아웃 포함)
- 공통 세그먼트 내 일부 라우트에만 레이아웃 추가하기

---

### src 디렉토리

Next.js는 app을 포함한 애플리케이션 코드를 선택적으로 `src` 디렉토리 안에 저장하는 것을 지원합니다. 이렇게 하면 프로젝트 루트에 주로 위치하는 설정 파일과 애플리케이션 코드를 깔끔하게 분리할 수 있습니다.

![Project Structure](/TIL/assets/img/2025-04-22-Projectstructureandorganization_8.png)

---

### 예제

아래 섹션은 흔히 사용하는 전략들을 아주 간략히 정리한 내용입니다. 가장 중요한 점은 여러분과 팀에게 맞는 방식을 선택하고, 프로젝트 전반에 걸쳐 일관성 있게 유지하는 것입니다.

> 참고: 아래 예시에서는 `components`와 `lib` 폴더를 일반적인 예시 폴더명으로 사용했으며, 이들의 이름은 프레임워크에서 특별한 의미를 가진 것은 아닙니다. 프로젝트에 따라 `ui`, `utils`, `hooks`, `styles` 등 다양한 이름을 사용해도 무방합니다.

---

#### app 폴더 밖에 프로젝트 파일 저장하기

| 전략 구분               | 설명                                                               |
| ----------------------- | ------------------------------------------------------------------ |
| app 폴더 분리           | app 폴더와는 별개로 components, lib 같은 폴더를 루트 혹은 src에 둠 |
| 역할별 폴더 구성        | 컴포넌트, 유틸리티, 훅 등을 역할에 맞춰 분리함                     |
| 팀/기능별으로 폴더 분리 | 팀 혹은 기능 단위로 폴더를 나눠 관리함                             |

---

위와 같은 방법으로 프로젝트를 체계적으로 구성하면 협업 효율이 올라가고 유지보수가 쉬워집니다. 꼭 정답은 없으니 팀 상황에 맞게 유연하게 적용해보세요!

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

이 전략은 애플리케이션 코드를 프로젝트 루트의 공유 폴더에 모두 저장하고, app 디렉터리는 오로지 라우팅 용도로만 사용하는 방식이에요.

![프로젝트 구조 1](/TIL/assets/img/2025-04-22-Projectstructureandorganization_9.png)

#### app 내부의 최상위 폴더에 프로젝트 파일 저장하기

이 전략은 애플리케이션 코드를 app 디렉터리 루트에 공유 폴더 형태로 모두 저장하는 방식이에요.

![프로젝트 구조 2](/TIL/assets/img/2025-04-22-Projectstructureandorganization_10.png)

#### 기능이나 라우트별로 프로젝트 파일 분리하기

이 전략은 전역에서 공유되는 애플리케이션 코드는 app 디렉터리 루트에 두고, 좀 더 특정한 애플리케이션 코드는 해당 라우트 세그먼트(경로 단위) 안에 나눠서 저장하는 방식이에요.

![프로젝트 구조 3](/TIL/assets/img/2025-04-22-Projectstructureandorganization_11.png)

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

### URL 경로에 영향 없이 라우트 정리하기

URL 경로를 변경하지 않고 라우트를 깔끔하게 정리하고 싶다면, 관련된 라우트들을 그룹으로 묶으면 좋아요. 이때, 괄호로 묶은 폴더 이름(예: `(marketing)`, `(shop)`)은 URL에서 제외됩니다.

예를 들어, 아래 구조에서 `(marketing)`과 `(shop)`은 URL에 나타나지 않고, 폴더 내의 라우트들이 그대로 URL에 반영돼요.

![route 그룹 예시](/TIL/assets/img/2025-04-22-Projectstructureandorganization_12.png)

또한 `(marketing)`과 `(shop)` 폴더 안에 `layout.js` 파일을 추가하면, 각각의 그룹마다 개별 레이아웃을 적용할 수 있습니다. 즉, URL 경로는 같아도 레이아웃은 다르게 설정 가능하다는 거죠.

![개별 레이아웃 적용 예시](/TIL/assets/img/2025-04-22-Projectstructureandorganization_13.png)

### 특정 세그먼트만 레이아웃에 포함시키기

특정 라우트만 하나의 레이아웃으로 묶고 싶다면, 해당 라우트들만 새로운 그룹(예: `(shop)`)으로 묶으면 됩니다. 그룹에 포함된 라우트들은 그룹 레이아웃을 공유하고, 밖에 있는 라우트는 영향을 받지 않아요.

예를 들어, `account`와 `cart`는 `(shop)` 그룹에 넣어 레이아웃을 공유하고, `checkout`은 그룹 밖에 둬서 별도의 레이아웃을 유지할 수 있습니다.

![특정 그룹에만 레이아웃 적용 예시](/TIL/assets/img/2025-04-22-Projectstructureandorganization_14.png)

이런 방법을 사용하면 프로젝트 구조를 깔끔하게 유지하면서도 URL 디자인에는 전혀 영향 주지 않을 수 있어서 정말 유용합니다!

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

### 특정 경로에 로딩 스켈레톤 적용하기

특정 라우트에만 로딩 스켈레톤을 적용하고 싶다면, 새로운 라우트 그룹 (예: /(overview))을 만들고 그 안에 loading.tsx 파일을 넣으면 됩니다.

<img src="/TIL/assets/img/2025-04-22-Projectstructureandorganization_15.png" />

이렇게 하면 loading.tsx 파일은 대시보드 내의 overview 페이지에만 적용되고, 다른 대시보드 페이지에는 영향을 주지 않으면서 URL 경로 구조도 유지할 수 있어요.

---

### 여러 개의 루트 레이아웃 만들기

여러 루트 레이아웃을 만들고 싶으면 최상위에 있던 layout.js 파일을 제거한 다음, 각 라우트 그룹 내부에 layout.js 파일을 각각 만들어 주면 됩니다. 이렇게 하면 완전히 다른 UI나 경험이 필요한 섹션을 애플리케이션 내에 구분하기 매우 편리해요. 이때 각 루트 레이아웃에는 `html`과 `body` 태그가 반드시 포함되어야 합니다.

<img src="/TIL/assets/img/2025-04-22-Projectstructureandorganization_16.png" />

위 예시처럼, (marketing)과 (shop)은 각각 자신만의 루트 레이아웃을 가지고 있게 됩니다.
