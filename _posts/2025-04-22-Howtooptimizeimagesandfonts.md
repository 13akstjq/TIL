---
title: "Nextjs 15에서 이미지와 폰트 최적화하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:05
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to optimize images and fonts"
link: "https://nextjs.org/docs/app/getting-started/images-and-fonts"
isUpdated: false
---

# 이미지와 폰트 최적화하는 방법

Next.js는 이미지와 폰트 최적화를 자동으로 지원해줘요. 이 글에서는 어떻게 이 기능들을 활용할 수 있는지 쉽게 알려드릴게요.

## 이미지 최적화하기

Next.js의 `Image` 컴포넌트는 기본 HTML의 `img` 태그를 확장한 건데요, 여기에는 여러 가지 좋은 점이 있어요:

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

이미지 최적화는 웹 성능 향상에 정말 중요한 부분인데요! Next.js의 `next/image` 컴포넌트를 사용하면 여러 가지 유용한 기능들을 쉽게 활용할 수 있습니다.

- **크기 최적화 (Size optimization)**: 각 디바이스에 맞게 자동으로 적절한 크기의 이미지를 제공해주고, WebP 같은 최신 이미지 포맷도 지원해서 용량을 줄여줘요.
- **시각적 안정성 (Visual stability)**: 이미지가 로딩되는 동안 레이아웃이 흔들리거나 이동하지 않도록 자동으로 잡아줍니다.
- **빠른 페이지 로드 (Faster page loads)**: 뷰포트에 들어올 때까지 이미지를 로드하지 않는 '네이티브 지연 로딩(lazy loading)'을 사용하며, 옵션으로 흐릿한 블러(blur-up) 플레이스홀더도 추가할 수 있어요.
- **유연한 자산 관리 (Asset flexibility)**: 로컬 이미지뿐만 아니라 원격 서버에 있는 이미지도 필요에 따라 크기를 조절하며 불러올 수 있습니다.

시작하려면 `next/image`에서 `Image`를 import하고, 컴포넌트 안에서 이렇게 사용해보세요.

```js
import Image from "next/image";

export default function Page() {
  return <Image src="" alt="" />;
}
```

- `src` 속성에는 로컬 이미지 경로나 외부 URL 모두 들어갈 수 있어요.
- `alt` 속성은 이미지 설명으로 SEO와 접근성에 매우 중요하니 꼭 넣어주세요!

---

### 추가 팁!

- `width`와 `height`를 명시해주면 레이아웃이 더 안정적이라 로딩 시 레이아웃 이동(Cumulative Layout Shift)을 방지할 수 있어요.
- 원격 이미지 URL을 써야 하면 `next.config.js`에 `domains` 설정을 해줘야 로딩이 허용됩니다.

```js
// next.config.js
module.exports = {
  images: {
    domains: ["example.com"], // 허용할 원격 이미지 도메인
  },
};
```

- `placeholder="blur"` 옵션을 주면 흐릿한 이미지가 먼저 보이면서 자연스러운 로딩 경험을 줄 수도 있고, 특히 느린 네트워크에서 효과적입니다.

이렇게만 활용해도 페이지 퍼포먼스와 사용자 경험이 꽤 개선될 거예요! 앞으로 프로젝트에 멋지게 적용해 보세요 :)

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

### 로컬 이미지 사용하기

Next.js에서는 이미지나 폰트 같은 정적 파일을 프로젝트 루트 디렉터리에 `public`이라는 폴더를 만들어서 관리할 수 있어요. 이렇게 `public` 폴더 안에 저장한 파일들은 코드에서 기본 URL(`/`)부터 시작해서 쉽게 불러올 수 있답니다.

예를 들어, `public/assets/img/2025-04-22-Howtooptimizeimagesandfonts_0.png`라는 이미지가 있다면 HTML에서는 이렇게 사용할 수 있어요:

```html
<img src="/TIL/assets/img/2025-04-22-Howtooptimizeimagesandfonts_0.png" />
```

---

더 나아가 Next.js에서 권장하는 방법인 `next/image` 컴포넌트를 써보면 이렇게 됩니다:

```jsx
import Image from "next/image";
import profilePic from "./me.png";

export default function Page() {
  return (
    <Image
      src={profilePic}
      alt="Picture of the author"
      // width={500} 자동으로 제공됨
      // height={500} 자동으로 제공됨
      // blurDataURL="data:..." 자동 제공 (로딩 시 블러 효과)
      // placeholder="blur" // 로딩 중에 흐릿하게 미리보기 하는 옵션 (선택 사항)
    />
  );
}
```

`next/image`를 쓰면 이미지 크기 최적화, 자동 레이지 로딩(lazy loading), 웹 최적화 포맷 변환 등 다양한 이점을 누릴 수 있어서 성능 개선에 특히 좋아요.

> TIP: `next/image`는 이미지를 직접 `import`로 불러와야 제대로 작동하니, 프로젝트 내 이미지 파일 경로를 정확히 지정해 주세요!  
> 그리고 `public` 폴더 내 이미지 파일을 사용할 땐 `src`에 문자열 경로(`/assets/img/example.png`)를 직접 넣으면 되고, import해서 사용하는 경우는 외부나 프로젝트 내부 파일을 가리킬 때 주로 쓰입니다.

이렇게 하면 로컬 이미지 자원을 깔끔하게 관리하면서 Next.js의 최적화 기능도 빵빵하게 사용할 수 있으니 꼭 활용해보세요!

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

Next.js는 이미지 파일을 가져올 때, 이미지의 고유 너비와 높이를 자동으로 판단해줘요. 이 값을 이용해서 이미지 비율을 정하고, 이미지가 로딩되는 동안 발생할 수 있는 레이아웃 흔들림(Cumulative Layout Shift)을 막아줍니다. 덕분에 사용자 경험이 훨씬 좋아지죠.

### 원격 이미지 사용하기

로컬 이미지뿐만 아니라, 원격 서버에 있는 이미지도 사용할 수 있어요. 이때는 `src` 속성에 이미지 URL을 바로 넣어주면 됩니다.

```js
import Image from "next/image";

export default function Page() {
  return (
    <Image src="https://s3.amazonaws.com/my-bucket/profile.png" alt="Picture of the author" width={500} height={500} />
  );
}
```

> 참고로, 원격 이미지 URL을 사용할 때도 `width`와 `height`를 꼭 명시해주셔야 합니다. 이렇게 하면 Next.js가 이미지 공간을 미리 확보해서 페이지가 흔들리지 않도록 해주거든요.

혹시 원격 이미지 도메인이 반복적으로 쓰인다면, `next.config.js` 파일에서 도메인을 허용해주는 설정도 해주시면 좋아요. 예를 들어:

```js
// next.config.js
module.exports = {
  images: {
    domains: ["s3.amazonaws.com"],
  },
};
```

이 설정을 해줘야 Next.js가 해당 도메인의 이미지를 최적화하고 사용할 수 있으니 꼭 기억하세요!

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

Next.js를 사용할 때, 빌드 과정에서 원격(remote) 이미지 파일에 직접 접근할 수 없기 때문에, 이미지 컴포넌트에 width, height 그리고 선택적으로 blurDataURL 같은 속성들을 직접 지정해줘야 해요. 이 width와 height 정보는 이미지가 로딩되면서 화면이 깜빡이거나 레이아웃이 순간적으로 바뀌는 현상(layout shift)을 막아주고, 올바른 비율(aspect ratio)을 유지하는 데 도움이 되죠.

그리고 원격 서버에서 이미지를 안전하게 불러오려면, next.config.js 파일에서 허용할 원격 이미지 URL 패턴을 정확하게 명시해줘야 해요. 이렇게 구체적으로 지정하는 이유는 보안상 악의적인 URL이 들어오는 걸 막기 위함입니다. 예를 들어 아래와 같이 특정 AWS S3 버킷에서만 이미지를 불러오도록 설정할 수 있어요:

```js
import { NextConfig } from "next";

const config: NextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: "https",
        hostname: "s3.amazonaws.com",
        port: "",
        pathname: "/my-bucket/**",
        search: "",
      },
    ],
  },
};

export default config;
```

위처럼 설정하면 s3.amazonaws.com 도메인의 /my-bucket 경로 밑에 있는 이미지들만 Next.js 이미지 최적화 기능을 통해 불러올 수 있게 돼요.

---

### 추가로 알려드리는 팁!

- 원격 이미지의 크기를 정확히 알기 어렵다면, 개발자 도구나 디자인 툴에서 미리 가로, 세로 크기를 측정해 넣는 걸 추천합니다.
- `blurDataURL` 속성은 낮은 해상도의 이미지 베이스64 데이터를 넣어 이미지가 로딩될 때 부드럽게 보여주는 효과를 줍니다. 사용자 경험을 개선하는 데 특히 유용해요.
- 때로는 이미지를 캐싱하기 위해 CDN을 사용합니다. 이럴 때도 해당 CDN 도메인에 맞게 remotePatterns 설정을 해줘야 하니 참고하세요.

---

## 폰트 최적화하기 (Optimizing Fonts)

Next.js는 기본적으로 디폴트 폰트를 빠르게 로딩하기 위한 몇 가지 기능을 제공하는데요, 웹폰트를 최적화하면 페이지 로딩 속도를 개선하고, 불필요한 폰트 파일 다운로드를 줄일 수 있습니다. 이 주제에 대해서도 곧 상세하게 다뤄볼게요!

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

next/font 모듈은 폰트를 자동으로 최적화해주고, 외부 네트워크 요청을 없애서 개인정보 보호와 성능을 동시에 잡아줍니다.

이 모듈은 폰트 파일을 자체 호스팅하는 기능도 내장하고 있어요. 덕분에 웹 폰트를 불러올 때 레이아웃이 흔들리는 현상(레이아웃 시프트) 없이 깔끔하게 로딩할 수 있답니다.

사용법도 간단해요! next/font에서 제공하는 `local` 혹은 `google` 폰트 모듈을 불러오고, 필요한 옵션을 주면서 함수를 호출해서 폰트를 적용할 엘리먼트의 `className`에 할당하면 됩니다.

아래는 구체적인 예시 코드인데요, 제가 좀 더 완성된 형태로 정리해봤어요.

```js
// 예를 들어 구글 폰트를 불러올 땐 이렇게!
import { Roboto } from "next/font/google";

const roboto = Roboto({
  subsets: ["latin"],
  weight: ["400", "700"],
  style: ["normal", "italic"],
});

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="ko">
      <body className={roboto.className}>{children}</body>
    </html>
  );
}
```

여기서 `Roboto`는 구글에서 제공하는 대표적인 구글 폰트 중 하나입니다. 옵션으로는 `subsets` (영문 등 문자 범위), `weight` (폰트 굵기), `style` (normal, italic 등)를 지정할 수 있어요.

- **장점!**
  - 폰트가 페이지에서 필요한 부분에만 로딩되어 초기 로딩 속도가 빨라집니다.
  - 폰트를 외부 서버에서 가져오는 대신 자체 서버에서 제공해 개인정보도 안심!
  - CSS에서 자주 발생하는 폰트 관련 CLS (Layout Shift) 문제를 예방할 수 있어요.

간단하게 `next/font`를 사용하는 방법 알려드렸는데, 실제 프로젝트에 도입해 보면 성능 개선이 꽤 느껴진답니다. 혹시 직접 폰트 파일을 프로젝트에 추가해서 쓰고 싶다면 `next/font/local` 모듈을 참고해보세요. `next/font/google`처럼 쓰되, 폰트 파일 경로를 지정해서 불러올 수 있습니다.

새 프로젝트에 폰트 적용 고민 중이라면, 이 방법 꼭 한 번 써보시길 추천드려요!

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

### 구글 폰트 자동 호스팅하기

웹사이트에 예쁜 글꼴을 적용할 때 구글 폰트를 많이 사용하시죠? 근데 여러분, 구글 폰트를 그냥 링크 걸어서 쓰면 사용자의 브라우저가 구글 서버에 직접 요청을 보내서 폰트를 가져오거든요. 이러면 약간 속도가 느려질 수도 있고, 개인정보 측면에서 꺼려질 수도 있죠.

그런데 Next.js에서는 구글 폰트를 배포할 때 자동으로 내 서버에서 직접 호스팅해서 폰트를 제공할 수 있어요. 이렇게 하면 사용자가 우리 사이트에 접속할 때 구글 서버로 요청이 나가지 않고, 우리 도메인에서 바로 폰트를 받으니 더 빠르고 깔끔해집니다.

아래처럼 간단하게 `next/font/google`에서 원하는 폰트를 import하고 설정해주면 끝!

```js
import { Geist } from "next/font/google";

const geist = Geist({
  subsets: ["latin"],
});

export default function RootLayout({ children }) {
  return (
    <html lang="en" className={geist.className}>
      <body>{children}</body>
    </html>
  );
}
```

- `subsets` 옵션은 폰트에서 사용할 문자의 범위를 설정해요. 예를 들어 `latin`은 영어권 문자에 해당하고, 한국어는 `korean` 서브셋을 따로 지원하는 폰트가 많으니 필요하면 추가해주면 됩니다.
- 이렇게 하면 폰트 파일들이 빌드 때 내 사이트에 포함되고, 별도 요청 없이 바로 렌더링되어 속도도 좋아져요.

참고로, 커스텀 폰트를 쓰거나 구글 폰트 말고 다른 외부 폰트를 사용할 때도 비슷하게 직접 호스팅하는 걸 추천합니다. 사이트 속도도 개선하고, 외부 서버 의존도를 낮출 수 있으니까요!

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

변수 폰트(variable fonts)를 사용하는 걸 추천해요! 성능도 좋고 유연하게 폰트를 조절할 수 있어서요. 만약 변수 폰트를 쓸 수 없는 상황이라면, 폰트의 굵기(weight)를 꼭 지정해줘야 해요.

예를 들어, Google 폰트인 Roboto를 Next.js에서 사용할 때는 이렇게 작성해요:

```js
import { Roboto } from "next/font/google";

const roboto = Roboto({
  subsets: ["latin"],
});

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" className={roboto.className}>
      <body>{children}</body>
    </html>
  );
}
```

위 코드에서 `Roboto`는 변수 폰트로 불러와서, 원하는 굵기나 스타일을 쉽게 조절할 수 있답니다.

---

### 로컬 폰트 사용하는 방법

로컬에 저장된 폰트를 사용하고 싶을 땐 `next/font/local`에서 폰트를 임포트하면 돼요. 그리고 폰트 파일(src)의 경로를 꼭 지정해줘야 해요.

```js
import localFont from "next/font/local";

const myFont = localFont({
  src: "./fonts/MyFont.woff2",
  weight: "400",
  style: "normal",
});

export default function RootLayout({ children }) {
  return (
    <html lang="en" className={myFont.className}>
      <body>{children}</body>
    </html>
  );
}
```

여기서 주의할 점은, 로컬 폰트를 쓸 땐 폰트 파일의 형식(woff, woff2, ttf 등)에 따라 브라우저 호환성이 달라질 수 있으니 여러 형식을 같이 넣어주는 것도 좋은 방법이에요.

---

추가로, 변수 폰트를 쓰면 파일 사이즈도 줄일 수 있는데, 왜냐하면 한 파일 내에서 여러 굵기와 스타일을 다룰 수 있기 때문이죠. 기존엔 굵기별로 따로 폰트 파일을 불러와야 했거든요. 그래서 성능 최적화에도 큰 도움이 돼요!

궁금한 점 있으면 댓글로 남겨주세요~

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

Next.js에서 `localFont`를 사용해서 로컬 폰트를 불러오는 방법에 대해 이야기해볼게요. 폰트를 프로젝트 안에 직접 넣고 사용할 때 정말 유용한 기능인데요, 예를 들어 아래처럼 간단히 사용할 수 있어요.

```js
import localFont from "next/font/local";

const myFont = localFont({
  src: "./my-font.woff2",
});

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" className={myFont.className}>
      <body>{children}</body>
    </html>
  );
}
```

이 코드의 핵심은 `localFont`가 폰트 파일 경로를 받아서 내부적으로 `@font-face`를 생성해주고, 관련 CSS 클래스를 제공합니다. `myFont.className`을 html 태그에 넣으면 페이지 전체에 해당 폰트가 적용돼요.

그리고 보통 하나의 폰트 패밀리에는 다양한 스타일과 두께가 있기 마련이죠? 예를 들면 일반체, 이탤릭, 굵은 글씨 등등. 이런 경우에는 `src`를 배열 형태로 넘겨서 각각의 스타일과 무게를 지정할 수 있습니다.

```js
const roboto = localFont({
  src: [
    {
      path: "./Roboto-Regular.woff2",
      weight: "400",
      style: "normal",
    },
    {
      path: "./Roboto-Italic.woff2",
      weight: "400",
      style: "italic",
    },
    {
      path: "./Roboto-Bold.woff2",
      weight: "700",
      style: "normal",
    },
    {
      path: "./Roboto-BoldItalic.woff2",
      weight: "700",
      style: "italic",
    },
  ],
});
```

이렇게 하면 폰트를 사용할 때 보통 CSS에서 하는 `font-weight`, `font-style` 같은 속성들도 그대로 적용되니 굉장히 편리해요. 페이지에서 `<p style={{ fontWeight: '700', fontStyle: 'italic' }}>` 이런 식으로 쓰면 `Roboto-BoldItalic.woff2`가 적용되는 거죠.

추가 팁으로, 이 방법은 폰트 파일을 직접 관리하기 때문에 구글 폰트 CDN과 달리 네트워크 지연이나 외부 서비스 의존이 없어서 성능 면에서도 좋고, 개인정보 보호에도 유리하답니다!

### 요약

| 특징                                 | 설명                                              |
| ------------------------------------ | ------------------------------------------------- |
| `localFont` 사용법                   | 폰트를 로컬에서 직접 불러옴                       |
| 단일 파일 폰트 지정                  | `src`에 한 개의 폰트 파일 경로 지정               |
| 여러 스타일/두께 폰트 지정           | `src`를 배열로 주고 각각의 `weight`, `style` 지정 |
| 자동으로 클래스 및 `@font-face` 생성 | 폰트를 클래스 이름으로 쉽게 적용 가능             |
| 성능 및 개인정보 보호                | 외부 호출 없이 빠르고 안전함                      |

Next.js 프로젝트에서 폰트를 더 세밀하게 컨트롤하고 싶다면, 이렇게 `localFont` 사용을 적극 추천해요!
