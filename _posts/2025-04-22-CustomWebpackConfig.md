---
title: "2025년 Next.js 15 맞춤형 Webpack 설정하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:33
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Custom Webpack Config"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/webpack"
isUpdated: false
---


# 커스텀 Webpack 설정하기

> 참고할 점: Webpack 설정을 변경하는 건 버전 규칙(semver)에 포함되지 않으니, 변경 시에 신중하게 진행하세요!

Next.js에 커스텀 Webpack 설정을 추가하기 전에, 먼저 Next.js가 이미 여러분의 요구사항을 기본적으로 지원하고 있는지 확인해보는 게 좋아요. 기본적으로 다음 기능들은 Next.js에서 지원하고 있습니다:

- CSS 가져오기 (CSS imports)
- CSS 모듈 (CSS modules)
- Sass/SCSS 가져오기 (Sass/SCSS imports)
- Sass/SCSS 모듈 (Sass/SCSS modules)

이 기능들은 Next.js에서 별도의 설정 없이도 바로 사용할 수 있어서, 간단한 스타일링 작업이라면 따로 Webpack 설정을 건드릴 필요가 없답니다.

만약 여러분이 좀 더 특별한 설정이 필요하거나, Next.js가 기본적으로 지원하지 않는 로더(loader)나 플러그인(plugin)을 추가하려는 경우에만 커스텀 Webpack 설정 작업을 하시면 돼요.  

그러니, Webpack 설정을 건드리기 전에 항상 공식 문서나 Next.js의 최신 지원사항을 꼭 먼저 살펴보세요! 그래야 불필요한 고민과 에러를 줄일 수 있습니다.  

추가로, Webpack 설정을 수정할 때는 프로젝트 전체 빌드에 영향을 주기 때문에 꼭 변경사항을 잘 테스트하고, 빌드 속도가 느려지는 부분도 체크해보시는 걸 추천드려요.

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

웹 개발할 때 자주 요청받는 기능들은 플러그인 형태로 제공되는 게 많아요. 예를 들어,

- `@next/mdx` : MDX 파일을 Next.js에서 바로 사용할 수 있게 해줍니다. (즉, Markdown과 JSX를 결합해서 쓸 수 있죠)
- `@next/bundle-analyzer` : 번들 크기를 시각화해줘서 성능 최적화에 도움을 줘요.

그리고 Next.js에서 기본적으로 쓰는 webpack 설정을 내가 원하는 대로 바꾸고 싶다면, `next.config.js` 파일 안에 webpack 설정 확장 함수를 넣으면 돼요. 아래처럼요:

```js
module.exports = {
  webpack: (
    config,
    { buildId, dev, isServer, defaultLoaders, nextRuntime, webpack }
  ) => {
    // 여기서 config를 원하는 대로 수정하세요

    // 수정한 config를 꼭 리턴해줘야 합니다.
    return config
  },
}
```

위 함수에서 `config`는 현재의 webpack 설정 객체고, 두 번째 인자에는 빌드 과정에 유용한 여러 정보가 담겨 있어요. 예를 들어 개발 모드인지 `dev`를 통해 알 수 있고, 서버사이드인지 `isServer`로 구분할 수도 있죠.

실제로 확장할 때 흔히 하는 작업 예를 들면:

- 특정 로더 추가하기 (예: 이미지, 스타일 등)
- 플러그인 추가하거나 교체하기
- 프로덕션 빌드에서 코드 압축 설정 변경하기 등

꼭 기억할 점은, 함수 마지막에 수정한 config를 반환해야만 원하는 대로 적용된다는 점이에요. 안 그러면 변경 사항이 무시됩니다.

이 부분만 잘 활용해도 Next.js의 번들링 과정을 유연하게 제어할 수 있으니 기회가 되면 한번 시도해보세요!

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

webpack 함수는 총 세 번 실행돼요. 서버 쪽에서 두 번(하나는 Node.js, 다른 하나는 Edge 런타임용), 그리고 클라이언트 쪽에서 한 번 이렇게요. 덕분에 isServer 속성을 활용해서 클라이언트와 서버용 webpack 설정을 쉽게 구분할 수 있답니다.

webpack 함수에 전달되는 두 번째 인자는 여러 정보들을 담은 객체인데, 주요 속성은 다음과 같아요:

| 속성명         | 타입                 | 설명                                                         |
| -------------- | -------------------- | ------------------------------------------------------------ |
| buildId        | String               | 빌드 간 고유 식별자로 사용돼요.                              |
| dev            | Boolean              | 현재 개발 모드인지 여부를 나타내요. (true면 개발 모드)         |
| isServer       | Boolean              | 서버 사이드 컴파일일 경우 true, 클라이언트 사이드 컴파일일 경우 false에요. |
| nextRuntime    | String \| undefined  | 서버 사이드 컴파일에서의 타겟 런타임 - "edge" 또는 "nodejs". 클라이언트 컴파일일 경우 undefined에요. |
| defaultLoaders | Object               | Next.js가 내부적으로 사용하는 기본 로더 설정입니다.          |

특히 defaultLoaders에는 babel 설정 같은 필수 로더들이 들어있어서, 커스텀 환경에서도 Next.js 기본 로더 구성을 참고하면서 손쉽게 webpack 구성을 변경할 수 있어요.

예를 들어, defaultLoaders.babel을 이렇게 사용할 수 있죠:

```js
module.exports = {
  webpack(config, { defaultLoaders }) {
    config.module.rules.push({
      test: /\.js$/,
      use: [
        defaultLoaders.babel, // 기본 babel-loader 설정을 이용
        {
          loader: 'my-custom-loader',
          options: { /* ... */ },
        },
      ],
    });
    return config;
  },
};
```

이처럼 Next.js의 기본 babel 설정을 활용하면, 직접 설정을 완전히 새로 만드는 것보다 훨씬 편리하고 안정적으로 빌드 구성을 확장할 수 있어요. 꼭 필요한 부분만 덧붙이면서 성능도 챙겨보세요!

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

자, 이번에는 Next.js에서 MDX 파일을 처리하기 위한 웹팩 설정 예제를 한번 살펴볼게요. 원본 코드는 @next/mdx 플러그인에서 따온 건데, MDX를 바벨로더와 함께 사용하는 예시입니다.

```js
// babel-loader에 의존하는 로더를 추가하는 예시 설정
module.exports = {
  webpack: (config, options) => {
    config.module.rules.push({
      test: /\.mdx/,  // .mdx 확장자를 가진 파일에 적용할 로더
      use: [
        options.defaultLoaders.babel,  // Next.js의 기본 babel 로더 사용
        {
          loader: '@mdx-js/loader',    // MDX 변환 로더 추가
          options: pluginOptions.options,  // 추가 옵션 지정 가능
        },
      ],
    });

    return config;
  },
};
```

여기서 중요한 점은, MDX 파일을 먼저 babel-loader가 변환하고, 그 다음에 @mdx-js/loader가 MDX 문법을 React 컴포넌트 코드로 바꾼다는 거예요.

> 참고로 `pluginOptions.options` 부분이 빠져 있다면, 직접 옵션을 지정하거나 빈 객체 `{}`로 대체해도 됩니다. 환경에 따라 다르니까 참고하세요.

---

#### nextRuntime과 isServer 플래그 관련 팁

그리고 문서 하단에 나온 `nextRuntime`에 관한 내용도 알아둬야 해요.

- `nextRuntime`이 `"edge"`나 `"nodejs"`일 때 `isServer`가 항상 `true`로 설정돼요.
- `"edge"` 런타임은 현재 미들웨어와 서버 컴포넌트(React Server Components)를 엣지 환경에서만 실행할 때 사용합니다.
- 이 부분은 Next.js가 런타임 환경을 구분해서 코드 실행 위치를 판단할 때 중요해요.

실제로 `"edge"` 환경은 지리적으로 가까운 서버에서 빠르게 응답하는 데 강점이 있으니, 미들웨어나 서버 컴포넌트 성능 최적화를 할 때 참고하시면 좋겠죠.

---

### 추가로 알려주는 팁!

Next.js 웹팩 설정 수정할 때는 종종 `.mdx`뿐만 아니라 `.md` 파일도 함께 처리할 일이 많아요. 만약 그런 경우라면 `test` 항목을 `/\.mdx?$/` 식으로 정규식 수정해서 한번에 처리하는 것도 추천합니다. 그리고 MDX를 쓸 때는 `@mdx-js/react` 패키지를 이용해서 MDX 내에서 React 컴포넌트를 자연스럽게 사용할 수 있으니, 프로젝트 요구에 맞춰 한번 적용해보세요.

---

필요하면 더 자세한 설정이나 MDX 활용법도 알려줄 수 있으니, 언제든 질문해 주세요!