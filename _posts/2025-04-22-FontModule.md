---
title: "Nextjs 15에서 Font Module 사용하는 방법 가이드"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:14
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Font Module"
link: "https://nextjs.org/docs/app/api-reference/components/font"
isUpdated: false
---


# 폰트 모듈

이번 글에서는 next/font/google과 next/font/local을 어떻게 활용할 수 있는지 알려드릴게요. 폰트를 효율적으로 불러오고 최적화하는 방법을 배우고 싶다면, next.js의 *Optimizing Fonts* 페이지도 함께 참고하시면 좋아요.

### Font 함수 인자들

폰트를 쓸 때 어떻게 인자를 넘기는지 궁금하시죠? 구글 폰트용 설정과 로컬 폰트용 설정을 각각 따로 살펴보면 이해가 쏙쏙 될 거예요.

---

### 구글 폰트 (Google Fonts) 사용법

구글 폰트를 쉽게 불러오려면 `next/font/google`의 함수를 씁니다. 이 함수는 폰트 이름을 넘겨주면 자동으로 최적화된 CSS와 폰트 파일을 로드해줘요.

- `weight`: 폰트 굵기 설정 (예: '400', '700')
- `style`: 이탤릭체 같은 스타일 지정 가능 ('normal', 'italic')
- `subsets`: 지원할 문자집합 선택 (한국어는 'latin', 'latin-ext', 'korean' 등)
- `variable`: CSS 변수로 쓰기 위한 옵션

### 로컬 폰트 (Local Fonts) 사용법

내 PC나 프로젝트에 직접 폰트 파일을 넣고 싶다면 `next/font/local`을 써보세요. 경로나 파일명을 지정해서 필요한 굵기, 스타일을 함께 넘길 수 있어요.

---

필요에 따라 구글 폰트랑 로컬 폰트를 함께 조합해서 쓰는 것도 가능합니다. 프로젝트에 따라 딱 맞는 방법을 골라 쓰시면 성능과 디자인 모두 잡을 수 있어요! 궁금한 점 있으면 언제든 질문해 주세요~

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

아래는 `next/font`에서 사용되는 주요 옵션들에 대한 정리입니다. 구글 폰트(`font/google`)와 로컬 폰트(`font/local`)에서 각각 어떤 옵션이 필수인지, 타입은 무엇인지 등을 알 수 있어요.

| Key                 | font/google 지원 여부 | font/local 지원 여부 | Type                          | Required          |
|---------------------|-----------------------|---------------------|-------------------------------|-------------------|
| [`src`](#src)           | ❌ 음영 (해당 없음)      | ✅                   | String 또는 객체 배열 (경로 포함) | 필수              |
| [`weight`](#weight)     | ✅                   | ✅                   | String 또는 배열             | 필수/선택 (경우에 따라) |
| [`style`](#style)       | ✅                   | ✅                   | String 또는 배열             | 선택               |
| [`subsets`](#subsets)   | ✅                   | ❌ 음영                | 문자열 배열                 | 선택               |
| [`axes`](#axes)         | ✅                   | ❌ 음영                | 문자열 배열                 | 선택               |
| [`display`](#display)   | ✅                   | ✅                   | String                      | 선택               |
| [`preload`](#preload)   | ✅                   | ✅                   | Boolean                     | 선택               |
| [`fallback`](#fallback) | ✅                   | ✅                   | 문자열 배열                 | 선택               |
| [`adjustFontFallback`](#adjustfontfallback) | ✅         | ✅                   | Boolean 또는 String          | 선택               |
| [`variable`](#variable) | ✅                   | ✅                   | String                      | 선택               |
| [`declarations`](#declarations) | ❌ 음영            | ✅                   | 객체 배열                   | 선택               |

---

### src

- **설명**  
  로컬 폰트 경로를 지정할 때 사용해요. 문자열 하나 또는 객체 배열 형태로 폰트 파일 경로를 넣을 수 있습니다.  
  예를 들어, 객체는 `{ path: 'path/to/font.woff2', weight: '400', style: 'normal' }` 이런 식으로 사용할 수 있어요.  

- **사용처**  
  오직 로컬 폰트를 로딩할 때(`next/font/local`)만 의미가 있습니다.  

---

### 추가 팁!

- 구글 폰트는 `src` 옵션이 없어요. 대신 `weight`나 `style`, `subsets`같은 옵션들을 사용해서 폰트를 다양하게 커스터마이즈 할 수 있습니다.
- `preload`가 `true`로 설정되면, 폰트가 미리 로딩되어 초기 렌더링시 폰트가 깜빡이지 않는 효과가 있어 사용자 경험이 더 좋아집니다.
- `fallback`으로 여러 글꼴을 지정해 두면, 만약 로딩에 실패해도 다른 글꼴로 대체되어 디자인이 깨지지 않아요.
- `variable` 옵션은 CSS 변수로 폰트 패밀리를 정의해 보다 유연하게 스타일을 적용할 때 유용합니다.

---

폰트 설정을 제대로 하면 웹사이트의 성능과 사용 체감이 확 좋아지니, 꼭 이 옵션들을 꼼꼼히 살펴보시길 바랍니다! 특히 Next.js 같은 프레임워크에서는 기본적으로 최적화해주니까, 이걸 잘 활용하는 게 핵심입니다.

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

웹 개발하면서 폰트를 쓸 때, `src` 속성에 폰트 파일 경로를 지정하는 게 필수인 거 다들 아시죠? 폰트가 어디에 있냐에 따라서 경로를 어떻게 써야 하는지 예를 들어서 설명해볼게요.

예를 들어,

- `src: './fonts/my-font.woff2'` 라고 적었다면, `my-font.woff2` 파일이 `app` 폴더 안에 있는 `fonts`라는 폴더 안에 있어야 해요.
- `src`에 배열처럼 여러 경로를 주는 경우도 있어요. 예를 들어,

```ts
src: [
  'path: `./inter/Inter-Thin.ttf`, weight: `100`',
  'path: `./inter/Inter-Regular.ttf`, weight: `400`',
  'path: `./inter/Inter-Bold-Italic.ttf`, weight: `700`, style: `italic`',
]
```

이렇게 여러 스타일과 두께(weight)를 각각 지정할 수도 있어요.

또한 만약에 `app/page.tsx`에서 `src: '../styles/fonts/my-font.ttf'`라고 폰트를 불러온다면, 실제로 프로젝트 루트에 있는 `styles/fonts` 폴더 안에 `my-font.ttf`가 위치해야 해요. 즉, 경로는 호출하는 위치 기준으로 상대 경로를 잘 맞춰주는 게 중요합니다.

---

### weight

이건 폰트 굵기를 지정하는 속성인데, 숫자가 커질수록 더 두꺼운 글자가 돼요. 보통 100부터 900까지 설정 가능합니다.

- 100: 얇은 글씨 (Thin)
- 400: 일반 글씨 (Regular)
- 700: 두꺼운 글씨 (Bold)

여기서 한 가지 팁! 만약 폰트 파일마다 굵기가 다 다르다면, 각각 다른 파일을 넣어주고 `weight`를 명시해줘야 나중에 CSS에서 굵기 조절할 때 자연스럽게 적용돼요.

---

### 추가 팁!

- 폰트 파일 경로 설정할 때, 상대 경로가 헷갈릴 수 있는데 `app` 폴더를 루트처럼 생각하는 경우가 많으니, 프로젝트 구조를 한 번 확실히 파악하고 써보세요.
- 그리고 폰트 포맷 종류도 중요해요. 보통 최신 웹에서는 `woff2`를 권장하는데, 크기도 작고 브라우저 호환성도 좋아서요.
- 여러 스타일과 굵기를 쓰고 싶으면 위에 예제처럼 배열 안에 객체 형태로 주는 방법이 가장 깔끔해요.

꼭 한 번 폰트 파일 위치와 경로, weight 옵션 등 기본 세팅을 확실히 잡고 시작하면 나중에 폰트 적용이 훨씬 편해질 거예요!

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

폰트 굵기(font weight)에 대해 이야기해볼게요!

폰트 굵기 설정은 다음과 같은 형식으로 사용할 수 있어요:

- **문자열(String)**: 해당 폰트에서 지원하는 굵기 값을 나열하거나, 만약 가변(variable) 폰트라면 범위 값을 줄 수 있어요.
- **배열(Array)**: 만약 구글 폰트(Google Font) 중에 가변 폰트가 아니라면, 사용할 굵기 값들을 배열 형태로 넣어줘야 해요.

이 설정은 React의 Next.js 라이브러리 중 `next/font/google`과 `next/font/local`에서 주로 적용됩니다.

특히 중요!

| 상황                         | 굵기 설정 필수 여부          |
|-----------------------------|-----------------------------|
| 가변 폰트(variable font)를 사용 | 선택적 (범위를 줄 수 있음)    |
| 가변 폰트가 아닌 구글 폰트 사용 | 반드시 필요 (배열 형태로 줘야 함) |
| 로컬 폰트(`next/font/local`) 사용  | 상황에 따라 필요할 수 있음      |

즉, 가변 폰트를 쓰면 좀 더 자유롭게 굵기 범위를 조절할 수 있는 반면, 일반적인 구글 폰트들은 원하는 굵기 값들을 명시해줘야 정상적으로 적용된다는 뜻이에요.

혹시 굵기 설정을 안 해주면 어떻게 될까? 대부분 기본 굵기(보통 400)로 적용되고, 원하는 다양한 굵기를 활용 못 할 수도 있으니 폰트 스타일을 다양하게 보여주고 싶다면 꼭 설정해주는 것이 좋아요.

예를 들어,

```js
import { Roboto } from 'next/font/google';

const roboto = Roboto({
  weight: ['400', '700'], // 400과 700 두 가지 굵기만 사용
});
```

이렇게 중복된 굵기를 배열로 지정해줌으로써 페이지 내에서 굵기 변화를 주기 쉽답니다.

요약하면, 폰트 굵기 설정은 사용자 경험 측면에서 폰트를 세밀하게 조정하는 중요한 옵션이므로, 사용하는 폰트 종류와 상황에 맞게 잘 활용해 보세요!

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

예시:

- weight: `400`  
  단일 weight 값을 나타내는 문자열입니다. 예를 들어 폰트 Inter의 경우 가능한 값은 `100`, `200`, `300`, `400`, `500`, `600`, `700`, `800`, `900` 또는 기본값인 `variable`입니다.
- weight: `100 900`  
  변수 폰트(variable font)에서 100부터 900 사이의 범위를 나타내는 문자열입니다.
- weight: [`100`, `400`, `900`]  
  변수 폰트가 아닌 경우에 가능한 3가지 값으로 이루어진 배열입니다.

### style

폰트 스타일을 지정하며, 다음과 같은 옵션들이 있습니다:  

(여기서 스타일 옵션 목록이 이어질텐데, 추가 정보가 있는 경우 알려주시면 함께 도움 드릴게요!)

---

폰트 weight 같은 경우는 특히 웹에서 폰트를 효율적으로 사용할 때 중요한데요,  
변수 폰트를 쓰면 여러 굵기를 한 파일로 관리할 수 있고, 필요한 굵기만 지정해서 쓰니까 로딩 속도도 최적화할 수 있어요.  
반면에 일반 폰트는 굵기마다 별도의 파일이 있어서 용량이 커지고 로딩이 느려질 수 있답니다.  

개발할 때 이 점 참고해서 상황에 맞게 weight를 잘 지정해보세요!

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

- 문자열 값으로 기본값은 `normal`이에요.
- 만약 폰트가 가변 구글 폰트가 아니라면 스타일이 여러 개일 수 있는데, 이럴 땐 스타일 값들이 배열 형태로 들어갑니다. 이건 `next/font/google`에서만 적용돼요.

이 설정은 `next/font/google`과 `next/font/local`에서 사용할 수 있습니다.

- 선택적(필수는 아님)

### 예시

| 속성               | 설명                         |
|------------------|----------------------------|
| `"normal"`       | 기본 스타일 (보통 굵기, 기울임 없음) |
| `["normal", "italic"]` | 보통과 이탤릭 스타일 같이 사용할 때 |


참고로, 구글 폰트를 다룰 때 스타일을 배열로 넘기면 여러 폰트 스타일을 한 번에 불러와서 상황에 따라 쉽게 사용할 수 있어서 편해요.  
가변 폰트는 `wght` 같은 굵기(weight) 변수 값을 직접 조절할 수 있지만, 비가변 폰트는 이런 식으로 여러 스타일을 배열에 담아 관리하면 되니 기억해두면 좋아요!

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

Font 스타일과 subsets 옵션에 대해 좀 더 쉽게 설명해볼게요.

---

## 스타일(style)에 대해 알아볼까요?

- **`italic`**: 문자열 형태에요. 예를 들어, `italic` 또는 `normal` 값을 줄 수 있는데, 주로 **next/font/google**에서 사용돼요. 즉, 글씨를 기울이거나(normal) 기본 상태로 설정할 때 쓰는 거죠.
  
- **`oblique`**: 이건 또 다른 문자열 스타일인데, 사실 **next/font/local** 쪽에서 사용해요. 어떤 값이든 넣을 수는 있지만, 보통 표준 폰트 스타일에서 많이 쓰는 값들을 넣는 경우가 많아요.
  
- **`['italic', 'normal']`**: 흥미롭게도, next/font/google에서는 이렇게 배열로 두 가지 값을 받을 때도 있어요. 첫 값과 두 번째 값이 각각 normal, italic 중 하나가 들어가는 식이죠.

즉, 같은 스타일 프로퍼티인데도 사용하는 환경(google 폰트인지, local 폰트인지)에 따라 받을 수 있는 값이 다르니 참고하세요.

---

## subsets 옵션은 뭐하는 거야?

**subsets**는 폰트의 문자 집합을 의미해요. 예를 들어, 한글 폰트라면 기본 라틴 문자뿐 아니라 한글도 포함되어야겠죠? 이걸 배열로 써서 어떤 문자 집합을 미리 불러올지 설정해요.

예를 들면:

```js
subsets: ['latin', 'cyrillic', 'korean']
```

이렇게 쓰면 '라틴', '키릴 문자', '한글'을 미리 로딩해줍니다.

중요한 점은, subsets 옵션을 이용하면 폰트에 대해 관련 링크 preload 태그가 `<head>`에 자동 삽입돼서 페이지 로딩 성능이 좋아져요. 그리고 preload 옵션이 true일 때 자동 반영되는 게 기본이니 따로 신경 쓸 필요 없답니다.

---

## 정리해볼까요?

| 옵션 이름 | 타입 | 사용처 | 설명 |
|-----------|-------|--------|------------------------------------------------------------------|
| style | string 또는 string 배열 | next/font/google, next/font/local | 폰트 스타일 지정 (italic, normal, oblique 등). 환경에 따라 받는 값이 다름 |
| subsets | string 배열 | next/font/google | 미리 불러올 폰트 문자 집합 배열. 성능 최적화에 도움 |

---

> 참고로, 구글 폰트에서 제공하는 subsets는 각 언어별로 최적화된 문자만 포함하도록 설계되어 있어서, 전체 폰트를 다 불러오는 것보다 훨씬 가볍고 빠릅니다. 한국어 앱이라면 꼭 한글 subsets를 지정하는 게 좋아요!

---

이제 스타일과 subsets 옵션을 좀 더 편하게 이해할 수 있겠죠? 다음에 폰트 설정할 때 이 점만 잘 신경 써도 성능과 사용자 경험이 좋아질 거예요!

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

- Optional

예시:

| 옵션명  | 설명                         |
|---------|------------------------------|
| subsets | [`latin`]: latin 서브셋을 포함한 배열 |

서브셋(subsets)은 폰트에서 사용하는 문자 집합의 일부분을 의미해요. 예를 들어, `latin`은 영어를 비롯한 라틴 문자들을 포함하는 서브셋이죠.

더 많은 서브셋 리스트는 Google Fonts에서 원하는 폰트 페이지를 확인하면 상세히 볼 수 있어요. 다양한 언어와 문자 지원 여부를 확인할 때 유용하답니다!

참고로, 서브셋을 적절히 선택하면 페이지 로딩 속도를 개선할 수 있으니, 필요한 문자 집합만 골라서 사용하는 게 좋아요.

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

### axes

일부 variable font(가변 글꼴)에는 추가 축(axes)이 포함되어 있을 수 있어요. 기본적으로는 파일 크기를 최소화하기 위해 **글꼴 두께(font weight)** 축만 포함되어 있답니다.  
그래서 만약 더 다양한 스타일(예: 기울기, 폭 등)을 활용하고 싶다면, 해당 폰트가 지원하는 축들이 무엇인지 확인하고 추가로 포함시켜야 해요.

예를 들어, 구글 폰트에서 제공하는 variable font 중 일부는 `wght`(글꼴 두께) 외에 `wdth`(글자 폭)나 `ital`(이탤릭) 같은 축들을 지원할 수 있죠.  
하지만 모든 폰트가 모든 축을 지원하는 건 아니니까, 사용하려는 폰트의 상세 설명을 참고하는 게 좋아요.

> **참고:** Next.js에서 `next/font/google`을 사용할 때, 이 옵션은 선택 사항이에요. 필요할 때만 추가해 주면 된답니다.

| 축 이름 (Axis Tag) | 설명                  | 예시                  |
|-----------------|---------------------|---------------------|
| wght            | 글꼴 두께 (Weight)      | 100 ~ 900           |
| wdth            | 글자 폭 (Width)         | 50% ~ 200%          |
| ital            | 이탤릭 (Italic)          | 0(기본), 1(이탤릭)    |

만약 여러분이 스타일 변화를 워낙 많이 주고 싶으면, 여러 축을 활용해보세요!  
그 덕분에 폰트 파일이 커질 수 있으니, 용도에 맞게 적절히 조절하는 게 중요합니다.  

궁금하면 직접 variable font를 지원하는 툴이나 브라우저에서 축 값을 조절해 보는 것도 재미있는 경험이 될 거예요!

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

예시를 들어 설명해볼게요:

- axes: [`slnt`]: 이건 Inter라는 가변 폰트에서 `slnt`라는 추가 axes(축)를 나타내는 배열이에요. 이 폰트는 기울기(slant)를 조절할 수 있거든요. 여러분이 사용하는 폰트가 어떤 axes를 지원하는지는 구글 가변 폰트 페이지에서 필터를 활용해 `wght`(굵기) 이외에 다른 축들을 찾아보면 쉽게 확인할 수 있어요.

---

### display 속성

`display` 속성은 폰트가 브라우저에 어떻게 보여질지 결정하는 값이에요. 가능한 문자열 값들은 다음과 같아요:

| 값      | 설명                                                                       |
|---------|----------------------------------------------------------------------------|
| `auto`  | 기본 동작으로, 브라우저가 최적이라고 판단하는 방식을 따릅니다.             |
| `block` | 폰트가 로드될 때까지 텍스트를 숨깁니다. (Flash of Invisible Text - FOIT)    |
| `swap`  | 폰트가 로드되기 전까지 시스템 기본 폰트로 대체하여 보여줍니다. (Flash of Unstyled Text - FOUT), 기본값이에요. |
| `fallback` | 짧은 시간 동안 기본 폰트로 보여주고, 폰트가 로드되면 변경됩니다.            |
| `optional` | 네트워크 상황이 좋지 않거나 폰트가 늦게 로드될 경우 기본 폰트를 계속 사용합니다.|

이 display 속성을 잘 활용하면 웹 폰트 로딩 시 발생할 수 있는 깜빡임이나 보이지 않는 텍스트 문제를 어느 정도 해결할 수 있어요. 기본적으로는 `swap`이 많이 쓰이는데, 이는 사용자 경험을 위해 빠르게 텍스트를 보여주고 나중에 폰트를 바꾸기 때문이에요. 만약 사용자에게 깜빡임이 전혀 보이지 않게 하려면 `block`을 선택할 수도 있지만, 이 경우 폰트가 늦게 로드될 때 텍스트가 아예 보이지 않는 문제가 생길 수 있어요.

웹 폰트 성능 최적화를 고려할 때 `display` 옵션을 꼭 체크해보세요!

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

Next.js 14부터 도입된 `next/font/google`와 `next/font/local`에서 `display` 옵션을 사용할 수 있어요. 이 옵션은 폰트 로딩 방식을 조절하는데, 특히 사용자 경험에 영향을 많이 끼치죠.

### display 옵션이란?

- **`optional`**: 브라우저가 폰트를 빨리 로드하지 못할 경우, 시스템 기본 폰트로 빠르게 폴백(fallback)하도록 하는 값이에요. 폰트가 늦게 로딩되면 폰트 플래시 현상(FOIT, Flash of Invisible Text)을 줄일 수 있어서 사용자 입장에서 깔끔한 경험을 할 수 있죠.

다른 display 값들도 있지만, `optional`은 특히 모바일 환경이나 네트워크 속도가 느릴 때 효과적이에요.

---

### 간단 사용 예시

```js
import localFont from 'next/font/local';

const myFont = localFont({
  src: './my-font.woff2',
  display: 'optional',  // 이 부분이 display 옵션 설정
});
```

또는 Google Fonts 사용 시:

```js
import { Roboto } from 'next/font/google';

const roboto = Roboto({
  weight: '400',
  display: 'optional',
});
```

---

### display 옵션 종류 정리표

| 값         | 설명                                             |
|------------|--------------------------------------------------|
| `auto`     | 기본값, 브라우저가 적절히 판단해서 렌더링함      |
| `block`    | 폰트가 로드될 때까지 텍스트가 보이지 않음(FOIT)  |
| `swap`     | 기본 폰트로 먼저 렌더링 후, 폰트가 로드되면 교체  |
| `fallback` | `block`처럼 잠깐 숨겼다가, 기본 폰트로 바꿈       |
| `optional` | 네트워크 상태에 따라 빠른 폰트 교체 또는 기본 폰트 사용 |

---

### 참고사항

- `optional`로 설정하면 네트워크가 나쁠 때 폰트 플래시 문제를 줄일 수 있지만, 폰트가 늦게 로드되거나 아예 로드되지 않는 경우에도 텍스트가 잘 보여서 사용자 경험이 개선돼요.
- 반대로 `block`은 폰트가 로딩될 때까지 텍스트가 아예 안 보이기 때문에 SEO나 접근성 측면에서 고려가 필요해요.
- 폰트 최적화를 위해선 프로젝트 상황에 맞게 `display` 옵션을 조절하는 것이 중요합니다.

---

궁금한 점 있으면 댓글로 남겨주세요! 폰트 로딩을 잘 설정하면 웹사이트 퍼포먼스와 사용자 경험이 확 달라진답니다.😉

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

### preload

`preload`는 폰트를 미리 불러올지 말지를 정하는 불리언(boolean) 값이에요. 기본값은 `true`로 설정되어 있죠.

이 옵션은 Next.js에서 `next/font/google`이나 `next/font/local`처럼 폰트를 불러올 때 자주 사용되는데요, 폰트를 미리 불러오면 페이지가 렌더링될 때 폰트가 더 빨리 적용되어 사용자 경험이 좋아지는 장점이 있어요.

하지만 꼭 미리 로딩이 필요한 게 아닐 때는 `preload: false`로 설정해서 네트워크 리소스를 아낄 수도 있답니다. 예를 들어, 특정 폰트를 나중에(사용자가 어떤 특정 모드를 선택했을 때) 써야 한다면 미리 로딩하지 않는 게 효율적일 수 있겠죠.

---

#### 정리하자면

| 옵션명 | 타입    | 기본값 | 설명                                    |
|-------|--------|-------|---------------------------------------|
| preload | boolean | true  | 폰트를 미리 로드할지 여부를 지정해요. |

---

참고로, 폰트 프리로드는 크롬 같은 주요 브라우저에서 더 빠른 렌더링을 도와주는 기술이에요. 다만 너무 많은 폰트를 한꺼번에 프리로드하면 오히려 초기 로딩 시간을 늘릴 수 있으니 적절히 사용하는 게 좋아요!

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

예시:

- preload: false

### fallback

폰트가 로드되지 않을 때 사용할 대체 폰트입니다. 기본 폰트 없이 대체할 폰트들을 문자열 배열 형태로 지정하는 거예요.

---

추가로 설명하자면, 웹에서 커스텀 폰트를 쓸 때 가끔 폰트가 늦게 로딩되거나 실패할 수 있잖아요? 이럴 때 미리 지정한 fallback 폰트들이 대신 적용돼서 사용자 경험이 깨지지 않도록 도와줘요. 예를 들어 'Roboto' 폰트가 로드되지 않으면 'Arial'이나 'sans-serif' 같은 기본 폰트로 바꿔주는 거죠.


fallback: ['Arial', 'Helvetica', 'sans-serif']


이런 식으로 배열로 적어주면 되고, 순서대로 브라우저에게 적용 가능한 폰트를 찾아 사용하게 해요. 이렇게만 잘 활용해도 웹사이트 텍스트가 깔끔하게 보여서 좋답니다!

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

- Optional

next/font/google과 next/font/local에서 사용돼요.

예를 들어:

- fallback: [`system-ui`, `arial`]: 폰트가 제대로 로드되지 않을 때 대신 사용할 폰트들을 배열로 설정하는 거예요. 여기서는 `system-ui`나 `arial` 같은 시스템 기본 폰트들이 fallback으로 들어가 있죠.

폰트 로딩에 있어 fallback 설정은 사용자 경험에서 꽤 중요해요. 왜냐하면 웹폰트가 느리게 로딩되거나 실패할 때, 아무 글씨도 안 보이거나 이상한 글꼴로 보이면 사용자가 불편하거든요. 그래서 `fallback`에 신뢰할 수 있는 기본 폰트를 적절히 지정해 두면, 렌더링이 순조롭게 이뤄져요.

혹시 여러분 프로젝트에서도 Google 폰트나 로컬 폰트를 쓸 때 이 `fallback` 옵션 꼭 활용해 보세요!

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

### adjustFontFallback

- **next/font/google**에서 사용하는 옵션으로, 자동으로 폰트 대체(fallback) 폰트를 적용할지 여부를 설정하는 불리언 값입니다. 이 기능을 켜면 Cumulative Layout Shift(CLS, 누적 레이아웃 이동)를 줄여서 사용자 경험을 더 좋게 만들어 줍니다. 기본값은 `true`입니다.
- **next/font/local**에서는 문자열 또는 불리언 false 값을 받으며, 마찬가지로 자동 폰트 대체를 설정합니다. 여기서는 `"Arial"`, `"Times New Roman"` 중 하나 또는 `false`를 사용할 수 있는데, 기본값은 `"Arial"`입니다.  

| 속성                  | next/font/google                | next/font/local                   | 기본값          | 설명                                      |
|---------------------|-----------------------------|------------------------------|---------------|-----------------------------------------|
| **adjustFontFallback** | boolean (true/false)           | string ("Arial", "Times New Roman") or false | true (google), "Arial" (local) | 자동 fallback 폰트 적용 여부 (CLS 감소 목적) |

> 참고로, CLS는 웹 페이지가 로딩될 때 글자가 갑자기 바뀌거나 레이아웃이 흔들리는 현상으로 사용자에게 불편을 주는데, 이 옵션을 적절히 사용하면 그런 현상을 줄일 수 있어서 페이지가 훨씬 안정적으로 느껴집니다. 로컬 폰트 쓸 때는 원하는 fallback 폰트를 직접 지정할 수 있는 점도 편리하니 참고하세요!

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

예시:

- `adjustFontFallback: false`: `next/font/google`에 사용할 때
- `adjustFontFallback: 'Times New Roman'`: `next/font/local`에 사용할 때

### variable

스타일을 CSS 변수 방식으로 적용할 때 사용할 CSS 변수 이름을 지정하는 문자열 값이에요.

---

조금 더 설명하자면, `adjustFontFallback` 옵션은 폰트가 로드되지 않았을 때 대체 폰트를 어떻게 처리할지 결정하는 설정입니다. 예를 들어, Google 폰트를 쓸 때는 기본값으로 그냥 false를 주면서 별도의 fallback 폰트 조정 없이 사용할 수 있지만, 로컬 폰트를 쓸 땐 직접 fallback 폰트를 지정할 수 있어요.

그리고 `variable` 속성은 CSS 변수를 활용할 때 변수 이름을 정하는 부분인데, 이걸 잘 활용하면 다크모드나 테마 변경 같은 동적 스타일 변경에 훨씬 유용합니다. 예를 들어, `"--my-font-variable"` 같은 이름으로 지정하면 CSS 내에서 `var(--my-font-variable)`로 폰트를 쉽게 관리할 수 있어요.

이런 식으로 적절한 옵션 조합을 통해 폰트 로딩과 스타일 적용을 더 세밀하게 제어할 수 있으니 참고하세요!

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

next/font/google과 next/font/local에서 사용하는 옵션 중 하나를 소개할게요.

### 선택적(옵션) 속성 예시

#### variable: `--my-font`
여기서 `variable`은 CSS 변수 이름을 지정할 때 쓰는 옵션이에요. 예를 들어 `--my-font`라고 하면, 이 폰트를 가리키는 CSS 변수가 `--my-font`로 선언된다는 뜻이죠.

```jsx
import localFont from 'next/font/local';

const myFont = localFont({
  src: './my-font.woff2',
  variable: '--my-font',
});
```

위처럼 설정하면, CSS에서 `var(--my-font)`로 이 폰트를 참조할 수 있어요.

왜 이게 유용하냐면, 프로젝트에서 여러 군데서 일관된 폰트를 쓰고 싶을 때, CSS 변수를 사용하면 한 번에 관리하기 편하거든요. 폰트를 바꾼다거나 할 때도 변수만 바꾸면 전체에 바로 반영되니까요.

마치 CSS 커스텀 프로퍼티를 사용하는 것과 같은 개념이에요. Next.js의 `next/font`는 이렇게 편리한 방식으로 폰트를 다룰 수 있게 도와주니까, 폰트 관리에 투자하는 시간이 훨씬 줄어들 거예요!

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

### declarations

여기서 말하는 declarations는 폰트를 정의할 때 사용하는 여러 속성들을 모아놓은 배열이에요. 쉽게 말해, `@font-face`를 좀 더 자세히 설정할 수 있도록 도와주는 key-value 쌍들의 집합이죠.

이건 주로 `next/font/local`에서 쓰이는데, 꼭 필요한 건 아니고 선택사항이에요. 그러니까 필요한 경우에만 폰트 스타일, 무게, 범위 같은 추가 정보를 덧붙이고 싶을 때 사용하면 좋아요.

예를 들면, `font-weight`나 `unicode-range` 같은 세부 설정을 통해 폰트가 더욱 정확하게 로드되도록 제어할 수 있어요. 이런 걸 잘 활용하면 웹사이트를 더 깔끔하고 빠르게 만들 수 있으니 참고해보세요!

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

예시:

- 선언: [' prop: `ascent-override`, value: `90%` ']

## 스타일 적용하기

폰트 스타일을 적용하는 방법은 크게 세 가지가 있어요:

- 인라인 스타일(inline style)로 직접 선언하기
- CSS 클래스에 스타일 지정하기
- JavaScript 등 스크립트로 동적으로 스타일 추가하기

이 중에서 어떤 방법을 쓸지는 프로젝트 상황에 따라 달라지는데, 간단한 테스트나 빠른 적용에는 인라인 스타일이 편하고, 유지보수가 중요한 경우 CSS 클래스로 관리하는 게 좋아요. 혹은 복잡한 사용자 인터랙션이 필요하면 JavaScript를 활용해서 동적으로 바꾸는 방법도 쓸 수 있답니다.

궁금하면 더 자세히 설명해줄게요!

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

좋아요, 이번에는 웹 개발할 때 자주 쓰이는 세 가지 CSS 관련 속성에 대해 이야기해볼게요: `className`, `style`, 그리고 CSS Variables에 대해서요.

---

### className

`className`은 리액트(React)에서 HTML 요소에 CSS 클래스를 지정할 때 쓰는 속성이에요. 보통 HTML에서는 `class`라고 적지만, 리액트에서는 `class`가 자바스크립트 예약어라서 `className`으로 표기해요.

예를 들어, 어떤 폰트를 불러왔다고 할 때, 그 폰트와 연결된 CSS 클래스를 다음처럼 사용할 수 있어요.

```jsx
<p className={inter.className}>Hello, Next.js!</p>
```

여기서 `inter.className`은 불러온 'Inter' 폰트와 연결된 고유한 클래스명이에요. 이렇게 하면 해당 폰트 스타일이 `p` 태그에 적용돼서 글자가 예쁘게 잘 보이게 되죠.

---

### style

`style`은 HTML 요소에 인라인 CSS를 직접 넣을 때 사용하는 속성이에요. 리액트에서는 스타일을 객체 형태로 넘겨줘야 해요.

```jsx
<p style={{ color: 'blue', fontSize: '16px' }}>Styled text</p>
```

이렇게 하면 글자 색이 파란색이고 글씨 크기가 16px인 텍스트를 만들 수 있죠. 간단해서 편하긴 한데, 코드가 길어지면 가독성이 떨어질 수 있으니 너무 남발하는 건 피하는 게 좋아요.

---

### CSS Variables (커스텀 프로퍼티)

CSS Variables, 즉 커스텀 프로퍼티는 CSS 안에서 변수를 선언하고 재사용할 수 있게 도와줘요. 예를 들어, 자주 쓰는 색상을 변수에 담아두면 유지보수할 때 아주 편하죠.

```css
:root {
  --main-color: #4CAF50;
  --padding: 10px;
}

.button {
  background-color: var(--main-color);
  padding: var(--padding);
}
```

리액트나 Next.js에서도 CSS 변수를 사용할 수 있는데, 특히 다크모드 같은 테마를 구현할 때 유용해요. 예를 들어, 자바스크립트로 CSS 변수를 조작해서 실시간으로 스타일이 바뀌게 만들 수도 있답니다.

---

### 추가 팁!

- **className**과 **style**은 상황에 따라 적절히 섞어 쓰는 게 좋아요. 복잡하거나 여러 요소에 적용하는 스타일은 CSS 클래스(`className`)로 관리하고, 간단히 몇 가지 스타일만 바꾸고 싶을 땐 `style`을 쓰는 게 효율적이죠.

- 프로젝트가 커질수록 CSS-in-JS 라이브러리(예: styled-components, emotion)를 활용해서 `className`과 `style`을 관리하면 훨씬 편리해져요.

---

요약하자면,  
- `className`: CSS 클래스를 붙일 때  
- `style`: 인라인 스타일 직접 쓸 때  
- CSS Variables: 반복되는 스타일 값들을 변수처럼 처리할 때

이걸 잘 활용하면 깔끔하고 유지보수가 쉬운 스타일 관리를 할 수 있으니 꼭 기억해 주세요!

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

### style

이 스타일은 로드된 폰트에 대해 읽기 전용 CSS 스타일 객체를 반환해주는데요, 이 스타일 객체는 HTML 요소에 바로 적용할 수 있어요. 여기에는 `style.fontFamily` 속성이 포함되어 있어서, 폰트 패밀리 이름과 함께 대체 폰트들도 쉽게 접근할 수 있습니다.

예를 들어, 아래처럼 사용할 수 있죠:

```jsx
<p style={inter.style}>Hello World</p>
```

위 코드를 보면, `inter.style`이 바로 `fontFamily` 정보를 포함한 CSS 스타일 객체를 반환해서, 텍스트에 원하는 폰트가 바로 적용되는 모습을 볼 수 있어요.

> 참고로, 이렇게 스타일 객체를 활용하면 별도로 CSS 파일을 수정하지 않아도 되고, 컴포넌트 단위로 쉽게 폰트를 적용할 수 있어서 유용합니다.

### CSS Variables

물론, CSS 변수 방식으로도 폰트를 제어할 수 있는데요, 이 방법은 스타일이 적용되는 범위를 더 세밀하게 조절할 수 있고, 전역 스타일과도 잘 어울립니다. 필요하다면 다음번에 CSS 변수 사용법에 대해서도 알려드릴게요!

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

외부 스타일 시트에 스타일을 설정하고 추가 옵션들을 지정하고 싶다면, CSS 변수 방법을 활용하는 게 좋아요.

폰트를 불러오는 것뿐만 아니라, CSS 변수가 정의된 CSS 파일도 함께 import한 다음, 폰트 로더 객체의 variable 옵션에 변수를 이렇게 설정하면 됩니다:

```js
import { Inter } from 'next/font/google'
import styles from '../styles/component.module.css'
 
const inter = Inter({
  variable: '--font-inter',
})
```

그리고 폰트를 적용하고 싶은 텍스트의 부모 컨테이너에는 이 폰트 로더의 변수 값을 className으로 넣어주고, 텍스트에는 외부 CSS 파일에서 가져온 styles의 클래스를 넣어주면 돼요.

간단하게 요약하면 이렇게 사용할 수 있죠:

| 역할           | 할당하는 className 값             |
| -------------- | ------------------------------- |
| 텍스트 부모 컨테이너 | `inter.variable` (예: `"--font-inter"`에 해당하는 클래스) |
| 텍스트 요소       | `styles`에서 임포트한 CSS 클래스 (예: `styles.textClass`) |

이 방법의 장점은 폰트 설정을 CSS 변수로 관리하다 보니, 폰트를 바꾸거나 조정할 때 외부 CSS만 수정하면 되서 유지보수가 훨씬 편해진다는 점이에요. 그리고 스타일과 폰트 로딩 코드를 깔끔하게 분리할 수 있어서 프로젝트 구조가 한결 명료해집니다.

참고로, CSS 변수는 전역적으로도 사용할 수 있으니, 이렇게 지정해 놓으면 여러 컴포넌트에서 동일한 폰트를 쉽게 재사용할 수 있는 점도 기억해두세요!

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

자, 위에 코드를 보면 React 컴포넌트 안에서 클래스를 이용해 글자 스타일을 지정하고 있어요. 간단히 설명하자면, `Hello World` 텍스트에 `Inter` 폰트를 적용하고, 폰트 두께는 200(얇음), 스타일은 이탤릭으로 주는 거죠.

먼저, 컴포넌트 내 JSX는 이렇게 생겼고요:

```jsx
<main className={inter.variable}>
  <p className={styles.text}>Hello World</p>
</main>
```

그리고 `component.module.css` 파일 안에 `.text` 클래스가 아래처럼 정의되어 있답니다:

```css
.text {
  font-family: var(--font-inter);
  font-weight: 200;
  font-style: italic;
}
```

여기서 중요한 점은 `var(--font-inter)` 부분이에요. CSS 변수로 지정된 폰트 이름인데, 아마 글로벌 CSS나 루트에서 `--font-inter`가 Inter 폰트를 가리키는 값으로 설정돼 있을 거예요. 이렇게 하면 폰트를 프로젝트 어느 곳에서나 일관되게 사용할 수 있고, 만약 폰트를 바꾸고 싶으면 변수값만 수정하면 돼서 관리도 편하답니다.

또, `font-weight: 200`은 폰트가 얇게 나오도록 해주고, `font-style: italic`으로 기울임꼴(이탤릭) 효과를 줍니다. 폰트는 사용자를 위한 시각적 느낌을 많이 좌우하니까, 이렇게 세심한 조정을 해주는 게 중요해요.

추가 팁을 하나 드리면, Inter 같은 웹폰트를 사용할 때는 로딩 속도를 위해서 구글 폰트 라이브러리를 사용하거나, 로컬에 폰트를 두고 `@font-face`로 불러오는 방법도 좋아요. 그리고 여러 웹폰트 옵션들(font-weight, font-style 등)을 미리 로드해두면, 사용자 경험이 더 매끄러워집니다!

정리해보면 간단하게:

| 부분          | 역할                               |
| ------------- | --------------------------------- |
| `className={styles.text}` | CSS 모듈에서 정의한 텍스트 스타일 적용 |
| `font-family: var(--font-inter)` | Inter 폰트 사용 설정(변수로 관리)       |
| `font-weight: 200`         | 얇은 글씨 두께 지정                   |
| `font-style: italic`       | 이탤릭체 스타일 적용                  |

이렇게 해놓고 필요하면 더 세밀하게 스타일도 추가하고, 폰트도 다양하게 적용해보세요. 폰트 하나 바꾸는 것만으로도 사이트 분위기가 확 달라질 수 있답니다!

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

## 폰트 정의 파일(font definitions file) 활용하기

로컬 폰트나 구글 폰트를 사용할 때마다 해당 폰트가 애플리케이션에 하나의 인스턴스로 호스팅됩니다. 그래서 여러 군데에서 같은 폰트를 쓴다면, 매번 호출하는 것보다 한 곳에서 폰트를 불러오고 필요한 곳에서 그 폰트 객체를 가져다 쓰는 게 훨씬 효율적이에요. 이걸 위해 폰트 정의 파일을 만드는 방법을 추천합니다.

예를 들어, 프로젝트 루트에 `styles` 폴더를 만들고 그 안에 `fonts.ts` 파일을 만듭니다.

그리고 이렇게 폰트 정의를 작성해보세요:

```ts
// styles/fonts.ts
import { localFont } from '@next/font/local';

export const myLocalFont = localFont({
  src: './path/to/your/font.woff2',
  weight: '400',
  style: 'normal',
  variable: '--my-local-font',
});

import { GoogleFont } from '@next/font/google';

export const myGoogleFont = GoogleFont({
  family: 'Roboto',
  weights: ['400', '700'],
  subsets: ['latin'],
  variable: '--my-google-font',
});
```

이제 이 `fonts.ts` 파일에서 내보낸 `myLocalFont`, `myGoogleFont` 같은 폰트 객체를 각각 필요한 컴포넌트나 페이지에서 불러와 사용하면 돼요.

---

### 추가 TIP!

- 이렇게 폰트를 한 곳에서 관리하면, 향후 폰트 이름이나 스타일, 굵기를 변경할 때 훨씬 간편해져요.
- `variable` 옵션을 지정해두면 CSS 변수로도 폰트를 사용할 수 있어서 스타일링 할 때 유연성이 증가한답니다.
- 특히 구글 폰트는 여러 폰트 원격 호출을 줄일 수 있어서 퍼포먼스에도 도움이 돼요.

한 번 깔끔하게 정리해두면 프로젝트 유지보수할 때 정말 큰 도움이 되니, 꼭 시도해보세요!

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

Next.js에서 구글 폰트와 로컬 폰트를 쉽게 불러와서 사용하는 방법 소개해볼게요. 

먼저, `next/font/google`과 `next/font/local`을 활용해서 폰트를 정의해봅시다.

```js
import { Inter, Lora, Source_Sans_3 } from 'next/font/google'
import localFont from 'next/font/local'

// 변수 폰트를 간단히 정의
const inter = Inter()
const lora = Lora()

// 일반(non-variable) 폰트의 두 가지 굵기 정의
const sourceCodePro400 = Source_Sans_3({ weight: '400' })
const sourceCodePro700 = Source_Sans_3({ weight: '700' })

// 로컬 폰트 정의 (styles 폴더에 GreatVibes-Regular.ttf가 있다고 가정)
const greatVibes = localFont({ src: './GreatVibes-Regular.ttf' })

export { inter, lora, sourceCodePro400, sourceCodePro700, greatVibes }
```

이렇게 하면 어느 컴포넌트에서든 폰트 클래스를 쉽게 불러와서 쓸 수 있어요.

```js
import { inter, lora, sourceCodePro700, greatVibes } from '../styles/fonts'

export default function Page() {
  return (
    <div>
      <p className={inter.className}>Inter 폰트로 인사하기</p>
      <p style={lora.style}>Lora 폰트로 인사하기</p>
      <p className={sourceCodePro700.className}>
        700 굵기의 Source_Sans_3 폰트로 인사하기
      </p>
      <p className={greatVibes.className}>Great Vibes 폰트로 멋진 제목</p>
    </div>
  )
}
```

타입스크립트 혹은 자바스크립트 환경에서 폰트 경로를 더 간편하게 관리하려면, `tsconfig.json` 또는 `jsconfig.json`에 alias를 등록하는 것도 좋아요.

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@fonts/*": ["styles/*"]
    }
  }
}
```

이 설정을 하면, 폰트 임포트할 때 이렇게 사용할 수 있죠:

```js
import { inter, lora } from '@fonts/fonts'
```

---

### 추가 팁!

- `next/font`는 SSR(서버사이드 렌더링) 환경에서도 최적화된 폰트를 자동으로 처리해주기 때문에, 따로 폰트 파일을 직접 관리하는 번거로움이 줄어들어요.
- 로컬 폰트는 상대경로가 정확해야 합니다. 폰트 위치가 바뀌면 해당 경로를 꼭 확인하세요.
- 폰트 로딩 전략이나 폰트 디스플레이 옵션도 `next/font`에서 조절할 수 있는데, 퍼포먼스 최적화에 관심이 있는 분들은 공식 문서를 한번 봐보면 좋습니다.
- `className` 과 `style` 옵션을 동시에 제공하는 경우가 있는데, 보통 변수 폰트는 `className`을, 비변수 폰트나 로컬 폰트는 `style` 혹은 `className` 모두 사용할 수 있으니 상황에 맞게 쓰시면 됩니다.

이렇게 세팅해두면 폰트를 다양하게 적용하기 편해서 UI 작업할 때 훨씬 수월해져요!

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

여러분, 오늘은 TypeScript나 JavaScript 프로젝트에서 경로 별칭(Path Alias)을 설정하는 법을 간단히 소개하려고 해요. 특히 폰트처럼 자주 쓰는 리소스를 깔끔하게 import하고 싶을 때 매우 유용하답니다!

먼저, `tsconfig.json` 혹은 `jsconfig.json` 파일에 다음과 같은 설정을 추가해 주세요:

```json
{
  "compilerOptions": {
    "paths": {
      "@/fonts": ["./styles/fonts"]
    }
  }
}
```

여기서 `"@/fonts"`란 별칭(alias)이 `./styles/fonts` 디렉터리를 가리키도록 해주는 거예요. 이제부터는 해당 폴더의 내용을 편하게 이렇게 불러올 수 있습니다:

```js
import { greatVibes, sourceCodePro400 } from '@/fonts'
```

이 방식의 장점은?  
- 복잡한 상대 경로(`../../../styles/fonts`) 대신 깔끔한 별칭 사용  
- 프로젝트 구조가 바뀌어도 alias만 수정하면 되니 유지보수 편리  
- 가독성 UP

---

### 버전별 차이 정리

| TypeScript 버전 | 경로 별칭 지원 상황                                             |
|-----------------|---------------------------------------------------------------|
| 2.0 이상        | `paths` 옵션으로 기본 지원 (단, `baseUrl`과 같이 설정 필요)     |
| 4.x 이상        | 점점 향상된 타입 추론과 함께 별칭 관련 IDE 지원 강화          |
| Next.js 등 프레임워크 | 프레임워크별 `jsconfig.json` 혹은 `tsconfig.json`에서 지원 및 별도 설정 필요 |

참고로, Next.js 프로젝트라면 `jsconfig.json` 파일에 위와 같은 `paths` 설정을 넣어주고 재시작해 주시면 바로 적용돼요!

---

### 추가 팁

- `baseUrl` 설정도 같이 해주면 편리합니다. 보통 이렇게 쓰죠:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/fonts": ["./styles/fonts"]
    }
  }
}
```

- VSCode 같은 IDE도 `jsconfig.json` 혹은 `tsconfig.json` 변경 사항을 바로 인식해서 자동완성 지원이 팍팍 될 거예요!

이렇게 하면 프로젝트를 좀 더 깔끔하게 관리할 수 있답니다. 다음에도 개발 팁으로 또 만나요! 😊

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

| Version    | Changes                                          |
|------------|--------------------------------------------------|
| `v13.2.0`  | `@next/font`가 `next/font`로 이름이 변경되었습니다. 이제 별도의 설치가 필요 없어요. |
| `v13.0.0`  | `@next/font`가 처음 도입되었습니다.              |

---

이번에 살펴볼 내용은 Next.js의 폰트 관련 변경사항이에요. 이전에는 `@next/font`라는 패키지를 별도로 설치해서 사용했는데, 13.2.0 버전부터는 이 패키지 이름이 `next/font`로 바뀌면서 따로 설치를 할 필요가 없어졌답니다.  

즉, Next.js 프로젝트에 기본 내장된 기능으로 제공되니 바로 import해서 쓸 수 있어요. 이렇게 하면 번들 크기를 줄이고, 설정도 훨씬 간편해져서 개발 편의성이 좋아졌죠.

추가로, `@next/font`가 도입된 13.0.0부터 이 기능을 사용할 수 있었으니, 만약 예전 프로젝트라면 최신 버전으로 업그레이드하는 걸 추천드려요. 최신 Next.js는 폰트를 다루기 편하게 만들어서 페이지 속도와 사용자 경험 모두 살릴 수 있답니다.