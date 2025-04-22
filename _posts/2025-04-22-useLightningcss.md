---
title: "Next.js 15에서 useLightningcss로 스타일 최적화하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:32
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "useLightningcss"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/useLightningcss"
isUpdated: false
---


# useLightningcss

이번에 소개할 기능은 **Lightning CSS**를 사용하는 실험적 지원 기능이에요. Lightning CSS는 Rust로 작성된 빠른 CSS 번들러이자 미니파이어(minifier)입니다. 간단히 말해, CSS 파일을 빠르게 묶고 용량을 줄여주는 도구라고 보면 돼요.

Next.js에서 이 기능을 활성화하려면 `next.config.js` 또는 `next.config.ts`에 아래처럼 설정을 추가하면 됩니다:

```js
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  experimental: {
    useLightningcss: true,
  },
}

export default nextConfig
```

### Lightning CSS를 사용하면 좋은 점

- **성능 향상**: Rust로 개발되어 속도가 굉장히 빠릅니다. 대용량의 CSS를 처리할 때 특히 유용해요.
- **최소화(minify) 효율 증가**: CSS 용량을 더 작게 만들어 네트워크 전송 시간을 줄여줍니다.
- **미래지향적**: 아직 experimental(실험적) 단계이긴 하지만, Next.js가 공식적으로 지원할 예정이니 앞으로 안정성과 기능이 더 좋아질 거예요.

### 참고할 점

- 실험적 기능이기 때문에, 일부 환경에서 예기치 못한 문제나 호환성 이슈가 발생할 수 있습니다. 프로덕션 환경에 도입하기 전에 충분한 테스트를 하는 걸 권장해요.
- 만약 CSS 처리 과정에 커스텀 설정이 많거나 특정 PostCSS 플러그인을 사용 중이라면, Lightning CSS가 해당 부분을 모두 지원하는지 확인해야 합니다.

앞으로 CSS 번들링 및 최적화에 관심 있다면 Lightning CSS는 꼭 한 번 시도해볼 만한 옵션입니다. 여러분도 Next.js 프로젝트에서 설정해보고 체감 성능 차이를 느껴보세요!