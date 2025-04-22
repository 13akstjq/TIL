---
title: "Next.js 15에서 poweredByHeader 비활성화하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:17
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "poweredByHeader"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/poweredByHeader"
isUpdated: false
---


# poweredByHeader 설정

Next.js를 사용하다 보면, 기본적으로 모든 응답 헤더에 `x-powered-by`라는 헤더가 붙어요. 이 헤더는 "이 사이트가 Next.js로 만들어졌다"는 걸 알려주는데, 보안상 노출을 줄이거나 단순히 이 헤더를 빼고 싶을 때가 있죠.

이럴 때는 `next.config.js` 파일을 열어서 `poweredByHeader` 설정을 꺼주면 돼요.

```js
module.exports = {
  poweredByHeader: false,
}
```

이렇게 하면 서버가 응답할 때 `x-powered-by` 헤더가 더 이상 붙지 않아요.

---

### 추가로 알아두면 좋은 점

- **보안성 강화**  
  노출되는 정보는 최소화하는 게 좋은 보안 습관이에요. `x-powered-by` 헤더를 빼면 해커가 사용하는 도구가 사이트의 기술 스택을 파악하는 걸 조금이나마 어렵게 만들 수 있습니다.

- **SEO나 서비스 성능에는 영향 없음**  
  이 설정은 단지 헤더 노출 여부만 바꾸는 거라 웹사이트 성능이나 검색엔진 최적화(SEO)에 영향은 없어요.

- **Next.js 기본 설정**  
  Next.js는 편의를 위해 기본으로 켜져있지만, 필요에 따라 언제든 끌 수 있으니 맞춤 설정 해주세요.

혹시 더 궁금한 내용이나 Next.js 설정에 대해 알고 싶다면 언제든 질문해주세요!