---
title: "Nextjs 15로 배우는 create-next-app 설치 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:38
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "create-next-app"
link: "https://nextjs.org/docs/app/api-reference/cli/create-next-app"
isUpdated: false
---


# create-next-app

Next.js로 새로운 프로젝트를 시작할 때 가장 간편한 방법 중 하나가 바로 create-next-app CLI를 사용하는 거예요. 이 도구는 기본 템플릿이나 GitHub에 공개된 여러 예제들을 바탕으로 새로운 Next.js 애플리케이션을 빠르게 만들어 줍니다.

### 기본 사용법

터미널에서 아래 명령어를 입력하면 돼요:

```bash
npx create-next-app@latest [프로젝트명] [옵션]
```

예를 들어, `my-next-app`이라는 이름의 프로젝트를 만들고 싶다면:

```bash
npx create-next-app@latest my-next-app
```

이렇게 입력만 하면 Next.js 환경이 자동으로 세팅되고, 곧바로 개발을 시작할 수 있어요.

---

### 추가 팁!

- **다양한 템플릿 사용 가능**  
  기본 템플릿뿐 아니라 Next.js 공식 GitHub 예제들을 템플릿으로 사용할 수 있어요. 예를 들어, `with-tailwindcss` 템플릿을 쓰고 싶다면 다음과 같이 명령어를 입력하면 됩니다:

  bash
  npx create-next-app@latest my-next-app --example with-tailwindcss
  

- **TypeScript 프로젝트 생성**  
  기본 JavaScript가 아니라 TypeScript 프로젝트를 만들고 싶을 때도 옵션 하나로 해결돼요:

  bash
  npx create-next-app@latest my-next-app --typescript
  

- **npm, yarn, pnpm 선택 가능**  
  프로젝트 생성 시 사용할 패키지 매니저를 직접 지정할 수도 있어요:

  bash
  npx create-next-app@latest my-next-app --use-npm
  

---

create-next-app 덕분에 복잡한 설정 없이 바로 Next.js 개발환경을 만들어 볼 수 있어서 미리 환경을 고민하는 시간을 줄여준답니다. 아직 Next.js로 개발을 시작하지 않았다면, 이걸로 한 번 쉽게 시작해보세요!

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

## 참고 자료

아래에 사용할 수 있는 옵션들이 정리되어 있어요:

| 옵션 | 설명 |
| --- | --- |
| `-h` 또는 `--help` | 사용 가능한 모든 옵션을 보여 줍니다 |
| `-v` 또는 `--version` | 버전 번호를 출력합니다 |
| `--no-*` | 기본 옵션을 반대로 설정합니다. 예: `--no-eslint` (ESLint 비활성화) |
| `--ts` 또는 `--typescript` | TypeScript 프로젝트로 초기화 (기본값) |
| `--js` 또는 `--javascript` | JavaScript 프로젝트로 초기화 |
| `--tailwind` | Tailwind CSS 설정으로 초기화 (기본값) |
| `--eslint` | ESLint 설정으로 초기화 |
| `--app` | App Router 프로젝트로 초기화 |
| `--api` | 라우트 핸들러만 포함하는 프로젝트 초기화 |
| `--src-dir` | `src/` 디렉토리 내부에서 초기화 |
| `--turbopack` | 개발용으로 기본 Turbopack 활성화 |
| `--import-alias <별칭>` | import 별칭 지정 (기본값은 `"@/*"`) |
| `--empty` | 빈 프로젝트로 초기화 |
| `--use-npm` | npm으로 부트스트랩을 명시적으로 실행 |
| `--use-pnpm` | pnpm으로 부트스트랩을 명시적으로 실행 |
| `--use-yarn` | Yarn으로 부트스트랩을 명시적으로 실행 |
| `--use-bun` | Bun으로 부트스트랩을 명시적으로 실행 |
| `-e` 또는 `--example [이름] [깃허브-URL]` | 예제 프로젝트로 부트스트랩 |
| `--example-path <예제-경로>` | 예제 프로젝트 경로를 별도로 지정 |
| `--reset-preferences` | 저장된 환경설정을 초기화 |
| `--skip-install` | 패키지 설치를 건너뜀 |
| `--yes` | 이전 환경설정이나 기본값을 모든 옵션에 사용 |

---

### 조금 더 알아보기!

- `--no-eslint` 처럼 `--no-` 접두어를 붙이면 기본적으로 활성화된 설정을 끌 수 있다는 점이 유용해요.
- 패키지 매니저를 직접 지정하는 옵션 (`--use-npm` 등)을 잘 활용하면 팀이나 개인 환경에 맞게 부트스트랩 방식을 제어할 수 있습니다.
- `--example` 옵션을 쓰면 이미 만들어진 예제 프로젝트를 기반으로 신속하게 시작할 수 있으니 참고하세요.

이 옵션들을 적절히 조합하면 원하는 환경에 딱 맞는 프로젝트 세팅이 가능합니다. 다음에 예제도 함께 살펴볼게요!

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

### 기본 템플릿으로 새 앱 만들기

기본 템플릿을 사용해서 새 앱을 만들고 싶다면, 터미널에 아래 명령어를 입력해 주세요:

```bash
npx create-next-app@latest
```

이후 아래와 같은 질문들이 차례로 나올 거예요:

| 질문 내용                    | 설명                                   |
|-----------------------------|------------------------------------|
| 프로젝트 이름을 어떻게 할까요?   | 새로 만들 앱의 이름을 정합니다.                |
| TypeScript를 사용할 건가요?      | TypeScript 사용 여부를 묻습니다.                  |
| ESLint를 설정하겠어요?          | 코드 품질을 유지하기 위해 ESLint 설정 여부를 물어봐요.       |
| Tailwind CSS를 추가할까요?      | 스타일링에 Tailwind CSS를 넣을지 선택합니다.           |
| `src/` 디렉터리를 사용할까요?   | 소스 코드가 들어갈 디렉터리 구조를 `src/`로 할지 묻습니다. |
| 테스트 라이브러리 설정할까요?    | Jest 등 테스트 도구 설정 여부를 확인해요.             |
| Git 저장소를 초기화할까요?       | 새 프로젝트에 Git 저장소를 초기화할지 묻습니다.          |

이렇게 여러 가지 옵션을 선택한 후에, Next.js 프로젝트가 자동으로 세팅되어 바로 개발을 시작할 수 있게 됩니다.

---

> 참고로, 선택 항목들은 프로젝트 성격에 따라 다르게 설정하면 좋아요. 예를 들어 TypeScript를 쓰면 안정성이 높아지고, Tailwind CSS는 빠른 스타일링에 도움을 주죠. ESLint와 테스트는 유지보수에 필수적인 도구들이니 프로젝트 규모와 상황을 고려해 선택하세요!

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

앞으로 Next.js 프로젝트를 시작할 때, 아래와 같은 질문들을 만나게 될 거예요. 이 질문들에 답하면 여러분 취향대로 딱 맞는 프로젝트 세팅이 자동으로 만들어집니다!

```js
프로젝트 이름이 뭐에요?  my-app
TypeScript를 사용하시겠어요?  아니오 / 예
ESLint를 함께 쓰시겠어요?  아니오 / 예
Tailwind CSS를 넣을까요?  아니오 / 예
코드를 `src/` 폴더 안에 넣을까요?  아니오 / 예
App Router를 사용하실래요? (추천)  아니오 / 예
next dev 명령어에 Turbopack을 쓸까요?  아니오 / 예
import alias를 커스터마이징 하실래요? (`@/*`가 기본값이에요)  아니오 / 예
```

이렇게 질문에 답변하고 나면, 내가 원하는 설정이 반영된 새 프로젝트가 뚝딱 생성됩니다. 정말 편하죠?

---

### 공식 Next.js 예제를 기반으로 새 앱 만들기

Next.js에서 제공하는 공식 예제를 쓰고 싶다면 `--example` 플래그를 사용하시면 돼요. 예를 들어서:

```bash
npx create-next-app@latest --example with-tailwindcss my-app
```

위명령은 Tailwind CSS가 미리 세팅된 예제로 프로젝트를 만들어줘요. 공식 문서나 GitHub에 다양한 예제가 준비되어 있으니 내 프로젝트에 꼭 맞는 템플릿을 찾아 활용해보세요.

---

추가 팁을 드리자면, `App Router`는 Next.js 13에서 도입된 최신 라우팅 방식인데요. 기존의 `pages` 폴더 기반 라우팅과 다르게 훨씬 직관적이고, 서버 컴포넌트라는 개념 덕분에 퍼포먼스도 좋아졌어요. 가능하다면 꼭 사용해 보시길 추천합니다!

그리고 `Turbopack`은 Next.js의 차세대 번들러로, webpack 보다 훨씬 빠르게 빌드해준답니다. 아직 실험적인 기능이라 큰 프로젝트에서는 조금 더 지켜보는 게 좋지만, 작은 프로젝트나 개발환경에서는 꽤 유용해요.

필요하다면 나중에 설정을 바꾸는 것도 어렵지 않으니 너무 걱정 마시고, 편한 방향으로 선택해보세요!

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
npx create-next-app@latest --example [example-name] [your-project-name]
```

이 명령어는 Next.js 공식 저장소에 있는 다양한 예제 중 하나를 골라 새로운 프로젝트를 만들 때 사용해요. 예제 이름([example-name])을 넣고, 프로젝트 이름([your-project-name])도 같이 정해주면 끝!

### Next.js 공식 예제 리스트 보기

Next.js 깃허브 저장소에 가면, 사용할 수 있는 예제들과 그 설정 방법이 잘 정리되어 있으니 참고하시면 좋아요.

### 공개된 깃허브 예제로 만들기

만약 Next.js 공식 예제뿐만 아니라, 깃허브에 공개된 다른 예제를 가져와서 프로젝트를 만들고 싶다면, `--example` 옵션에 깃허브 저장소 URL을 넣어주면 돼요.

예를 들어:
```bash
npx create-next-app@latest --example https://github.com/vercel/next.js/tree/canary/examples/with-redux my-redux-app
```
위 명령어는 Next.js 공식 깃허브 저장소 중 "with-redux" 예제를 다운로드해서 `my-redux-app`이라는 이름으로 프로젝트를 생성해줍니다.

---

제가 한 가지 팁을 더 드리자면, 이렇게 예제로 시작하면 기본 구축된 구조를 한눈에 보고 바로 개발에 돌입할 수 있어서 꽤 편리해요. 특히 Next.js에는 다양한 상황별 예제가 많으니, 내가 만들고 싶은 기능과 비슷한 걸 먼저 선택해서 로드맵을 짜보는 건 어떨까요? 예제를 활용하면 처음부터 코드를 직접 작성하는 부담도 줄어들고, Next.js 컨벤션도 자연스럽게 익힐 수 있어서 일석이조입니다!

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

여러분, Next.js 프로젝트를 깔끔하게 시작하고 싶을 때 공식 템플릿이나 다른 깃허브 예제 레포를 바로 가져오는 방법이 있는데, 바로 `create-next-app` 명령어를 활용하는 거예요.

```bash
npx create-next-app@latest --example "https://github.com/.../" [your-project-name]
```

이 명령어를 풀어보자면,

- `npx create-next-app@latest` : 최신 버전의 `create-next-app`을 실행하는 거고,
- `--example "https://github.com/.../"` : 내가 원하는 예제 템플릿이 있는 깃허브 URL을 직접 지정할 수 있어요.
- `[your-project-name]` : 새로 만들 프로젝트의 폴더 이름이 됩니다.

예를 들어, 누군가가 만든 멋진 Next.js 템플릿이 있다면 그 깃허브 주소를 위 예제 자리에 넣고 프로젝트를 바로 시작할 수 있죠. 덕분에 기본 템플릿부터 내가 원하는 커스텀 구조까지 손쉽게 빠르게 셋업 가능!

추가 팁을 조금 드리자면, 만약 자주 쓰는 예제가 있다면 이렇게 한번 설치해 놓고 그 뒤엔 그냥 `next dev` 해서 바로 개발 스타트할 수 있어서 개발 속도가 꽤 빨라집니다. 또 공식 템플릿 말고 커뮤니티에서 공유하는 여러가지 스타터들이 있으니 여러 가지 테스트 해보면서 맞는 걸 골라보는 것도 좋아요.

요약해서, 직접 깃허브에서 예제를 불러와 손쉽게 Next.js 프로젝트 시작하는 법이니 꼭 기억해두시면 좋아요!