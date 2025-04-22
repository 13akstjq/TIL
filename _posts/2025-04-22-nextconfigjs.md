---
title: "Next.js 15 next.config.js 기본 설정 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:59
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "next.config.js"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js"
isUpdated: false
---


# next.config.js 설정하기

Next.js는 프로젝트 최상단(예: package.json이 위치한 같은 폴더)에 `next.config.js` 파일을 만들어 다양한 설정을 할 수 있어요. 이 파일은 기본적으로 `module.exports`로 구성한 객체를 내보내는 형태입니다.

```js
// @ts-check
 
/** @type {import('next').NextConfig} */
const nextConfig = {
  /* 여기에 설정 옵션들을 적어주세요 */
}
 
module.exports = nextConfig
```

이렇게 하면 Next.js가 프로젝트를 빌드하거나 실행할 때 이 설정을 참조하게 되죠.

---

## ECMAScript 모듈 방식으로 작성하기

최근에는 Node.js가 ECMAScript Modules(ESM)을 더 잘 지원하면서, `next.config.js` 파일을 `ESM` 방식으로 작성하고 싶을 때도 있어요. 예를 들어 `export default`를 사용하는 식이죠.

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  /* 설정 작성 */
}

export default nextConfig
```

단, 이렇게 하려면 `package.json`에 `"type": "module"`을 명시하거나, `next.config.mjs`와 같이 `.mjs` 확장자를 사용해야 해요. 그렇지 않으면 여전히 CommonJS 형식(`module.exports`)을 사용해야 합니다.

### 참고로!  
- 기본적으로 Next.js는 CommonJS 방식을 선호하고, 대부분 예제도 이 방식을 따라요.  
- 만약 ESM 방식으로 작성했다면, Node.js 버전 호환과 프로젝트 설정을 꼼꼼히 확인하는 게 좋아요.  
- 또, 환경 변수 설정이나 커스텀 웹팩 등 고급 설정을 할 때 이 차이가 영향을 줄 수 있으니 주의하세요!

---

`next.config.js`는 Next.js 프로젝트를 내 입맛에 맞게 조정하는 아주 중요한 파일이에요. 설정 가능한 옵션들은 [공식 문서](https://nextjs.org/docs/api-reference/next.config.js/introduction)를 참고하면 더 다양한 기능을 활용할 수 있어요!

필요한 설정을 넣고 내 프로젝트를 더 똑똑하게 만들어봅시다! 🚀

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

next.config.js는 일반적인 Node.js 모듈이지 JSON 파일이 아니에요. 이 파일은 Next.js 서버와 빌드 단계에서 사용되고, 브라우저 빌드에는 포함되지 않아요.

만약 ECMAScript 모듈 방식을 사용하고 싶다면, next.config.mjs 파일을 쓸 수 있는데요, 이렇게 작성하면 됩니다:

```js
// @ts-check

/**
 * @type {import('next').NextConfig}
 */
const nextConfig = {
  /* 여기에 설정 옵션을 넣으면 돼요 */
}

export default nextConfig
```

> 참고로 좋은 정보! 현재 next.config 파일은 .cjs, .cts, .mts 확장자는 지원하지 않으니 꼭 .js 나 .mjs 확장자를 사용해야 해요.

추가로, next.config.js는 서버 사이드에서만 동작하기 때문에 브라우저에 환경 설정이 노출되지 않는다는 점이 큰 장점이에요. 그래서 보안에 민감한 설정이나 API 키 같은 걸 넣어도 안전하게 사용할 수 있답니다.

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

## 함수 형태로 설정하기

Next.js 설정을 할 때, 단순히 객체를 내보내는 것 말고도 함수를 사용해서 설정할 수 있어요. 이렇게 하면 환경에 따라 동적으로 설정값을 다르게 줄 수 있어서 유용하답니다.

```js
// @ts-check

export default (phase, { defaultConfig }) => {
  /**
   * @type {import('next').NextConfig}
   */
  const nextConfig = {
    /* 여기에 설정 옵션 작성 */
  }
  return nextConfig
}
```

예를 들어, 빌드 시점(phase)에 따라 다른 옵션을 주거나, 기본 설정(defaultConfig)을 참고할 수도 있어요.

### 비동기 설정도 가능해요!

여기까지 보면 그냥 동기 함수 같지만, async 함수로 만들 수도 있어요. 예를 들어 외부 API에서 설정 정보를 받아 와야 한다면, async/await를 이용해서 처리할 수 있답니다.

```js
export default async (phase, { defaultConfig }) => {
  // 예: 외부 API에서 설정 가져오기
  const externalSettings = await fetchSomeSettings()

  return {
    ...defaultConfig,
    ...externalSettings,
  }
}
```

이렇게 하면 설정 값도 API 호출 결과에 따라 유연하게 바꿀 수 있으니, 복잡한 환경에서도 활용하기 딱 좋겠죠?

---

다만, async 설정을 쓸 때는 빌드 시간에 네트워크 요청이 포함되니까 빌드 속도가 느려질 수 있어요. 꼭 필요한 경우에만 사용하는 게 좋아요!

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

Next.js 12.1.0 버전부터는 async 함수를 사용해서 설정 파일을 작성할 수 있어요. 예를 들어:

```js
// @ts-check

module.exports = async (phase, { defaultConfig }) => {
  /**
   * @type {import('next').NextConfig}
   */
  const nextConfig = {
    /* 여기에 설정 옵션을 작성하세요 */
  }
  return nextConfig
}
```

여기서 `phase`는 현재 설정이 로드되는 상황(컨텍스트)을 의미해요. 어떤 상황에서 설정이 호출되는지 구분해서 설정을 다르게 할 수 있죠. Next.js에서는 여러 가지 phase를 제공합니다.

그리고 이런 phase 값들은 `next/constants` 모듈에서 가져올 수 있어요. 예를 들어 이렇게요:

```js
const { PHASE_DEVELOPMENT_SERVER, PHASE_PRODUCTION_BUILD } = require('next/constants')
```

이걸 활용하면 특정 상황에서만 설정을 다르게 하거나, 개발 모드와 프로덕션 모드에 맞는 설정을 분리할 수 있답니다.

### Phase 종류 예시

| 상수                          | 의미                                   |
|----------------------------|--------------------------------------|
| `PHASE_DEVELOPMENT_SERVER` | 개발 서버 실행 시                       |
| `PHASE_PRODUCTION_BUILD`    | 프로덕션 빌드 시                      |
| `PHASE_PRODUCTION_SERVER`   | 프로덕션 서버 실행 시                 |
| `PHASE_EXPORT`              | 정적 HTML Export 수행 시               |

이렇게 상황에 따라 설정을 구분해놓으면 훨씬 유연하고 관리하기 쉬워져요. 예를들어 개발환경에선 디버깅 옵션을 켜고, 프로덕션에선 최적화 옵션을 켜는 식으로 말이죠.

만약 더 복잡한 설정을 원한다면, async 함수 내에서 API 호출이나 파일 읽기 같은 비동기 작업도 할 수 있어서 설정을 더 동적으로 만들어 줄 수 있어요. Next.js 12.1.0부터 이 기능 덕분에 한층 더 풍부한 설정 구성이 가능해졌답니다!

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

자, 오늘은 Next.js 설정 파일을 다루는 방법에 대해서 이야기해볼게요. Next.js를 사용하다 보면 각 환경(development, production 등)에 따라 다르게 설정하고 싶을 때가 있잖아요? 이럴 때 유용한 예제를 하나 가지고 왔어요.

### Next.js 설정 파일 예제 (JavaScript)

```js
// @ts-check

const { PHASE_DEVELOPMENT_SERVER } = require('next/constants')

module.exports = (phase, { defaultConfig }) => {
  if (phase === PHASE_DEVELOPMENT_SERVER) {
    return {
      /* 개발 서버 환경일 때만 적용할 설정을 여기에 작성하세요 */
    }
  }

  return {
    /* 개발 서버가 아닌 모든 환경에서 공통적으로 적용할 설정을 여기에 작성하세요 */
  }
}
```

자, 이 코드는 Next.js의 설정 파일인 `next.config.js`에서 주로 사용됩니다. `phase`라는 매개변수를 받아서 현재 실행 중인 환경을 판단할 수 있어요. 예를 들면, `PHASE_DEVELOPMENT_SERVER`는 로컬 개발 서버를 의미하니까 개발용 설정을 넣고, 그렇지 않은 경우엔 production 등 다른 환경 설정을 넣으면 된답니다.

> 참고로, `defaultConfig` 매개변수에는 Next.js가 기본으로 제공하는 설정 값들이 포함돼 있어 필요할 때 활용할 수 있어요.

---

그리고 요즘은 TypeScript를 많이 쓰니 Next.js 설정에도 TypeScript를 적용할 수 있어요. 그래서 `next.config.ts` 라는 파일을 사용하면 훨씬 더 타입 안정성을 갖추면서 편하게 작업할 수 있죠.

### Next.js 설정 파일 예제 (TypeScript)

```ts
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  /* 원하는 설정 옵션을 작성하세요 */
}

export default nextConfig
```

여기서 중요한 점은 `NextConfig` 타입을 임포트해서 사용하면, 지원하는 설정 옵션들을 타입으로 미리 확인하고 잘못된 설정을 방지할 수 있다는 점이에요.

---

### 추가 팁!

- `phase`가 다양한 값이 있으니 필요하면 `next/constants`에서 다른 상수들도 확인해보세요. (예: `PHASE_PRODUCTION_BUILD`, `PHASE_EXPORT`)
- 설정이 많아지면 환경 변수도 함께 관리하는 게 좋아요. `.env.local`, `.env.production` 같은 파일을 Next.js가 자동으로 읽어줍니다.
- Next.js 공식 문서에서는 환경에 따라 설정 분리하기 좋은 패턴이 많으니 한번 살펴보시면 좋아요.

혹시 더 궁금한 점 있으면 언제든지 물어봐 주세요! 개발하면서 이런 설정들은 꼭 익혀두면 나중에 디버깅도 수월해지고 프로젝트 관리도 편하답니다.

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

여기 주석 처리된 줄들은 next.config.js에서 허용하는 설정들을 넣을 수 있는 자리예요. 이 설정들은 이 파일에 정의되어 있죠.

하지만, 꼭 모든 설정을 알아야 하는 건 아니에요! 각 설정이 무슨 역할을 하는지를 다 이해할 필요 없이, 필요에 따라 원하는 기능을 이 부분에서 찾아보고 어떻게 하면 되는지 참고하면 돼요.

> 참고로, target Node.js 버전에 지원되지 않는 최신 JavaScript 기능은 사용하지 않는 게 좋아요. next.config.js 파일은 Webpack이나 Babel로 변환되지 않으니까요.

아래 페이지에서 next.config.js로 설정할 수 있는 모든 옵션들을 자세히 확인할 수 있어요:

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

## 단위 테스트 (실험적 기능)

Next.js 15.1부터는 `next/experimental/testing/server` 패키지에 next.config.js 파일을 단위 테스트할 수 있는 유틸리티가 포함되어 있어요.

여기서 `unstable_getResponseFromNextConfig` 함수는 next.config.js에 정의된 headers, redirects, rewrites 함수들을 실제 요청 정보와 함께 실행하고, 라우팅 결과를 담은 `NextResponse` 객체를 돌려줍니다.

> 다만, `unstable_getResponseFromNextConfig` 함수는 next.config.js에 정의된 필드만 고려해서 결과를 반환하기 때문에, 미들웨어(middleware)나 파일 시스템 라우트(filesystem routes)는 반영되지 않아요. 그래서 실제 프로덕션 환경에서의 동작과 테스트 결과가 약간 다를 수 있다는 점 기억하세요.

---

제가 좀 더 덧붙이자면, 앞으로 이런 실험적 기능들이 안정화되면, 별도의 E2E(End-to-End) 테스트 없이도 설정 파일을 좀 더 쉽고 빠르게 검증할 수 있을 것 같아요. 현재는 아직 불안정하니, 중요한 부분은 반드시 프로덕션 환경에서 추가 테스트를 하는 것을 추천합니다!

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

이번에 소개할 코드는 Next.js의 실험적 API를 활용해서 리다이렉트 설정을 테스트하는 방법에 관한 예제예요. Next.js 개발할 때 리다이렉트(redirects) 설정 잘 되어 있는지 테스트하고 싶다면 꽤 유용하게 쓸 수 있죠.

```js
import {
  getRedirectUrl,
  unstable_getResponseFromNextConfig,
} from 'next/experimental/testing/server'

const response = await unstable_getResponseFromNextConfig({
  url: 'https://nextjs.org/test',  // 테스트할 URL
  nextConfig: {
    async redirects() {
      return [{ source: '/test', destination: '/test2', permanent: false }]
    },
  },
})

expect(response.status).toEqual(307)  // 임시 리다이렉트인지 확인
expect(getRedirectUrl(response)).toEqual('https://nextjs.org/test2')  // 리다이렉트 목적지 확인
```

### 핵심 포인트
- `unstable_getResponseFromNextConfig`는 Next.js 설정(여기서는 `redirects`)에 따라 주어진 URL에 대한 응답 객체를 생성해줘요.
- `getRedirectUrl`은 응답 헤더 중 리다이렉트 위치(URL)를 쉽게 추출할 수 있게 도와줍니다.
- 여기서는 '/test'가 '/test2'로 임시 리다이렉트(307) 되는지 검사중이에요.

### 참고할 점
- 아직 `unstable_` 접두어가 붙어있으니 API가 안정화되지 않았고, 향후 바뀔 수도 있어요.
- 실제 프로젝트에서는 이런 테스트를 CI 파이프라인에 넣어두면 배포 전에 리다이렉트 설정 및 동작을 자동으로 검증할 수 있어 매우 편리하답니다.

### 마무리
이렇게 Next.js의 내부 설정을 직접 불러와서 테스트할 수 있는 방법이 있어서, 리다이렉트뿐 아니라 다른 설정도 비슷하게 점검할 수 있어요. 실험적인 API지만 앞으로 점점 정식화될 가능성이 크니 주목해 보세요! 

필요하면 `next.config.js` 내 redirects 설정 외에 rewrites, headers 등도 유사하게 검증할 수 있다는 점도 덧붙여 드립니다.