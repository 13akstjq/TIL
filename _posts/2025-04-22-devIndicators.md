---
title: "Next.js 15에서 devIndicators 활성화 및 활용법 완벽 가이드"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:04
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "devIndicators"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/devIndicators"
isUpdated: false
---


# devIndicators 소개

개발할 때 현재 보고 있는 **라우트(route)** 정보를 화면에 살짝 보여주는 표시기를 설정할 수 있는 기능인데요, 이름처럼 개발하면서 디버깅할 때 꽤 유용합니다.

```js
devIndicators: false | {
  position?: 'bottom-right' | 'bottom-left' | 'top-right' | 'top-left', // 기본값은 'bottom-left'
},
```

- **false로 설정하면** 표시기가 아예 안 보이게 되고,
- **객체로 설정하면** 표시기가 화면 어디에 나타날지 지정할 수 있어요.

기본 위치는 좌측 하단(bottom-left)인데, 필요에 따라 원하는 위치로 바꿔서 쓸 수 있습니다.

---

### 한 번에 짚고 가는 팁!

- `devIndicators`는 **개발 모드에서만 동작**합니다. 실제 프로덕션 빌드에서는 자동으로 표시되지 않으니까 걱정하지 않아도 되죠.
- **표시기를 끈다고 해서 에러가 안 보이는 게 아닙니다**! Next.js가 빌드 에러나 런타임 에러 메시지는 여전히 보여주니 개발에 지장 없으니 안심하시고요.
- 만약 화면에 표시되는 위치 때문에 UI가 살짝 가려진다고 생각하면, 위치 속성(`position`)을 살짝 옮겨보세요.

---

### 마무리

개발할 때 여기저기 화면 확인하느라 바쁘거나, 어떤 페이지에서 에러가 났는지 헷갈릴 때 이 devIndicators 기능이 꽤 도움 됩니다. 혹시 어디 있었지 싶은 여러분, 지금 바로 `next.config.js`에서 한번 설정해보세요~!

필요에 따라 끄거나, 위치 조정해가면서 편하게 개발 환경을 맞춰보시길 바랍니다 :)

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

## 문제 해결하기 (Troubleshooting)

### 인디케이터가 라우트를 정적(static)으로 표시하지 않을 때

만약 어떤 라우트가 정적으로 처리되길 기대했는데, 인디케이터가 이 라우트를 동적(dynamic)으로 표시한다면, 그 라우트가 정적 렌더링에서 제외(옵트아웃)되었을 가능성이 큽니다.

이게 맞는지 확인하려면, 터미널에서 다음 명령어로 애플리케이션을 빌드해보세요:

```bash
next build --debug
```

그 후 출력 메시지를 보면,  
- 정적(혹은 프리렌더)된 라우트는 ○ (동그라미) 기호로 표시되고,  
- 동적 라우트는 ƒ (함수) 기호로 표시됩니다.  

예를 들어 아래와 같은 형태로 확인할 수 있죠.

> 참고로 여기서 동적 라우트는 빌드 시점에 HTML이 생성되지 않고, 요청 시마다 서버 또는 클라이언트에서 렌더링이 이뤄지는 라우트를 말해요.  
> 반면 정적 라우트는 미리 컴파일 단계에서 HTML을 만들어서 빠르게 응답할 수 있도록 한 거예요.

이 방법으로 어떤 라우트가 왜 정적이 아닌지, 혹은 원래 기대와 달리 표시되는 이유를 찾아낼 수 있습니다.

---

추가 팁을 조금 드리자면, Next.js를 사용할 때 라우트가 정적이 되려면 `getStaticProps`를 사용하거나, 혹은 별도의 동적 데이터 가져오기 함수가 없어야 해요. 만약 `getServerSideProps` 같은 서버 사이드 렌더링 함수를 쓰면 당연히 동적 라우트가 됩니다.  
조금 더 깊게 캐치하고 싶다면, 라우트 파일의 내보낸(export) 함수를 한 번 점검해보세요!

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

여러분, Next.js에서 라우트가 정적(static) 렌더링을 안 하고 동적(dynamic) 렌더링을 선택하는 경우가 있는데요. 위 표처럼 `/products/[id]` 같은 경로가 동적으로 서버에서 렌더링된다는 걸 볼 수 있어요.

```js
Route (app)                              Size     First Load JS
┌ ○ /_not-found                          0 B               0 kB
└ ƒ /products/[id]                       0 B               0 kB
 
○  (Static)   prerendered as static content
ƒ  (Dynamic)  server-rendered on demand
```

왜 이런 선택이 발생하는 걸까요? 주된 두 가지 이유가 있어요.

| 이유                                      | 설명                                                                                     |
|-----------------------------------------|----------------------------------------------------------------------------------------|
| 동적 API 사용 (Dynamic APIs)             | 런타임에 실행되는 정보를 필요로 하는 API가 있을 때, 예를 들어 사용자의 로그인 상태에 따라 다른 데이터를 보여줘야 할 때.      |
| 캐시되지 않는 데이터 요청 (Uncached data) | ORM이나 데이터베이스 드라이버에 직접 데이터를 요청하는 상황. 이 경우 요청마다 데이터가 달라지므로 정적으로 미리 만들기 어려움. |

만약 여러분의 라우트에서 위 두 가지 상황 중 하나라도 해당하면, 정적 렌더링은 어렵다고 볼 수 있어요. 이럴 때는 어떻게 해야 할까요? Next.js가 제공하는 좋은 방법들이 있어요.

### `loading.js` 혹은 React의 `Suspense` 활용하기

정적 렌더링이 어려워서 서버에서 데이터를 받아와야 할 때, 유저 경험을 끌어올리기 위해 로딩 상태를 잘 처리하는 것이 중요하죠. Next.js에서는 특정 라우트 또는 컴포넌트에 `loading.js` 파일을 만들어서 로딩 화면을 쉽게 관리할 수 있고, React 18의 `Suspense`와 스트리밍 기능을 이용해 점진적으로 UI를 보여줄 수도 있어요.

이렇게 하면 사용자 입장에서는 페이지가 느리게 느껴지지 않고, 필요한 콘텐츠가 준비되는 대로 자연스럽게 화면에 나타나도록 할 수 있답니다.

---

#### 간단 요약!

- 동적 API나 데이터베이스 호출이 있으면 정적 렌더링 못 함.
- 이럴 때는 `loading.js`나 `Suspense`로 로딩 상태 UX 챙기기!
- 꼭 정적 렌더링만 고집하지 말고, 상황에 맞게 스트리밍 방식도 활용해 보세요.

필요하면 더 자세한 예제도 만들어서 공유할게요. 궁금한 점 있으면 언제든 물어보세요!

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

## 버전 히스토리

| 버전           | 변경 사항                                                                                                   |
| -------------- | ---------------------------------------------------------------------------------------------------------- |
| `v15.2.0`      | 새로운 `position` 옵션을 추가해 화면 표시기가 더 좋아졌어요! 대신에 `appIsrStatus`, `buildActivity`, `buildActivityPosition` 옵션은 이제 더 이상 사용되지 않으니 참고하세요. |
| `v15.0.0`      | `appIsrStatus` 옵션을 통해 정적인(on-screen) 표시기가 새롭게 추가되었습니다.                                    |

버전 기록을 보면 기능이 점점 개선되어 가는 모습이 보이죠? 특히 화면에 표시되는 인디케이터 같은 UI 요소는 사용자의 편의성을 크게 증가시키니까 이런 업데이트는 반가운 소식이에요. 개발할 때는 deprecated 된 옵션들을 미리 확인하고 최신 옵션으로 바꿔주는 것이 나중에 문제를 줄이는 데 도움이 됩니다!