---
title: "실무에서 필요한 타입스크립트 기반 typedRoutes 활용법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:31
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "typedRoutes"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/typedRoutes"
isUpdated: false
---


# typedRoutes란?

typedRoutes는 **타입이 보장되는 링크**(statistically typed links)를 실험적으로 지원하는 기능이에요. 이 기능을 사용하려면 Next.js의 App Router를 쓰고, 프로젝트가 TypeScript로 작성되어 있어야 해요.

보통 라우팅할 때, 문자열로 경로를 직접 적다가 오타가 생기거나 경로가 바뀌어도 에러가 나지 않아 나중에 문제를 발견하곤 하죠. 그런데 typedRoutes를 활성화하면, TypeScript가 링크 경로 자체를 타입으로 체크해줘서 이런 실수를 미연에 방지할 수 있어요.

## 설정 방법

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    typedRoutes: true,
  },
}

module.exports = nextConfig
```

next.config.js 파일에서 `experimental.typedRoutes` 옵션을 `true`로 설정하면 끝!

---

## 추가로 알아두면 좋은 점

- 현재는 실험적 기능이라 안정화가 덜 되었을 수 있어요. 최신 Next.js 버전과 호환되는지 확인이 필요해요.
- App Router가 아닌 예전의 pages 디렉터리 방식에서는 사용할 수 없어요.
- 타입이 자동으로 생성되기 때문에 경로가 바뀌면 바로 코드 내에서 타입 오류를 통해 알려주니 유지보수가 훨씬 수월해집니다.
- 실제로 사용해보면, 링크를 만들 때 `href="/posts/[id]"` 같은 동적 경로를 다룰 때도 타입이 보장되어 편리해요.

Next.js + TypeScript 조합을 쓰시는 분이라면 꼭 한 번 써보길 추천드려요! 프로젝트의 안전성과 생산성이 크게 올라갈 겁니다.