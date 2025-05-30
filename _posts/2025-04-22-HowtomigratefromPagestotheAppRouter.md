---
title: "Nextjs 13에서 Pages에서 App Router로 마이그레이션하는 방법 완벽 가이드"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:20
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to migrate from Pages to the App Router"
link: "https://nextjs.org/docs/app/guides/migrating/app-router-migration"
isUpdated: false
---


# Pages에서 App Router로 마이그레이션하는 법

이번 글에서는 Next.js 12에서 13으로 업데이트하면서, 기존의 pages 디렉토리 기반 애플리케이션을 app 디렉토리 기반으로 점진적으로 옮기는 방법을 알려드릴게요. 뿐만 아니라 pages와 app 디렉토리 양쪽에서 모두 작동하는 업그레이드 팁도 함께 다룹니다.

## 업그레이드하기

(이하 본문이 계속 이어지겠죠)

---

Next.js 13은 app 디렉토리로 라우팅 방식을 대대적으로 바꾸면서 더 유연하고 강력해졌어요. 하지만 갑작스럽게 모두 바꾸기엔 부담스러우니, 페이지 단위로 조금씩 옮길 수 있도록 설계된 점도 매력 포인트입니다.

특히 기존에 pages 기반으로 만들어둔 큰 프로젝트라면, 한 번에 바꾸기보단 섹션별로 앱 디렉토리 구조를 도입하면서 자연스럽게 최신 기능들을 써볼 수 있어요.

예를 들어 `pages/about.js`는 아직 냅두고, 새로 만들거나 자주 바꾸는 부분부터 app 폴더 내부의 컴포넌트 및 레이아웃으로 점차 변경해보는 걸 추천합니다.

---

뒤에서 더 자세히 살펴볼게요!

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

### Node.js 버전

최소 Node.js 버전이 이제 v18.17로 올라갔어요. 자세한 내용은 [Node.js 공식 문서](https://nodejs.org/en/docs/)에서 확인할 수 있으니 참고해 보세요.

### Next.js 버전

Next.js를 13버전으로 업데이트하려면, 아래 명령어를 사용하는 패키지 매니저에 맞춰 실행하면 됩니다:

```bash
# npm 사용 시
npm install next@13

# yarn 사용 시
yarn add next@13

# pnpm 사용 시
pnpm add next@13
```

---

> 여기서 잠깐!  
> Next.js 13은 여러 신기능과 성능 개선이 함께 들어왔어요. 예를 들어, 새로운 App Router가 도입되어 페이지 라우팅이 더 유연해졌고, React Server Components도 지원하죠. 업데이트 전에 꼭 [Next.js 13 공식 문서](https://nextjs.org/blog/next-13)를 한 번 살펴보시면 도움이 될 거예요!

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

```bash
npm install next@latest react@latest react-dom@latest
```

### ESLint 버전 업데이트

만약 ESLint를 사용하고 있다면, ESLint 버전도 함께 업그레이드해주는 게 좋아요!

```bash
npm install -D eslint-config-next@latest
```

> 참고로 ESLint 설정 파일(.eslintrc.js 등)에서 `eslint-config-next`를 베이스로 사용한다면, 버전이 맞지 않으면 Next.js와 충돌이 날 수 있어서 꼭 같이 업데이트 해주세요.

그리고 혹시 ESLint가 처음이라면, Next.js에서 기본적으로 권장하는 ESLint 설정을 따라서 쓰는 걸 추천해요. 코딩 컨벤션을 맞추고 코드 품질을 높이는 데 도움 되니까요!

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

알아두면 좋은 팁: ESLint 설정을 변경한 후에는 VS Code에서 ESLint 서버를 재시작해줘야 변경 사항이 제대로 반영될 수 있어요. 재시작 방법은 간단해요! Mac에서는 cmd+shift+p, Windows에서는 ctrl+shift+p를 눌러 커맨드 팔레트를 열고 'ESLint: Restart ESLint Server'를 검색해서 실행하면 됩니다.

---

## 다음 단계

업데이트를 마쳤다면 아래 섹션들을 참고해서 다음 과정을 진행해보세요:

- **새로운 기능 업그레이드하기**: 향상된 Image 컴포넌트와 Link 컴포넌트를 비롯한 새로운 기능들로 업그레이드하는 방법을 알려주는 가이드입니다.  
- **pages 디렉터리에서 app 디렉터리로 마이그레이션하기**: 기존의 pages 디렉터리에서 점진적으로 app 디렉터리로 전환하는 단계별 가이드로, 프로젝트 구조를 최신 방식으로 바꾸는 데 도움을 줍니다.

---

+ 팁을 한 가지 더 드리자면, ESLint 서버 재시작은 설정 변경 후 바로바로 효과를 보고 싶을 때 꼭 해주는 게 좋아요. 가끔은 VS Code를 전체 종료하고 다시 여는 것보다 훨씬 빠른 해결책이 될 때가 많답니다!

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

## Next.js 13에서 새 기능 업그레이드하기

Next.js 13에서는 새로운 App Router가 도입되면서 여러 가지 기능과 컨벤션들이 추가됐어요. 이 새로운 라우터는 `app` 디렉토리 안에서 사용 가능하고, 기존의 `pages` 디렉토리와도 함께 공존할 수 있답니다.

중요한 점은, Next.js 13으로 업그레이드한다고 해서 반드시 App Router를 바로 사용해야 하는 건 아니에요! 기존에 쓰던 `pages` 디렉토리 기반 라우팅도 계속 쓸 수 있고, 두 디렉토리 모두에서 동작하는 최신 기능들도 사용할 수 있다는 점이죠. 예를 들면, 업데이트된 `Image` 컴포넌트, `Link` 컴포넌트, `Script` 컴포넌트, 그리고 폰트 최적화 같은 기능들이 그런 것들이에요.

### `Image` 컴포넌트

Next.js 13에서 `Image` 컴포넌트가 훨씬 더 좋아졌어요! 이전 버전에 비해서 성능과 사용성이 많이 개선됐고, 이제부터는 자동으로 이미지 최적화가 더 강화되었답니다.

#### 주요 특징
- **자동 사이즈 조절**: 이미지 크기를 코드에 명시하지 않아도, Layout을 통해 적절히 크기를 잡아줘요.
- **새로운 레이아웃 옵션들**: `fill`, `fixed`, `intrinsic` 등 다양한 레이아웃 옵션이 추가되어 원하는 스타일에 맞게 조절 가능해요.
- **새로운 `priority` 옵션**: 중요한 이미지를 먼저 불러올 수 있게끔 설정할 수 있어요.
- **향상된 WebP 변환**: 웹 최적화를 위해 자동으로 WebP 포맷 변환을 지원하고 있어요.

저는 개인적으로 Next.js 13의 `Image` 컴포넌트를 사용하면서 페이지 로딩 속도가 확실히 빨라진 느낌이에요. 그리고 개발자가 직접 신경 써야 하는 부분이 줄어들어서, 더욱 편리하게 이미지를 다룰 수 있다는 점이 너무 좋더라고요.

---

참고로, Next.js 새 기능은 공식 문서가 정말 잘 되어 있으니, 새로운 App Router를 천천히 공부해보고 싶다면 [Next.js 공식 문서](https://nextjs.org/docs)에서 자세한 내용을 꼭 확인해 보시길 추천드릴게요!

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

Next.js 12 버전에서 Image 컴포넌트에 새로운 개선사항이 도입되었는데, 임시로 next/future/image에서 불러오는 방식이었어요. 이 개선사항에는 클라이언트 쪽 자바스크립트가 줄어들고, 이미지 확장과 스타일링이 더 쉬워졌으며, 접근성도 좋아졌고 네이티브 브라우저의 지연 로딩(lazy loading) 기능도 지원되었답니다.

그런데 Next.js 13 버전에서는 이 새로운 동작 방식이 next/image의 기본값으로 완전히 자리잡았어요!

기존 프로젝트에서 이 새로운 Image 컴포넌트로 쉽게 마이그레이션할 수 있도록 두 가지 코드를 변환해주는 'codemod' 스크립트를 제공합니다:

| 코덱모드 이름                     | 설명                                                                                           | 주의사항                                                 |
|-----------------------------|----------------------------------------------------------------------------------------------|------------------------------------------------------|
| next-image-to-legacy-image   | next/image로 임포트한 부분을 자동으로 next/legacy/image로 안전하게 이름 변경해줌. 기존 컴포넌트의 동작이 그대로 유지됨. | 이 변환은 안전해서 바로 적용 가능.                         |
| next-image-experimental      | 인라인 스타일을 과감하게 추가하고, 사용되지 않는 props를 제거함. 기존 컴포넌트 동작이 바뀌어 새로운 기본 동작을 따라가게 됨. | 사용하려면 먼저 next-image-to-legacy-image codemod를 실행해야 함. 변경점이 꽤 크니까 테스트 철저히 할 것! |

마이그레이션할 때 참고해 보세요!

추가로, Image 컴포넌트의 변경덕분에 SEO 최적화, 페이지 렌더링 속도 개선과 같은 장점도 누릴 수 있답니다. 특히 lazy loading은 이미지가 화면에 보일 때만 로드되니 초기 로딩 속도가 확실히 빨라지고, 불필요한 데이터 낭비도 줄어들어요. Next.js 13으로 업데이트 하면서 Image 컴포넌트도 꼭 점검해보세요!

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

### `Link` 컴포넌트 변화 소개

안녕하세요! 오늘은 Next.js의 `Link` 컴포넌트 사용법이 어떻게 바뀌었는지 쉽게 알려드릴게요.

옛날 버전인 Next.js 12까지는 `Link`를 사용할 때, 내부에 꼭 `<a>` 태그를 직접 감싸줘야 했어요. 예를 들어, 이렇게요:

```js
<Link href="/about">
  <a>About</a>
</Link>
```

만약 `<a>`를 안 쓰면 제대로 작동하지 않았죠. 그런데 12.2버전부터는 이게 실험적인 기능으로 바뀌었고, Next.js 13에서는 아예 기본 동작이 바뀌었답니다. 이제는 `<Link>` 자체가 내부적으로 `<a>` 태그를 자동으로 렌더링해줘서, 우리가 굳이 `<a>` 태그를 감싸줄 필요가 없어요!

즉, 이렇게 간단하게 써도 됩니다:

```js
<Link href="/about">
  About
</Link>
```

이렇게 하면 Next.js가 내부적으로 `<a>` 태그를 알아서 처리해주니까 훨씬 편하죠.

---

> 개인적으로 느낀 점은, 개발할 때 코드가 깔끔해지고, 실수로 `<a>`를 빼먹는 실수도 방지할 수 있다는 점이 좋았어요. 그리고 스타일링도 `Link` 컴포넌트에 바로 props를 넘겨서 할 수 있으니 편리하답니다!

---

요약하자면:  
- Next.js 12까지는 `<Link>` 안에 직접 `<a>` 태그를 넣어야 했어요.  
- Next.js 13부터는 `<Link>`가 자동으로 `<a>` 태그를 렌더링합니다.  
- 코드가 더 깔끔해지고 사용하기 편해졌답니다!

이렇게 간단하지만 개발자 경험을 꽤 개선한 부분이라 꼭 알아두세요~!

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

Next.js 13로 링크를 업그레이드하려면 `new-link` 코덱모드(codemod)를 활용할 수 있어요. 이건 자동으로 코드를 바꿔주니까 한 번 활용해보면 편리하답니다.

### `Script` 컴포넌트

Next.js 13에서 `next/script`의 동작 방식이 업데이트되면서, pages와 app 디렉토리 양쪽을 지원하지만, 원활하게 마이그레이션 하려면 몇 가지 주의할 점이 있어요.

| 변경 사항 | 설명 |
| --------- | ----- |
| beforeInteractive 스크립트 이동 | 기존에 `_document.js`에 넣었던 `beforeInteractive` 스크립트는 `app/layout.tsx` 루트 레이아웃 파일로 옮겨야 해요. 그래야 Next.js 13의 새로운 앱 구조에서 제대로 작동해요. |
| experimental worker strategy 제한 | `experimental`로 설정된 워커 전략은 아직 `app` 디렉토리에서 완전히 지원되지 않습니다. 따라서 이 전략을 사용 중이라면 삭제하거나, `lazyOnload` 같은 다른 전략으로 바꿔야 해요. |
| 이벤트 핸들러(onLoad, onReady, onError) | Server Component에서는 이벤트 핸들러가 동작하지 않아요. 따라서 이런 핸들러들은 반드시 Client Component로 옮기거나 아니면 삭제해야 합니다. |

추가 팁으로, 만약 여러분의 프로젝트가 아직 완전히 앱 디렉토리 구조로 전환되지 않았다면, 이 변경 사항들을 적용하기 전에 꼭 백업을 해놓고 점진적으로 바꾸는 것을 추천해요. 

그리고 이 `Script` 컴포넌트는 외부 스크립트를 다룰 때 굉장히 유용한데, 적절하게 전략을 변경하면 페이지 렌더링 성능도 훨씬 좋아질 수 있어요. Next.js 13을 더 잘 활용하는 데 있어서 중요한 부분이니 꼭 체크해보세요!

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

### 폰트 최적화(Font Optimization)

예전에는 Next.js가 폰트 CSS를 인라인(inline) 방식으로 넣어주는 방식으로 폰트를 최적화해줬어요. 그런데 Next.js 13부터는 완전히 새롭게 설계된 **next/font** 모듈이 등장했습니다. 이 모듈을 사용하면 폰트 로딩 방식을 더 세밀하게 조절할 수 있으면서도, 여전히 빠른 성능과 개인정보 보호까지 챙길 수 있답니다.

특히 next/font는 기존의 pages 디렉토리 뿐만 아니라 새로운 app 디렉토리 환경에서도 모두 지원된다는 점이 큰 장점이에요.

예전처럼 CSS를 인라인으로 넣는 방식은 pages에서는 여전히 작동하지만, app에서는 작동하지 않으니 앞으로는 꼭 next/font 모듈을 써야 해요.

더 자세한 사용법은 Next.js 공식 문서의 [Font Optimization](https://nextjs.org/docs/basic-features/font-optimization) 페이지를 참고하면 되고, 직접 써보면 적용도 쉽고 효과도 확실히 느껴질 거예요!

---

> **추가 팁!**  
> next/font는 구글 폰트뿐 아니라 로컬 폰트도 쉽게 불러올 수 있어서, 사용자 맞춤 폰트를 쓸 때도 아주 유용해요. 그리고 폰트를 가져올 때 불필요한 파일을 제외하고 필요한 글자나 변형만 포함시켜서, 번들 크기가 훨씬 줄어드는 효과까지 있답니다. 실 서비스에 적용하면 조금 더 쾌적한 웹 경험을 제공할 수 있어요!

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

## pages에서 app으로 마이그레이션하기

> 🎥 영상 보기: App Router 점진적 도입 방법 배우기 → YouTube (16분)

App Router로 이동하는 건 Next.js가 기반으로 삼고 있는 React 기능들, 예를 들어 서버 컴포넌트(Server Components), 서스펜스(Suspense) 등을 처음 사용해보는 경험일 수 있어요. 여기에 Next.js의 새로운 기능들, 예를 들어 특수 파일(special files)과 레이아웃(layouts) 등이 더해지면서 마이그레이션은 새로운 개념, 사고방식, 그리고 동작 방식까지 익혀야 하는 일이 됩니다.

그래서 저희는 이 모든 변화를 한꺼번에 적용하기보다는, 마이그레이션 과정을 작게 쪼개서 단계별로 진행할 것을 추천해요. app 디렉토리는 pages 디렉토리와 동시에 작동하도록 의도적으로 설계되어 있어서, 페이지 단위로 점진적 마이그레이션이 가능하답니다.

---

### 조금 더 풀어 보면

- **서버 컴포넌트(Server Components)**: 서버 쪽에서 렌더링되기 때문에 성능 향상과 번들 크기 감축에 도움이 돼요.
- **서스펜스(Suspense)**: 데이터가 준비될 때까지 UI를 잠시 멈춰놓고 로딩 상태를 보여주는 React 기능입니다.
- **레이아웃(Layouts)**: 앱 전반에 걸친 공통 UI를 쉽게 관리할 수 있게 해줍니다.

마이그레이션 중에도 기존 pages 구조를 완전히 버리지 않고, 신규 app 구조와 병행해가며 일부분씩 바꿔나갈 수 있어서 부담이 적죠. 

즉, 한번에 다 바꾸려 하지 말고, '한 페이지씩, 한 기능씩' 천천히 옮겨가면서 적응하는 게 가장 좋은 방법이에요. 나중에 완전히 app 디렉토리 기반으로 옮겼을 때도 안정적이고 원활한 전환이 될 테니까요. 

이렇게 점진적으로 배우면서 바꾸는 과정, 생각보다 덜 어렵고 훨씬 효율적일 거예요!

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

Next.js의 최신 버전(13.4 이상)부터는 앱 디렉토리(app directory)를 활용한 새로운 방식으로 라우팅과 레이아웃 구성이 가능해졌어요. 이번 포스트에서는 앱 디렉토리를 이용한 Nested Routes(중첩 라우트)와 Layouts(레이아웃) 사용법을 한번 쉽게 정리해볼게요!

---

### 1. 앱 디렉토리는 Nested Routes와 Layouts를 지원해요.

- 폴더 안에 또 폴더를 만들어 중첩 라우트를 정의할 수 있어요.
- 각 폴더 내에 특별한 파일 `page.js`를 두면 그 경로가 실제 라우트 URL이 돼요.
- 예를 들어 `app/blog/page.js`는 `/blog` 경로, `app/blog/post/page.js`는 `/blog/post` 경로가 되는 식입니다.

### 2. 특수 파일 규칙

| 파일명         | 역할                                  | 비고                          |
| ------------- | ----------------------------------- | ----------------------------- |
| `page.js`     | 해당 경로 고유의 UI를 정의             | 라우트마다 고유 UI 구성 가능   |
| `layout.js`   | 여러 라우트에서 공유하는 UI를 정의      | 중첩 레이아웃 구성 가능       |

`page.js`와 `layout.js`는 `.js`, `.jsx`, `.tsx` 확장자를 사용할 수 있어요. 즉, TypeScript도 문제없답니다!

### 3. 공통 레이아웃과 개별 페이지 UI 구분!

- `layout.js`는 자식 경로 여러 개에 공통적으로 적용되는 레이아웃을 담당해요.
- `page.js`는 그 경로 하나만의 UI를 책임져요.

이런 구조 덕분에, 공통 헤더, 사이드바 등은 `layout.js`에 넣고, 경로별 고유한 내용은 `page.js`로 깔끔하게 분리할 수 있어요.

### 4. 앱 폴더 안에 컴포넌트, 스타일, 테스트 파일도 한 곳에!

- `app` 폴더 내에 컴포넌트, 스타일 파일, 테스트, 심지어 헬퍼 함수도 같이 두고 관리할 수 있어요.
- 덕분에 프로젝트 구조가 훨씬 직관적이고 깔끔해진답니다.

### 5. 데이터 패칭 방식 변화

- 기존 `getServerSideProps`, `getStaticProps`는 `app` 디렉토리에서는 새로운 API로 대체되었어요.
- `getStaticPaths`도 이제 `generateStaticParams`로 교체되었죠.
- 서버 사이드 렌더링 및 정적 생성 로직을 훨씬 모던한 방식으로 작성할 수 있어요.

### 6. 페이지 기반 특수 파일들 대체

| 기존 파일                       | 대체 파일                          |
| ----------------------------- | -------------------------------- |
| `pages/_app.js`, `pages/_document.js` | `app/layout.js` (루트 레이아웃)     |
| `pages/_error.js`              | 경로별 `error.js` (더 세밀한 에러처리)|
| `pages/404.js`                 | `not-found.js`                   |
| `pages/api/*` (API Routes)     | `route.js` (Route Handler)       |

---

### [Step 1] app 디렉토리 만들고 Next.js 버전 최신화하기

앱 디렉토리를 써보려면 Next.js 13.4 이상이 필요하니 최신 버전으로 업데이트하는 것부터 시작해 볼게요!

```bash
npm install next@latest
```

---

### 추가 꿀팁!

- 중첩 라우트 구조때문에 URL 구조가 더 명확해져서 SEO에도 긍정적이에요.
- 레이아웃을 여러 겹으로 쌓을 수 있어서 복잡한 UI도 깔끔하게 설계 가능!
- 이제 `_app.js`나 `_document.js` 없이 루트 레이아웃 하나로 전역 설정 관리할 수 있으니 유지보수가 훨씬 편해졌어요.

---

한번 직접 `app` 폴더를 만들어서 중첩 라우트와 레이아웃을 적용해보는 걸 추천해요. 부담없이 프로젝트에 도입해보면 Next.js의 새로운 매력을 확실히 체감할 수 있을 거예요!

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

프로젝트 루트(또는 src/ 디렉토리)에 새로 app 디렉토리를 만들어주세요.

### 2단계: 루트 레이아웃 만들기

이번에는 app 디렉토리 안에 app/layout.tsx 파일을 생성할 거예요. 이 파일은 app 안에 있는 모든 라우트에 적용되는 루트 레이아웃 역할을 합니다.

```tsx
export default function RootLayout({
  // 레이아웃은 children prop을 반드시 받아야 해요.
  // 이 children에는 중첩된 레이아웃이나 페이지들이 들어가게 됩니다.
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

여기서 살짝 중요한 팁을 드리자면, 이 RootLayout은 Next.js의 `app` 디렉토리 구조에서 가장 기본 뼈대가 되기 때문에, 페이지마다 공통적으로 들어가는 요소들(예: `<head>`, 네비게이션 바, 푸터 등)을 이 안에 넣으면 유지보수가 훨씬 편해져요.

그리고 `<html lang="en">` 부분은 사이트의 기본 언어를 설정하는 거라, 한국어 사이트라면 `lang="ko"`로 바꾸는 게 좋아요!

조금 더 완성도를 높이고 싶다면 `<head>` 태그도 커스터마이징해서 메타데이터나 폰트 링크, 파비콘 등을 넣어주는 것도 추천드려요.

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

Next.js의 새로운 app 디렉토리 구조를 사용할 때 알아두면 좋은 몇 가지 핵심 포인트를 정리해봤어요!

---

### 1. app 디렉토리에 **루트 레이아웃(root layout)** 필수!

- `app` 폴더 안에는 반드시 루트 레이아웃 파일이 있어야 해요.
- 이 루트 레이아웃은 `html`, `body` 태그를 직접 정의해야 해요. Next.js가 자동으로 만들어주지 않거든요.
- 기존에는 `pages/_app.tsx`와 `pages/_document.tsx`를 사용해서 전체 앱의 구조나 문서 태그를 조절했지만, 이제는 루트 레이아웃이 그 역할을 대신해요.
- 레이아웃 파일 확장자는 `.js`, `.jsx`, `.tsx` 중 자유롭게 선택할 수 있어요.

---

### 2. SEO 관련 `head` 태그 관리도 간편해요!

- `head` 태그 안에 들어가는 메타정보(예: `title`, `description`)를 관리할 땐 Next.js가 제공하는 내장 SEO 지원을 활용하면 좋아요.
- 아래처럼 `metadata` 객체를 내보내기(export) 하면 페이지별로 메타정보가 자동으로 적용돼요.

```ts
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'Home',
  description: 'Welcome to Next.js',
}
```

---

### 3. 기존 `_app.js`와 `_document.js` 마이그레이션

- app 디렉토리를 사용할 때는 이 두 파일이 필요 없어요. 이 둘이 제공하던 기능 대부분이 루트 레이아웃으로 통합되었기 때문입니다.
- 만약 이전에 작성해둔 커스텀 `_document.js`나 `_app.js`가 있다면, 꼭 app 라우팅 구조에 맞춰 루트 레이아웃으로 다시 작성해주는 게 좋아요.

---

### 개인적인 팁 한마디!

- 루트 레이아웃 내에서 글로벌 CSS나 공통 UI 컴포넌트를 한번만 감싸주면 되니까, 코드가 훨씬 깔끔해지고 유지보수도 쉬워졌어요.
- 또, `metadata` 덕분에 SEO 설정이 페이지마다 편하게 분리되어서 관리가 편해졌답니다.
- 다만, `html`과 `body`를 직접 선언해야 하니 꼭 잊지 말고 작성해주세요!

---

다음 포스트에서는 app 디렉토리 구조를 활용하는 구체적인 예제와 함께 useRouter 훅 대신 사용할 새로운 라우팅 방법도 소개할게요. 기대해주세요! 😊

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

만약 기존에 _app이나 _document 파일이 있다면, 그 안에 있던 내용들(예: 전역 스타일들)을 새로 만든 루트 레이아웃(app/layout.tsx)으로 옮길 수 있어요. 그런데 중요한 점은, app/layout.tsx에 있는 스타일은 pages/* 경로에 적용되지 않는다는 거예요. 그래서 마이그레이션하는 동안에는 기존 _app과 _document 파일을 유지하는 게 좋습니다. 그래야 pages/* 경로들이 갑자기 깨지지 않거든요. 모든 마이그레이션이 완전히 끝난 후에야 이 두 파일을 안전하게 지울 수 있답니다.

또 하나, 만약 React Context 프로바이더를 사용 중이라면, 이걸 Client Component로 옮겨야 해요. Next.js의 새로운 앱 디렉토리 구조에서는 서버 컴포넌트와 클라이언트 컴포넌트를 명확하게 구분해서 관리하거든요.

#### getLayout() 패턴에서 Layouts로 마이그레이션하기 (선택 사항)

기존 pages 디렉토리에서는 페이지별로 레이아웃을 적용하기 위해 Page 컴포넌트에 getLayout()이라는 프로퍼티를 붙이는 방식을 많이 썼어요. 하지만 app 디렉토리에서는 **중첩 레이아웃(nested layouts)**을 네이티브로 지원하기 때문에 이 패턴이 불필요해요. 즉, 페이지별로 다른 레이아웃을 적용하고 싶으면, 레이아웃 파일들을 계층적으로 분리해서 구조화하면 된답니다. 훨씬 깔끔하고 유지보수도 쉬워지죠.

---

이제 이런 방식으로 코드를 새롭게 정리하다 보면, 각 페이지에서 레이아웃을 직접 관리할 필요 없이 폴더 구조만으로도 레이아웃이 매끄럽게 연결되는 걸 경험할 수 있을 거예요. Next.js 13부터 도입된 app 디렉토리 방식이 가져다주는 가장 큰 장점 중 하나입니다!

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

### 3단계: next/head 마이그레이션하기

Next.js의 `pages` 디렉토리에서는 `next/head` 컴포넌트를 이용해서 HTML 문서의 `<head>` 부분, 예를 들어 `title`이나 `meta` 태그를 관리하죠. 그런데 `app` 디렉토리로 넘어오면 `next/head` 대신 새로운 내장 SEO 지원 기능을 사용해야 해요.

기존에는 이렇게 썼었죠:

```js
import Head from 'next/head'
 
export default function Page() {
  return (
    <>
      <Head>
        <title>My page title</title>
      </Head>
    </>
  )
}
```

---

새로운 `app` 디렉토리에서는 `head` 관리가 훨씬 간편해졌어요. 이제는 페이지 컴포넌트 내에서 별도로 `<Head>` 컴포넌트를 쓸 필요 없이, `metadata`라는 방식을 사용해 `title` 같은 SEO 관련 정보를 설정할 수 있어요.

아래는 간단한 예시입니다:

```js
export const metadata = {
  title: 'My page title',
  description: '페이지 설명을 여기에 작성하세요',
}

export default function Page() {
  return (
    <div>
      {/* 페이지 콘텐츠 */}
    </div>
  )
}
```

이렇게 `metadata`를 export 하면 Next.js가 알아서 `<head>`에 적절한 태그들을 넣어줘요. 덕분에 코드도 훨씬 깔끔해지고, SEO 관리도 편해집니다.

> 참고로 `metadata`에서 지원하는 옵션은 `title`, `description`, `openGraph`, `twitter` 등 굉장히 다양하니 필요에 맞게 활용해보세요.

---

요약하자면,

| 이전 방식 (`pages` 폴더)        | 새로운 방식 (`app` 폴더)                   |
|---------------------------|------------------------------------|
| `import Head from 'next/head'` 사용 | `metadata` 객체 export로 설정             |
| JSX 안에 `<Head>...</Head>` 작성    | 컴포넌트 밖에 `export const metadata = {...}` 작성 |

이제부터는 `head` 요소를 직접 다루기보단, `metadata`를 통해 쉽고 깔끔하게 SEO를 관리하는 방법에 적응해 보세요! 이는 Next.js가 미래 지향적으로 제공하는 공식적인 추천 방식이기도 합니다.

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

자, 위 코드는 Next.js에서 메타데이터를 설정하는 아주 기본적인 예시예요.

```js
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'My Page Title',
}

export default function Page() {
  return '...'
}
```

여기서 `metadata` 객체를 통해 페이지의 타이틀을 설정하고 있어요. 이 방식은 Next.js 13 이상에서 메타데이터를 좀 더 체계적으로 관리할 수 있게 해주는 방법 중 하나랍니다.

---

### 메타데이터 옵션 리스트

Next.js에서 지원하는 메타데이터 옵션은 상당히 다양해요. 예를 들면:

| 옵션 이름          | 설명                       |
|-----------------|--------------------------|
| title           | 페이지의 제목 설정               |
| description     | 페이지 설명(SEO에 중요)         |
| keywords        | 검색 최적화를 위한 키워드 배열      |
| openGraph       | 오픈그래프 태그 설정 (SNS 공유 시) |
| twitter         | 트위터 카드 정보 설정           |
| robots          | 크롤러를 위한 접근 제어 설정       |
| manifest       | PWA용 웹앱 매니페스트 연결         |

이 외에도 다양한 옵션들이 있으니 공식 문서를 참고하면 좋아요.

---

### Step 4: 페이지 마이그레이션하기

Next.js 프로젝트를 새 버전으로 옮기면서 기존 페이지들을 어떻게 마이그레이션해야 하는지 고민되시죠? 가장 중요한 건 메타데이터를 더 이상 `head` 태그나 `next/head` 컴포넌트로 관리하지 않고, 위처럼 `metadata` 객체로 관리하는 방식으로 바꾸는 거예요.

또한, 서버 컴포넌트(Server Component)를 적극 활용해서 페이지 성능과 유지보수성을 높일 수 있어요. 예를 들어, 데이터 페칭도 페이지 바로 위에서 처리하고 UI는 순수하게 렌더링만 하도록 분리할 수 있답니다.

마이그레이션할 때는 아래 포인트를 기억하면 좋아요:
- **메타데이터:** 기존 `Head` 대신 `metadata` 객체 활용
- **서버 컴포넌트:** 서버에서 데이터 처리하고 UI는 클라이언트 컴포넌트와 분리 가능
- **라우팅:** app 폴더 기반 라우팅을 잘 이해하고 기존 pages 폴더 구조와 맞게 조정
- **스타일링:** 글로벌 스타일, CSS 모듈, Tailwind 등 스타일링 방법은 동일하지만 적용 위치에 따라 차이가 있을 수 있음

마이그레이션 전후로 빌드 및 렌더링 결과를 꼭 확인하며 진행하는 게 좋아요!

필요하면 마이그레이션 사례도 따로 정리해볼 테니 궁금한 점 있으면 언제든지 질문해 주세요. 😊

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

안녕하세요 여러분! 오늘은 Next.js의 새로운 `app` 디렉토리에 대해 이야기해볼게요. 기존 `pages` 디렉토리와 비교하면서 어떤 점이 달라졌는지, 그리고 마이그레이션하는 가장 쉬운 방법도 함께 소개해드릴게요.

---

### 1. 기본 컴포넌트 유형의 변화

- `app` 디렉토리 내의 페이지들은 **기본적으로 Server Components**로 작동해요.
- 반면, 기존 `pages` 디렉토리의 페이지들은 **Client Components**가 기본이었죠.

쉽게 말해, `app` 디렉토리는 서버에서 렌더링하는 컴포넌트를 기본으로 하되 필요하면 클라이언트 컴포넌트를 따로 만들어서 사용한다는 점에서 차이가 있어요.

---

### 2. 데이터 패칭 방식 변화

기존 `pages` 디렉토리에서는 `getServerSideProps`, `getStaticProps`, `getInitialProps` 같은 함수들을 사용했는데요, `app` 디렉토리는 이 복잡한 API들을 **더 간단한 API로 교체**했답니다. 데이터를 가져오는 방법이 훨씬 깔끔해졌죠.

---

### 3. 라우팅 방식과 파일 구조

`app` 디렉토리는 중첩 폴더 구조를 이용해 라우트를 정의하고, 각 폴더 안에 `page.js` 파일을 만들어서 해당 경로를 외부에서 접근할 수 있게 해요.

예를 들어, 기존 `pages`와 `app` 디렉토리 라우팅 비교는 다음과 같아요:

| pages 디렉토리       | app 디렉토리             | 라우트 경로       |
|---------------------|-------------------------|------------------|
| index.js            | page.js                 | /                |
| about.js            | about/page.js           | /about           |
| blog/[slug].js      | blog/[slug]/page.js     | /blog/post-1     |

---

### 4. 마이그레이션 가이드: 가장 쉬운 방법

`pages`에서 `app`으로 옮길 때, 저희가 추천하는 방법은 크게 두 단계예요.

1. **Step 1**: 기존에 기본 내보내기(default export)했던 페이지 컴포넌트를 **새로운 클라이언트 컴포넌트로 옮기기**  
   - 이 때 `use client` 지시어를 추가하는 걸 잊지 마세요! 클라이언트 전용 컴포넌트임을 명시하는 거죠.

2. **Step 2**: `app` 디렉토리 내에 새로운 `page.js` 파일을 만들고, 거기에 이 클라이언트 컴포넌트를 임포트해서 사용하기

---

### 덧붙여서: 왜 이렇게 하는 게 좋을까?

이 방식이 가장 **`pages` 디렉토리와 비슷한 동작을 보장하기 때문에** 마이그레이션이 가장 쉽고 안정적이에요. 급진적으로 구조를 바꾸지 않고도 기존의 방식을 최대한 유지하면서 새로운 `app` 디렉토리의 장점도 누릴 수 있거든요.

---

이상으로 Next.js의 `app` 디렉토리 기본 사항과 마이그레이션 방법에 대해 간단히 정리해봤어요. 새로워진 Next.js를 활용해서 더 깔끔하고 효율적인 프로젝트를 만들어보세요!

궁금한 점 있으면 댓글로 알려주세요~

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

자, 이제 Next.js 앱에서 'Client Component'를 만들어보는 방법에 대해 설명해볼게요. 너무 딱딱하지 않게 쉽게 풀어서 이야기할게요!

---

## Step 1: 새로운 Client Component 만들기

먼저, `app` 디렉터리 안에 새로운 파일을 만들어줘요. 예를 들면 `app/home-page.tsx` 같은 파일이죠. 이 파일에서 Client Component를 정의하려면, 파일 맨 위에 `'use client'`라는 지시어를 꼭 추가해야 해요. 이건 컴포넌트를 클라이언트 쪽에서 실행하겠다는 뜻이에요.

매우 중요한 점! `pages` 디렉토리에서 작업할 때처럼, Next.js가 초기 페이지 로드 때 이 Client Component를 정적 HTML로 미리 렌더링(prerender)하는 최적화 과정이 있답니다.

기존에 `pages/index.js`에 있던 기본 페이지 컴포넌트를 이제 `app/home-page.tsx`로 옮겨주세요.

다음은 간단한 예시 코드에요:

```js
'use client'

// 이 컴포넌트는 클라이언트 컴포넌트예요.
// props로 데이터를 받고, state와 useEffect 같은 훅을 쓸 수 있죠.
// 그리고 초기 페이지 로드 시 서버에서 미리 렌더링 돼요.
export default function HomePage({ recentPosts }) {
  return (
    <div>
      {recentPosts.map((post) => (
        <div key={post.id}>{post.title}</div>
      ))}
    </div>
  )
}
```

---

같은 내용을 조금 더 쉽게 설명하자면: 

- `'use client'`를 붙이면 Next.js가 "아, 이건 브라우저에서 실행될 컴포넌트구나"라고 인식해요.
- 클라이언트 컴포넌트 안에서는 **State 관리, 이벤트, useEffect** 같은 React 훅을 활용할 수 있어서 인터랙티브한 UI를 만들기 좋아요.
- 하지만 Next.js 최적화 덕분에, 초기 화면은 서버에서 미리 렌더링해서 빠르게 띄워줍니다!

---

## Step 2: 새로운 페이지 만들기

이 부분은 다음 메시지에 더 자세히 다룰게요!

---

필요하면 더 궁금한 점이나 코딩 팁도 많이 얘기해 줄게요. Next.js 앱 구조, Client/Server Components 개념이 처음이면 조금 헷갈릴 수 있는데, 천천히 익히면 아주 강력한 UI를 만들 수 있어요!

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

자, 이번에는 Next.js의 app 디렉토리에서 페이지와 라우팅을 설정하는 방법을 한번 알아볼게요. 기존 pages 디렉토리에서 하던 방식과 비교하면서 쉽게 이해할 수 있도록 설명해볼게요.

---

### 1. app/page.tsx 만들기 (Server Component)

먼저, `app` 폴더 안에 `page.tsx` 파일을 하나 만들어주세요. 여기서 중요한 점은 **app 디렉토리 내 파일들은 기본적으로 서버 컴포넌트(Server Component)**라는 거예요. 서버 컴포넌트는 서버에서 데이터를 직접 받아오고 렌더링을 담당해서 클라이언트보다 효율적이고 느린 데이터 요청에도 영향이 적답니다.

```tsx
// app/page.tsx

// client 컴포넌트인 홈 페이지를 import 해줘요
import HomePage from './home-page'

async function getPosts() {
  const res = await fetch('https://...')
  const posts = await res.json()
  return posts
}

export default async function Page() {
  // 서버 컴포넌트 안에서 데이터를 바로 fetch해요
  const recentPosts = await getPosts()
  
  // 받아온 데이터를 client 컴포넌트에 넘겨줍니다
  return <HomePage recentPosts={recentPosts} />
}
```

> TIP: `fetch` 작업을 서버 컴포넌트에서 하면 클라이언트가 데이터를 기다리는 상황이 줄어든다는 장점이 있어요.

---

### 2. pages/index.js의 데이터 fetching 로직 옮기기

기존에 `pages/index.js`에서 데이터를 fetch 했던 로직, 그대로 `app/page.tsx`에 옮겨오면 됩니다. 그리고 페이지 컴포넌트가 async 함수로 정의되어 있어야 데이터를 await로 처리할 수 있어요.

---

### 3. 클라이언트 컴포넌트 가져오기

위 코드에서 `HomePage`는 클라이언트 컴포넌트라면 `app/home-page.tsx` 같은 위치에 작성되어 있어야 해요. `useState`, `useEffect` 같은 클라이언트 전용 훅들을 사용하는 컴포넌트라면 `use client` 지시어를 최상단에 적어줘야 합니다.

```tsx
// app/home-page.tsx
'use client'

import React from 'react'

export default function HomePage({ recentPosts }) {
  return (
    <div>
      <h1>최근 게시물</h1>
      <ul>
        {recentPosts.map(post => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  )
}
```

---

### 4. 새로운 라우팅 훅 사용하기

예전 `pages` 폴더 대비 `app` 디렉토리에서는 routing도 조금 달라졌어요. `useRouter`를 비롯한 라우팅 관련 훅들이 `next/navigation`에서 새롭게 제공됩니다.

```tsx
import { useRouter, usePathname, useSearchParams } from 'next/navigation'

export default function MyComponent() {
  const router = useRouter()
  const pathname = usePathname()
  const searchParams = useSearchParams()

  // 라우팅 변경, 현재 경로, 쿼리 파라미터 읽기 등에 사용 가능
}
```

기존 `next/router` 대신 이 훅들을 꼭 사용해주세요!

---

### 5. 개발 서버 실행하고 확인하기

이제 모두 마쳤으면 개발 서버를 실행해 봅시다.

```bash
npm run dev
```

또는

```bash
yarn dev
```

그리고 브라우저에서 `http://localhost:3000`에 접속하면, 기존 index 페이지가 `app/page.tsx`를 통해 렌더링 되는 걸 볼 수 있을 거예요.

---

## 정리!

| 단계 | 설명 |
|---|---|
| 1 | `app/page.tsx` 생성하여 Server Component로 페이지 작성하기 |
| 2 | 기존 pages의 데이터 fetching 로직을 서버 컴포넌트 내에서 처리하도록 옮기기 |
| 3 | 클라이언트 컴포넌트는 `use client` 지시어 붙이고, 데이터 props로 받기 |
| 4 | `next/navigation`의 새로운 routing 훅(`useRouter`, `usePathname`, `useSearchParams`) 사용하기 |
| 5 | 개발 서버 돌려서 잘 되는지 확인하기 |

---

Next.js의 `app` 디렉토리는 서버와 클라이언트 컴포넌트가 명확히 구분되고, React의 최신 기능들(Suspense, Server Components 등)을 활용할 수 있어서 성능도 좋아지고 개발도 편해졌어요. 처음 접하면 헷갈릴 수 있으니 차근차근 바꾸면서 적응해보세요!

궁금한 점 있으면 언제든 질문해 주세요!

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

Next.js에서 `useRouter` 훅에 큰 변화가 있었어요! 특히 `app` 디렉터리에서는 이전에 쓰던 `next/router`에서 가져오는 `useRouter`가 지원되지 않고, 대신 `next/navigation`에서 가져오는 새로운 훅을 써야 해요.

---

### 핵심 정리

| 이전(useRouter from next/router) | 최신(useRouter from next/navigation) |
|---|---|
| `pages` 디렉터리에서만 사용 가능 | `app` 디렉터리에서 사용 가능 (pages에서는 기존 걸 계속 사용 가능) |
| `pathname` 문자열 반환 | 반환하지 않음. 대신 `usePathname()` 사용 |
| `query` 객체 반환 (검색 파라미터 + 동적 라우팅 파라미터 섞여 있음) | 반환하지 않음. 대신 `useSearchParams()`와 `useParams()`를 각각 사용 |
| `isFallback`, `locale`, `basePath`, `asPath`, `isReady`, `route` 등 여러 프로퍼티 존재 | 여러 속성이 제거됨 (예: `isFallback` 사라짐, `locale` 관련 제거 등) |
| 서버 및 클라이언트 컴포넌트에서 사용 가능 | 오로지 클라이언트 컴포넌트에서만 사용 가능 |

---

### 새로운 훅 사용법 예시

```jsx
'use client'

import { useRouter, usePathname, useSearchParams } from 'next/navigation'

export default function ExampleClientComponent() {
  const router = useRouter()
  const pathname = usePathname()
  const searchParams = useSearchParams()

  // 예시: 쿼리 파라미터 'id' 값 가져오기
  const id = searchParams.get('id')

  const goHome = () => {
    router.push('/')
  }

  return (
    <div>
      <p>현재 경로: {pathname}</p>
      <p>id 파라미터: {id}</p>
      <button onClick={goHome}>홈으로 이동</button>
    </div>
  )
}
```

---

### 조금 더 알아볼 점

- `useSearchParams`는 브라우저 URL의 쿼리스트링 부분을 다룰 때 쓰고,
- `useParams`는 `[slug]` 같은 동적 세그먼트 파라미터 값을 가져올 때 사용해요.
- `usePathname`은 현재 경로를 문자열로 알려줍니다.

이 훅들만 조합하면 이전 `useRouter`가 했던 거의 모든 일을 하실 수 있답니다.

---

### 몇 가지 팁

1. **클라이언트 컴포넌트에서만 사용 가능!**  
   서버 컴포넌트에서는 작동하지 않으니, 위 예시에선 `'use client'` 지시어가 필요한 이유예요.

2. **라우팅 이벤트 감지는?**  
   `usePathname`과 `useSearchParams`를 조합해서 경로 변경을 감지하세요. 예전처럼 `router.events`는 지원되지 않아요.

3. **국제화(i18n) 지원 변화**  
   `locale`, `locales` 등 국제화 관련 속성이 사라진 점 참고하세요. `app` 디렉토리에서는 다르게 i18n을 다뤄야 합니다.

4. **`isReady` 사라짐**  
   예전엔 라우터가 초기화될 때까지 기다리는 상태였는데, 이제는 클라이언트 렌더링 단계에서 알아서 처리합니다.

---

새로운 `useRouter`가 구조가 더 깔끔해졌지만, 그만큼 `usePathname`, `useSearchParams`, `useParams` 같은 훅들을 같이 잘 활용해야 하는 점 기억하세요! Next.js 앱 디렉터리 기반 라우팅을 공부 중이라면 이 변화는 꼭 알아두시고 작업하세요~

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

useRouter() API 참고하기

#### 페이지와 앱 사이에서 컴포넌트 공유하기

Next.js에서 페이지 라우터와 앱 라우터를 동시에 지원하는 컴포넌트를 만들려면, `next/compat/router`에서 제공하는 `useRouter` 훅을 사용하는 게 좋아요. 이 훅은 페이지 디렉토리에서 쓰이는 `useRouter`와 같지만, 두 라우터 환경 사이에서 컴포넌트를 공유할 때 호환성을 유지하도록 설계되어 있답니다.

그리고 언젠가 앱 라우터만 사용하게 될 시점이 오면, 그때는 `next/navigation`에서 제공하는 최신 `useRouter`로 바꾸는 걸 권장해요. 이렇게 하면 더 깔끔하고 최신 패턴에 맞는 코드가 되니까요!

---

### 6단계: 데이터 가져오기 방법 마이그레이션하기

다음 단계에서는 데이터 패칭 관련 메서드를 어떻게 마이그레이션하는지 살펴볼 거예요. Next.js 13부터는 새롭게 도입된 앱 라우터 환경에 맞춰 데이터 가져오는 방법도 많이 바뀌었으니, 꼭 참고해서 업데이트해보시길 바랍니다.

---

**덧붙여서!**  
만약 여러분이 컴포넌트를 여러 라우터 환경에서 쓸 일이 많다면, `next/compat/router` 쪽 `useRouter` 훅을 사용하는 게 당분간은 가장 안정적인 방법이에요. 나중에 앱 라우터만 쓰게 되면, `next/navigation`에서 `useRouter`가 훨씬 더 가볍고 빠른 API를 제공하니 그때 갈아타는 걸 추천드립니다!

추가로, `useRouter`를 통해 라우팅 정보를 얻는 법뿐 아니라, 프로그램적으로 라우팅을 제어하는 방법도 익히면 개발 효율이 훨씬 올라가요. 필요하면 언제든 더 자세히 공유할게요!

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

Next.js에서 데이터 패칭 방식을 다루는 걸 보면, 전통적으로 pages 디렉토리에서는 getServerSideProps와 getStaticProps를 많이 사용해왔어요. 그런데 app 디렉토리에서는 이 패턴이 조금 바뀌어서, fetch()와 React Server Components를 기반으로 더 간단하게 데이터를 가져올 수 있도록 바뀌었답니다.

---

## app 디렉토리의 데이터 패칭 방식

```js
export default async function Page() {
  // 이 요청은 수동으로 무효화될 때까지 캐시됩니다.
  // getStaticProps와 비슷한 역할.
  // 'force-cache'가 기본값이라 생략 가능해요.
  const staticData = await fetch(`https://...`, { cache: 'force-cache' })
 
  // 이 요청은 매 요청마다 다시 불러옵니다.
  // getServerSideProps와 비슷하죠.
  const dynamicData = await fetch(`https://...`, { cache: 'no-store' })
 
  // 이 요청은 10초 동안만 캐시되고 재검증됩니다.
  // getStaticProps의 revalidate 옵션과 유사해요.
  const revalidatedData = await fetch(`https://...`, {
    next: { revalidate: 10 },
  })
 
  return <div>...</div>
}
```

여기서 중요한 점은, 이제 더 이상 별도의 데이터 패칭 함수들을 페이지 밖에서 선언할 필요 없이 컴포넌트 내부에서 일반적인 async 함수처럼 fetch를 호출할 수 있다는 점이에요. 그리고 fetch 옵션을 통해 원하는 캐싱 전략을 직접 지정할 수 있죠.

---

## pages 디렉토리의 getServerSideProps

pages 디렉토리에서는 getServerSideProps가 서버에서 데이터를 가져와서 props로 넘겨주고, 그 데이터를 기반으로 초기 HTML을 렌더링해요. 이렇게 서버에서 미리 렌더링된 페이지가 클라이언트에서 "hydrating" 되어 인터랙티브해집니다.

간단히 말해서:

- **getServerSideProps:** 서버에서 매 요청마다 실행
- **getStaticProps:** 빌드 시점에 한번 실행, 필요시 revalidate 옵션으로 주기적 재생성

---

## 한마디 정리!

- app 디렉토리에서는 React Server Components와 fetch의 cache 옵션을 이용해 데이터 패칭을 직접 관리.
- pages 디렉토리에서는 기존처럼 getServerSideProps, getStaticProps를 통해 데이터를 패칭.
- 앞으로 Next.js를 사용하면서 app 디렉토리를 많이 쓰게 되면 더 선언적이고 코드가 직관적인 fetch 기반 방식을 자주 보게 될 거예요.

---

### 추가 팁

- `cache: 'force-cache'`는 기본값이니, 특별히 동적 데이터를 자주 변경시키지 않는다면 생략해도 무방해요.
- 반대로 `cache: 'no-store'`를 사용하면 실제로 서버에서만 데이터를 요청하고 클라이언트나 중간 캐시는 전혀 사용하지 않으니 주의하세요.
- `next: { revalidate: 10 }`를 활용하면 10초마다 캐시를 재검증해서 최신 데이터를 일정 주기로 반영할 수 있어요.

이제 Next.js 데이터 패칭 방식이 훨씬 유연하고 직관적으로 변해서 개발할 때 훨씬 편해질 거예요! 궁금한 점 있으면 언제든 물어봐 주세요~

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

Next.js의 pages 디렉토리 방식과 App Router (app 디렉토리) 방식에서 서버 사이드 데이터 패칭을 어떻게 하는지 비교해볼게요.

---

### pages 디렉토리 방식 (기존 방식)

```js
export async function getServerSideProps() {
  const res = await fetch(`https://...`)
  const projects = await res.json()

  return { props: { projects } }
}

export default function Dashboard({ projects }) {
  return (
    <ul>
      {projects.map((project) => (
        <li key={project.id}>{project.name}</li>
      ))}
    </ul>
  )
}
```

- `getServerSideProps`는 페이지 요청 시마다 서버에서 데이터 패칭 후 HTML을 렌더링해서 클라이언트에 보냅니다.
- 장점: 최신 데이터를 항상 보여줌
- 단점: 페이지 요청마다 데이터 패칭, 클라이언트로 전송되는 자바스크립트 번들에는 데이터 패칭 코드가 없음

---

### App Router 방식 (app 디렉토리)

```js
// 이 함수 이름은 자유롭게 지어도 됩니다
async function getProjects() {
  const res = await fetch(`https://...`, { cache: 'no-store' })
  const projects = await res.json()

  return projects
}

export default async function Dashboard() {
  const projects = await getProjects()

  return (
    <ul>
      {projects.map((project) => (
        <li key={project.id}>{project.name}</li>
      ))}
    </ul>
  )
}
```

- React 서버 컴포넌트(Server Components) 내에서 바로 비동기 함수로 데이터를 패칭할 수 있어요.
- `{ cache: 'no-store' }` 옵션을 주면 `getServerSideProps`처럼 매 요청마다 항상 최신 데이터를 받아옵니다.
- 클라이언트에 불필요한 자바스크립트가 덜 전달되므로 초기 로딩이 더 빠릅니다.
- 서버에서 렌더링된 HTML이 바로 클라이언트에 전달되어 SEO에도 좋습니다.

---

### 추가 팁!

- `cache` 옵션에는 `'force-cache'`(기본값, ISR 적용 가능)나 `'no-store'` 외에도 다양한 값이 있어요. 상황에 따라 적절히 선택해 주세요.
- 서버 컴포넌트 명령문 안에서는 클라이언트 전용 훅 같은 것들을 사용할 수 없으니 주의!
- 점점 Next.js가 React Server Components 방식을 중심으로 발전하고 있으니 새 프로젝트에서는 app 디렉토리 방식을 고려해보세요.

---

Next.js에서도 이렇게 진화한 서버사이드 데이터 패칭 방식을 활용하면 성능과 개발 경험 모두 잡을 수 있답니다! 궁금한 점 있으면 언제든 물어보세요~

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

#### Request 객체 접근하기

Next.js의 `pages` 디렉토리에서는 Node.js의 HTTP API를 바탕으로 요청(request) 관련 데이터를 쉽게 가져올 수 있어요.

예를 들어, `getServerSideProps` 함수에서 `req` 객체를 받아서 요청의 쿠키나 헤더 정보를 조회할 수 있답니다. 이렇게 하면 서버 사이드에서 특정 쿠키 값이나 인증 헤더 등을 읽어서 조건에 따라 페이지 렌더링을 다르게 처리할 수 있죠.

아래 예시를 보시죠:

```js
// pages 디렉토리 내에서

export async function getServerSideProps({ req, query }) {
  const authHeader = req.getHeaders()['authorization']; // 요청 헤더에서 'authorization' 정보 가져오기
  const theme = req.cookies['theme']; // 요청 쿠키에서 'theme' 값 가져오기

  return { props: { authHeader, theme } } // props로 페이지 컴포넌트에 전달
}

export default function Page(props) {
  return (
    <div>
      <p>Authorization Header: {props.authHeader}</p>
      <p>Theme from Cookie: {props.theme}</p>
    </div>
  )
}
```

### 여기서 한 가지 팁!
- `req.cookies`를 쓰려면 쿠키 파서를 따로 설치하거나 설정해야 할 수도 있어요. 보통 Next.js에서는 내장된 API가 자동으로 쿠키를 파싱해주긴 하지만, 커스텀 미들웨어를 사용할 때 주의해야 합니다.
- 헤더 이름은 대소문자를 구분하지 않지만, 보통 `authorization`처럼 소문자로 접근하는 게 편해요.

이처럼 서버 사이드에서 요청 관련 데이터를 활용하면 인증, 테마 설정, 사용자 맞춤 데이터 처리 등 다양한 기능을 구현할 수 있어서 강력하답니다!

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

이번에는 Next.js의 app 디렉토리에서 제공하는 새로운 읽기 전용 함수인 headers와 cookies에 대해 알려드릴게요. 이 둘은 서버 컴포넌트(Server Components) 내에서 요청(request) 데이터를 쉽게 가져올 수 있게 도와줘요.

---

### headers 와 cookies 함수란?

- **headers**: Web Headers API를 기반으로 만들어졌으며, 서버 컴포넌트 안에서 요청 헤더 정보를 가져올 때 사용해요.
- **cookies**: Web Cookies API를 기반으로 하며, 서버 컴포넌트에서 쿠키 정보를 가져올 수 있어요.

즉, 클라이언트가 보낸 요청 헤더와 쿠키 값을 서버 컴포넌트 내에서 간편하게 읽을 수 있도록 Next.js에서 제공하는 기능이죠.

---

### 간단한 코드 예시

```jsx
// app 디렉토리 예시
import { cookies, headers } from 'next/headers'

async function getData() {
  const authHeader = (await headers()).get('authorization')
  
  return '...'
}

export default async function Page() {
  // 직접 Server Components 내부나 데이터 요청 함수에서 cookies, headers 사용 가능
  const theme = (await cookies()).get('theme')
  const data = await getData()
  return '...'
}
```

위 예시처럼 `headers()`와 `cookies()` 함수는 async 함수로, 호출하면 각각 헤더와 쿠키 객체를 반환해요. 그리고 그 객체에서 `.get('키 이름')`으로 내가 원하는 값을 꺼낼 수 있죠.

---

### 좀 더 알면 좋은 점

- **읽기 전용임을 기억하세요**: 이 함수들은 요청 시 전달된 헤더나 쿠키를 읽기만 할 수 있어요. 수정이나 생성 기능은 없답니다.
- **클라이언트 컴포넌트에서는 사용 불가**: 이 함수들은 서버 컴포넌트에서만 사용할 수 있기 때문에 클라이언트 컴포넌트에서 호출하려면 다른 방법을 써야 해요.
- **getStaticProps 대신 사용 가능**: 기존에 `getStaticProps`로 서버에서 요청 데이터를 다뤘던 부분을 app 디렉토리 기반 서버 컴포넌트에서 훨씬 간단하게 처리할 수 있게 된 거죠.

---

### 마무리

Next.js의 app 디렉토리에서 headers와 cookies 함수는 요청 데이터를 안전하고 간편하게 다룰 수 있도록 해줘서, 서버 컴포넌트를 작성할 때 진짜 편리하답니다. 이제 상태 관리나 요청 처리 로직에 대해 걱정 없이, 필요한 헤더나 쿠키를 바로바로 읽어와서 활용해보세요!

필요하다면 더 깊게 활용하는 방법도 알려드릴게요. 언제든 질문해주세요!

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

이번 글에서는 Next.js에서 페이지를 미리 렌더링하고 데이터를 가져오는 방법을 비교해볼게요. 특히 pages 디렉터리와 app 디렉터리에서 각각 어떻게 데이터 페칭을 하는지 살펴보겠습니다.

---

## pages 디렉터리: getStaticProps로 빌드 시 데이터 가져오기

pages 디렉터리에서는 `getStaticProps` 함수로 빌드 시점에 데이터를 가져와 페이지를 미리 렌더링해요. 외부 API나 데이터베이스에서 필요한 데이터를 가져와서 페이지 컴포넌트에 props로 넘겨줄 수 있죠.

```js
// pages/index.js

export async function getStaticProps() {
  const res = await fetch(`https://...`)
  const projects = await res.json()

  return { props: { projects } }
}

export default function Index({ projects }) {
  return projects.map((project) => <div key={project.id}>{project.name}</div>)
}
```

- 여기서 `getStaticProps`는 빌드 타임에서만 실행되고, HTML을 미리 만들어두기 때문에 빠른 초기 로딩이 장점이에요.
- 단, 빌드 후에 데이터 변경사항이 있으면 다시 빌드해야합니다.
- `project` 리스트를 렌더링할 때는 React의 key prop 챙기는 걸 잊지 마세요!

---

## app 디렉터리: fetch() 사용 시 기본 캐시 전략

app 디렉터리에서는 서버 컴포넌트 내부에서 `fetch()`를 직접 사용해서 데이터를 가져올 수 있는데요, 이때 기본 옵션이 `cache: 'force-cache'`라서 기본적으로 데이터를 캐싱해요. 즉, 빌드 시 캐싱된 값을 계속 사용하다가 수동으로 캐시를 무효화할 때까지 재요청하지 않습니다.

```js
// app/page.js

// 함수 이름은 자유롭게 지어도 됩니다.
async function getProjects() {
  const res = await fetch(`https://...`)
  const projects = await res.json()

  return projects
}

export default async function Index() {
  const projects = await getProjects()

  return projects.map((project) => <div key={project.id}>{project.name}</div>)
}
```

- 서버 컴포넌트는 async 함수로 작성 가능해서 데이터를 서버에서 바로 받아올 수 있어요.
- `fetch`의 기본 캐싱 정책 덕분에 getStaticProps와 유사하게 캐시된 데이터를 재사용할 수 있답니다.
- 물론 필요하면 `fetch` 옵션으로 `cache: 'no-store'` 또는 `revalidate` 값을 조정해서 데이터 최신화 정책을 관리할 수도 있어요.

---

## 요약

| 특징                   | pages 디렉터리 (getStaticProps)               | app 디렉터리 (fetch 기본 캐시)                    |
|-----------------------|----------------------------------------------|---------------------------------------------------|
| 데이터 페칭 시점         | 빌드 타임                                     | 서버 컴포넌트 실행 시 (기본 캐시 포함)                |
| 데이터 캐싱 정책          | 빌드 후에는 고정, 다시 빌드해야 업데이트           | 기본 `force-cache` (캐시 수동 무효화 가능)               |
| 사용법                  | `getStaticProps` 함수 별도 선언               | 서버 컴포넌트 안에서 바로 fetch                      |
| 서버 컴포넌트 지원 여부    | 없음                                         | 지원                                            |

---

Next.js 13 버전부터는 app 디렉터리를 점점 더 권장하기 때문에, 기존 pages 디렉터리에서 했던 데이터 페칭 로직을 점차 바꾸는 것도 좋은 방법이에요. 단, 프로젝트 상황에 따라 필요한 캐싱 전략이나 데이터 최신화 정책을 잘 고민해서 적용하는 게 중요합니다.

추가로, app 디렉터리에서는 React Server Components를 활용해서 클라이언트 측 JavaScript를 최소화하는 장점도 있어요. 다음에 React Server Components에 대해서도 한번 같이 살펴볼게요!

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

#### 다이나믹 경로 (getStaticPaths)

Next.js에서 **pages** 디렉토리에선 `getStaticPaths` 함수를 사용해서 미리 빌드 타임에 생성할 동적 경로들을 정의해줍니다. 예를 들어, 게시글(id가 1, 2인 글)을 미리 만들어두고 싶을 때 이렇게 설정할 수 있죠.

```js
// pages 디렉토리 예시
import PostLayout from '@/components/post-layout'
 
export async function getStaticPaths() {
  return {
    paths: [{ params: { id: '1' } }, { params: { id: '2' } }],
  }
}
 
export async function getStaticProps({ params }) {
  const res = await fetch(`https://.../posts/${params.id}`)
  const post = await res.json()
 
  return { props: { post } }
}
 
export default function Post({ post }) {
  return <PostLayout post={post} />
}
```

- `getStaticPaths`는 어떤 경로를 정적 페이지로 미리 만들어둘지 알려주는 역할을 해요.
- `params` 객체 안에 있는 `id`는 동적 라우트 이름과 일치해야 합니다. 예를 들어 `[id].js` 라면 `id`가 되어야 하죠.
- 그런 다음 `getStaticProps`에서 `params`를 받아와서 실제 데이터를 불러와 페이지에 props로 넘깁니다.

---

그런데 **Next.js 13부터는 app 디렉토리 구조를 쓸 때는 `getStaticPaths`가 아니라** `generateStaticParams`라는 함수로 바뀌었어요. 

`generateStaticParams` 함수가 하는 역할은 거의 비슷하지만, 요즘엔 app 디렉토리에서는 이걸 더 권장하고 있어요. 다음과 같이 쓸 수 있습니다.

```js
export async function generateStaticParams() {
  return [{ id: '1' }, { id: '2' }]
}
```

그리고 컴포넌트 쪽에선 `props` 대신에 동적 세그먼트를 바로 받아올 수도 있죠.

---

### 참고로!

- `getStaticPaths` 혹은 `generateStaticParams` 둘 다 빌드 시점에 호출돼서 어떤 경로를 미리 만들어야 할지 결정합니다. 그래서 데이터가 바뀔 가능성이 잦다면 빌드 후에도 데이터가 반영되는 `ISR(Incremental Static Regeneration)` 옵션을 함께 사용하는 게 좋아요.
- `generateStaticParams`는 app 디렉토리의 새로운 “서버 컴포넌트” 개념과도 맞닿아 있어서, 페이지를 더 쉽게 설계할 수 있게 만들어줍니다. Next.js 13 이상부터는 무조건 `generateStaticParams`를 쓰는 게 트렌드라고 생각하면 편해요.

---

저도 처음에 pages → app 디렉토리 쪽으로 옮겼을 때 좀 헷갈렸는데, 이 부분만 잘 쓰면 Next.js 13의 동적 라우팅도 확실히 깔끔하게 관리할 수 있으니 꼭 써보시길 추천드립니다!

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

여러분, Next.js의 `generateStaticParams` 함수에 대해 들어보셨나요? 기존에 많이 쓰이던 `getStaticPaths`와 비슷한 역할을 하지만, API가 훨씬 단순해지고 레이아웃 안에서도 사용할 수 있게 진화했습니다. 여기선 어떤 점이 좋은지, 그리고 어떻게 사용하면 좋은지 쉽게 알려드릴게요!

일단, `generateStaticParams`는 반환 형식이 조금 다른데요. 예전 `getStaticPaths`처럼 중첩된 파라미터 객체 배열이나 경로 문자열 배열 대신, 그냥 각 세그먼트가 담긴 객체 배열을 반환하면 됩니다. 예를 들어:

```js
// `app` 디렉토리 안에서
import PostLayout from '@/components/post-layout'

export async function generateStaticParams() {
  return [{ id: '1' }, { id: '2' }]
}

async function getPost(params) {
  const res = await fetch(`https://.../posts/${params.id}`)
  const post = await res.json()

  return post
}

export default async function Post({ params }) {
  const post = await getPost(params)

  return <PostLayout post={post} />
}
```

위 예시는 포스트 ID별로 미리 페이지를 생성할 때 사용되죠. `params` 객체를 받아서 필요한 데이터를 가져오고, 그걸 레이아웃 컴포넌트에 넘겨주는 방식이에요.

---

### 네이밍도 훨씬 직관적으로!
기존에 `getStaticPaths`라고 하면 "뭔가 'get' 하는 느낌인데, 정적 경로 정보가 필요한 거지?" 싶은 이름이었죠. 하지만 요즘은 `generateStaticParams`처럼 "생성(generate)"이라는 단어가 들어가서, 실제로 경로를 만드는 과정임을 더 명확히 나타냅니다.

또한, `Paths` 대신 `Params`를 쓰는 것도 큰 변화입니다. 왜냐하면 요즘 Next.js의 라우팅은 중첩 라우팅이나 여러 동적 세그먼트를 사용할 때가 많아서, 경로(path)보다 세그먼트별 파라미터(params)를 잘 표현해 주는 이름이 더 적합하거든요.

---

### `fallback` 속성은 이제 어떻게?
기존에 `getStaticPaths`에서는 `fallback`이라는 옵션으로 "경로가 미리 생성되지 않은 경우 처리 방법"을 지정했죠. 그런데 `generateStaticParams` 사용 시에는 이런 옵션이 더 이상 필요하지 않습니다. 왜냐하면 `app` 디렉토리 기반의 새로운 라우팅 모델이 페이지 생성 자체를 더 선언적이고 유연하게 다루기 때문이에요.

---

### 마치며
- `generateStaticParams`는 더 간단하고 명확한 API를 제공해요.
- 레이아웃 레벨에서도 쉽게 경로 파라미터를 생성할 수 있답니다.
- 네이밍도 훨씬 직관적이고, `fallback` 설정 또한 사라져서 사용법이 깔끔해졌어요.

Next.js의 최신 `app` 디렉토리에서 동적 라우팅과 정적 생성 기능을 쓸 때 꼭 기억하세요! 앞으로 더 많은 기능이 나오면 또 소개해 드릴게요. 궁금한 점 있으면 댓글로 남겨주세요~

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

Next.js에서 페이지를 정적으로 생성할 때, `getStaticPaths`와 `fallback` 옵션이 어떻게 작동하는지 한 번 살펴볼게요. 보통 `pages` 디렉토리에서 많이 쓰는데, 여기에선 특정 페이지를 빌드 타임에 미리 생성하지 않고 상황에 따라 다르게 처리하는 방식을 알려줍니다.

---

### pages 디렉토리에서 fallback 옵션

`getStaticPaths` 함수는 어떤 경로들을 미리 만들어둘지 알려주는 역할을 하고, `fallback` 옵션이 빌드 시 미리 생성되지 않은 경로에 대해서 어떻게 동작할지 결정해요.

- `fallback: true`  
  빌드 때 없는 페이지라도, 요청 시점에 페이지를 생성하면서 "로딩 중" 같은 fallback UI를 보여줄 수 있어요.
- `fallback: false`  
  빌드에 없는 페이지는 무조건 404 페이지를 반환해요.
- `fallback: 'blocking'`  
  요청이 들어오면 서버에서 페이지 완성까지 기다렸다가 완성된 페이지를 보여줘요. 로딩 중 UI 없이 자연스럽게 보여주는 느낌!

예시 코드:

```js
// `pages` 디렉토리 예시

export async function getStaticPaths() {
  return {
    paths: [], // 미리 생성할 경로를 정의
    fallback: 'blocking' // 요청 시 페이지가 없으면 바로 생성 후 반환
  };
}

export async function getStaticProps({ params }) {
  // params를 활용해 데이터 패칭
  ...
}

export default function Post({ post }) {
  return (
    // 데이터 기반 컴포넌트 렌더링
    ...
  );
}
```

---

### app 디렉토리에서는 어떻게 다를까?

Next.js 새로운 앱 디렉토리 방식에서는 `generateStaticParams`라는 함수로 미리 생성할 경로들을 정의하고, `config.dynamicParams` 옵션이 새롭게 등장했어요. 이 옵션은 `generateStaticParams`에 포함되지 않은 동적 경로들을 어떻게 처리할지 결정합니다.

| config.dynamicParams | 동작 방식                                     |
|----------------------|---------------------------------------------|
| true (기본값)         | 미리 없던 경로라도 요청 시 생성 (on demand)  |
| false                | generateStaticParams에 없는 경로는 404 반환 |

이렇게 바뀌면서 페이지의 유연성이 좀 더 좋아졌고, 앱 디렉토리에선 빌드 방식과 동작 흐름이 좀 더 명확해졌어요. 기존 `fallback`과 역할은 비슷하지만, 옵션 이름과 적용되는 방식이 달라졌으니 참고하세요!

---

### 정리하며

- `pages` 디렉토리에서는 `getStaticPaths`의 `fallback` 옵션이 동적 경로에 대응.
- `app` 디렉토리에서는 `generateStaticParams` + `config.dynamicParams`로 대응.
- `fallback: 'blocking'` 같은 옵션은, 사용자에게 매끄럽게 완성된 페이지를 보여주고 싶을 때 유용.
- 새롭게 앱 디렉토리를 사용한다면 `config.dynamicParams`의 기본값이 true임을 기억!

이 내용 참고해서 Next.js에서 동적 경로를 더 잘 다뤄보세요. 혹시 페이지 생성 타이밍이나 방법에 대해 더 궁금한 부분 있으면 언제든 물어봐 주세요!

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

Next.js의 새로운 App 디렉토리에서, 기존 `pages` 디렉토리에서 사용하던 `getStaticPaths`의 `fallback: true | false | 'blocking'` 옵션을 대체하는 방식이 도입되었어요. 

특히 `fallback: 'blocking'` 옵션은 이제 `dynamicParams`에서는 제외됐는데, 그 이유는 `'blocking'`과 `true`가 스트리밍 환경에서는 거의 차이가 없기 때문입니다.

아래 예제를 볼까요?

```js
// app 디렉토리 내 예제

export const dynamicParams = true;

export async function generateStaticParams() {
  return [ /* 미리 생성할 경로들 반환 */ ];
}

async function getPost(params) {
  // params를 기반으로 포스트 데이터 조회
  ...
}

export default async function Post({ params }) {
  const post = await getPost(params);

  return (
    // 포스트 데이터를 바탕으로 렌더링
    ...
  );
}
```

위 코드를 보면 `dynamicParams`가 `true(기본값)`으로 설정돼 있어요. 이 경우, 만약 미리 생성된 경로가 아니라면, 해당 경로 요청 시 서버에서 렌더링 후 결과를 캐시합니다. 쉽게 말해, 없는 페이지를 처음 요청하면 서버가 페이지를 바로 만들어서 보여주고, 이후에는 저장해둔 결과를 빠르게 보여주는 방식이죠.

---

### Incremental Static Regeneration (ISR) (예전 getStaticProps + revalidate 개념)

이 부분은 예전 `pages` 디렉토리에서 `getStaticProps`를 쓸 때 `revalidate` 옵션을 주는 것과 비슷해요. ISR은 빌드 타임에 미리 페이지를 만들어 놓고, 일정 시간이 지나면 다시 서버에서 페이지를 재생성하는 방식이죠. 이렇게 하면 매우 최신 상태의 페이지를 사용자에게 제공하면서도 정적인 페이지의 이점을 유지할 수 있어요.

App 디렉토리에서도 이런 개념이 비슷하게 적용되고 있으니, `generateStaticParams`와 `dynamicParams`를 적절히 활용해보면 성능과 최신성, 유지보수성을 모두 잡을 수 있습니다.

---

### 정리하자면!

| 옵션             | 설명                                                                     |
|----------------|------------------------------------------------------------------------|
| `dynamicParams = true`  | 없는 경로 요청 시 서버에서 바로 생성 후 캐싱. 기본값.                                  |
| `generateStaticParams()` | 미리 생성할 경로 목록을 지정. 주로 빌드 타임에 호출됨.                                    |
| `fallback: 'blocking'` (pages) | 앱 디렉토리에선 적용 안 함. 스트리밍 환경에서는 `true`와 비슷한 역할을 하므로 제거됨.      |

Next.js가 이런 식으로 SSR, SSG, ISR 방식을 더 자연스럽고 유연하게 지원하고 있어서, 개발자가 상황에 맞게 잘 선택해서 쓰기 좋아졌답니다.  

필요하면 더 자세한 예제나 활용법도 알려줄게요!

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

Next.js에서 페이지를 자동으로 재생성하는 방법, `pages` 디렉토리와 `app` 디렉토리 각각에서 어떻게 다르게 동작하는지 알아볼게요.

---

### `pages` 디렉토리에서 `getStaticProps`의 `revalidate`

`pages` 디렉토리에서는 `getStaticProps` 함수 안에 `revalidate` 필드를 넣으면 됩니다. 이 값은 몇 초마다 페이지를 다시 생성할지 정하는 거예요. 예를 들어 `revalidate: 60`이면, 처음 빌드 후 60초가 지나면 페이지가 다시 백그라운드에서 생성됩니다.

```js
export async function getStaticProps() {
  const res = await fetch(`https://.../posts`)
  const posts = await res.json()
 
  return {
    props: { posts },
    revalidate: 60, // 여기서 60초마다 재생성을 설정
  }
}
 
export default function Index({ posts }) {
  return (
    <Layout>
      <PostList posts={posts} />
    </Layout>
  )
}
```

- 이 방식은 페이지 단위로 작동해요.
- 페이지 전체가 정적으로 빌드되고, 60초마다 업데이트된 내용을 반영해 새로 빌드됩니다.
- 사용자가 방문할 때마다 항상 최신 데이터가 나오진 않지만, 정해진 시간 간격으로 최신 상태를 유지할 수 있어요.

---

### `app` 디렉토리에서 `fetch`의 `revalidate` 옵션

`app` 디렉토리(Next.js 13의 새로운 앱 라우터 사용 시)에서는 데이터 패칭을 할 때 `fetch` 함수에 `{ next: { revalidate: 60 } }` 옵션을 붙여주면, 이 요청 결과를 60초 동안 캐싱합니다.

```js
async function getPosts() {
  const res = await fetch(`https://.../posts`, { next: { revalidate: 60 } })
  const data = await res.json()
 
  return data.posts
}
 
export default async function PostList() {
  const posts = await getPosts()
 
  return posts.map((post) => <div>{post.name}</div>)
}
```

- 여기서는 데이터 페칭 단위에서 재검증 시간을 설정하는 차이점이 있어요.
- `fetch` 캐시는 60초 동안 유지되고, 그 사이 다시 요청하면 캐시된 데이터를 바로 씁니다.
- 60초가 지나면 최신 데이터를 받아오고 캐시를 새로 만듭니다.
- 이걸 합치면 부분 부분 데이터를 정적으로 캐싱하면서도 페이지 전체는 서버 컴포넌트로 동작하게 할 수 있어요.

---

### 요약하자면

| 특징             | `pages` 디렉토리 (`getStaticProps`) | `app` 디렉토리 (`fetch`의 revalidate)  |
|----------------|-----------------------------|------------------------------|
| 동작 단위         | 페이지 단위                      | 데이터 요청 단위                   |
| 재생성 주기 설정 방식 | `revalidate` 필드로 설정             | `fetch` 메서드 옵션으로 설정         |
| 정적 생성 방식     | ISR(Incremental Static Regeneration) | 서버 컴포넌트 + 캐시 재검증             |
| 업데이트 시점      | 방문 시점 기준 60초마다 백그라운드 생성    | 60초 간 캐시 사용 후 다음 요청 시 재검증   |

---

### 팁!

- 만약 `app` 디렉토리에서 `fetch`에 `next: { revalidate: 0 }`를 넣으면 항상 최신 데이터를 가져오게 돼요.
- 반대로 `next: { revalidate: false }`는 캐시를 아예 하지 않는 뜻이니, 기본 행동과 다르니 주의하세요.
- `pages`에서 ISR 쓰던 것보다 `app` 디렉토리의 캐시 제어가 훨씬 세밀하고 유연해서 최신 데이터 처리에 더 좋은 편입니다.

---

Next.js 13 이후로는 `app` 디렉토리를 적극 활용하는 걸 추천해요. 서버 컴포넌트이면서도 유연한 캐싱 전략을 쓸 수 있기 때문에 앱 성능, 사용자 경험 면에서 더 나은 결과를 기대할 수 있답니다!

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

#### API 라우트 (API Routes)

Next.js에서 API 라우트는 여전히 `pages/api` 디렉토리에서 그대로 동작해요. 기존 방식과 달라진 점 없이 예전처럼 사용 가능합니다. 그런데 요즘에는 `app` 디렉토리 안에서 **Route Handlers**라는 더 발전된 방식을 많이 씁니다.

Route Handlers는 Web 표준의 Request와 Response API를 활용해서, 특정 경로에 대한 요청을 더 세밀하게 제어할 수 있게 해줘요. 예를 들어, `GET` 요청을 처리하는 함수를 이렇게 작성할 수 있죠:

```js
export async function GET(request: Request) {
  // 여기에 요청 처리 로직 작성
}
```

즉, 함수 하나로 HTTP 메서드별 요청 핸들러를 만들 수 있다는 이야기인데요. 덕분에 서버 코드를 더 깔끔하고 명확하게 관리할 수 있어요.

참고로, Route Handlers를 활용하면 응답 헤더 설정이나 쿠키 다루기 같은 것도 표준 API를 통해 자연스럽게 할 수 있으니, Next.js로 풀스택 개발할 때 아주 편리합니다.

---

### 간단 정리

| 구분           | 설명                                             |
|----------------|--------------------------------------------------|
| API Routes     | `pages/api`에서 전통적으로 사용, 변경 없이 동작  |
| Route Handlers | `app` 디렉토리에서 새롭게 지원, Web Request/Response API 사용 |

작업하는 프로젝트가 최신 버전이고 `app` 디렉토리를 쓴다면 Route Handlers를 적극 활용해보세요! 기존 `pages/api`도 당분간 지원하니 상황에 맞게 선택하시면 됩니다.

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

> 참고할 점: 이전에 클라이언트에서 외부 API를 호출하기 위해 API 라우트를 사용했다면, 이제는 서버 컴포넌트를 활용해 더 안전하게 데이터를 가져올 수 있어요. 데이터 fetching에 대해 더 자세히 알고 싶다면 공식 문서를 참고해보세요.

#### 싱글 페이지 애플리케이션 (SPA)

만약 동시에 SPA에서 Next.js로 마이그레이션하고 있다면, 관련된 문서를 꼭 확인해 보세요. 마이그레이션 과정에서 좋은 팁과 가이드가 포함되어 있으니 도움이 될 거예요.

### 7단계: 스타일링

Next.js로 옮겨오면서 스타일링도 신경 써줘야 해요. CSS, Sass, 또는 CSS-in-JS 등 다양한 스타일링 방법을 사용할 수 있는데, Next.js가 공식적으로 지원하는 방법들을 활용하면 깔끔하고 유지보수 쉬운 스타일링을 할 수 있답니다. 스타일링 관련해서는 따로 정리한 내용도 있으니 필요하면 알려드릴게요!

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

페이지 디렉토리에서는 글로벌 스타일시트가 오직 pages/_app.js에서만 적용되도록 제한되어 있었어요. 그런데 이제 app 디렉토리에서는 이 제한이 풀려서, 글로벌 스타일을 레이아웃(layout), 페이지, 컴포넌트 어디서든 자유롭게 추가할 수 있게 되었답니다.

여기서 스타일을 다룰 수 있는 방법들을 간단히 정리하면 다음과 같아요:

| 스타일 종류          | 설명                                    |
|-------------------|---------------------------------------|
| CSS Modules       | 각 컴포넌트에 scoped된 CSS를 적용하는 방법               |
| Tailwind CSS      | 유틸리티 클래스 기반의 빠른 스타일링 프레임워크               |
| Global Styles     | 전체 애플리케이션에 적용하는 전역 스타일                   |
| CSS-in-JS         | 자바스크립트 파일 내에서 직접 스타일을 작성하는 방식         |
| External Stylesheets | 외부 CSS 파일을 불러와 사용하는 방법                       |
| Sass              | CSS 전처리기 중 하나로, 변수나 중첩 같은 기능을 지원           |

---

### Tailwind CSS

만약 Tailwind CSS를 사용 중이라면, app 디렉토리를 tailwind.config.js에 추가해줘야 해요. 이렇게 하면 Next.js의 새 구조와 Tailwind가 잘 연동되어 스타일이 제대로 적용됩니다.

예를 들어, tailwind.config.js 파일에서 아래처럼 설정해봅시다:

```js
module.exports = {
  content: [
    "./app/**/*.{js,ts,jsx,tsx}", // app 디렉토리에 있는 모든 파일을 대상으로 설정
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

이렇게 해두면 app 디렉토리 내에 있는 모든 컴포넌트, 페이지, 레이아웃 파일에서 Tailwind 클래스를 제대로 인식하고 스타일이 적용될 거예요.

---

추가로, app 디렉토리 구조에 익숙하지 않은 분들을 위해 한 가지 팁을 알려드리면, app 디렉토리 안에서는 기본적으로 서버 컴포넌트가 활성화되어 있어요. 서버 컴포넌트에서는 CSS나 스타일을 다루는 방식에 약간 제한이 있을 수 있으니, 스타일이 꼭 필요한 클라이언트 컴포넌트는 `'use client'` 지시어를 붙여서 명확하게 구분해주는 게 좋습니다.

궁금한 점 있으면 언제든 질문해 주세요!

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

이번 시간에는 Next.js 앱에서 Tailwind CSS를 설정하는 간단한 방법을 공유할게요! Tailwind CSS는 요즘 프론트엔드에서 가장 인기 있는 유틸리티 퍼스트 CSS 프레임워크 중 하나인데, 클래스만 붙여서 빠르게 스타일을 만들 수 있어서 개발 속도가 확실히 빨라집니다.

먼저, Tailwind가 어디서 스타일을 찾아야 하는지 알려주는 설정파일(tailwind.config.js)을 수정해야 해요. 여러분이 만든 컴포넌트나 페이지에 있는 모든 파일을 지정해야 Tailwind가 정확히 필요한 스타일만 뽑아낼 수 있거든요.

```js
module.exports = {
  content: [
    './app/**/*.{js,ts,jsx,tsx,mdx}', // 여기에 Next.js app 폴더의 모든 JS/TS/MDX 파일 포함
    './pages/**/*.{js,ts,jsx,tsx,mdx}', // pages 폴더도 포함
    './components/**/*.{js,ts,jsx,tsx,mdx}', // 컴포넌트 폴더도 잊지 말고 넣기
  ],
}
```

이렇게 지정해주면, Tailwind가 여러분 프로젝트 안의 모든 화면과 컴포넌트에서 사용된 클래스를 잘 찾아낼 수 있어요.

다음은 글로벌 CSS를 프로젝트에 불러와야 해요. Next.js 13버전부터 app 디렉토리를 사용하는 경우는 app/layout.js에 아래처럼 임포트해주면 된답니다.

```js
import '../styles/globals.css'

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

tailwind가 들어간 globals.css 안에 기본 Tailwind 지시어(@tailwind base, @tailwind components, @tailwind utilities)가 들어있다고 가정한 거에요. 그래야 Tailwind 스타일이 전역에서 적용되어요.

---

### 여기서 잠깐!

- `content` 배열에 프로젝트 내 Tailwind 클래스를 사용하는 모든 파일 경로를 넣어야 최적화된 CSS가 만들어져요. 안 넣으면, 실제 쓸 CSS가 빠지거나(디자인 깨짐), 쓰지 않는 CSS가 포함되어 파일 용량이 커져요.
- Next.js 13버전부터는 `app/layout.js`가 없어도 `app/page.js` 같은 파일에 `import '../styles/globals.css'`를 하는 방식을 간혹 쓰기도 하는데, 공식 문서는 layout에서 임포트하는 걸 권장해요.
- 만약 `mdx` 파일을 사용 중이라면 `.mdx` 확장자도 꼭 포함시켜야 Tailwind가 MDX 컨텐츠 내 스타일도 인식해요.

Tailwind CSS와 Next.js 조합으로 스타일 작업 할 때는, 꼭 위 내용을 체크해보시고 세팅하시면 더 편하고, 빠른 개발이 가능하답니다. 궁금한 점 있으면 언제든 댓글이나 DM 주세요!

더 자세한 Tailwind CSS 활용법은 [공식 Tailwind 문서](https://tailwindcss.com/docs/guides/nextjs)에서 확인할 수 있어요!

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

## App Router와 Pages Router 함께 사용하기

Next.js에서는 App Router와 Pages Router라는 두 가지 라우팅 방식을 지원하는데요. 이 둘을 동시에 사용할 때는 조금 주의해야 할 점이 있습니다.

서로 다른 라우터가 관리하는 경로 간 이동할 때, 기본적으로 하드 네비게이션(페이지 전체를 다시 로드하는 방식)이 발생해요. 그래서 next/link 컴포넌트가 제공하는 자동 링크 프리패칭 기능도 서로 다른 라우터 간에는 동작하지 않습니다.

그렇다고 해서 두 라우터를 함께 쓸 수 없는 건 아니에요! 적절히 최적화하면 App Router와 Pages Router 사이에서도 빠르고 부드러운 페이지 전환을 경험할 수 있습니다. 조금 더 깊게 들어가면 각각의 라우팅 방식을 이해하고, 프리패칭 동작을 직접 구현하거나, 상태를 공유해서 사용자 경험을 개선할 수 있어요.

### 추가 팁으로
- 앱 규모가 크고, 점진적으로 App Router로 마이그레이션할 때 이 부분을 신경 쓰면 좋아요.
- 필요하다면, 두 라우터 간 공유 가능한 글로벌 상태 관리 라이브러리(Recoil, Zustand, Redux 등)를 활용해 UI 일관성을 유지할 수 있어요.
- 공식 문서나 커뮤니티에서 제공하는 팁들을 참고하면서 점진적으로 적용해 보세요.

필요하면 어디서든 추가로 질문해 주세요. 함께 고민해봅시다!

---

## Codemods

Next.js에서 라우팅 구조를 변경하거나 코드 스타일을 바꿀 때 도움을 주는 자동 수정 도구인 Codemods도 알아두시면 좋아요.

설명과 함께 다음 포스트에서 자세히 다뤄볼게요!

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

Next.js에서는 기능이 더 이상 사용되지 않을 때 코드를 쉽게 업그레이드할 수 있도록 도와주는 Codemod 변환 도구를 제공합니다. Codemod는 자동으로 코드를 변경해 주기 때문에, 직접 하나하나 고치느라 시간을 낭비할 필요가 없어서 정말 편리하답니다.

더 자세한 내용과 사용법은 [Codemods 공식 문서](https://nextjs.org/docs/codemods)를 참고해 보세요. 작업 전에는 꼭 코드 백업이나 버전 관리(Git)를 활용해서 안전하게 변경하는 걸 추천할게요! 혹시 Codemod를 처음 사용한다면, 간단한 테스트 파일부터 적용해보는 것도 좋은 방법입니다.

이런 도구들을 활용하면, Deprecated된 기능 때문에 스트레스 받지 않고 손쉽게 최신 버전으로 코드를 유지할 수 있으니 꼭 한 번 써보시길 바랍니다!