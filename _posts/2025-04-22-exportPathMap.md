---
title: "Nextjs 15에서 exportPathMap로 정적 페이지 경로 설정하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:07
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "exportPathMap"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/exportPathMap"
isUpdated: false
---


# exportPathMap 알아보기

> 참고로 이 기능은 next export에서만 사용 가능하고, 현재는 pages의 getStaticPaths나 app의 generateStaticParams로 대체되어 점점 사용이 줄어드는 추세에요.

exportPathMap은 내보낼 때 요청 경로(request paths)를 페이지(destination)와 매핑해주는 기능입니다. 이걸 설정하면, next export를 할 때 원하는 경로나 이름으로 페이지를 만들 수 있어요. 그리고 exportPathMap에 정의된 경로는 `next dev` 모드에서도 똑같이 사용할 수 있답니다.

그럼 간단한 예제로 시작해볼게요! 다음과 같은 페이지들이 있는 앱에서 커스텀 exportPathMap을 만들어볼 거예요:

---

예를 들어, 다음과 같은 페이지들이 있다고 가정해봐요:

- /about
- /blog/post1
- /blog/post2

이제 exportPathMap을 어떻게 설정해야 할까요?

---

```js
// next.config.js

module.exports = {
  exportPathMap: async function (
    defaultPathMap,
    { dev, dir, outDir, distDir, buildId }
  ) {
    return {
      '/': { page: '/' },
      '/about-us': { page: '/about' }, // 원하는 경로로 매핑 가능
      '/blog/post-1': { page: '/blog/post1' },
      '/blog/post-2': { page: '/blog/post2' },
    }
  },
}
```

---

여기서 `/about-us`가 실제 페이지 경로가 아니라 `/about` 페이지로 이동하게 해주고 있어요. 이렇게 하면 내보낸 정적 사이트에서 URL을 좀 더 커스터마이징할 수 있죠.

---

### 추가로 알려드릴 팁!

- exportPathMap은 페이지가 많거나 동적으로 경로를 여러 개 만들어야 할 때 유용합니다.
- 하지만 Next.js가 공식적으로 권장하는 방식은 `getStaticPaths` 또는 `generateStaticParams`를 활용하는 거예요. 이 함수들은 페이지 내부에서 직접 경로를 정의하고, 자동으로 정적 HTML을 생성해주기 때문에 관리하기 훨씬 편하답니다.
- 앞으로 새 프로젝트라면 exportPathMap보단 `getStaticPaths`(pages 디렉토리)나 `generateStaticParams`(app 디렉토리) 방식을 먼저 고려해보세요!

필요하다면 exportPathMap 활용법이나 위 함수들에 대해 더 자세히 소개해드릴게요. 편하게 질문해주세요!

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

Next.js에서 여러 페이지를 정적으로 내보내고 싶을 때 `exportPathMap` 설정을 사용해요. 예를 들어, 다음과 같이 `pages/index.js`, `pages/about.js`, `pages/post.js` 파일이 있다고 가정할게요.

여기서 `next.config.js`에 다음과 같은 `exportPathMap`을 추가하면:

```js
module.exports = {
  exportPathMap: async function (
    defaultPathMap,
    { dev, dir, outDir, distDir, buildId }
  ) {
    return {
      '/': { page: '/' },
      '/about': { page: '/about' },
      '/p/hello-nextjs': { page: '/post', query: { title: 'hello-nextjs' } },
      '/p/learn-nextjs': { page: '/post', query: { title: 'learn-nextjs' } },
      '/p/deploy-nextjs': { page: '/post', query: { title: 'deploy-nextjs' } },
    }
  },
}
```

이 설정은 빌드할 때 `/`, `/about`, `/p/hello-nextjs`, `/p/learn-nextjs`, `/p/deploy-nextjs` 경로들을 각각 대응되는 페이지에 연결해 줘요. 

하지만 여기서 주의할 점이 있어요! `query`를 이용한 파라미터 전달은 `next export` 환경에서는 제한적이에요. 왜냐하면 Next.js에서 자동으로 정적 최적화된 페이지나 `getStaticProps`를 사용하는 페이지들은 HTML로 미리 렌더링되기 때문에, 빌드 후에 쿼리 정보를 추가로 넘길 수 없거든요. 즉, `exportPathMap`에서 `query`를 쓰려면 페이지에서 `getInitialProps`를 사용하고 있어야 합니다.

---

### 추가 팁!

- **동적 라우팅과 정적 내보내기**: Next.js 9 버전부터는 `[param]` 형태의 동적 라우팅 지원과 함께 `getStaticPaths`로 동적 경로를 만들어 낼 수 있어요. 이 방법이 요즘은 더 권장돼서 `exportPathMap` 대신 사용되곤 합니다.
- **`exportPathMap` 사용 예시**: 구버전이나 Next.js의 `next export`를 꼭 써야 하는 경우에 유용해요.
  
---

### 간단 정리

| 설정 항목               | 설명                                                         |
|---------------------|------------------------------------------------------------|
| exportPathMap       | 내보낼 경로와 페이지 매핑을 정의하는 함수                            |
| query               | 각 경로에 전달할 쿼리 파라미터 (※ `getInitialProps`와 함께 사용 가능) |
| 자동 정적 최적화       | `getStaticProps` 등과 함께 빌드 타임에 HTML이 생성되는 방식                |
| 제한 사항             | 자동 정적 최적화 페이지에서 `query` 못 씀                            |

---

이렇게 설정해 두면 정적 사이트 생성 시 원하는 경로가 자동으로 만들어져서, 별도의 서버 없이도 깔끔하게 서비스할 수 있어요. 만약 Next.js로 정적 웹사이트를 만들 예정이라면, `exportPathMap`와 `getStaticProps`/`getStaticPaths` 차이점은 꼭 알아두시는 게 좋아요!

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

페이지들은 HTML 파일로 내보내지게 돼요. 예를 들어, `/about` 페이지는 `/about.html` 파일로 변환되는 식이죠.

여기서 중요한 게 `exportPathMap`이라는 비동기 함수인데요, 이 함수는 두 개의 인자를 받아요. 

- 첫 번째 인자는 `defaultPathMap`으로, Next.js가 기본적으로 사용하는 경로 맵이에요.
- 두 번째 인자는 객체 형태로 전달되는데, 이 안에는 다음과 같은 값들이 들어있답니다:

| 필드명  | 설명                                                                                          |
|---------|---------------------------------------------------------------------------------------------|
| dev     | `exportPathMap`가 개발 모드에서 호출될 때는 `true`, 즉 실제 `next export` 실행 시에는 `false`에요. 개발 모드에선 이 함수로 라우트를 정의해요. |
| dir     | 프로젝트 디렉토리의 절대 경로입니다.                                                         |
| outDir  | `out/` 디렉토리의 절대 경로로, `-o` 옵션으로 설정할 수 있어요. `dev`가 `true`일 땐 `null`이에요.  |
| distDir | `.next/` 디렉토리의 절대 경로로, `distDir` 설정으로 조정 가능해요.                            |
| buildId | 생성된 빌드 아이디입니다.                                                                    |

이 함수가 반환하는 건 페이지들의 맵 객체예요. 여기서 키(key)는 페이지의 경로명(pathname)이고, 값(value)은 객체인데요, 이 객체는 아래와 같은 필드를 받을 수 있답니다:

(아래는 대표적인 필드 예시입니다.)

| 필드명  | 설명                                                       |
|---------|------------------------------------------------------------|
| page    | 렌더링할 실제 페이지 경로, 예를 들어 `/about` 같은 경로명    |
| query   | 라우트에 필요한 쿼리 파라미터들을 객체 형태로 넣을 수 있어요   |

---

**팁 하나!**  
`exportPathMap`을 잘 활용하면 동적 라우트도 정적으로 내보낼 수 있어요. 예를 들어 블로그 글마다 페이지를 만드려면, 동적으로 글 목록을 불러와서 각각 `/posts/post1.html`, `/posts/post2.html` 이런 식으로 경로를 지정해 주는 거죠! 그래서 Next.js를 사용할 때 완전한 정적 사이트를 만드는 데 꼭 필요한 함수라고 볼 수 있어요.  

`next.config.js` 파일에서 이 함수 정의하는 걸 잊지 마세요!

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

- page: String - pages 디렉터리 내부에서 렌더링할 페이지입니다.
- query: Object - prerendering 시 getInitialProps에 전달되는 쿼리 객체입니다. 기본값은 ''입니다.

> 내보낸 경로명(pathname)은 파일명(예: /readme.md)도 가능합니다. 하지만 만약 .html과 다른 확장자라면, 콘텐츠를 제공할 때 Content-Type 헤더를 text/html로 설정해줘야 할 수도 있어요.

## 트레일링 슬래시 추가하기

Next.js에서 페이지를 index.html 파일로 내보내고, URL 끝에 슬래시(/)를 붙이도록 설정할 수 있어요. 예를 들어, /about 페이지는 /about/index.html로 내보내지고, 사용자는 /about/ 경로로 접근할 수 있게 됩니다. 이 방법은 Next.js 9 이전 버전의 기본 동작이었답니다.

---

좀 더 설명하자면, 트레일링 슬래시를 사용하는 이유는 SEO 최적화나 서버 설정에 따라 URL 끝에 슬래시를 붙이는 것이 더 깔끔하거나 일관된 주소 체계를 유지하는 데 도움을 준다는 점이에요. Next.js에서 이 옵션을 켜려면 `next.config.js`에서 `trailingSlash: true`를 설정하면 됩니다!

```js
// next.config.js
module.exports = {
  trailingSlash: true,
}
```

이렇게 하면, 모든 경로가 `/about/`처럼 슬래시가 붙은 형태로 자동 리다이렉트되거나 정적 파일이 생성돼서 편리하게 사용할 수 있답니다!

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

Next.js에서 trailing slash(끝에 슬래시) 설정하는 법과 출력 디렉토리 바꾸는 방법에 대해 알려드릴게요.

---

### 1. trailing slash 추가하기

보통 URL 끝에 슬래시(/)가 있냐 없냐에 따라 SEO나 서버 설정에서 차이가 나기도 하죠. Next.js에서는 쉽게 설정할 수 있어요.

`next.config.js` 파일을 열고 아래처럼 `trailingSlash` 옵션을 `true`로 설정하면, 모든 페이지의 URL 끝에 자동으로 슬래시가 붙어요.

```js
module.exports = {
  trailingSlash: true,
}
```

이렇게 하면 `/about` 대신 `/about/`처럼 URL이 처리됩니다. 

> 참고로, trailing slash를 사용하는 게 좋은지 아닌지는 프로젝트 상황에 따라 다르니, 팀이나 서버 환경에 맞춰 선택하세요!

---

### 2. 출력 디렉토리(custom output directory) 변경하기

Next.js의 `next export` 명령어를 쓰면 정적 사이트를 만들 수 있는데, 기본 출력 폴더가 `out`이에요. 그런데, 다른 폴더를 쓰고 싶으면 `-o` 옵션을 사용해서 바꿀 수 있답니다.

예를 들어,

```bash
next export -o build
```

이렇게 하면 빌드 결과물이 `build` 폴더에 생성돼요.

---

| 기능                           | 기본 설정    | 변경 방법                              |
|------------------------------|------------|-------------------------------------|
| URL 끝에 trailing slash 추가하기 | 없음          | `next.config.js`에 `trailingSlash: true` 설정 |
| 출력 디렉토리 변경              | `out`       | `next export -o <원하는폴더명>`          |

---

앞으로 정적 사이트나 URL처리가 조금더 헷갈릴 때 참고하시면 좋을 것 같아요! 필요하면 `next.config.js`에서 다른 설정들도 찾아보면서 내 프로젝트에 맞게 잘 조절해보세요~

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

`next export -o outdir` 명령어는 Next.js에서 정적 사이트를 생성(export)하면서 결과물을 `outdir` 폴더에 저장하는 역할을 해요. 쉽게 말해, Next.js 프로젝트를 정적인 HTML 파일로 뽑아내서 별도의 서버 없이도 웹사이트를 운영할 수 있게 해주는 거죠.

그런데 여기서 경고 메시지가 하나 나왔네요.

> Warning: Using exportPathMap is deprecated and is overridden by getStaticPaths inside pages. We don`t recommend using them together.

이 메시지를 풀어보면, `exportPathMap` 옵션은 이제 구식(deprecated)이 되었고, `pages` 폴더 내에서 사용되는 `getStaticPaths` 함수가 그 역할을 대신한다는 뜻이에요. 그리고 둘을 동시에 사용하는 건 권장되지 않아요.

### 간단 요약

| 용어            | 설명                                                         |
|-----------------|--------------------------------------------------------------|
| `exportPathMap` | 과거에 정적 경로를 직접 정의하기 위해 쓰던 함수인데, 이제 deprecated돼요. |
| `getStaticPaths` | Next.js 9.3부터 도입된 함수로, 다이나믹 라우팅을 위한 경로 생성을 담당해요. |

---

### 왜 `getStaticPaths`를 써야 할까?

`exportPathMap`은 종종 경로를 직접 하드코딩하거나 한 곳에서 한꺼번에 경로를 정의하는 방식이라 유연성이 떨어지고, 최신 Next.js의 기능들과 잘 안 맞아요.

반면, `getStaticPaths`는 페이지별로 경로를 선언할 수 있어서 다이나믹 라우팅과 SSG(Static Site Generation)를 아주 자연스럽게 지원합니다.

---

### 정리하면서 한 가지 팁!

- **새 프로젝트**를 시작하거나 **Next.js 9.3 이상**을 쓴다면, `getStaticPaths`와 `getStaticProps` 조합을 랜딩 페이지나 다이나믹 라우팅 페이지에 적용해보세요.
- 이미 `exportPathMap`으로 작업 중인 프로젝트가 있다면, 점차 `getStaticPaths` 쪽으로 마이그레이션 하는 게 좋습니다.
- 그리고 `next export` 명령을 쓸 때는, `getStaticPaths`로 생성한 경로가 잘 포함되는지 꼭 확인하세요.

---

혹시 더 궁금한 점이 있으면 언제든 물어봐 주세요! Next.js로 멋진 사이트 만드세요 :)