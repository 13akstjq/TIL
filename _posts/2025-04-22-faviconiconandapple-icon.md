---
title: "2025년 Next.js 15에서 favicon, icon, apple-icon 쉽게 설정하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:48
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "favicon, icon, and apple-icon"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/metadata/app-icons"
isUpdated: false
---


# favicon, icon, 그리고 apple-icon

웹 애플리케이션이나 웹사이트를 만들 때, 한 번쯤 들어봤을 'favicon', 'icon', 'apple-icon'에 대해 이야기해볼게요. 이 파일들은 우리 앱이나 사이트에 아이콘을 설정할 수 있게 해주는데요. 

이 아이콘들은 웹 브라우저 탭, 스마트폰 홈 화면, 그리고 검색 엔진 결과에 표시되어 웹사이트의 정체성을 보여주고, 사용자 경험을 조금 더 풍부하게 만들어줘요.

아이콘을 설정하는 방법에는 크게 두 가지가 있어요: 

(아래에서 각각의 방법과 구체적인 설정법도 함께 소개할게요.)

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

# Next.js에서 앱 아이콘 설정하기: 이미지 파일과 코드 이용법

안녕하세요! 오늘은 Next.js 프로젝트에서 앱 아이콘을 어떻게 설정하는지, 특히 이미지 파일이나 코드를 사용하는 방법에 대해 다뤄볼게요. 아이콘은 웹사이트나 앱을 한눈에 알아볼 수 있게 해주니까 정말 중요하죠. 자, 그럼 시작해볼까요?

---

## 1. 이미지 파일 사용하기 (.ico, .jpg, .png)

### 기본 개념
Next.js에서는 favicon, icon, 또는 apple-icon 이미지 파일을 `/app` 디렉터리 안에 넣어서 앱 아이콘을 설정할 수 있어요. 여기서 중요한 점은 favicon 이미지 파일은 `/app` 폴더의 최상위에 위치해야 한다는 점이에요.

예를 들어, 이런 형식으로 위치시키면 돼요:


/app/favicon.ico
/app/apple-icon.png


### 어떻게 작동하나요?
Next.js가 빌드하거나 실행할 때, 이 파일들을 자동으로 감지해서 `<head>` 태그 안에 적절한 `<link>` 태그를 넣어줘요. 덕분에 우리가 하나하나 수동으로 태그를 추가하지 않아도 돼서 정말 편하답니다.

### 주의할 점
- favicon은 보통 `.ico` 형식을 많이 쓰지만, 요즘은 크기가 크고 투명한 PNG 파일도 흔하게 사용됩니다.
- iOS 기기용 애플 아이콘(apple-touch-icon)도 추가해주면 좋죠.
- `/app` 최상위에 위치하지 않으면 자동 인식이 안 되니 꼭 위치를 확인하세요!

---

## 2. 코드로 아이콘 생성하기 (.js, .ts, .tsx)

### 이미지 대신 코드로 아이콘을 만들고 싶다면?

Next.js나 React에서는 SVG를 컴포넌트로 만들어서 아이콘을 직접 렌더링할 수도 있어요. 예를 들면, `.tsx` 파일 안에 SVG 코드를 넣고, 필요할 때마다 해당 아이콘 컴포넌트를 호출하는 방식이죠.

이 방법의 장점은:

- 아이콘 크기, 색상, 애니메이션 등 커스터마이즈가 자유롭다
- 별도의 이미지 파일 추가 없이 관리가 간편하다
- 타입스크립트와도 잘 호환된다

### 간단한 예시

```tsx
import React from 'react';

const MyIcon = () => (
  <svg width="32" height="32" viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg">
    <circle cx="16" cy="16" r="16" fill="#4CAF50" />
    <path d="M16 8L24 24H8L16 8Z" fill="white" />
  </svg>
);

export default MyIcon;
```

이렇게 만든 아이콘을 페이지나 컴포넌트에서 불러와서 쓸 수 있어요.

---

## 요약 비교 테이블

| 방법               | 장점                                    | 단점                               | 사용 추천 상황                      |
|--------------------|-----------------------------------------|----------------------------------|----------------------------------|
| 이미지 파일 (.ico, .png) | 자동으로 `<head>`에 태그 추가, 간편              | 위치 제약 있음, 커스터마이징 불가     | 기본적인 favicon 설정 시           |
| 코드 생성 (.js, .tsx)     | 커스터마이징 자유, 타입 안정성, 애니메이션 가능 | `<head>`에 자동 추가 안됨, 직접 추가 필요 | 커스텀 아이콘 UI 컴포넌트 만들 때  |

---

## 팁 한마디!

favicon은 단지 웹사이트 탭에 표시되는 것 뿐 아니라, 모바일 홈 화면 바로가기 아이콘, 웹 애플리케이션 아이콘 등 다양하게 사용돼요. 그러니 퀄리티 좋은 여러 크기 아이콘들을 준비해서 사용하는 게 좋아요. 추가로, [favicon.io](https://favicon.io/) 같은 온라인 툴을 통해 손쉽게 favicon을 생성하고 변환할 수도 있답니다.

---

Next.js 프로젝트에 맞는 아이콘 설정 방법, 이해하는 데 도움이 되었길 바라요! 궁금한 점 있으면 언제든지 댓글로 물어봐 주세요 :)

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

아래는 favicon, icon, apple-icon 파일 관련 규칙을 정리한 표입니다. 공식 문서나 개발하면서 참고하기 딱 좋아요!

| 파일 명명 규칙       | 지원하는 파일 타입                              | 유효한 위치          |
|--------------------|-------------------------------------------|-------------------|
| [favicon](#favicon)      | `.ico`                                    | `app/`            |
| [icon](#icon)            | `.ico`, `.jpg`, `.jpeg`, `.png`, `.svg`  | `app/**/*`        |
| [apple-icon](#apple-icon) | `.jpg`, `.jpeg`, `.png`                    | `app/**/*`        |

### favicon

favicon은 웹 브라우저 탭에 표시되는 아주 작은 아이콘을 의미하죠. 여기서는 `favicon.ico` 파일을 루트 경로인 `/app` 폴더에 넣으면 됩니다.

```js
<link rel="icon" href="/favicon.ico" sizes="any" />
```

이렇게 하면 기본적으로 모든 크기에서 아이콘이 잘 보이게 돼요.

---

추가로 알려드리자면, 아이콘 파일들을 `app` 내부 어디에 넣어도 되는데(`app/**/*`), favicon은 꼭 루트(`app/`)에 놓아야 한다는 점 기억하세요. 그리고 애플 기기(iPhone, iPad 등)에서 홈 화면에 추가할 때 사용하는 `apple-icon`은 PNG, JPG 같은 이미지 포맷을 활용합니다.

개발하면서 아이콘을 여러 가지 사이즈로 준비하면 다양한 환경에서 사용자 경험이 더 좋으니, 가능하면 다양한 크기로 아이콘을 만들어 두는 걸 추천해요. 예를 들어, 16x16, 32x32, 180x180 같은 크기를 만들어 놓으면 좋죠!

필요한 경우 SVG 사용도 고려할 수 있는데, SVG는 벡터 기반이라 해상도에 상관없이 선명하게 표시된답니다. 하지만 모든 브라우저나 플랫폼에서 100% 지원하는 건 아니니까 favicon용으로는 `ico`가 여전히 표준이에요.

참고로 React, Next.js 같은 프레임워크에서는 이 폴더 구조에 맞춰 아이콘 파일을 배치하고, 필요한 메타 태그나 링크를 `<head>`에 추가해주면 쉽게 적용할 수 있어요!

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

### 아이콘 설정하기

웹사이트에 아이콘을 추가하는 방법, 생각보다 간단해요! 보통 파비콘(favicon)이라고 부르는 아이콘은 `.ico`, `.jpg`, `.jpeg`, `.png`, `.svg` 같은 이미지 파일을 사용해요.

기본적인 아이콘 추가 태그는 이렇게 생겼답니다:

```js
<link
  rel="icon"
  href="/icon?<generated>"
  type="image/<generated>"
  sizes="<generated>"
/>
```

- `rel="icon"`: 이 링크가 아이콘이라는 뜻이에요.
- `href`: 실제 아이콘 파일 경로를 넣으면 되죠. `<generated>`는 파일명이나 쿼리 스트링으로 바꿔주세요.
- `type`: 이미지 형식을 명시해줘요. 예를 들면, `image/png`, `image/svg+xml` 처럼요.
- `sizes`: 아이콘 크기(px)를 말해주는데, 여러 크기를 지원하는게 좋습니다. 보통 `16x16`, `32x32` 등으로 설정해요.

예를 들어, 사이트 루트에 `favicon.png`라는 파일을 쓴다면 이렇게 쓸 수 있어요:

```html
<link rel="icon" href="/favicon.png" type="image/png" sizes="32x32" />
```

---

### 애플 터치 아이콘 (apple-icon)

아이폰이나 아이패드에서 홈 화면에 웹사이트를 추가할 때 뜨는 아이콘은 별도로 설정하는 게 좋아요. 이걸 "애플 터치 아이콘"이라고 불러요.

```html
<link rel="apple-touch-icon" href="/apple-icon.png" sizes="180x180" />
```

- `rel`: `apple-touch-icon`으로 설정하세요.
- `href`: 아이콘 파일 경로.
- `sizes`: 보통 `180x180` 크기를 많이 사용해요.

애플 터치 아이콘을 설정해두면 iOS 기기에서 홈 화면에 북마크를 추가할 때 선명하고 예쁜 아이콘이 나타나서 사용자 경험이 훨씬 좋아진답니다!

---

### 추가 팁!

1. **여러 사이즈 지원하기**  
모든 기기에서 최적의 아이콘 품질을 위해 16x16, 32x32, 48x48, 180x180 크기의 아이콘을 준비해두면 좋아요.

2. **SVG 아이콘 사용하기**  
요즘 브라우저들은 SVG도 잘 지원하니, 벡터 형식인 SVG를 사용할 경우 화면 해상도에 관계없이 선명한 아이콘을 보여줄 수 있어요.

3. **manifest.json과 함께 쓰기**  
PWA(Progressive Web App)를 고려한다면, `manifest.json` 파일에도 아이콘 정보를 넣으면 앱 설치 시 아이콘이 제대로 보입니다.

---

아이콘 설정은 기본 중 기본이지만, 사용자가 웹사이트를 더 오래 기억하고 편리하게 하도록 만드는 데 큰 역할을 하니 꼭 신경 써보세요!

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

애플 터치 아이콘(apple-touch-icon) 설정하는 법에 대해 쉽게 알려드릴게요. iOS 기기에서 홈 화면에 웹사이트를 추가할 때 이 아이콘이 보여지는데요, 생각보다 간단해요!

아래 예시처럼 `<link>` 태그를 쓰시면 되는데, 이미지 파일은 보통 jpg, jpeg, png 형식 중 한 가지로 준비하시면 돼요.

```html
<link
  rel="apple-touch-icon"
  href="/apple-icon-180x180.png"
  type="image/png"
  sizes="180x180"
/>
```

여기서 각각의 의미를 한번 살펴볼게요.

| 속성(rel)             | 역할                                |
|-------------------|-----------------------------------|
| `rel="apple-touch-icon"` | 이 링크가 애플 터치 아이콘임을 명시            |
| `href`               | 아이콘 이미지 경로 (보통 루트 디렉터리에 위치)         |
| `type`               | 이미지 타입 (jpg, png 등)               |
| `sizes`              | 아이콘 크기(px 단위, 가로x세로)               |

### 여러 개 아이콘 등록하기

화면 크기에 따라 다르게 적용되도록 여러 크기의 아이콘을 등록할 수도 있어요. 이럴 때는 파일 이름에 숫자를 붙여 차례로 관리하시면 됩니다.

예를 들어,

- apple-icon-120x120.png  
- apple-icon-152x152.png  
- apple-icon-180x180.png  

처럼요.

브라우저가 필요한 아이콘 크기를 판단해 알맞은 파일을 자동으로 선택합니다.

### 추가 팁!

- 아이콘 파일은 프로젝트의 루트(/) 혹은 앱의 root segment에 있어야 적용돼요.  
- 간혹 favicon과 apple-touch-icon을 혼동하는 경우 있는데, 둘은 기술적 목적과 사용하는 위치가 다릅니다. favicon은 브라우저 탭, apple-touch-icon은 iOS 홈 화면이죠.  
- 요즘은 `.svg` 아이콘을 쓰기도 하는데요, 이 경우 `sizes="any"`를 지정해서 아이콘 크기를 제한하지 않는 식으로 만들어 줍니다.  
- 아이콘 이미지는 정사각형(가로, 세로가 같은)으로 만드는 게 좋아요. 그래야 크기가 바뀌어도 깨지거나 왜곡되지 않습니다.

### 자동으로 아이콘 생성하는 방법 (.js, .ts, .tsx)

아이콘 이미지 관리를 직접 하기 귀찮으면, 아래처럼 코드로 아이콘을 생성하고 설정 파일에서 자동으로 등록하는 방법도 있어요.

```js
import { generateAppleTouchIcons } from 'your-icon-generator-library';

generateAppleTouchIcons({
  src: 'logo.png',
  outputDir: '/public/icons',
  sizes: [120, 152, 180], // 생성할 아이콘 크기들
});
```

이런 방식은 프로젝트 빌드 과정에 포함시키면 최신 로고를 기준으로 자동으로 아이콘 이미지를 만들어줘서 관리하기 훨씬 편합니다!

---

이제 여러분도 iOS 홈 화면에 예쁜 아이콘을 쉽게 추가할 수 있겠죠? 😊 아이콘용 이미지는 180x180px 정도로 준비하면 가장 무난하니 참고하세요!

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

이미지 파일을 직접 사용하는 것 외에도, 코드를 통해 아이콘을 프로그래밍 방식으로 생성할 수 있어요.

예를 들어, `icon`이나 `apple-icon`이라는 라우트를 만들어서 기본 내보내기(default export)로 함수를 작성하면 앱 아이콘을 동적으로 만들 수 있답니다.

| 파일명 컨벤션 | 지원 파일 타입               |
|--------------|---------------------------|
| `icon`       | `.js`, `.ts`, `.tsx`      |
| `apple-icon` | `.js`, `.ts`, `.tsx`      |

가장 간단한 아이콘 생성 방법은 Next.js의 `next/og` 패키지에서 제공하는 `ImageResponse` API를 활용하는 거예요. 이 API를 사용하면 서버 사이드에서 이미지(아이콘)를 동적으로 그려서 반환할 수 있죠.

예를 들어, 다음과 같은 식으로 작성할 수 있어요:

```ts
import { ImageResponse } from 'next/og';

export const runtime = 'edge';

export default function Icon() {
  return new ImageResponse(
    (
      <div
        style={{
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
          fontSize: 128,
          fontWeight: 'bold',
          color: '#0070f3',
        }}
      >
        🚀
      </div>
    ),
    {
      width: 512,
      height: 512,
    }
  );
}
```

위 예시는 "🚀" 이모지를 활용해 512x512 크기의 아이콘 이미지를 만들어요. 이렇게 하면 별도의 이미지 파일 없이도 코드를 통해 나만의 아이콘을 동적으로 생성할 수 있답니다.

추가로, `icon`과 `apple-icon` 라우트는 각각 일반 앱 아이콘과 iOS용 애플 아이콘을 생성할 때 활용해요. 필요에 따라 서로 다른 스타일이나 해상도로 아이콘을 만들어 줄 수도 있답니다.

이렇게 아이콘을 코드로 생성하는 방법은 커스터마이징이 쉽고, 빌드할 때마다 최신 상태의 아이콘을 자동으로 만들어주기 때문에 꽤 유용해요! 한번 시도해보시길 추천드려요.

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

Next.js에서 Open Graph(og) 이미지나 아이콘을 생성하는 방법을 소개할게요! 이번에 살펴볼 코드는 32x32 크기의 PNG 아이콘을 동적으로 생성하는 예제입니다. 코드를 보면서 직접 어떤 짓을 하는지, 그리고 알아두면 좋은 팁도 같이 정리해볼게요.

---

## 동적 아이콘 생성 코드

```jsx
import { ImageResponse } from 'next/og';

// 이미지 메타데이터 (크기, 타입)
export const size = {
  width: 32,
  height: 32,
};
export const contentType = 'image/png';

// 이미지 생성 함수
export default function Icon() {
  return new ImageResponse(
    (
      <div
        style={{
          fontSize: 24,
          background: 'black',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
          color: 'white',
        }}
      >
        A
      </div>
    ),
    {
      ...size, // 위에서 정의한 32x32 크기를 그대로 사용
    }
  );
}
```

여기서 중요한 점은 `ImageResponse`라는 Next.js의 API를 이용해서 JSX 형태로 이미지를 만들어낸다는 거예요. 즉, 평소 웹에서 HTML/CSS를 쓰듯 그걸 그대로 이미지로 변환하는 느낌이죠!

### key 포인트!

- `fontSize`, `background`, `color` 등을 사용해서 화면 중앙에 대문자 'A'가 흰색으로 나타나도록 디자인했어요.
- `size` 객체를 재사용해서 이미지 크기를 일관성 있게 지정했어요.
- 이렇게 만든 이미지는 `image/png` 타입이기 때문에 아이콘이나 썸네일 등으로 활용 가능하죠.

---

## HTML에서 생성한 아이콘 사용하기

```html
<link rel="icon" href="/icon?<generated>" type="image/png" sizes="32x32" />
```

위 태그는 만들어진 아이콘을 `favicon` 등으로 사용할 때 쓰는 근본적인 형태입니다. `?<generated>` 부분은 동적으로 생성된 이미지를 캐시 방지용 쿼리 스트링일 수도 있고, 실제 운영환경에 맞춰 적절하게 수정해 써야 합니다.

---

## 알아두면 좋은 점들

| 구분           | 설명                                                                                   |
| -------------- | -------------------------------------------------------------------------------------- |
| 생성 시점       | 기본적으로 아이콘은 빌드 시점에 한번 정적으로 생성되고 캐시됩니다.                      |
| 동적 데이터 사용 | 동적 API나 uncached 데이터를 이용하면 런타임에 매번 새로 생성할 수 있어요.             |
| 여러 아이콘 생성 | 한 파일에서 `generateImageMetadata`라는 함수를 쓰면 여러 아이콘을 한번에 만들 수 있어요. |
| favicon 제한    | `ImageResponse`로 favicon(대표 아이콘)은 직접 생성 불가. 대신 기본 favicon.ico 파일 사용하세요. |
| 앱 아이콘 특성  | 앱 아이콘은 특수한 라우트 핸들러로 동적으로 생성되지만, 캐시는 기본값으로 적용됩니다.    |

즉, 아이콘을 다루면서 ‘정적 생성’과 ‘동적 생성’ 방식 차이를 이해하는 게 중요해요. 그래야 빌드 속도와 최종 사용자 경험 사이 균형을 맞출 수 있답니다.

---

이번 포스트는 Next.js의 Open Graph 이미지 생성 기능을 통해 동적으로 아이콘을 만드는 방법을 소개했어요. Open Graph 이미지는 주로 SNS 공유 시 미리보기에 뜨는 이미지로 많이 쓰이지만, 이렇게 아이콘이나 프로필 이미지 생성에도 응용 가능하답니다.

혹시 『더 멋진 아이콘 디자인』을 원한다면 SVG, Canvas API, 또는 외부 그래픽 파일을 활용하는 방법도 한번 시도해보세요. Next.js가 제공하는 이런 이미지 생성 기능은 입맛에 맞게 스타일링과 로직을 마음껏 변경할 수 있다는 점이 정말 매력적입니다.

앞으로도 이런 실용적인 기능을 쉽고 재밌게 알려드릴게요. 즐겁게 개발하세요! 🚀

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

기본 내보내기 함수는 다음과 같은 props를 받습니다:

#### params (옵션)

루트 세그먼트부터 해당 아이콘 (`icon` 또는 `apple-icon`)이 위치한 세그먼트까지의 동적 라우트 파라미터들을 담고 있는 객체입니다.

```js
export default function Icon({ params }: { params: { slug: string } }) {
  // ...
}
```

여기서 `params.slug`는 URL 경로에서 동적으로 변하는 부분을 나타내는데요, 예를 들어 `/posts/hello-world` 라는 경로가 있다면, slug는 `"hello-world"`가 됩니다. 이런 동적 파라미터를 이용하면 여러 페이지를 하나의 컴포넌트로 유연하게 처리할 수 있어서 편리해요.

> 참고로, Next.js 같은 프레임워크에서는 이런 동적 라우팅을 지원해서 SEO 최적화에도 용이하니 꼭 활용해보세요!

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

아래는 React 또는 Next.js 같은 환경에서 동적인 라우팅과 관련된 파일과 URL 경로, 그리고 params가 어떻게 매핑되는지를 정리한 표입니다.

| Route                       | URL        | `params`                  |
|-----------------------------|------------|---------------------------|
| `app/shop/icon.js`           | `/shop`    | `undefined`               |
| `app/shop/[slug]/icon.js`    | `/shop/1`  | `{ slug: '1' }`           |
| `app/shop/[tag]/[item]/icon.js` | `/shop/1/2` | `{ tag: '1', item: '2' }` |

### Returns

기본으로 export되는 함수는 다음과 같은 타입 중 하나를 반환해야 합니다:

- Blob
- ArrayBuffer
- TypedArray
- DataView
- ReadableStream
- Response

> 참고로, `ImageResponse`가 이 반환 타입 요건을 만족합니다.

---

### 조금 더 쉽게 설명해볼게요!

위 표는 Next.js나 비슷한 프레임워크에서 동적 라우팅을 구성할 때 파일 이름과 실제 URL 매칭, 그리고 params가 어떻게 넘어오는지를 보여주는 예시예요. 예를 들어 `app/shop/[slug]/icon.js` 파일은 `/shop/1` URL로 접속했을 때, `{ slug: '1' }`라는 파라미터를 받아서 그에 맞는 동작을 하게 됩니다.

그리고 함수가 반환해야 하는 타입은 이미지나 스트림, 혹은 응답 객체 등 다양한 형태가 될 수 있어요. 특히 `ImageResponse`라는 타입이 있는데, 이건 Next.js에서 이미지를 동적으로 생성할 때 자주 사용하는 반환 타입입니다.

만약 서버 사이드에서 이미지를 생성해서 클라이언트로 보내야 하는 상황이라면, `ImageResponse`를 반환하면 자동으로 올바른 이미지 응답이 만들어져서 편리하답니다!

---

### 참고로, 이런 동적 라우팅을 활용하면

- URL 하나하나마다 다른 데이터를 보여주고 싶을 때
- API 경로를 간편하게 관리하고 싶을 때
- 여러 단계의 파라미터를 받아 처리해야 할 때

굉장히 유용합니다. Next.js의 파일 기반 라우팅이 정말 강력한 이유 중 하나죠!

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

### Config exports

아이콘의 메타데이터를 설정하고 싶을 때, 아이콘이나 애플 아이콘 경로에서 `size`와 `contentType` 변수를 내보내(export)서 설정할 수 있어요.

| Option       | Type                                                                                             |
|--------------|--------------------------------------------------------------------------------------------------|
| `size`       | `{ width: number; height: number }` - 아이콘의 가로, 세로 크기를 직접 지정할 수 있어요.                        |
| `contentType`| `string` - 이미지 MIME 타입을 지정합니다. 예를 들어 `image/png`, `image/svg+xml` 등이 있죠. [더 알아보기](https://developer.mozilla.org/docs/Web/HTTP/Basics_of_HTTP/MIME_types#image_types) |

#### size

`size`는 아이콘의 너비(width)와 높이(height)를 숫자로 지정할 수 있는 속성입니다. 예를 들면:

```js
export const size = { width: 64, height: 64 };
```

이렇게 지정해주면, 아이콘이 실제로 64x64 픽셀 크기로 인식돼서, 제대로 크기가 맞게 처리되죠.

---

추가로, `contentType`을 제대로 지정하는 것도 중요한데요. 아이콘 파일 형식이 무엇인지 정확히 알려줘야 웹 브라우저가 올바르게 이미지를 해석해요. 예를 들어 PNG 이미지는 `image/png`, SVG는 `image/svg+xml`과 같이 설정해 줍니다.

이 자잘한 설정 덕분에 아이콘을 사용할 때 크기나 형식 문제로 발생하는 오류들을 방지할 수 있답니다. 만약 커스텀 아이콘을 직접 만들어서 쓰거나, 아이콘 라이브러리에서 가져온 파일을 사용할 때 꼭 확인해 보세요!

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

안녕하세요, 여러분! 오늘은 웹 아이콘을 다룰 때 자주 사용하는 몇 가지 코드 조각과 관련된 내용을 살펴보려고 해요. 특히, 리액트 컴포넌트에서 아이콘을 정의할 때와 HTML에서 파비콘(favicon)을 설정할 때 알아두면 좋은 부분들을 정리해볼게요.

## 1. 아이콘 크기 설정

먼저, 주어진 코드에서 `size`라는 객체를 보면 다음과 같아요:

```js
export const size = { width: 32, height: 32 }
```

이렇게 크기를 명시해두면, 아이콘 컴포넌트 내부에서 이 사이즈 값을 활용해서 적절한 크기로 아이콘을 렌더링할 수 있습니다. 예를 들어, SVG 아이콘을 만들 때 `width`와 `height`를 이 값으로 넣어주면 유용하죠.

```js
export default function Icon() {
  return (
    <svg width={size.width} height={size.height} /* ... */>
      {/* SVG 내용 */}
    </svg>
  )
}
```

## 2. HTML에서 아이콘 링크 태그 설정

아래 코드는 HTML 문서의 `<head>` 부분에 아이콘을 연결할 때 쓰는 링크 태그입니다.

```js
<link rel="icon" sizes="32x32" />
```

여기서 `rel="icon"`은 브라우저가 이 리소스를 사이트의 대표 아이콘으로 사용하도록 하는 역할을 해요. `sizes="32x32"`는 아이콘의 크기를 명시하는 속성인데요, 실제로는 이미지 파일 경로(`href`)도 꼭 넣어줘야 아이콘이 제대로 인식될 수 있습니다.

예를 들면 다음과 같이 작성하는 게 일반적입니다.

```html
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />
```

이렇게 하면 브라우저는 32x32 픽셀 크기의 PNG 형식 아이콘 파일을 파비콘으로 사용할 수 있게 되죠.

## 3. 아이콘 MIME 타입 정의

```js
export const contentType = 'image/png'

export default function Icon() {}
```

`contentType`은 이미지나 파일의 형식을 나타내는 MIME 타입인데, 아이콘 파일이 PNG 형식이라면 `'image/png'`로 설정하는 게 맞아요. 서버나 API에서 아이콘 데이터를 전송할 때, 또는 `<link>` 태그에 `type="image/png"` 속성을 지정할 때 이 값이 필요합니다.

---

### 한 줄 정리

| 항목          | 설명                                    | 예시                              |
| ------------- | -------------------------------------- | -------------------------------- |
| `size` 객체   | 아이콘 크기를 지정 (너비, 높이)         | `{ width: 32, height: 32 }`       |
| `<link rel>`  | 파비콘 연결 태그, `href`와 함께 사용     | `<link rel="icon" type="image/png" sizes="32x32" href="/favicon.png" />` |
| `contentType` | 아이콘 파일의 MIME 타입 지정             | `'image/png'`                    |

---

### 추가 팁!

- 파비콘은 다양한 크기로 준비하는 게 좋아요. 모바일이나 고해상도 화면에서는 16x16, 32x32, 48x48, 96x96 등 여러 사이즈 중 적합한 걸 골라 사용하거든요.
- SVG 아이콘을 사용할 경우 `type="image/svg+xml"`로 지정하면 더 선명하고 유연한 아이콘 활용이 가능해요.
- React에서 아이콘 컴포넌트를 만들 때는 크기, 색상 등을 props로 받아 커스터마이징할 수 있게 만드는 것도 좋은 방법입니다.

아이콘 설정, 이제 막막하지 않죠? 앞으로도 유용한 웹 개발 팁들로 찾아올게요~! 😊

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
<link rel="icon" type="image/png" />
```

#### Route Segment Config

icon과 apple-icon은 특별한 Route Handlers로, Pages와 Layouts에서 사용하는 것과 동일한 라우트 세그먼트 구성 옵션을 사용할 수 있어요.

## 버전 히스토리 (Version History)


---

위 내용은 HTML에서 파비콘(favicon)을 설정할 때 사용하는 `<link>` 태그의 예시입니다. 여기서 `rel="icon"`은 브라우저 탭에 보이는 아이콘을 지정하죠. `type="image/png"`는 아이콘 파일 타입이 PNG임을 알려주는 부분이에요.

그리고 Next.js나 유사한 프레임워크에서 사용하는 라우트 세그먼트 설정에 관한 내용도 나와 있는데요. `icon`과 `apple-icon`은 일반적인 페이지나 레이아웃과 마찬가지로 설정 옵션을 쓸 수 있어서, 아이콘 관련 라우팅 처리를 쉽게 할 수 있다는 뜻입니다.

---

추가로, 웹사이트 파비콘을 설정할 때 단순히 `favicon.ico` 파일만 사용하는 시대는 점점 줄고 있어요. 요즘은 다양한 해상도와 기기별로 최적화된 여러 아이콘 파일들을 `<link rel="icon" />`, `<link rel="apple-touch-icon" />` 등으로 세밀하게 관리하는 추세예요. 이렇게 하면 모바일 기기나 다양한 플랫폼에서 더 예쁘고 선명한 아이콘을 보여줄 수 있습니다.

만약 Next.js 13 이상 버전을 사용하고 있다면, 라우트 세그먼트를 이용해 아이콘이나 애플 아이콘 같은 리소스들을 별도로 관리하는 것도 좋습니다. 그러면 유지보수가 편하고, 특정 페이지별로 아이콘을 다르게 설정하는 것도 가능해져요.

---

참고로, 파비콘 설정과 관련해서 지원하는 이미지 포맷은 다음과 같은데요:

| 이미지 포맷 | 설명                           |
|-------------|------------------------------|
| `.ico`      | 가장 전통적인 파비콘 포맷        |
| `.png`      | 투명도 지원하는 현대적인 포맷    |
| `.svg`      | 벡터 이미지로, 크기 조절 가능     |
| `.gif`      | 움직이는 아이콘으로도 가능         |

프로젝트 성격에 맞게 잘 선택해서 사용하세요. 위 표처럼 여러 포맷을 함께 제공하면, 다양한 디바이스와 브라우저에서 더 안정적으로 작동합니다.

필요하면 다음 글에서 실제 프로젝트에 적용하는 예시도 공유할게요!

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

아래는 해당 내용을 Markdown 형식으로 바꾼 표입니다. 

| Version   | Changes                                    |
|-----------|--------------------------------------------|
| `v13.3.0` | `favicon` icon과 `apple-icon`이 새롭게 추가됨 |

이 부분은 웹 개발할 때 자주 쓰이는 favicon과 apple-icon 관련 업데이트 소식인데요, favicon은 브라우저 탭에 보이는 작은 아이콘을 말하고, apple-icon은 아이폰이나 아이패드 등의 홈 화면에 웹앱을 추가했을 때 보여지는 아이콘이에요. 이렇게 지원을 추가하면 사용자가 더 깔끔하고 프로페셔널한 느낌을 받을 수 있답니다! 혹시 웹사이트에 이런 아이콘들을 넣고 싶다면, 각 플랫폼별 기본 사이즈와 포맷을 맞춰주는 것도 잊지 마세요~