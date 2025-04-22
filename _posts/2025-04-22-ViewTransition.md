---
title: "Nextjs 15에서 ViewTransition API로 페이지 전환 자연스럽게 하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:33
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "ViewTransition"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/viewTransition"
isUpdated: false
---


# ViewTransition 알아보기

여러분, React에서 새롭게 실험 중인 **View Transitions API** 들어보셨나요? 이 기능은 브라우저의 네이티브 View Transitions API를 활용해 UI 상태 변화 사이를 자연스럽고 부드럽게 연결해주는 마법 같은 기술이에요.

React에서 이걸 쓰려면, `next.config.js` 파일 안에 `viewTransition` 설정을 켜줘야 해요. 설정 방법은 아주 간단해요:

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    viewTransition: true,
  },
}

module.exports = nextConfig
```

이렇게 해주면, 여러분의 Next.js 프로젝트에서 이 새 기능을 써볼 수 있답니다.

---

### 추가 팁!

- 이 API는 아직 **실험적(Experimental)** 이기 때문에, 모든 브라우저에서 완벽히 지원되는 것은 아니에요. 크롬 최신 버전에서 주로 잘 작동하니 테스트할 때 참고하세요.
- View Transitions를 잘 활용하면 페이지 이동이나 UI 상태 변경이 훨씬 자연스러워서 사용자 경험이 훅 올라가요.
- 이 기능 적용 후에는 기존의 복잡한 애니메이션 라이브러리 코드를 많이 줄일 수도 있습니다.
  
꼭 한 번 써보시고, 여러분만의 멋진 UI 전환 효과를 만들어 보세요!

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

> 중요한 공지: 이 기능은 Next.js 팀에서 개발하거나 유지보수하는 게 아니에요. React 팀에서 실험적으로 제공하는 API라 아직 초기 단계에 있으며, 실제 프로덕션 환경에선 쓰는 걸 권장하지 않습니다. 구현 방식도 계속 바뀌고 있어서 앞으로 React 버전 업데이트에 따라 동작이 달라질 수 있어요. 이 기능을 사용하려면 이런 실험적인 특성을 충분히 이해하고 있어야 합니다. 자세한 동작 방식은 React 깃허브의 pull request와 관련 토론을 참고하는 게 좋아요.

## 사용법

이 기능을 활성화하면, React에서 `ViewTransition` 컴포넌트를 이렇게 불러올 수 있습니다:

```js
import { unstable_ViewTransition as ViewTransition } from 'react'
```

---

여기서 `unstable_` 접두어는 '실험적'이라는 뜻이에요. 앞으로 API가 바뀔 가능성이 크다는 신호로 생각하시면 됩니다. 개발할 때 참고하되, 안정성을 원한다면 공식적으로 완성된 기능이 나올 때까지 기다리는 게 좋겠죠?

또한, 이 기능은 최신 브라우저에서만 지원될 가능성이 크고, 완성도나 성능 개선이 계속 진행 중이라 실제 프로젝트에 바로 넣기보단 호기심 있게 테스트해보는 용도로 활용해보세요.

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

하지만 현재 문서와 예제가 제한적이라 이 기능이 어떻게 작동하는지 이해하려면 React 소스 코드와 관련 토론을 직접 참고해야 해요.

### 라이브 데모

Next.js 뷰 전환 데모를 확인해보세요.
직접 기능이 어떻게 동작하는지 볼 수 있어요.

이 API가 발전함에 따라 문서도 업데이트되고 더 많은 예제가 공유될 예정이에요.  
하지만 지금 당장은 이 기능을 실제 서비스에 바로 사용하는 건 권장하지 않아요.  
새로운 기능은 언제나 기대되지만, 안정성 면에서 좀 더 검증된 이후에 사용하는 게 안전하니까요!

참고로, 이런 신기능들은 빠르게 변할 수 있으니 GitHub 같은 곳에서 최신 이슈나 PR도 계속 살펴보면 도움이 됩니다.  
그리고 직접 코드를 읽고 테스트해보면서 이해도를 높이는 것도 좋은 방법이에요.