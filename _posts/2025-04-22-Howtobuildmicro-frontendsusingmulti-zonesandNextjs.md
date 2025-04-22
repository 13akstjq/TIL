---
title: "Next.js와 Multi-Zones를 활용한 마이크로 프론트엔드 구축 방법 (2025 최신)"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:35
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to build micro-frontends using multi-zones and Next.js"
link: "https://nextjs.org/docs/app/guides/multi-zones"
isUpdated: false
---

# Multi-Zones와 Next.js로 마이크로 프론트엔드 만들기

안녕하세요! 오늘은 'Multi-Zones'라는 개념을 활용해 Next.js로 마이크로 프론트엔드를 구축하는 방법에 대해 이야기해볼게요. '마이크로 프론트엔드'는 큰 애플리케이션을 여러 작은 애플리케이션으로 쪼개 관리하는 방법인데, Multi-Zones 방식은 특히 도메인 내에서 각기 다른 경로 집합(Path Set)별로 Next.js 애플리케이션을 분리하는 거랍니다.

## Multi-Zones가 왜 필요할까요?

대형 웹사이트를 운영하다보면, 서로 관련 없는 페이지들이 한 곳에 모여있을 때가 많아요. 예를 들어, 블로그 섹션과 로그인 후 대시보드, 그리고 일반 사용자용 페이지를 한 앱에서 모두 관리한다면, 빌드 시간이 길어지고 불필요한 코드도 함께 묶이게 됩니다. Multi-Zones를 적용하면 이런 문제를 이렇게 해결할 수 있어요:

- 각 영역(Zone)별로 Next.js 앱을 따로 배포해 빌드 타임을 줄이고,
- 각 앱에 필요한 코드만 포함해 번들 사이즈를 최소화하며,
- 완전히 분리된 앱이므로 서로 다른 프레임워크도 혼용할 수 있습니다. 예를 들어, 메인 페이지는 Next.js, 블로그 섹션은 React SPA, 혹은 Vue.js 앱으로 분리할 수 있죠.

## 예제를 들어볼까요?

만약 다음과 같은 페이지 세트를 가지고 있다면:

- `/blog/*` : 모든 블로그 포스트
- `/dashboard/*` : 로그인 후 대시보드 관련 페이지
- `/*` : 여기에 포함되지 않은 나머지 일반 페이지

각각을 별도의 Next.js 애플리케이션으로 분리해서 관리하면, 훨씬 유연하고 관리하기 좋은 구조가 됩니다.

---

이렇게 분리된 애플리케이션들은 서로 독립적으로 개발 및 배포 가능하니, 팀별로 담당 영역 개발에 집중하기에도 좋고, 필요에 따라 기술 스택도 다르게 유지할 수 있는 점도 장점이에요.

다음에는 이런 각각의 Zone을 설정하고 라우팅하는 방법, 그리고 배포 시 어떻게 연동하는지 자세한 설정법을 알아볼게요!

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

Multi-Zones 기능을 활용하면, 같은 도메인에서 서비스되면서 사용자 입장에서는 똑같이 보이는 세 개의 애플리케이션을 만들 수 있어요. 하지만 각 애플리케이션은 독립적으로 개발하고 배포할 수 있다는 점이 큰 장점이죠.

![Multi-Zones 예시 이미지](/TIL/assets/img/2025-04-22-Howtobuildmicro-frontendsusingmulti-zonesandNextjs_0.png)

여기서 재미있는 부분은 '존(zone)' 단위로 페이지 이동할 때의 동작이에요.

- 같은 존 내에서 페이지를 이동하면 **소프트 네비게이션(soft navigation)**이 일어나는데, 이때는 페이지를 새로 고침하지 않고 빠르게 이동이 가능해요. 그림 예시에서 `/`에서 `/products`로 이동하는 경우가 바로 이 소프트 네비게이션에 해당하죠.
- 반대로, 한 존에서 다른 존으로 이동할 때는 **하드 네비게이션(hard navigation)**이 발생해요. 즉, 현재 페이지의 리소스를 언로드(해제)하고 새로운 페이지 리소스를 다시 로드하는 작업이 이뤄지는 거죠. 예를 들어 `/`에서 `/dashboard`로 이동하는 경우가 여기에 포함돼요.

이 점을 잘 활용하려면, 자주 함께 방문하는 페이지들을 같은 존에 배치하는 게 좋아요. 그래야 하드 네비게이션이 불필요하게 발생하지 않아 사용성도 높아지고, 로드 시간도 줄어들 테니까요.

---

추가로, 이런 Multi-Zones 구조는 마이크로 프론트엔드 아키텍처에 아주 적합해요. 즉, 대규모 애플리케이션을 여러 개의 작은 프론트엔드 애플리케이션으로 분할해서 팀별로 독립적 개발과 배포가 가능하게 만들어 주거든요. Next.js를 사용하면 이러한 Multi-Zones 지원을 비교적 쉽게 구현할 수 있으니, 관심 있는 분들은 공식 문서나 관련 튜토리얼도 함께 참고해보길 추천합니다!

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

## 존(zone) 정의하는 방법

존(zone)이란, 보통의 Next.js 애플리케이션과 비슷한데, 각 존마다 충돌을 방지하기 위해 `assetPrefix`라는 설정을 추가하는 걸 말해요. 이걸로 페이지나 정적 파일들이 다른 존과 겹치지 않게 관리할 수 있답니다.

예를 들어, `assetPrefix`를 `/blog-static`으로 설정하면 이렇게 작성해요:

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  assetPrefix: "/blog-static",
};
```

그럼 Next.js가 로드하는 JavaScript, CSS 같은 에셋들은 이 `assetPrefix`를 붙여서 `/blog-static/_next/...` 경로로 제공돼요. 이렇게 하면 여러 존에서 동일한 `_next` 폴더를 공유할 때 생길 수 있는 충돌 문제를 자연스럽게 해결할 수 있죠.

### 참고로 알아두면 좋은 점!

- `assetPrefix` 설정을 할 때는 실제로 배포 환경에서 이 경로가 올바르게 매핑되는지 꼭 확인하세요. 예를 들면, CDN이나 웹서버 설정이 `/blog-static` 경로의 요청을 제대로 처리해야 합니다.
- 만약 로컬에서 개발할 때도 이 프리픽스를 쓰면 경로 이슈가 생길 수 있으니, 보통 개발 모드에선 `assetPrefix`를 빈 문자열로 두고, 프로덕션 빌드에서만 적용하는 방법도 있어요.

이렇게 하면 한 서버 내 여러 개의 Next.js 서비스(존)를 안정적으로 운영할 수 있으니, 멀티 테넌트 환경이나 여러 애플리케이션을 한 곳에서 관리할 때 유용해요!

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

Next.js에서 기본 애플리케이션, 즉 다른 특정 zone으로 라우팅되지 않는 경로를 처리하는 애플리케이션은 따로 assetPrefix를 지정할 필요가 없어요.

참고로, Next.js 15 버전 이전에는 정적 자산(static assets)을 제대로 처리하기 위해 추가적인 rewrites가 필요했었는데요, Next.js 15부터는 이런 작업이 더 이상 필요하지 않답니다.

예를 들어, 아래처럼 assetPrefix를 지정하고, rewrites를 통해 정적 자산 경로를 다시 매핑해주는 코드가 있었어요.

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  assetPrefix: "/blog-static",
  async rewrites() {
    return {
      beforeFiles: [
        {
          source: "/blog-static/_next/:path+",
          destination: "/_next/:path+",
        },
      ],
    };
  },
};
```

이렇게 하면 `/blog-static` 경로 아래의 `_next` 정적 파일들이 제대로 처리되도록 해줬죠.

---

### 요청을 올바른 zone으로 라우팅하는 방법

Next.js를 여러 zone으로 나눠서 서비스할 때, 요청이 적절한 zone으로 잘 전달되도록 라우팅 설정이 필요해요. 이를 통해 각 zone별로 개별 Next.js 앱을 독립적으로 운영하면서도 마치 하나의 서비스처럼 보이게 할 수 있습니다.

일반적으로 다음과 같은 방식으로 라우팅을 구현합니다:

| 요청 경로 (Request Path) | 라우팅 대상 (Destination Zone)        | 비고                         |
| ------------------------ | ------------------------------------- | ---------------------------- |
| `/blog`, `/blog/*`       | 블로그 zone 애플리케이션              | blog 관련 콘텐츠 제공        |
| `/shop`, `/shop/*`       | 쇼핑 zone 애플리케이션                | 쇼핑몰 서비스 제공           |
| 기타                     | 기본 zone (기본 Next.js 애플리케이션) | 전체 서비스의 기본 응답 처리 |

이때 각 zone 애플리케이션은 자신만의 assetPrefix를 가지고 있을 수 있고, 이전 Next.js 버전에서는 정적 자산 때문에 추가 rewrites 설정이 필요했지만, Next.js 15부터는 이런 설정 없이도 assetPrefix만으로 처리 가능해졌습니다.

---

#### 끝으로, 제가 경험해본 팁 하나!

Next.js 앱을 멀티 zone으로 구성할 때는 배포와 라우팅 규칙을 명확히 정리하는 게 중요해요. 특히 CDN 캐싱 등과 연동할 때는 assetPrefix를 잘 활용해서 정적 자산들이 꼬이지 않도록 관리하는 걸 추천합니다. 그래야 배포 발란스도 맞고, 사용자 경험도 좋아진답니다!

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

Multi Zone 환경에서는 각 경로(path)를 올바른 존(zone)으로 라우팅해줘야 해요. 왜냐하면 서로 다른 존은 각기 다른 애플리케이션에서 서비스되기 때문이죠. 보통 HTTP 프록시를 사용해서 이 작업을 하곤 하는데, Next.js 애플리케이션 중 하나를 이용해 도메인 전체의 요청을 라우팅하는 방법도 있어요.

그럼 Next.js에서 어떻게 라우팅을 할 수 있을까요? 바로 rewrites 기능을 사용하는 거예요. 존마다 서비스하는 경로가 다르면, 그 경로를 해당 존의 도메인으로 보내는 rewrite 규칙을 추가해주면 됩니다. 예를 들어, `/blog` 경로는 블로그 존에서 처리한다면 아래처럼 쓸 수 있어요.

```js
async rewrites() {
  return [
    {
      source: '/blog',
      destination: `${process.env.BLOG_DOMAIN}/blog`,
    },
    {
      source: '/blog/:path+',
      destination: `${process.env.BLOG_DOMAIN}/blog/:path+`,
    }
  ];
}
```

여기서 중요한 건 `destination`이 실제 요청을 처리하는 존의 완전한 URL(스킴 + 도메인)을 포함해야 한다는 점이에요. 예를 들어 `https://blog.example.com/blog` 같은 주소가 될 거예요. 프로덕션 환경에선 당연히 존의 실제 도메인을 넣으면 되고, 로컬 개발 중일 땐 `http://localhost:3001/blog` 같은 식으로 로컬 주소로도 라우팅할 수 있습니다.

추가로, rewrites는 클라이언트가 요청한 URL을 실제 내부적으로 다른 경로나 도메인으로 바꿔주는 기능이라서, 사용자가 URL을 바꾸지 않고도 다른 서비스에 요청을 보낼 수 있다는 장점이 있어요. 이런 방식으로 멀티 존 아키텍처를 구성하면 관리가 비교적 편하고, 각 존별로 독립된 애플리케이션을 운영하면서도 하나의 도메인 아래에서 자연스럽게 서비스할 수 있습니다.

정리하면:

| 역할        | 설명                                          |
| ----------- | --------------------------------------------- |
| source      | 사용자가 요청하는 경로                        |
| destination | 실제 서비스하는 존의 URL (스킴 + 도메인 포함) |

이렇게 쓰고, 필요한 경로마다 반복해서 추가해주면 돼요.

Next.js 자체가 라우팅 기능이 뛰어나기는 한데, 멀티 존 환경에서는 이런 rewrite 기능을 활용하면 훨씬더 유연하게 운용할 수 있어서 꼭 한 번 적용해 보시길 추천드립니다!

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

> 참고할 점: URL 경로는 각 존(zone)마다 고유해야 해요. 예를 들어, 두 존 모두 /blog 경로를 사용하려고 하면 라우팅 충돌이 발생할 수 있어요.

### 미들웨어를 활용한 라우팅 요청 처리

요청을 라우팅할 때 `rewrites`를 사용하는 게 지연 시간(latency)을 줄이는 데 추천되지만, 상황에 따라 동적으로 라우팅 결정을 해야 할 때는 미들웨어를 쓸 수도 있어요. 예를 들어, 마이그레이션 기간에 특정 경로의 라우팅을 기능 플래그(feature flag)로 제어하고 싶을 때 미들웨어가 아주 유용하답니다.

아래는 그런 경우를 위한 간단한 미들웨어 예시예요:

```js
export async function middleware(request) {
  const { pathname, search } = request.nextUrl;
  if (pathname === "/your-path" && myFeatureFlag.isEnabled()) {
    return NextResponse.rewrite(`${rewriteDomain}${pathname}${search}`);
  }
}
```

여기서 `myFeatureFlag.isEnabled()`는 기능 플래그가 활성화됐는지 체크하는 함수고, 활성화되면 `/your-path` 경로로 들어오는 요청을 `rewriteDomain`으로 재작성(리라이트)해서 넘겨줘요.

---

추가로, 미들웨어를 너무 남발하면 오히려 성능 저하가 발생할 수 있으니, 꼭 필요한 경우에만 활용하는 게 좋아요. 그리고 미들웨어가 실행될 때는 서버에서 처리되므로, 요청 지연이 생길 수 있답니다. 따라서 단순 경로 변경이 목적이라면 rewrites 설정을 우선 고려해보세요!

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

## 존(zone) 간 링크 연결하기

서로 다른 존에 있는 경로(path)로 연결할 때는 Next.js의 `Link` 컴포넌트 대신 일반 HTML의 `<a>` 태그를 사용하는 게 좋아요. 그 이유는 Next.js의 `Link` 컴포넌트는 상대 경로에 대해 사전에 데이터를 불러오고 부드럽게 페이지를 이동하는 동작(prefetch 및 soft navigation)을 시도하는데, 이 방식이 존을 넘나들 때는 제대로 작동하지 않거든요.

즉, 존 간 이동은 그냥 `<a href="...">` 태그를 써서 새로운 페이지를 불러오는 방식을 써야 버그 없이 쓸 수 있어요.

---

## 코드 공유하기

여러 존을 구성하는 Next.js 애플리케이션들은 각기 다른 저장소(repository)에 있을 수도 있고, 한 저장소(monorepo)에 함께 모아 둘 수도 있어요. 개인적으로는 모노레포 구조가 코드 공유나 유지보수 면에서 훨씬 편리하다고 생각해요.

만약 존이 각기 다른 레포에 흩어져 있다면, 공용(public) 또는 사설(private) NPM 패키지 형태로 코드를 공유하는 방법도 있어요. 예를 들어, 공통 유틸리티 함수나 컴포넌트들을 NPM 패키지로 만들어서 필요할 때마다 설치해서 사용하는 거죠.

사실 개발하는 팀이나 프로젝트 규모에 따라 적합한 방식을 선택하면 되지만, 모노레포를 사용한다면 VSCode 같은 도구에서 한 번에 코드 검색도 되고, 빌드하고 테스트하기도 훨씬 수월해진다는 장점이 커요.

---

### 추가로 알아두면 좋은 팁

- **모노레포 관리 툴**: `Nx`, `Turborepo`, `Lerna` 같은 도구를 활용하면 여러 애플리케이션과 패키지를 효과적으로 관리할 수 있어요.
- **버전 관리**: 공유 NPM 패키지를 사용할 땐, 버전 관리를 잘 해야 하며, SemVer(유의적 버전관리)를 따르는 게 좋아요.
- **빌드 속도**: 모노레포에서는 캐싱과 병렬 빌드를 적극 활용해서 빌드 속도를 개선할 수 있어요.

이렇게 존 간 연동과 코드 공유를 잘 설계하면 여러 팀과 프로젝트가 협업하기 훨씬 수월해지니 한 번쯤은 이런 구조도 고민해보시길 추천드립니다!

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

페이지들이 서로 다른 존(zone)에 걸쳐 있고 각각 출시되는 시기가 다를 수 있어서, 기능 플래그(feature flags)를 사용하면 다양한 존에서 기능을 동시에 켜거나 끌 때 굉장히 유용해요.

특히 Vercel에서 Next.js 애플리케이션을 운영할 때는, 여러 존(zone)에 영향을 주는 코드를 한 번의 git 푸시로 배포하기 위해 모노레포(monorepo)를 사용하는 방법도 추천드려요. 이렇게 하면 관리가 훨씬 편해지고 배포 과정도 깔끔해집니다.

---

## 서버 액션(Server Actions)

Multi-Zones 환경에서 서버 액션을 사용할 때는 사용자 앞단(origin)을 명시적으로 허용해줘야 해요. 왜냐하면 사용자 도메인이 여러 애플리케이션을 서빙할 수 있기 때문인데요. 이를 위해 `next.config.js` 파일에 다음과 같은 설정을 추가해주면 됩니다:

```javascript
module.exports = {
  experimental: {
    serverActions: true,
  },
  // 사용자 도메인을 명시적으로 허용합니다.
  serverActions: {
    allowedOrigins: ["https://user-facing-domain.com"],
  },
};
```

> 참고로, `allowedOrigins` 설정은 사용자의 브라우저에서 오는 요청이 신뢰할 수 있는 도메인에서 발생한 것인지 체크하는 역할을 합니다. 이걸 정확하게 설정해주지 않으면, 보안 문제가 발생할 수 있으니 꼭 필요한 도메인만 넣어주세요!

이 외에도 multi-zone 환경에서 서버 액션을 제대로 활용하려면 CORS 설정이나 인증 관련 부분도 꼼꼼하게 챙기는 게 좋아요. 개발할 때 미리 여러 존의 도메인 구조를 파악해두면 배포와 운영이 훨씬 수월해질 거예요.

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

Next.js에서 `serverActions.allowedOrigins` 설정에 대해 알아볼게요!

```js
const nextConfig = {
  experimental: {
    serverActions: {
      allowedOrigins: ["your-production-domain.com"],
    },
  },
};
```

이 설정은 Next.js가 실험적으로 도입한 Server Actions 기능과 관련돼요. Server Actions는 서버 쪽에서 실행되는 함수들을 클라이언트 사이드에서 직접 호출할 수 있게 해주는데, 이때 보안상 어디서 호출할 수 있는지를 제한하는 것이 중요해요. `allowedOrigins`는 그런 호출을 허용할 도메인 목록을 지정해 주는 역할을 해요.

예를 들어, 위 코드에서 `your-production-domain.com` 도메인에서만 Server Actions가 허용되도록 설정한 거죠. 이렇게 하면, 다른 도메인에서 임의로 서버 액션을 호출하는 걸 막아서 보안이 강화돼요.

### 추가로 알아두면 좋은 점

- 이 기능은 아직 실험 단계(`experimental`)이기 때문에, 사용하려면 Next.js 버전과 문서를 수시로 체크하는 것이 좋아요.
- 개발 환경에서는 `allowedOrigins`에 `localhost`도 넣어야 테스트가 가능해요.
- 여러 도메인을 허용하고 싶다면 배열 안에 여러 도메인을 추가하면 됩니다.
- 만약 CORS(Cross-Origin Resource Sharing)에 익숙하다면, 이 옵션이 CORS 정책과 유사하게 동작한다고 생각하면 쉬워요.

```js
const nextConfig = {
  experimental: {
    serverActions: {
      allowedOrigins: ["your-production-domain.com", "localhost:3000"],
    },
  },
};
```

실제 배포할 때는 꼭 실제 서비스 도메인만 넣어두고, 테스트할 때 일시적으로 localhost를 추가하는 걸 추천해요!

더 자세한 내용과 최신 정보는 공식 Next.js 문서에서 `serverActions.allowedOrigins` 항목을 참고하세요. 늘 변화가 빠른 Next.js에서 이런 실험적인 기능들을 잘 활용하면, 더 안전하고 효율적인 서버-클라이언트 통신이 가능해질 거예요!
