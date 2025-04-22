---
title: "Next.js 15에서 trailingSlash 설정으로 URL 끝 슬래시 처리하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:29
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "trailingSlash"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/trailingSlash"
isUpdated: false
---


# trailingSlash 설정하기

Next.js를 사용하다 보면 URL 끝에 슬래시(/)가 붙는 경우와 안 붙는 경우가 있는데요, 기본적으로 Next.js는 슬래시가 붙은 URL을 슬래시가 없는 버전으로 리다이렉트해 줍니다. 예를 들면 `/about/`로 접속하면 `/about`으로 자동 이동하는 식이죠.

근데 가끔은 반대로, 슬래시가 없는 URL을 슬래시가 붙은 URL로 리다이렉트하고 싶을 때가 있어요. 이럴 때, `next.config.js` 파일에서 `trailingSlash` 설정을 바꿔주면 됩니다.

```js
module.exports = {
  trailingSlash: true,  // true로 설정하면 슬래시가 없는 URL은 슬래시가 있는 URL로 리다이렉트됩니다.
}
```

### 추가 팁!
- 이 설정은 SEO에도 중요한 역할을 해요. URL이 일정한 형식을 갖춰야 중복 콘텐츠 문제를 막을 수 있거든요.
- 만약 여러분이 정적 사이트 생성(Static Site Generation)을 하고 있다면, 이 옵션을 사용하면 빌드 시 정적 파일들이 `/about/index.html`처럼 폴더 형태로 생성되기 때문에 호스팅 환경에 따라 좀 더 편리할 수 있습니다.

간단하니까 애플리케이션 특성에 맞게 한 번 적용해 보세요!

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

이 옵션을 설정하면 /about 같은 URL이 /about/으로 리다이렉트됩니다.

그런데 trailingSlash: true 옵션을 사용할 때는 예외가 몇 가지 있어요. 그 예외에 해당하는 URL들은 뒤에 슬래시가 붙지 않습니다:

- 확장자가 있는 정적 파일 URL들 (예: .txt, .png, .json 등)
- .well-known/ 경로 하위에 있는 모든 경로들

즉, 아래와 같은 URL들은 변경 없이 그대로 유지됩니다:

| URL                              | 설명               |
|---------------------------------|--------------------|
| /file.txt                       | 텍스트 파일         |
| images/photos/picture.png       | 이미지 파일         |
| .well-known/subfolder/config.json | .well-known 하위 설정 파일 |

추가로 말씀드리자면, 이렇게 슬래시 유무가 중요한 이유는 웹서버나 프레임워크에서 URL을 해석하는 방식 때문인데요, 보통 폴더 경로를 나타낼 때는 뒤에 슬래시를 붙이고, 파일 경로는 붙이지 않아요. 그래서 이런 규칙을 잘 지키면 SEO와 사용자 경험에도 더 좋아집니다.

만약 프로젝트에서 이 옵션을 바꿔야 할 일이 있다면, 변경 후에 URL 리다이렉트 설정도 한 번 꼭 확인해보시는 걸 추천드려요!

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

output: "export" 설정과 함께 사용하면, /about 페이지가 기본적인 /about.html 대신 /about/index.html로 출력됩니다. 이게 왜 좋냐면, URL 경로가 좀 더 깔끔해지고 폴더 구조와도 맞아떨어져서 SEO나 서버 설정 시에 유용할 수 있어요.

## 버전 히스토리

| 버전      | 변경사항               |
|-----------|-----------------------|
| v9.5.0    | trailingSlash 옵션 추가 | 

이 trailingSlash 옵션은 페이지 경로 끝에 슬래시(/)를 붙일지 말지를 결정하는 옵션이라, 웹사이트 구조를 좀 더 유연하게 관리할 수 있게 해줍니다. 만약 여러분이 정적 사이트 생성기처럼 페이지를 폴더 구조로 관리하고 싶다면 이 옵션을 적극 활용해보세요!