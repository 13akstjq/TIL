---
title: "Nextjs 15에서 reactStrictMode 사용하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:19
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "reactStrictMode"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/reactStrictMode"
isUpdated: false
---


# reactStrictMode

> 참고할 점: Next.js 13.5.1 버전부터는 app router를 사용할 때 기본적으로 Strict Mode가 활성화되어 있어서, 위에서 말한 설정은 오직 pages 디렉터리에 대해서만 필요해요. 물론 `reactStrictMode: false`로 설정해서 Strict Mode를 끌 수도 있습니다.

> 추천: Next.js 애플리케이션에서 Strict Mode를 꼭 켜두는 걸 강력하게 추천해요. 앞으로 React가 발전해가는 방향에 잘 대비할 수 있기 때문이죠.

## React의 Strict Mode란?

Strict Mode는 개발할 때만 활성화되는 기능으로, 앱에서 발생할 수 있는 잠재적인 문제를 미리 파악할 수 있도록 도와줘요. 예를 들어, 위험한 라이프사이클 메서드 사용이나 구버전 API 사용 같은 흔히 간과하기 쉬운 부분들을 찾아내죠.

좀 더 쉽게 말하면, 실수를 잡아내는 ‘개발자 전용 사이렌’ 같은 역할을 한다고 보면 돼요. 그래서 실서비스에 영향을 주지 않고도 코드 품질을 높이고 문제를 미리 예방할 수 있습니다.

### 참고로 알아두면 좋은 점

- Strict Mode를 켜면 일부 컴포넌트가 두 번 렌더링되기도 하는데, 이것도 문제를 찾아내기 위한 의도된 동작이에요.
- 라이프사이클 메서드 중 일부가 더 이상 권장되지 않거나 향후 React 버전에서 제거될 예정인 경우 Strict Mode가 알려줍니다.
- 앱이 커질수록 이런 문제를 미리 발견하는 게 복잡한 버그 잡는 것보다 훨씬 효율적이에요.

Next.js를 쓰면서 `next.config.js`에 아래처럼 설정하면 됩니다:

```js
module.exports = {
  reactStrictMode: true,
};
```

마지막으로, Strict Mode가 자동으로 켜지는 app router 환경에서는 따로 설정할 필요 없으니 참고하세요!

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

Next.js 런타임은 기본적으로 React의 Strict Mode 규격을 잘 준수하고 있어요. 만약 여러분이 Next.js 프로젝트 전체에 Strict Mode를 적용하고 싶다면, 아래처럼 `next.config.js` 파일에 옵션을 설정해주면 됩니다.

```js
module.exports = {
  reactStrictMode: true,
}
```

이렇게 하면 프로젝트 전체가 Strict Mode로 동작하게 돼요.

하지만 만약 팀이나 프로젝트 상황상, 전체 앱에 한꺼번에 Strict Mode를 적용하기 부담스럽다면 걱정하지 마세요! 꼭 전체에 적용할 필요 없이, React의 `<React.StrictMode>` 컴포넌트를 사용해서 페이지 단위로 조금씩 마이그레이션 할 수 있답니다.

예를 들어, 특정 페이지 파일에서 아래처럼 감싸주면 그 페이지만 Strict Mode로 작동하게 돼요.

```jsx
import React from 'react';

export default function MyPage() {
  return (
    <React.StrictMode>
      {/* 여기에 페이지 컴포넌트 JSX */}
    </React.StrictMode>
  );
}
```

이렇게 하면 단계적으로 코드 품질을 높여가면서 안정적으로 Strict Mode를 적용할 수 있죠.

사실 Strict Mode는 개발 중에만 활성화되는 체크 기능이라, 프로덕션에는 영향이 없는 점도 참고하세요. React에서 권장하는 좋은 코딩 습관을 지키고, 잠재적인 버그를 미리 잡아내기에 매우 유용하니, 서서히 도입해보시길 추천합니다!