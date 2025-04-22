---
title: "Next.js 14 버전으로 업그레이드하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:44
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to upgrade to version 14"
link: "https://nextjs.org/docs/app/guides/upgrading/version-14"
isUpdated: false
---


# Next.js 14 버전으로 업그레이드하는 방법

## 13버전에서 14버전으로 업그레이드하기

Next.js 14 버전으로 업데이트하려면 아래 명령어를 사용하면 돼요. 자신이 주로 사용하는 패키지 매니저에 맞춰 실행해 주세요!

```bash
npm i next@next-14 react@18 react-dom@18 && npm i eslint-config-next@next-14 -D
```

이 명령어는 Next.js, React, React DOM을 각각 최신 버전(14 버전과 React 18)으로 업데이트해주고, 개발 환경에서 ESLint 설정도 맞게 업그레이드해 줍니다.

> 참고로, Next.js는 React 최신 버전과 호환되도록 설계돼 있어서 React도 같이 업데이트해야 원활한 개발 가능해요.

npm 대신 yarn을 쓰는 분들은,

```bash
yarn add next@next-14 react@18 react-dom@18 && yarn add -D eslint-config-next@next-14
```

이렇게 하면 됩니다.

---

### 추가 팁

- 버전 업그레이드 후에는 의존성 충돌이 없는지, 개발 서버가 정상 실행되는지 꼭 확인하세요.
- Next.js 14 버전에서는 여러 가지 새로운 기능과 최적화가 들어갔는데, 공식 릴리즈 노트를 한 번 쭉 읽어보는 것도 좋아요.
- 아무래도 메이저 버전 업그레이드이다 보니 기존 코드 일부는 수정이 필요할 수 있으니, 마이그레이션 문서나 changelog도 참고해 주세요.

저도 최근에 Next.js 13에서 14로 넘어가면서 성능이 좀 더 좋아진 것 같고 깔끔한 API들도 마음에 들었어요. 한번 도전해 보시길 추천드릴게요!

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

안녕하세요, 여러분! 오늘은 Next.js 14 버전과 React 18을 설치하는 방법을 소개해 드릴게요. 그리고 ESLint 설정도 최신 버전으로 맞추는 팁도 함께 알려드리겠습니다.

사실 요즘에는 패키지 매니저가 여러 개 있어서 npm, yarn부터 pnpm, 심지어 bun까지 다양한 선택지가 있죠. 그래서 각 패키지 매니저별로 설치 명령어를 깔끔하게 정리해봤어요.

| 패키지 매니저 | 설치 명령어 |
|---------------|-------------|
| yarn          | `yarn add next@next-14 react@18 react-dom@18 && yarn add eslint-config-next@next-14 -D` |
| pnpm          | `pnpm i next@next-14 react@18 react-dom@18 && pnpm i eslint-config-next@next-14 -D` |
| bun           | `bun add next@next-14 react@18 react-dom@18 && bun add eslint-config-next@next-14 -D` |

여기서 `-D` 옵션은 ESLint 설정을 개발환경용 의존성(devDependencies)으로 설치하겠다는 의미입니다. ESLint는 코드 품질 유지에 필수적인 도구니까 꼭 함께 설치해 주세요!

> **참고!** 만약 TypeScript를 사용 중이라면 `@types/react`와 `@types/react-dom` 타입 선언 패키지도 꼭 최신 버전으로 업그레이드 해주세요. 그래야 타입 오류 없이 쾌적하게 개발할 수 있답니다.

```bash
yarn add @types/react@latest @types/react-dom@latest -D
# 또는
pnpm i @types/react@latest @types/react-dom@latest -D
# bun은 현재 타입 선언 패키지 직접 설치가 조금 다를 수 있으니 공식 문서 참고하세요.
```

끝으로, Next.js 14 버전은 많은 성능 개선과 새로운 기능을 담고 있어서 이번 기회에 업그레이드 해보시면 정말 좋을 거예요! 혹시 설치 중 문제가 발생하거나, 관련해서 더 궁금한 점 있으면 언제든지 댓글로 질문 주세요. 즐거운 개발 되세요~ 🚀

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

### Next.js v14 요약

안녕하세요! 오늘은 Next.js 최신 버전인 v14의 주요 업데이트 내용을 쉽게 정리해볼게요. 버전 업 할 때 꼭 체크해야 할 부분들이니 한 번 살펴보시길 추천해요.

| 주요 변경 사항 | 설명 |
|---|---|
| Node.js 최소 버전 상향 | 기존 16.14에서 18.17로 최소 버전이 올라갔어요. 16.x는 공식적으로 지원이 종료됐기 때문에 최신 환경에서 개발하려면 Node 버전을 꼭 업그레이드하세요. |
| next export 명령어 삭제 | 기존에 사용하던 `next export` 커맨드가 없어지고, 대신 `output: 'export'` 설정으로 변경됐어요. 정적 사이트 생성 시 설정 방식이 조금 바뀌었으니 공식 문서를 참고해 주세요. |
| ImageResponse import 경로 변경 | 이미지 생성용 `ImageResponse` 컴포넌트의 import 경로가 `next/server` 대신 `next/og`로 바뀌었습니다. 자동 변환 도구(codemod)가 제공되니 쉽게 적용 가능해요. |
| @next/font 패키지 제거 | 커스텀 폰트 관련 패키지였던 `@next/font`가 완전히 사라지고, Next.js 내장 `next/font`로 통합됐습니다. 이 부분도 자동 변환 스크립트를 활용해보세요. |
| next-swc WASM 타겟 제거 | 성능 향상에 사용되던 WebAssembly(WASM) 타겟이 제거됐습니다. 앞으로는 다른 방식으로 최적화가 이뤄질 예정이에요. |

---

#### 추가 팁!

- Node.js 버전 업그레이드는 Next.js뿐 아니라 다른 최신 라이브러리들도 잘 지원하니, 개발 환경 개선을 위한 좋은 기회예요.
- codemod라는 도구가 자동으로 코드 전반에 걸쳐 변경점을 적용해주니, 수동 작업에 따른 실수를 줄일 수 있어요. 명령어 몇 줄로 업데이트가 가능해서 큰 도움이 됩니다!
- `next export` 설정 변경은 정적 사이트 생성(SSG) 프로젝트라면 꼭 확인해야 할 부분입니다. 기존 방식과 조금 달라서 배포 플로우에 영향이 있을 수 있으니 사전에 테스트해보세요.

---

Next.js가 계속 발전하면서 새로운 개발 패턴과 최적화가 도입되고 있어요. 빠르게 변경 사항을 따라가면 더 편리하고 안정적인 웹 앱 개발이 가능하니 꾸준히 최신 문서도 살펴보시길 권해드립니다! 도움이 되셨다면 좋아요와 구독도 잊지 마세요 😊