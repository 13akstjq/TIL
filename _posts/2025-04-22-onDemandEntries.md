---
title: "Next.js 15 onDemandEntries 사용 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:16
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "onDemandEntries"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/onDemandEntries"
isUpdated: false
---


# onDemandEntries 설정으로 Next.js 개발 서버 메모리 관리하기

Next.js에서 개발할 때 서버가 빌드된 페이지들을 메모리에 얼마나 오래, 몇 개까지 유지할지 제어할 수 있는 옵션이 있다는 걸 아셨나요? 이 옵션이 바로 **onDemandEntries**입니다.

기본값도 있지만, 직접 설정을 바꿔주면 개발환경에서 페이지 리로딩 속도나 메모리 사용량을 조절하는 데 도움이 될 수 있어요.

예를 들어, `next.config.js` 파일에 다음처럼 추가하면 됩니다:

```js
module.exports = {
  onDemandEntries: {
    // 페이지를 메모리에 유지하는 최대 시간 (밀리초 단위)
    maxInactiveAge: 25 * 1000,  // 25초 동안 비활성 페이지 유지
    // 동시에 메모리에 유지할 페이지 수
    pagesBufferLength: 2,
  },
}
```

- **maxInactiveAge**: 개발 서버가 사용하지 않는 페이지를 메모리에 남겨두는 시간이에요. 기본값은 25초 정도입니다. 너무 짧게 하면 자주 페이지를 다시 빌드해서 느릴 수 있고, 너무 길면 메모리 낭비가 발생할 수 있죠.
- **pagesBufferLength**: 서버가 한 번에 메모리에 유지할 페이지 수입니다. 이 숫자를 늘리면 여러 페이지를 빠르게 전환할 때 빌드 시간을 줄일 수 있지만, 메모리도 더 많이 사용해요.

### 참고! onDemandEntries가 어떻게 도움이 될까?

앱 개발 중에 페이지를 자주 수정하거나 이동할 때, 이 옵션을 조절하면 번거로운 리로딩이나 대기 시간을 줄일 수 있어요. 특히 큰 프로젝트에서는 이걸 적절히 설정하면 개발 속도가 확실히 빨라집니다.

### 추가 팁

- 너무 짧거나 너무 길게 설정하지 말고, 본인 프로젝트 상황에 맞게 한번씩 조절해보세요.
- `pagesBufferLength`는 너무 크게 설정하면 메모리 사용량이 커지므로, 적절히 테스트해서 맞추는 게 좋습니다.
- 프로덕션 환경에는 영향을 미치지 않는 설정이니 걱정 없이 개발할 때만 조절하세요!

그럼 오늘도 Next.js 개발 즐겁게 하시길 바랍니다! 😊