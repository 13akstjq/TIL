---
title: "Nextjs 15 에서 reactCompiler 사용하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:18
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "reactCompiler"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/reactCompiler"
isUpdated: false
---


# reactCompiler

Next.js 15에서는 React Compiler를 공식 지원하기 시작했어요. 이 컴파일러는 성능을 쑥 올려주는 마법 같은 역할을 하는데요, 컴포넌트 렌더링을 자동으로 최적화해줘서 개발자가 직접 useMemo나 useCallback 같은 API를 통해 메모이제이션을 신경 쓸 필요가 훨씬 줄어들게 되었답니다.

사용 방법도 간단해요! Next.js 15 버전으로 업그레이드한 뒤, `babel-plugin-react-compiler`를 설치하면 끝!  
설치 명령어는 다음과 같아요:

```bash
npm install babel-plugin-react-compiler
```

> 참고로, 이 컴파일러는 특히 복잡한 상태 로직을 가진 대규모 애플리케이션에서 효과가 크게 나타나요. 기존에 매번 리렌더링 때문에 고민했던 부분들이 자연스럽게 해소될 수 있으니 꼭 활용해보세요.

또 한 가지, babel 플러그인을 설정하는 부분은 `babel.config.js`나 `.babelrc` 파일에 다음과 같이 추가해주시면 돼요:

```js
module.exports = {
  plugins: ["babel-plugin-react-compiler"],
};
```

이렇게만 해주면 Next.js 15와 함께 더욱 빠르고 깔끔한 React 앱을 만들 준비가 끝난 거예요! 앞으로는 불필요한 렌더링 걱정 없이 개발에 집중할 수 있겠네요 :)

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

그럼 next.config.js 파일에 experimental.reactCompiler 옵션을 추가해볼게요:

```js
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  experimental: {
    reactCompiler: true,
  },
}

export default nextConfig
```

> 참고: React Compiler는 현재 Next.js에서 Babel 플러그인을 통해서만 사용할 수 있어요. 이 옵션을 켜면 Next.js 기본 Rust 기반 컴파일러를 사용하지 않게 되어서, 빌드 시간이 조금 더 길어질 수 있어요. 하지만 Next.js 팀에서 앞으로 React Compiler를 기본 컴파일러로 지원할 계획이라고 하니 기대해도 좋습니다!

## 주석(Annotations)

'Annotations'는 코드나 설정 등에 부가적인 설명이나 메타데이터를 달아주는 역할을 해요. 예를 들면, 함수나 컴포넌트 위에 달리는 주석들이나 TypeScript의 데코레이터(@) 같은 것도 일종의 애노테이션이죠. Next.js에서는 여러 실험적 옵션들에 대해 주석이나 문서화 작업을 통해 개발자를 돕고 있답니다.

추가로, React Compiler를 통해 얻을 수 있는 장점은 다음과 같아요:

- 더 빠른 렌더링 속도
- 중간 빌드 산출물 감소
- 향상된 개발자 경험(에러 메시지 개선 등)

하지만 아직은 실험 단계라서 호환성 문제나 빌드 속도 저하가 생길 수 있으니, 프로덕션 환경에서는 신중하게 테스트해보는 걸 추천해요.

필요하면 이 옵션을 켜고 끄면서 빌드 성능과 동작 안정성을 직접 체험해보는 것도 좋은 공부가 될 거예요!

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

이번에는 Next.js에서 컴파일러를 'opt-in' 모드로 설정하는 방법을 공유할게요. 이 설정을 통해 필요한 컴포넌트나 훅에만 React의 메모이제이션(캐싱)을 적용할 수 있는데요, 크게 두 단계가 있어요.

### 1. Next.js 설정 변경하기

먼저 `next.config.js` 혹은 `next.config.ts` 파일에서 컴파일러의 `compilationMode`를 `'annotation'`으로 설정해줘야 해요. 이렇게 하면 코드 내 특정 위치에만 메모이제이션을 활성화할 수 있게 돼요.

```js
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  experimental: {
    reactCompiler: {
      compilationMode: 'annotation',
    },
  },
}

export default nextConfig
```

`experimental` 아래에 있는 설정이라 아직 완전히 안정화된 기능은 아닐 수 있으니, 프로젝트 상황에 맞게 테스트해보는 걸 추천합니다.

### 2. 특정 컴포넌트에 메모이제이션 적용하기

설정이 끝나면, 메모이제이션을 적용하고 싶은 컴포넌트나 훅에 `'use memo'`라는 디렉티브를 추가하세요. 아래처럼 컴포넌트 함수 최상단에 문자열로 선언하면 됩니다.

```js
export default function Page() {
  'use memo'
  // 이 컴포넌트는 React 메모이제이션의 혜택을 받게 돼요.
}
```

이걸 붙이는 순간, React 컴파일러가 이 컴포넌트를 최적화 대상으로 인식해 불필요한 리렌더링을 줄일 수 있어요.

---

### 추가 팁!
- `compilationMode`에 `'annotation'` 외에도 `'global'` 모드가 있는데, 이건 모든 컴포넌트에 자동으로 최적화를 적용하는 방식이에요. 하지만, 상황에 따라 부작용이 있을 수 있으니 꼭 필요한 부분에만 최적화를 적용하려면 `'annotation'` 모드를 추천합니다.
- React의 이와 같은 실험적인 최적화 기능은 프로젝트 규모가 크거나 복잡한 상태일 때 성능에 도움이 될 수 있어요. 하지만 작거나 단순한 프로젝트에서는 오히려 디버깅이 복잡해질 수 있으니 신중히 적용하세요.

이상으로 Next.js에서 React 컴파일러를 opt-in 모드로 활용하는 방법이었습니다. 필요할 때만 선택적으로 메모이제이션을 켜서 성능과 관리의 균형을 잘 맞춰보세요!

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

참고로, React에서는 "use no memo"라는 지시어도 사용할 수 있는데요, 이건 반대로 컴포넌트나 훅을 메모이제이션에서 제외하고 싶을 때 쓰는 방법이에요. 즉, 특정 컴포넌트나 훅이 리렌더링 될 때마다 항상 새롭게 실행되도록 하고 싶다면 이 지시어를 적용할 수 있죠.

메모이제이션(memoization)이란, 컴포넌트가 같은 props나 상태를 가진다면 이전에 계산한 결과를 재사용해서 불필요한 렌더링을 줄이는 최적화 기법이에요. 그런데 때로는 업데이트가 항상 필요하거나 캐싱을 원하지 않는 상황도 있거든요. 그럴 때 "use no memo"를 사용하는 게 도움이 됩니다.

참고로 "use no memo"는 공식 React API에 포함된 기능은 아니고, 보통 개발자들이 직접 구현하거나 특정 라이브러리에서 제공하는 기능인데요. 이름만큼이나 'no memo' 효과를 내어 성능을 제어하는 데 사용됩니다. 따라서 사용 시 문서나 라이브러리 가이드를 꼭 확인하는 게 좋아요.