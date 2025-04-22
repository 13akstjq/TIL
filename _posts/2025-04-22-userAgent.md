---
title: "Nextjs 15 웹사이트에서 userAgent 쉽게 확인하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:58
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "userAgent"
link: "https://nextjs.org/docs/app/api-reference/functions/userAgent"
isUpdated: false
---


# userAgent 사용하기

Next.js에서 `userAgent` 헬퍼는 Web Request API를 확장해서, 들어오는 요청(request)에서 유저 에이전트(user agent) 정보를 쉽게 다룰 수 있도록 도와줘요. 예를 들어, 어떤 기기에서 접속했는지(device type)를 파악하는 데 아주 유용하죠.

```js
import { NextRequest, NextResponse, userAgent } from 'next/server'

export function middleware(request: NextRequest) {
  const url = request.nextUrl
  const { device } = userAgent(request)

  // device.type에는 'mobile', 'tablet', 'console', 'smarttv',
  // 'wearable', 'embedded' 또는 'desktop'(undefined일 때) 같은 값이 들어와요.
  const viewport = device.type || 'desktop'

  url.searchParams.set('viewport', viewport)
  return NextResponse.rewrite(url)
}
```

위 코드에서는 미들웨어 안에서 요청 객체 `request`를 이용해 `userAgent(request)`를 호출하고, 그 결과로부터 기기 타입(`device.type`)을 얻고 있어요. 그리고 이 정보를 쿼리 파라미터로 추가해서 나중에 라우팅이나 렌더링 시 조건문으로 활용할 수 있도록 했답니다.

---

## isBot 속성 활용하기

`userAgent` 헬퍼는 단순히 기기 타입뿐 아니라, 방문자가 봇(bot)인지 아닌지도 알려주는 `isBot` 속성도 제공해요. 이걸 활용하면, 봇이 방문할 때 SEO 최적화나 캐싱 전략을 다르게 적용하는 등의 스마트한 대응이 가능하죠.

예시를 들어볼게요:

```js
import { NextRequest, NextResponse, userAgent } from 'next/server'

export function middleware(request: NextRequest) {
  const { isBot } = userAgent(request)

  if (isBot) {
    // 봇 방문자라면, 예를 들어, 캐싱을 더 강하게 적용하거나 
    // 특정 페이지로 리다이렉트 할 수도 있어요.
    console.log('봇 방문 감지됨!')
  }

  return NextResponse.next()
}
```

이처럼 `isBot`은 크롤러나 검색 엔진 봇 감지에 유용하니, SEO를 신경 쓰는 프로젝트라면 꼭 활용해보세요.

---

### 참고: device.type 예상 값 정리

| device.type  | 설명                                      |
|--------------|-------------------------------------------|
| mobile       | 모바일 기기 (스마트폰 등)                 |
| tablet       | 태블릿 기기                              |
| console      | 게임 콘솔                                |
| smarttv      | 스마트 TV                               |
| wearable     | 스마트 워치 같은 웨어러블 디바이스       |
| embedded     | 내장형 디바이스                          |
| undefined    | 데스크탑 브라우저 (특별한 타입 없을 때) |

---

마지막으로, `userAgent` 헬퍼는 사용자 단말 정보 기반 맞춤 UI나 미디어 쿼리 없이 기기 특성 판별이 필요한 상황에서 정말 강력한 도구니까, Next.js 프로젝트를 다룰 때 많이 활용해보면 좋습니다!

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

요청이 알려진 봇(bot)인지 여부를 나타내는 불리언 값이 있어요.

## browser

요청에 사용된 브라우저에 대한 정보를 담고 있는 객체입니다.

- name: 브라우저 이름을 나타내는 문자열이에요. 만약 식별이 불가능하면 undefined가 될 수 있습니다.
- version: 브라우저 버전을 나타내는 문자열이며, 역시 알 수 없는 경우엔 undefined가 됩니다.

---

참고로, 클라이언트가 어떤 브라우저를 쓰는지 아는 건 다양한 이유에서 유용해요. 예를 들어, 특정 브라우저에서만 발생하는 버그를 파악하거나, 특정 버전에 맞게 기능을 조정하는 거죠. 

또, 사용자 경험을 개선하기 위해서도 브라우저 정보는 중요해요. 만약 너무 오래된 버전을 쓰는 사용자가 많다면, 최신 기능 지원 여부를 알 수 있으니까요.

Markdown 방식으로 표로 정리해봤어요!

| 속성명   | 설명                                 | 타입        | 비고                |
|---------|------------------------------------|------------|---------------------|
| isBot   | 요청이 알려진 봇인지 여부           | Boolean    | true 또는 false      |
| name    | 브라우저 이름                      | String     | 식별 불가 시 undefined |
| version | 브라우저 버전                      | String     | 식별 불가 시 undefined |

이렇게 정보를 잘 활용하면 API를 더 똑똑하게, 사용자에게 더 친절하게 만들 수 있답니다!

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

## device

요청에 사용된 기기에 대한 정보를 담고 있는 객체입니다.

| 속성명  | 설명                                                                                   |
|---------|----------------------------------------------------------------------------------------|
| model   | 기기의 모델명을 나타내는 문자열입니다. 값이 없을 경우 `undefined`일 수 있어요.             |
| type    | 기기 종류를 나타내는 문자열로, `console`, `mobile`, `tablet`, `smarttv`, `wearable`, `embedded` 중 하나거나 `undefined`일 수 있어요. |
| vendor  | 기기 제조사명을 표시하는 문자열입니다. 없으면 `undefined`가 될 수 있어요.                   |

기기 정보를 활용하면 어떤 환경에서 접속했는지 파악하기 쉽기 때문에 UI나 기능을 그에 맞게 최적화하기 좋아요. 예를 들어 스마트 TV와 모바일은 화면 크기 차이가 크니, 그에 맞게 인터페이스를 조절할 수 있겠죠?

## engine

다음은 `engine`에 대한 설명이 필요하다면 언제든 알려주세요! 기본적으로 `engine`은 브라우저나 플랫폼 동작의 핵심적인 렌더링 엔진 정보를 나타내는 경우가 많아요. 웹 개발 시엔 이 정보를 통해 브라우저 호환성을 체크하는 데 유용하죠.

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

브라우저 엔진(engine)에 대한 정보를 담고 있는 객체에 대해 조금 쉽게 설명해볼게요.

### 브라우저 엔진 정보 객체

이 객체는 우리가 사용하는 브라우저가 어떤 엔진을 쓰고 있는지 알려줘요. 엔진이라는 건 쉽게 말해, 웹페이지를 해석하고 화면에 보여주는 소프트웨어 핵심 부분이에요.

- **name**: 이건 엔진의 이름을 문자열로 알려줘요. 예를 들어 Amaya, Blink, EdgeHTML, Gecko, WebKit 등 꽤 다양한 엔진들이 있고, 어떤 브라우저가 어느 엔진을 쓰는지 판단할 수 있죠.
- **version**: 그리고 그 엔진의 버전도 문자열로 알려주고, 가끔은 undefined일 수도 있어요.

브라우저 엔진을 알면, 웹 개발할 때 특정 엔진에서 잘못 표시되거나 동작하는 문제를 잡는데 도움이 돼요. 예를 들면, 구글 크롬은 Blink 엔진, 사파리는 WebKit 엔진을 쓰는 식이에요.

---

### OS (운영체제) 정보 객체

이 객체는 컴퓨터나 모바일 기기에서 어떤 운영체제를 쓰고 있는지 알려줘요. OS 정보는 웹사이트를 최적화하거나, 특정 OS에서만 작동하는 기능을 설정할 때 쓰이죠.

---

표로 정리하면 이렇게 될 거예요.

| 속성    | 설명                                                                                                     |
|---------|--------------------------------------------------------------------------------------------------------|
| name    | 브라우저 엔진 이름 (예: Amaya, Blink, EdgeHTML, Gecko, WebKit 등)                                      |
| version | 엔진 버전 문자열, 없으면 undefined                                                                     |
| os      | 운영체제 정보 객체                                                                                      |

필요하면 OS 정보에 대해서도 더 자세히 다뤄볼게요. 실제로 개발할 때는 userAgent 라이브러리나 플랫폼 감지 API를 통해 이런 정보를 쉽게 얻을 수 있죠.

필요하면 내가 사용해본 라이브러리도 추천해줄게요. 혹시 궁금하면 말해주세요!

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

자, 오늘은 운영체제(OS)와 CPU에 대한 정보를 담고 있는 데이터 구조에 대해 얘기해볼게요. 개발하면서 시스템 정보를 처리해야 할 때 종종 마주치는 내용인데, 이해하기 쉽게 정리해볼게요.

---

### 운영체제 정보 (OS)

- `name`: 운영체제의 이름을 문자열로 나타내요. 예를 들어, 'Windows', 'Linux', 'macOS' 같은 것들이죠. 만약 정보가 없으면 undefined일 수 있어요.
- `version`: 운영체제의 버전을 문자열로 나타내요. 예를 들어 '10.0.19042' 같은 숫자나 'Big Sur'처럼 이름 형태가 될 수도 있죠. 역시 없으면 undefined일 수도 있어요.

이렇게 이름과 버전을 알면 해당 운영체제에 맞는 분기 처리를 하거나 업데이트 정보를 관리하는 데 유용하죠.

---

### CPU 정보

CPU 정보는 보통 CPU 아키텍처 *architecture*에 초점을 맞춥니다. 

| 필드          | 설명                                   | 예시                                                   |
|--------------|--------------------------------------|--------------------------------------------------------|
| architecture | CPU 아키텍처를 나타내는 문자열입니다. | 68k, amd64, arm, arm64, armhf, avr, ia32, ia64, mips 등 |

여기서 가능한 아키텍처 값들은 꽤 다양합니다. 흔히 개발할 때 많이 듣는 건 `amd64` (x86-64), `arm64` (애플 M1/M2, 최신 모바일 기기), `ia32` (x86 32비트), 그리고 `arm` (주로 모바일용) 정도가 있죠.

---

#### 추가 팁!

- CPU 아키텍처와 OS 정보를 함께 알면, 예를 들어 어느 환경에서 특정 바이너리가 돌아가는지 정확히 파악할 수 있어요.
- 웹 개발할 때도 클라이언트나 서버의 환경을 파악해서 최적화 또는 디버깅하는 데 도움이 됩니다.
- Node.js 환경에서는 `process.arch` (CPU 아키텍처)랑 `process.platform` (운영체제) 같은 내장 변수를 활용해 비슷한 정보를 얻을 수 있어요.

---

필요할 때 이 정보를 잘 활용해서 더 똑똑한 환경별 로직을 작성해 보세요. 그럼 오늘 내용은 여기까지!