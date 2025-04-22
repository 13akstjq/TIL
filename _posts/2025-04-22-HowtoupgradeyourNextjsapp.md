---
title: "Next.js 최신버전으로 업그레이드 하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:20
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to upgrade your Next.js app"
link: "https://nextjs.org/docs/app/getting-started/upgrading"
isUpdated: false
---


# Next.js 앱 업그레이드 쉽게 하는 방법

## 최신 버전으로 업데이트하기

Next.js 앱을 최신 버전으로 업그레이드하고 싶다면, 공식에서 제공하는 업그레이드 코드를 이용하면 정말 간단해요! 터미널에서 아래 명령어만 쓱 실행하면 끝납니다:

```bash
npx @next/codemod@canary upgrade latest
```

이 명령어는 현재 프로젝트에 설치된 Next.js 버전을 자동으로 최신 버전으로 바꿔주고, 코드에 필요한 변경점들도 함께 처리해줘서 편리하답니다.

---

### 팁 하나 더!

- 위 명령어는 `npx`를 이용해서 바로 실행하는 거라, 별도의 패키지 설치 없이도 사용 가능합니다.
- 혹시 직접 `package.json`에서 버전을 수정하고 싶다면, `"next"` 버전을 최신 버전으로 바꾸고 `npm install` 또는 `yarn`을 실행해도 돼요.
- 새로운 버전에서 변경된 주요 기능이나 deprecated된 부분이 있을 수 있으니, [Next.js 공식 릴리즈 노트](https://nextjs.org/blog)도 한 번 살펴보세요. 예상치 못한 버그 방지에 큰 도움이 된답니다!

Next.js 덕분에 React 기반 SSR(서버사이드 렌더링)이 훨씬 쉬워졌는데, 꾸준히 업데이트하면서 최신 기능도 맛보고 앱도 더 빠르게 만들어봐요~!

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

만약 수동으로 업그레이드하는 걸 선호한다면, 최신 버전의 Next.js와 React를 이렇게 설치할 수 있어요:

```bash
npm i next@latest react@latest react-dom@latest eslint-config-next@latest
```

---

## 캐나리(Canary) 버전

최신 캐나리 버전으로 업데이트하고 싶다면, 먼저 Next.js가 최신 버전인지 확인하고 현재 프로젝트가 정상적으로 작동하는지 테스트하는 게 중요해요. 안정적인 상태를 확인한 다음, 아래 명령어를 실행하면 됩니다:

```bash
npm i next@canary react@canary react-dom@canary eslint-config-next@canary
```

캐나리 버전은 아직 테스트 중인 기능들이 포함되어 있어서, 최신 기능을 미리 써보고 싶거나 버그를 발견해서 직접 피드백하고 싶을 때 유용해요. 그러나 안정성은 정식 버전보다 떨어질 수 있으니, 중요 프로젝트라면 충분히 테스트한 뒤에 도입하는 걸 추천드려요.

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
npm i next@canary
```

### 캔버리 버전에서 사용할 수 있는 기능들

현재 캔버리 버전에서 제공되는 주요 기능들을 살펴볼게요!

- **캐싱(Caching)** :  
  성능 향상을 위해 콘텐츠를 효율적으로 저장하고 재사용하는 기능이에요. 이 기능 덕분에 페이지 로딩 속도가 빨라지고 서버 부담도 줄일 수 있답니다.

캔버리 버전은 보통 최신 기능을 먼저 접해보고 싶을 때 추천드려요. 안정성은 정식 버전보다 조금 떨어질 수 있으니 중요한 프로젝트엔 주의가 필요해요 :)

만약 캐싱 기능 외에도 어떤 기능들이 있는지 더 궁금하다면, 다음에 또 자세히 다뤄보도록 할게요!


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

이번에는 개발하면서 자주 만날 수 있는 몇 가지 키워드와 용어들을 정리해볼게요. 각각 어떤 의미인지, 그리고 어떻게 활용할 수 있는지 함께 살펴보자고요!

| 키워드         | 설명                                                                                 |
|----------------|--------------------------------------------------------------------------------------|
| use cache      | 캐시를 사용하는 옵션이나 설정. 서버나 클라이언트에서 자주 요청되는 데이터를 임시로 저장해서 빠르게 응답할 수 있게 도와줘요. |
| cacheLife      | 캐시가 유지되는 시간. 즉, 캐시 데이터를 얼마 동안 유효하게 쓸지 결정하는 값이에요. 예를 들어, 10분 동안 캐시 유지라면 10분 뒤엔 새로운 데이터를 가져오죠. |
| cacheTag       | 캐시를 식별하거나 그룹핑하기 위한 태그. 이를 통해 특정 태그가 붙은 캐시만 선택적으로 삭제하거나 갱신할 수 있어요. |
| dynamicIO      | 동적으로 입출력을 처리하는 기능. 예를 들어, 사용자 입력이나 요청에 따라 실행되는 코드가 동적으로 바뀔 때 쓰여요. 서버와의 실시간 데이터 교환 같은 곳에서 중요해요. |

이제 인증(Authentication) 관련된 용어들도 살펴볼까요? 인증 로직을 짤 때 자주 접하게 되니까 알아두면 좋아요.

| 키워드           | 설명                                                                                  |
|------------------|---------------------------------------------------------------------------------------|
| forbidden        | 접근 권한이 없는 자원에 대해 서버가 요청을 거부할 때 사용하는 상태. 보통 HTTP 403 에 해당해요. |
| unauthorized     | 인증이 필요한 자원에 대해 인증이 안 된 상태에서 접근하려고 할 때 사용하는 상태. HTTP 401과 연결돼요. |
| forbidden.js     | 접근 금지(403) 관련 로직을 처리하는 자바스크립트 파일일 가능성이 높아요. 예외 처리를 담당하거나 사용자에게 알림을 줄 때 사용하겠죠? |
| unauthorized.js  | 인증되지 않은 상태(401)를 처리하는 자바스크립트 파일. 로그인 페이지로 리다이렉트하거나 경고 메시지를 띄우는 용도로 쓰일 수 있어요. |
| authInterrupts   | 인증과 관련된 작업 도중에 흐름을 중단시키거나 인터셉트(가로채기) 하는 기능. 예를 들어, 인증 실패 시 다른 동작을 막고 로그인 화면으로 보내는 등 제어할 때 유용하답니다. |

---

### 덧붙여서

캐시 관련 개념들은 개발 성능 최적화에서 정말 중요해요. 예를 들어, 대규모 서비스에서는 무한정 캐시 데이터를 유지하면 메모리가 부족해질 수 있으니까 `cacheLife` 값을 적절히 조정하는 게 중요하죠. 또한, 만약 여러 캐시 데이터를 그룹화해야 한다면 `cacheTag`를 활용해서 필요한 부분만 빠르게 업데이트할 수 있답니다.

그리고 인증 처리 로직은 보안에 직결되니, 상태 코드(403, 401)를 클라이언트에 명확하게 알려주는 게 중요해요. 그리고 인증 실패나 권한 부족 상황에서 사용자에게 적절한 피드백을 주는 것도 잊지 마시고요.

혹시 실제 코드 구현과 관련해서 추가로 궁금한 점 있으면 언제든 말해 주세요!