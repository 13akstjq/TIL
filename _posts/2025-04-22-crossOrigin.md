---
title: "Next.js 15에서 crossOrigin 속성 사용하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:04
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "crossOrigin"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/crossOrigin"
isUpdated: false
---


# crossOrigin

next/script 컴포넌트를 사용해서 생성되는 모든 `script` 태그에 `crossOrigin` 속성을 추가하고, 크로스 오리진 요청을 어떻게 처리할지 설정하고 싶을 때 `crossOrigin` 옵션을 사용하면 돼요.

예를 들어, 이렇게 설정할 수 있어요:

```js
module.exports = {
  crossOrigin: 'anonymous',
}
```

### 이 옵션, 왜 쓸까?

보통 다른 도메인에서 자원을 불러올 때 CORS 정책에 걸리기 쉬운데, 이 속성을 지정하면 브라우저가 스크립트를 어떻게 처리할지 알려주는 거예요. 예컨대, `anonymous`로 설정하면 자격 증명(쿠키, HTTP 인증 등) 없이 요청하게 된다거나 말이죠.

## Options

옵션으로는 주로 다음 값을 쓸 수 있어요:

| 옵션 값   | 설명                                                 |
|----------|------------------------------------------------------|
| `anonymous` | 자격 증명 없이 크로스 오리진 요청을 보냅니다.          |
| `use-credentials` | 쿠키, 인증 토큰 등을 포함한 자격 증명과 함께 요청됩니다. |

사실, `crossOrigin`을 제대로 설정하면, 브라우저 캐싱 전략에도 영향을 미치고 보안상 이점도 얻을 수 있어요. 예를 들어, `anonymous` 속성이 설정된 스크립트는 만약 CORS가 실패하면 로드가 차단되니까, 무분별한 외부 스크립트가 실행되는 걸 막을 수 있죠.

꼭 필요한 경우에만 써보고, 사용 전에 도메인의 CORS 정책도 확인하는 게 좋아요!

더 자세한 내용이나 궁금한 점 있으면 편하게 물어봐 주세요~

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

안녕하세요! 오늘은 웹 개발할 때 이미지나 리소스를 불러올 때 사용하는 `crossOrigin` 속성에 대해 간단하게 알려드릴게요.

`crossOrigin` 속성은 주로 `<img>`, `<script>`, `<link>` 태그 같은 요소에서 외부 리소스를 가져올 때, 브라우저가 CORS(Cross-Origin Resource Sharing) 정책을 어떻게 처리할지 결정해줍니다. 여기에 두 가지 주요 옵션이 있습니다.

| 옵션          | 설명                                       |
| ------------- | ------------------------------------------ |
| `anonymous`   | `crossOrigin="anonymous"` 속성을 추가해요. 서버에 인증 정보(쿠키나 HTTP 인증 정보)를 보내지 않고 리소스를 요청합니다. 대부분의 경우 이 설정을 추천해요. |
| `use-credentials` | `crossOrigin="use-credentials"` 속성을 추가해요. 요청에 인증 정보를 포함시켜서 서버에 보냅니다. 주로 인증된 사용자에게만 보여줘야 하는 리소스를 불러올 때 사용합니다. |

### 잠깐, 왜 `crossOrigin` 설정이 필요하냐고요?

브라우저는 보안을 위해 기본적으로 다른 도메인에서 불러오는 리소스에 제한을 둡니다. 예를 들어, 당신의 웹사이트에서 다른 도메인의 이미지를 불러왔을 때, 그 이미지에 대해 직접적인 접근(예: 캔버스에 그려서 픽셀 정보를 읽는 것)이 제한될 수 있죠. 이때 `crossOrigin` 속성을 설정하면, 서버가 CORS 규칙에 따라 맞게 응답하면 이런 제한을 우회할 수 있습니다.

### 팁!

- `anonymous`를 쓸 때는 서버가 반드시 CORS 헤더를 적절히 설정해줘야 해요 (`Access-Control-Allow-Origin`).
- `use-credentials`를 쓰면 서버가 `Access-Control-Allow-Credentials: true` 헤더를 포함시켜야 하고, `Access-Control-Allow-Origin`은 와일드카드(`*`)가 아니어야 합니다.

요약하면, 평범한 이미지나 스크립트 로딩에는 `anonymous`를 주로 쓰고, 인증이 필요한 리소스에는 `use-credentials`를 써보세요. 이렇게 설정하면 보안도 챙기고, 개발도 한결 수월해질 거예요! 😊