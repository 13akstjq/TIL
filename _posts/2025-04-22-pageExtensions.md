---
title: "Next.js 15 pageExtensions로 마크다운 페이지 만들기"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:17
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "pageExtensions"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/pageExtensions"
isUpdated: false
---


# pageExtensions 설정으로 확장자 추가하기

안녕하세요! 오늘은 Next.js에서 기본으로 인정하는 페이지 확장자 설정을 바꾸는 방법을 살짝 알려드릴게요. 기본적으로 Next.js는 `.tsx`, `.ts`, `.jsx`, `.js` 파일만 페이지로 인식하는데요, 가끔은 마크다운 파일(`.md`, `.mdx`)도 페이지로 쓰고 싶을 때가 있죠? 그런 경우엔 `pageExtensions` 옵션을 활용하면 됩니다.

예를 들어, 마크다운(MD, MDX) 파일을 페이지로 추가하고 싶을 땐, 이렇게 해보세요:

```js
const withMDX = require('@next/mdx')()

/** @type {import('next').NextConfig} */
const nextConfig = {
  pageExtensions: ['js', 'jsx', 'ts', 'tsx', 'md', 'mdx'],
}

module.exports = withMDX(nextConfig)
```

여기서 `pageExtensions` 배열에 원하는 확장자를 추가해줌으로써, Next.js가 해당 확장자의 파일들도 자동으로 페이지로 인식해요. 그리고 `@next/mdx` 같은 플러그인을 통해 MDX 파일을 컴포넌트처럼 다룰 수 있게 도와주죠.

---

> **Tip!**  
> 만약 여러분이 `.md`, `.mdx` 외에 또 다른 확장자를 쓰고 싶다면, 이 배열에 추가만 하면 됩니다. 예를 들어 `.vue` 나 `.svelte` 같은 확장자를 넣으면 되는데, 그걸 다루는 로더나 플러그인이 추가로 필요하다는 점 꼭 기억하세요!

Next.js는 생각보다 커스터마이징이 자유로워서, 여러분 프로젝트에 필요한 파일 확장자는 얼마든지 추가해서 쓰셔도 좋아요. 이렇게 하면 페이지 관리를 더 유연하게 할 수 있답니다. 😉

그럼 오늘 내용도 도움이 되셨으면 좋겠습니다! 다음에도 유용한 팁으로 찾아올게요~