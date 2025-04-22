---
title: "Next.js 15 Turbopack 번들러 사용법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:29
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "turbopack"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/turbopack"
isUpdated: false
---


# Turbopack 설정하기

안녕하세요! 오늘은 Next.js에서 **Turbopack** 옵션을 커스터마이징하는 방법에 대해 이야기해볼게요. Turbopack은 파일 변환 방법이나 모듈 해석 방식을 직접 설정할 수 있게 해주는데요, 덕분에 프로젝트에 맞게 빌드 과정을 세밀하게 조절할 수 있어요.

기본적인 설정 예시는 다음과 같아요:

```js
import type { NextConfig } from 'next'
 
const nextConfig: NextConfig = {
  turbopack: {
    // 여기서 Turbopack 커스터마이징 옵션들을 설정합니다.
  },
}
 
export default nextConfig
```

### 알아두면 좋은 점!
Turbopack은 Next.js에 기본 내장된 기능들이 많아서 별도의 로더(loader) 설정 없이도 잘 돌아간다는 점이 큰 장점이에요.

- CSS 처리나 최신 자바스크립트 컴파일을 기본적으로 지원해서,
- 따로 `css-loader`, `postcss-loader` 혹은 `babel-loader` 같은 로더를 설치하거나 설정할 필요가 없어요.
- 특히 `@babel/preset-env`를 쓰는 경우라면 추가 설정 없이도 최신 문법을 문제없이 빌드할 수 있다는 뜻이죠.

이게 왜 좋은지 궁금하다면, 보통 Webpack으로 직접 셋업할 때 이 로더들 때문에 설정이 엄청 복잡해질 수 있는데 Turbopack은 그런 번거로움을 줄여준다고 생각하면 돼요.

### 추가로!
Turbopack은 아직 발전 중인 빌드 도구라서, 만약 기존 Webpack 환경에서 옮겨온다면 일부 옵션들은 다르게 동작할 수 있어요. 문서와 커뮤니티를 수시로 체크해서 최신 변경사항을 따라가는 걸 추천합니다!

혹시 Turbopack 설정에 대해 더 궁금한 점이나 도움이 필요하다면 편하게 물어봐 주세요. 함께 공부해봐요!

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

### 옵션들

turbo 설정에서 사용할 수 있는 옵션들은 다음과 같아요:

| 옵션               | 설명                                                                 |
|--------------------|----------------------------------------------------------------------|
| `root`             | 애플리케이션의 루트 디렉토리를 설정해요. 절대 경로여야 합니다.       |
| `rules`            | Turbopack을 사용할 때 적용할 지원되는 webpack 로더들의 리스트입니다. |
| `resolveAlias`     | 별칭으로 된 import들을 실제 로드할 모듈로 매핑해요.                   |
| `resolveExtensions`| 파일을 import할 때 해결할 확장자 리스트를 지정해요.                   |

---

여기서 `root` 옵션은 프로젝트가 어디서부터 시작하는지 알려주는 역할을 해요. 보통 절대 경로를 줘야 하니까, 예를 들어 `/Users/username/projects/my-app` 같은 식으로요.

`rules`는 webpack에서 익숙한 부분인데, Turbopack에서도 비슷하게 특정 파일 유형에 대해 어떤 로더(예: Babel, 스타일 로더 등)를 적용할지 설정하는 거예요.

`resolveAlias`는 자주 쓰이는 경로나 모듈 이름에 별칭을 붙여서 import할 때 좀 더 간편하게 하기 위한 옵션입니다. 예를 들어, `@components`를 `src/components`로 매핑하면 import 구문이 훨씬 깔끔해지죠.

마지막으로 `resolveExtensions`는 파일 확장자를 굳이 다 붙이지 않고도 import할 수 있게 해줍니다. 예를 들어, `index.js`, `index.tsx` 등이 있을 때 `.js`, `.tsx` 등 원하는 확장자를 배열로 넣으면 편리하죠.

이렇게 옵션을 설정해두면 Turbopack을 좀 더 유연하고 효율적으로 사용할 수 있습니다!

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

### 지원하는 로더(Loaders)

Turbopack의 webpack 로더 구현과 함께 테스트를 마친 로더들은 다음과 같습니다:

| 로더 이름             | 설명                                  |
|-----------------------|-------------------------------------|
| babel-loader          | 최신 자바스크립트를 구형 브라우저에서도 돌아가게 변환해주는 로더          |
| @svgr/webpack         | SVG 아이콘을 React 컴포넌트로 변환해주는 로더                        |
| svg-inline-loader     | SVG를 인라인으로 불러올 수 있게 해주는 로더                       |
| yaml-loader           | YAML 파일을 자바스크립트 객체로 변환해서 불러올 수 있는 로더          |
| string-replace-loader | 문자열 치환 작업을 빌드 과정에서 처리해주는 로더                      |
| raw-loader            | 파일을 문자열 그대로 불러올 때 사용하는 로더                         |
| sass-loader           | SASS/SCSS 파일을 CSS로 변환해주는 로더                           |

이렇게 자주 쓰이는 로더들은 Turbopack 환경에서도 안정적으로 작동한다는 점, 개발자 입장에서 참 반갑죠? 특히 Babel, SASS 같이 거의 필수적인 로더들이 지원되니까 프론트엔드 개발에 큰 걱정 없이 사용할 수 있습니다.

---

## 예제 코드들 (Examples)

직접 적용해볼 수 있는 예제들도 함께 살펴보시면 이해가 훨씬 쉬워질 거예요. 앞으로 다양한 예제들을 차근차근 소개해드릴 예정이니 기대해주세요!

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

### 루트 디렉토리

Turbopack은 모듈을 해석할 때 **루트 디렉토리(root directory)** 를 기준으로 삼아요. 즉, 프로젝트 루트 밖에 있는 파일들은 모듈로 인식하지 않아서 불러오지 못해요.

여기서 중요한 점은, Next.js가 **자동으로 프로젝트의 루트 디렉토리**를 찾아준다는 것입니다. Next.js는 아래와 같은 파일들 중 하나가 있는 위치를 루트 디렉토리로 인식하는 거죠:

| 파일 이름           |
|---------------------|
| pnpm-lock.yaml      |
| package-lock.json   |
| yarn.lock           |
| bun.lock            |
| bun.lockb           |

예를 들어, `package-lock.json`이 있는 폴더가 바로 루트 디렉토리로 설정돼서, 이 위치를 기준으로 모듈 경로나 파일 경로를 해석하게 됩니 다.

---

### 덧붙여서!

- 만약 이 파일들이 없는 상태라면, Next.js가 루트 디렉토리를 제대로 인식하지 못할 수 있으니, 항상 패키지 매니저 별 락 파일 중 하나 정도는 프로젝트에 꼭 넣어두는 게 좋아요.
- 터보팩(Turbopack)은 특히 빠른 빌드 환경을 제공하는 모듈 번들러이기 때문에, 프로젝트 구조가 깔끔하게 정리되어야 원하는 퍼포먼스를 낼 수 있어요.
- 그리고 만약 루트 밖에 있는 파일을 참조해야 한다면, 그 파일을 프로젝트 내부로 옮기거나 심볼릭 링크를 활용하는 방법도 한 번 고려해보세요!

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

프로젝트 구조가 다르거나, 예를 들어 워크스페이스(workspaces)를 사용하지 않는 경우 root 옵션을 수동으로 설정할 수 있어요:

```js
const path = require('path')
module.exports = {
  turbopack: {
    root: path.join(__dirname, '..'),
  },
}
```

이렇게 하면 Turbopack이 프로젝트의 루트 디렉터리를 정확히 인식할 수 있답니다. 프로젝트 구조가 독특할 때 꼭 기억해두세요!

---

### webpack 로더 설정하기

Turbopack은 기본적으로 다양한 기능을 내장하고 있지만, 때로는 webpack 로더 지원이 필요할 때가 있죠. 다행히도 이미 많은 webpack 로더들이 Turbopack과 호환됩니다. 다만, 현재는 약간의 제한 사항들이 있으니 참고가 필요해요.

예를 들어, 최신 웹 기술이나 특정 파일 형태에 대해 완벽하진 않지만 꾸준히 개선되고 있으니 필요에 따라 적절히 사용해보면 좋습니다. 혹시 여러분이 사용하는 로더가 제대로 동작하지 않는다면 공식 문서나 커뮤니티에서 업데이트 상황을 확인해보는 것도 추천해요!

추가로, Turbopack은 성능을 최우선으로 설계되어 있기 때문에 일부 복잡한 webpack 로더들은 완벽하게 지원하지 않을 수 있어요. 그럴 땐 가능하면 Turbopack 전용 플러그인이나 기능을 활용하는 걸 고민해보세요. 미래에는 더 많은 로더와 플러그인이 원활히 지원될 예정이니 기대해도 좋습니다!

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

webpack 로더 API는 아직 핵심 기능만 구현된 상태예요. 현재는 인기 있는 로더 몇 가지를 지원하지만, 앞으로 더 많은 API를 확장할 계획입니다. 그리고 한 가지 중요한 점! JavaScript 코드를 반환하는 로더만 지원하고 있어서, 스타일시트나 이미지 같은 파일을 변환하는 로더는 아직 사용할 수 없어요.

또한, webpack 로더에 옵션을 전달할 때는 순수한 자바스크립트 기본 자료형(primitive), 객체, 배열만 허용됩니다. 예를 들어 `require()`를 통해 플러그인 모듈을 옵션 값으로 넘기는 건 지원하지 않아요.

로더를 설정하려면 `next.config.js` 파일에 설치한 로더 이름과 옵션을 추가하면 됩니다. 여기서는 파일 확장자별로 어떤 로더들을 적용할지 매핑해 주는 거죠.

아래 예시는 `.svg` 파일을 `@svgr/webpack` 로더를 사용해 React 컴포넌트로 불러올 수 있게 설정한 코드예요.

```js
module.exports = {
  turbopack: {
    rules: {
      '*.svg': {
        loaders: ['@svgr/webpack'],
        as: '*.js',
      },
    },
  },
}
```

즉, 위 설정을 하면 프로젝트에서 `.svg` 파일을 그냥 import해서 JSX 안에서 컴포넌트처럼 쓸 수 있어 정말 편리하죠! 이런 식으로 필요에 따라 다른 로더들도 설정해서 사용할 수 있으니, 앞으로 로더 API가 확장되면 더 다양한 파일 처리도 가능해질 것 같아요.

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

> 참고할 점: Next.js 13.4.4 버전 이전까지는 turbo.rules가 turbo.loaders라는 이름이었고, *.mdx 같은 파일 확장자는 지원하지 않고 .mdx와 같이 특정 확장자만 받았다는 점이에요.

### 별칭(Alias) 해결하기

Turbopack은 모듈 해석 방식을 별칭(alias)을 통해 커스터마이징할 수 있어요. 웹팩(webpack)의 resolve.alias처럼 동작한다고 생각하면 이해하기 쉬워요.

별칭을 설정하려면 next.config.js 파일에서 import 패턴을 원하는 경로로 매핑해주면 됩니다. 예를 들어, 특정 경로를 더 간단하거나 명확한 이름으로 바꿔 불러올 수 있죠.

```js
// next.config.js
module.exports = {
  experimental: {
    turbopack: true,
  },
  turbopack: {
    resolve: {
      alias: {
        '@components': './src/components',
        '@utils': './src/utils',
      },
    },
  },
};
```

이렇게 설정하면 코드 내에서 `@components/Button`처럼 경로를 간단히 쓸 수 있어요. 별칭을 잘 활용하면 경로 관리가 훨씬 편해지고, 프로젝트가 커져도 유지보수가 훨씬 수월해집니다.

> TIP
> 별칭을 설정할 때는 vscode 같은 편집기에서 path alias가 인식되도록 `jsconfig.json`이나 `tsconfig.json`에도 동일한 별칭을 설정해주면 개발 경험이 더 좋아져요!

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
module.exports = {
  turbopack: {
    resolveAlias: {
      underscore: 'lodash', // underscore 패키지를 lodash로 대체
      mocha: { browser: 'mocha/browser-entry.js' }, // 브라우저 환경에서는 mocha를 mocha/browser-entry.js로 대체
    },
  },
}
```

위 설정은 `underscore`를 불러올 때 실제로는 `lodash` 패키지를 쓰도록 바꾸는 거예요. 그러니까, `import underscore from 'underscore'`라고 적어도 내부적으로는 `lodash`가 불러와지는 거죠.

재밌는 점은 Turbopack이 Node.js의 조건부 익스포트(conditional exports)처럼 조건에 따라 별도의 aliasing을 지원한다는 거예요. 현재는 'browser' 조건만 지원하는데, 예를 들어 위 설정에서는 브라우저 환경일 때 `mocha`를 `mocha/browser-entry.js`로 바꿔서 불러오도록 해준 거죠.

---

### 커스텀 확장자 해결하기 (Resolving custom extensions)

Turbopack에서 자주 만나는 문제 중 하나가 커스텀 확장자를 가진 파일을 제대로 인식하지 못하는 경우예요. 예를 들어 `.jsx`나 `.ts` 같은 확장자를 직접 명시하지 않고 import할 때 자동으로 찾아내야 하는 상황이 있죠.

Turbopack에서는 `resolve` 옵션을 통해 확장자 우선순위를 지정할 수 있어요. 예를 들어 아래처럼 쓸 수 있습니다:

```js
module.exports = {
  turbopack: {
    resolve: {
      extensions: ['.tsx', '.ts', '.jsx', '.js'],
    },
  },
}
```

이렇게 하면 파일을 import할 때 확장자를 생략해도 Turbopack이 순서대로 확장자를 붙여가며 찾아줍니다. 예를 들어 `import Component from './MyComponent'` 할 때 `MyComponent.tsx`, `MyComponent.ts` 등을 찾는 식이죠.

---

추가 팁!  
요즘 프로젝트에서는 다양한 파일 확장자를 다루다 보니, alias와 resolve 설정을 잘 조합하는 게 생산성을 크게 높여줘요. 특히, 라이브러리 교체(aliasing)와 타입스크립트+JSX 지원(resolve extensions)을 같이 하면 깔끔하고 빠른 빌드 환경을 만들 수 있답니다.

궁금한 점 있으면 언제든 물어봐 주세요!

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

Turbopack에서도 webpack의 resolve.extensions 설정처럼 모듈을 불러올 때 사용하는 파일 확장자를 커스텀할 수 있어요.

설정을 바꾸려면 next.config.js 파일에 `resolveExtensions` 필드를 추가해주면 됩니다. 예를 들어:

```js
module.exports = {
  turbopack: {
    resolveExtensions: ['.mdx', '.tsx', '.ts', '.jsx', '.js', '.mjs', '.json'],
  },
}
```

위처럼 설정하면 기본 확장자 목록이 해당 리스트로 완전히 바뀌니까, 기존에 자주 쓰던 확장자들(`.js`, `.json` 등)을 꼭 포함시켜야 해요. 안 그러면 의도치 않게 모듈이 제대로 해석되지 않을 수 있거든요.

추가로, 이런 설정들은 주로 TypeScript, MDX 같은 파일들을 함께 사용할 때 유용해요. 예를 들어 TSX 파일도 같이 처리하거나, 마크다운 확장자인 MDX를 빌드에 포함시키고 싶을 때 말이죠.

참고로, 확장자 순서도 중요할 수 있는데, Turbopack이 왼쪽부터 차례대로 확장자를 탐색해서 가장 먼저 매치되는 모듈을 사용하니까, 자주 쓰거나 우선순위가 높은 확장자를 앞쪽에 배치하는 게 좋아요!

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

더 자세한 내용과 Webpack에서 Turbopack으로 앱을 마이그레이션하는 방법에 대한 가이드는 Turbopack의 Webpack 호환성 문서를 참고하세요.

## 버전 히스토리

| 버전       | 변경사항                          |
|------------|---------------------------------|
| 15.3.0     | `experimental.turbo`가 `turbopack`으로 변경됨.  |
| 13.0.0     | `experimental.turbo` 기능 도입됨.               |

여기서 한 가지 팁! Turbopack은 Next.js 팀에서 개발한 매우 빠른 번들러인데요, 기존 Webpack보다 훨씬 빌드 속도와 HMR(Hot Module Replacement) 성능이 뛰어나서 점점 더 대세로 떠오르고 있어요. 물론 아직 실험적인 기능이 많으니, 프로젝트에 바로 적용하는 것보단 충분히 테스트해보는 걸 추천합니다!