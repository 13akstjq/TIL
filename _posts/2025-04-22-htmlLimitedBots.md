---
title: "Nextjs 15 htmlLimitedBots사용 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:11
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "htmlLimitedBots"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/htmlLimitedBots"
isUpdated: false
---


# htmlLimitedBots 옵션 알아보기

안녕하세요! 오늘은 Next.js 설정 중 `htmlLimitedBots`에 대해 쉽고 재밌게 이야기해볼게요. 이 설정은 특정 봇(bots)이나 크롤러(crawlers)에게 ‘스트리밍 메타데이터(streaming metadata)’ 대신 ‘블로킹 메타데이터(blocking metadata)’를 보내고 싶을 때 사용해요.

---

### htmlLimitedBots란?

웹페이지를 로드할 때, 보통 사용자의 브라우저에 최적화된 방식으로 데이터를 보내게 되는데요. 이 중에서 일부 봇이나 크롤러는 스트리밍 방식이 제대로 작동하지 않을 수 있어요. 그래서 이런 봇들에게는 스트리밍 대신 **정적으로 완성된 HTML 메타데이터**를 보내도록 설정할 수 있답니다.

### 사용법

`NextConfig`에서 `htmlLimitedBots` 키에 봇의 유저 에이전트(user agent) 이름들을 '|'(파이프 기호)로 구분해서 적어주세요.

```ts
import type { NextConfig } from 'next'

const config: NextConfig = {
  htmlLimitedBots: 'MySpecialBot|MyAnotherSpecialBot|SimpleCrawler',
}

export default config
```

위 코드를 보면, `MySpecialBot`, `MyAnotherSpecialBot`, 그리고 `SimpleCrawler` 이 세 가지 유저 에이전트에 대해 차단(블로킹) 메타데이터를 보내도록 설정하는 거예요.

---

### 참고 사항

- 이 옵션을 지정해주지 않으면 Next.js 내부의 기본 봇 리스트가 적용돼요.
- 봇이나 크롤러가 특정 방식의 메타데이터를 필요로 할 때, 이 설정으로 유연하게 대응할 수 있어서 SEO나 크롤링 최적화에 도움을 줍니다.
- 스트리밍 대신 블로킹 메타데이터를 보내는 이유는, 봇들이 스트리밍 데이터를 제대로 처리하지 못할 가능성 때문에 안정적인 크롤링 결과를 보장하기 위해서예요.

---

### 기본 봇 리스트

(여기에 Next.js의 기본값으로 설정된 봇 리스트를 보여주면 좋지만, 공식 문서에서 확인하면 편해요!)

---

### 추가 팁

만약 내가 관리하는 사이트에 너무 많은 봇이 방문해서 스트리밍 처리가 오작동하거나 느려진다면, 여기 추가해서 블로킹 메타데이터로 대응하는 것도 좋은 방법입니다. 그리고 SEO 도구를 이용해 어떤 봇들이 내 사이트에 방문하는지 확인하는 것도 잊지마세요!

---

다음에도 재미있고 유용한 개발 이야기를 들고 올게요! 궁금한 점 있으면 댓글로 알려주세요~ 😊

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

Next.js는 기본적으로 제한된 HTML 봇 리스트를 포함하고 있어요.

`htmlLimitedBots` 설정을 직접 지정하면 Next.js의 기본 리스트를 덮어쓰게 되서, 어떤 유저 에이전트가 이 동작을 적용받을지 완전히 제어할 수 있어요. 하지만 이건 고급 설정에 해당되니, 대부분의 경우엔 기본 설정만으로도 충분하답니다.

버전 히스토리도 간단히 살펴볼게요.

| Version | Changes                     |
|---------|-----------------------------|
| 15.2.0  | `htmlLimitedBots` 옵션 도입 | 

참고로, 이런 봇 리스트 제어는 SEO나 크롤링 관련 최적화를 할 때 유용하게 활용할 수 있어요. 필요하지 않으면 그냥 기본값 쓰는 걸 추천합니다!