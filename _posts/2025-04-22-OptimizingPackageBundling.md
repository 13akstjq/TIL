---
title: "Next.js 15에서 패키지 번들링 최적화하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:04
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Optimizing Package Bundling"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/package-bundling"
isUpdated: false
---


# 패키지 번들링 최적화하기

외부 패키지를 번들링하면 애플리케이션의 성능을 크게 향상시킬 수 있어요. Next.js에서는 기본적으로 Server Components와 Route Handlers 안에서 임포트한 패키지들을 자동으로 번들링해줍니다. 이번 글에서는 패키지 번들링을 어떻게 분석하고, 더 최적화할 수 있는지 알려드릴게요.

## 자바스크립트 번들 분석하기

`@next/bundle-analyzer`는 Next.js에서 사용 가능한 플러그인으로, 애플리케이션 번들의 크기를 관리하는 데 도움을 줍니다. 이 플러그인은 각 패키지와 그 의존성들의 크기를 시각적으로 보여주는 리포트를 만들어줘서, 큰 크기의 패키지를 찾아내거나 코드 스플리팅, 혹은 지연 로딩(lazy loading)을 적용하는 데 활용할 수 있죠.

### 추가 팁!

- 번들 크기가 예상보다 크다면, 대체할 수 있는 가벼운 라이브러리가 있는지 찾아보세요.
- 필요하지 않은 패키지는 과감히 삭제하는 것도 중요합니다.
- 코드 스플리팅을 활용해 페이지별로 필요한 코드만 로드하는 것도 좋은 방법이에요.

필요하다면 `package.json`에 아래 명령어를 추가해서 쉽게 번들 분석을 실행할 수 있습니다.

```json
"scripts": {
  "analyze": "cross-env ANALYZE=true next build"
}
```

그리고 터미널에서 `npm run analyze` 또는 `yarn analyze` 명령어를 실행하면, 번들 분석 리포트가 뜹니다.

패키지 번들 사이즈를 잘 관리하면 로딩 속도가 빨라지고 사용자 경험이 훨씬 좋아지니 꼭 신경 써보세요!

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

### 설치 방법

플러그인을 설치하려면 아래 명령어 중 하나를 터미널에 입력하세요:

```bash
npm i @next/bundle-analyzer
# 또는
yarn add @next/bundle-analyzer
# 또는
pnpm add @next/bundle-analyzer
```

그다음, `next.config.js` 파일에 번들 분석기 설정을 추가해주면 됩니다.

---

참고로, `@next/bundle-analyzer`는 Next.js 프로젝트에서 번들 크기를 시각적으로 확인할 수 있게 도와줘서, 최적화 작업할 때 정말 유용한 도구예요. 어떤 파일이 용량이 큰지, 어디에 리소스를 많이 쓰는지 쉽게 파악할 수 있거든요.

`next.config.js`에 설정하는 예시는 다음과 같아요:

```js
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true',
});

module.exports = withBundleAnalyzer({
  // 기존 Next.js 설정들...
});
```

실행할 때 `ANALYZE=true` 환경 변수를 주면 번들 분석기가 활성화되고, 그렇지 않으면 비활성화되어 평상시에는 빌드 속도에 영향을 주지 않아요.

이를 통해 필요할 때만 번들 분석기를 켜고, 평상시에는 쾌적한 개발 환경을 유지할 수 있습니다!

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

이번에는 Next.js 프로젝트에서 번들 사이즈를 쉽게 분석할 수 있는 방법을 알려드릴게요. 번들러 분석기를 붙여서, 빌드 결과물을 시각적으로 확인할 수 있게 해주는 도구인데요. 다음과 같이 `@next/bundle-analyzer` 패키지를 활용하면 됩니다.

먼저, `next.config.js` 파일에 아래처럼 설정을 추가해 주세요:

```js
/** @type {import('next').NextConfig} */
const nextConfig = {}

const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true',
})

module.exports = withBundleAnalyzer(nextConfig)
```

여기서 핵심은 `ANALYZE` 환경 변수를 `true`로 설정해서, 번들 분석 기능을 켜는 거예요. 실제 빌드를 할 때만 분석기가 동작하도록 설정해서, 평소에는 성능에 영향이 없도록 해 줍니다.

분석 보고서를 보려면, 아래 명령어 중 하나를 내리면 끝!

```bash
ANALYZE=true npm run build
# or
ANALYZE=true yarn build
# or
ANALYZE=true pnpm build
```

이렇게 하면, 빌드가 완료된 후에 `.next` 폴더 안에 번들 분석 리포트가 생성되는데요, 기본적으로는 브라우저에서 시각적인 트리맵 형태를 확인할 수 있어요.

---

### 조금 더 팁!

- 번들 분석기를 쓰는 이유는 무엇일까요? 프로젝트가 커질수록 예상치 못한 패키지가 번들에 포함되거나, 너무 큰 라이브러리가 들어가서 페이지 로딩 속도가 느려질 수 있어요. 이걸 시각적으로 확인하고 최적화 방향을 잡을 수 있답니다.
- `withBundleAnalyzer`처럼 Next.js는 여러가지 플러그인 형태의 설정을 감싸서 사용할 수 있어요. 만약 다른 기능을 같이 쓰는 중이라면 `compose` 형태로 합쳐주는 방법도 찾을 수 있습니다.
- 만약 자동으로 보고서를 열고 싶다면, 추가 설정을 주거나 별도의 스크립트를 작성해도 좋습니다.

이제 내 프로젝트 번들이 얼마나 크고, 어디에 무거운 부분이 있는지 손쉽게 분석해 보세요! 개발할 때 정말 큰 도움이 될 거예요.

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

보고서는 브라우저에서 세 개의 새 탭을 열어서 확인할 수 있어요. 이렇게 주기적으로 애플리케이션의 번들 파일을 평가하는 습관은 시간이 지나도 앱 성능을 잘 유지하는 데 큰 도움이 된답니다.

## 패키지 임포트 최적화하기

예를 들어 아이콘 라이브러리 같은 패키지는 수백 개의 모듈을 한꺼번에 내보내서, 개발 환경이나 실제 서비스 환경 모두에서 성능 저하를 일으킬 수 있어요.

그럴 때는 next.config.js 파일에 `optimizePackageImports` 옵션을 추가해보세요. 이 설정을 하면 실제로 사용하는 모듈만 로딩해서 불필요한 코드가 빠져나가거든요. 게다가 여전히 여러 개의 네임드 익스포트를 사용하는 것처럼 깔끔하게 import 문을 작성할 수 있어서, 개발할 때도 편리하답니다.

---

추가로, 이 방법은 번들 크기를 줄여 로딩 속도를 개선할 뿐 아니라, 특히 대형 프로젝트에서 빌드 시간도 단축시켜주니까 꼭 적용해보길 권해요!

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

Next.js 설정에서 `optimizePackageImports`와 `serverExternalPackages` 옵션에 대해 알아볼게요.

---

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    optimizePackageImports: ['icon-library'],
  },
}

module.exports = nextConfig
```

위 코드는 `experimental.optimizePackageImports`에 `'icon-library'`라는 특정 라이브러리를 넣어둔 모습인데요, 이 옵션은 Next.js가 이 라이브러리를 임포트할 때 최적화하도록 지시하는 역할을 해요.

### 자동 최적화되는 라이브러리가 있다?!  
사실 Next.js는 기본적으로 자주 쓰이는 몇몇 라이브러리들을 알아서 최적화해주고 있어서, 모든 라이브러리를 `optimizePackageImports`에 적어줄 필요는 없답니다. 예를 들어 React, lodash 등 자주 쓰이는 라이브러리는 이미 최적화 대상에 포함되어 있어서 따로 추가 안 해도 된다구요.  
> [Next.js 공식 문서](https://nextjs.org/docs/pages/building-your-application/configuring/optimizing-packages#opt-in-to-automatic-package-import-optimization)에서 최적화 대상 라이브러리 ‘풀 리스트’를 확인할 수 있어요. 

### 서버 사이드에서 번들링 제외하기  

Next.js에서는 **서버 컴포넌트(Server Components)** 와 **라우트 핸들러(Route Handlers)** 내부에서 임포트한 패키지들은 기본적으로 번들링되어 배포돼요. 근데 가끔씩은, 특정 패키지를 번들링 대상에서 제외하고 싶을 때가 있습니다. 이럴 때는 `serverExternalPackages` 옵션을 사용하면 되는데요! 예를 들면 이렇게요:

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    serverExternalPackages: ['some-server-only-package'],
  },
}

module.exports = nextConfig
```

`serverExternalPackages`에 등록한 패키지는 서버 번들에서 제외되고, 런타임에 직접 Node.js 환경에서 불러와 사용하게 됩니다. 이러면 서버 번들 크기를 좀 더 가볍게 유지하거나, 예를 들어 네이티브 모듈이나 빌드 시점에 번들링 불가능한 라이브러리를 다룰 때 도움이 돼요.

---

### 정리하자면  

| 설정 옵션               | 역할                                                         |
|------------------------|------------------------------------------------------------|
| `experimental.optimizePackageImports` | 특정 클라이언트 라이브러리를 import 최적화하도록 설정        |
| `experimental.serverExternalPackages` | 서버 컴포넌트, 라우트 핸들러 번들에서 제외할 패키지를 지정함   |

이 옵션들은 `experimental`이 붙어있는걸로 보아 아직 완전히 안정화된 기능은 아니지만, 프로젝트에 맞게 적절히 활용하면 번들 크기 최적화에 꽤 도움이 될 수 있어요.

---

Next.js로 개발하면서 번들 최적화에 관심이 많다면 꼭 참고해보시고, 직접 적용해서 번들 크기 변화를 한 번 살펴보세요! 최적화가 잘 되면 페이지 로딩 속도와 사용자 경험이 훨씬 개선될 거예요. :)

필요하면 더 자세한 설정법이나 실전 예제도 공유해드릴게요!

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

안녕하세요 여러분! 오늘은 Next.js에서 `serverExternalPackages` 옵션에 대해 간단히 이야기해보려고 해요. 

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  serverExternalPackages: ['package-name'],
}

module.exports = nextConfig
```

위 코드처럼 `next.config.js`에 `serverExternalPackages`에 외부 패키지 이름을 명시해주면, Next.js 서버 쪽에서 해당 패키지를 외부 모듈로 처리하도록 설정할 수 있어요. 

이 옵션은 서버에서만 사용하는 패키지나, Next.js가 기본적으로 번들링하지 않는 패키지를 명시할 때 유용합니다. 특히 서버 사이드에서 동작하는 Node.js 패키지를 사용할 때 문제가 발생할 수 있는데, 이때 `serverExternalPackages`에 해당 패키지를 추가해주면 해결되곤 하죠.

> 참고로 Next.js가 이미 호환성을 검증하고 자동으로 제외 처리하는 인기 패키지 목록이 따로 있어요. 여러분이 직접 추가하기 전에 [Next.js 공식 문서](https://nextjs.org/docs)에서 지원하는 패키지 리스트를 확인해보는 게 좋아요. 이렇게 하면 중복 설정을 줄일 수 있거든요!

---
추가로 팁을 드리자면, 서버 외부 패키지를 추가할 때는 버전 호환성도 꼭 체크하세요. Next.js가 업데이트되면서 패키지 호환성도 변화할 수 있습니다. 그리고 외부 패키지를 너무 많이 추가하면 번들링 최적화가 어려워질 수 있으니 꼭 필요한 패키지만 추가하는 걸 추천해드려요.

필요하면 Next.js의 Webpack 설정을 커스텀해서 좀 더 세밀한 번들링 조정도 가능합니다. 나중에 기회되면 Webpack 설정과 함께 리뷰해볼게요!

궁금한 점 있으면 댓글 남겨주세요~ 개발 재미있게 함께 해봐요! 🚀