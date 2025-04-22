---
title: "Next.js 15에서 generateEtags로 효율적인 캐시 관리 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:08
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "generateEtags"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/generateEtags"
isUpdated: false
---


# generateEtags 설정하기

Next.js에서는 기본적으로 모든 페이지에 대해 **etag**를 자동으로 생성해줘요. etag는 브라우저가 이전에 받아온 리소스가 변경되었는지 확인할 때 쓰는 값인데요, 캐시 전략에 따라 이 기능을 끄고 싶을 때가 있어요.

예를 들어, 내가 직접 캐시 정책을 세밀하게 조절하고 싶거나, etag 때문에 캐시가 제대로 동작하지 않는다고 생각될 때 generateEtags 옵션을 꺼주면 돼요.

방법은 아주 간단해요! 프로젝트 루트에 있는 `next.config.js` 파일을 열고, 아래처럼 `generateEtags` 옵션을 `false`로 설정해주면 됩니다.

```js
module.exports = {
  generateEtags: false,
}
```

이렇게 하면 Next.js가 페이지를 렌더링할 때 etag를 생성하지 않아서, 캐시 동작을 좀 더 직접 컨트롤할 수 있어요.

---

### 참고로!

- etag를 끄면 클라이언트가 서버에 리소스 변경 여부를 확인하는 요청을 덜 보내게 만들 수 있지만, 캐시가 제대로 동작하지 않을 수도 있으니 신중하게 사용하세요.
- 서버 캐시, CDN 캐시 등을 별도로 활용한다면 etag가 필요 없을 수도 있어요.
- 특히 API 응답에는 etag를 켜놓고, HTML 페이지에는 끄는 식으로 상황에 맞게 설정하면 좋답니다.

혹시 캐시 관련해서 더 궁금한 점 있으면 알려주세요!