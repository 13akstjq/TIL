---
title: "Next.js 15에서 Production Browser Source Maps 설정하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:18
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "productionBrowserSourceMaps"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/productionBrowserSourceMaps"
isUpdated: false
---


# productionBrowserSourceMaps

개발할 때는 **Source Maps**가 기본적으로 켜져 있어서, 디버깅하기가 편해요. 근데 문제는, 이렇게 소스맵이 켜진 상태로 프로덕션(실제 서비스) 빌드를 하면, 사용자가 내 소스 코드를 어느 정도 볼 수 있게 된다는 거예요. 그래서 Next.js는 프로덕션 빌드 시에는 소스맵을 끄는 게 기본 설정이에요.

그런데 만약 여러분이 일부러 프로덕션 환경에서도 소스맵을 사용하고 싶다면, 설정에서 `productionBrowserSourceMaps` 플래그를 `true`로 바꾸면 돼요:

```js
module.exports = {
  productionBrowserSourceMaps: true,
}
```

### 왜 프로덕션에서 소스맵을 켜는 걸 선택할까?

- 디버깅이 훨씬 쉬워짐: 프로덕션 환경에서 발생하는 오류도 원래 소스 코드 위치로 쉽게 매핑돼서 문제를 빠르게 찾을 수 있어요.
- 에러 추적 툴과 같이 사용: Sentry 같은 에러 추적 서비스에 소스맵을 업로드하면, 실제 코드 라인 단위까지 에러를 파악할 수 있어서 유지보수가 좋아지죠.

### 주의할 점!

- **코드 노출 위험**: 소스맵은 결국 여러분의 원본 코드를 어느 정도 노출시키니까, 보안이 매우 중요한 프로젝트라면 신중하게 결정해야 해요.
- 빌드 시간 증가: 소스맵 생성으로 인해 빌드 시간이 약간 더 걸릴 수 있어요.

그래서 정리하자면, 개발 중에는 당연히 소스맵 켜고 디버깅하는 게 편리하고, 프로덕션에서는 기본적으로 끄지만 상황에 따라 켜서 쓰면 된다는 이야기입니다. 필요에 맞게 설정을 조절해보세요!

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

Next.js에서 productionBrowserSourceMaps 옵션을 활성화하면, 소스 맵(source maps)이 JavaScript 파일과 같은 디렉터리에 생성돼요. 그리고 Next.js가 요청이 들어오면 이 파일들을 자동으로 서빙해주죠.

하지만 여기서 알아둘 점이 몇 가지 있어요:

| 장점 / 단점          | 설명                                         |
|------------------|--------------------------------------------|
| 장점                | 디버깅할 때 원본 소스 코드를 쉽게 확인할 수 있어요.      |
| 단점 1             | 빌드 속도가 느려질 수 있어요.                         |
| 단점 2             | 빌드 시 메모리 사용량이 증가해요.                       |

그래서 프로덕션 환경에서 디버깅을 자주 해야 하거나 버그 추적이 필요한 특별한 상황이 아니라면, 이 옵션을 활성화하는 걸 신중하게 결정하는 게 좋아요. 소스 맵을 포함하면 빌드 시간이 길어지고 서버 자원도 더 쓰이기 때문에, 적절한 타이밍에 켜는 걸 추천합니다.

추가로, 만약 빌드 사이즈 최소화에 집중하고 싶다면, 소스 맵 없이 문제를 해결하는 방법도 고민해보세요! 예를 들어, 개발 환경에서만 소스 맵을 켜고 프로덕션 빌드에선 끄는 식으로 설정할 수 있거든요.