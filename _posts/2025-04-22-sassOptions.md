---
title: "Next.js 15에서 sassOptions 설정 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:24
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "sassOptions"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/sassOptions"
isUpdated: false
---


# sassOptions란 무엇인가요?

`sassOptions`는 Next.js에서 Sass 컴파일러를 설정할 때 사용하는 옵션입니다. 간단히 말해서, Sass 파일을 어떻게 처리할지 세부적으로 설정할 수 있게 도와주는 녀석이죠.

아래 예제를 보면서 살짝 살펴볼게요:

```js
import type { NextConfig } from 'next'

const sassOptions = {
  additionalData: `
    $var: red;
  `,
}

const nextConfig: NextConfig = {
  sassOptions: {
    ...sassOptions,
    implementation: 'sass-embedded',
  },
}

export default nextConfig
```

위 코드에서 `additionalData`는 Sass 파일이 컴파일될 때마다 자동으로 추가되는 코드입니다. 여기서는 Sass 변수 `$var`에 `red` 색상을 지정하고 있죠. 이렇게 하면 모든 Sass 파일에서 이 변수를 사용할 수 있게 돼서 일일이 변수 선언할 필요 없이 편리해요.

그리고 `implementation` 옵션에 `'sass-embedded'`를 지정했는데요, 이는 Dart Sass의 임베디드 버전을 사용한다는 뜻입니다. Next.js는 보통 기본적으로 Dart Sass를 사용하지만, 여기에 다른 Sass 컴파일러를 직접 명시해주고 싶을 때 쓰는 부분이에요.

> 참고로, Next.js는 `sassOptions`의 여러 속성을 모두 타입으로 관리하고 있지 않아서, `implementation` 말고는 타입 추론이 조금 제한적이에요. 그래서 필요한 옵션만 적절히 넣어주면 됩니다.

---

### 추가로 알아두면 좋은 팁!

- **`additionalData` 활용법**  
  예를 들어 여러 Sass 파일에서 공통으로 사용할 변수나 믹스인, 함수 등이 있다면, `additionalData`에 미리 등록해두면 코드 중복을 줄일 수 있어요.

- **퍼포먼스 최적화**  
  Sass 설정에 따라 빌드 속도에 영향을 줄 수 있으니, 꼭 필요한 옵션만 넣고 불필요한 설정은 피하는 게 좋아요.

- **Next.js 공식 문서 확인하기**  
  Next.js 자체 문서나 Sass 공식 문서를 참고하면 더 많은 옵션과 사례를 확인할 수 있어요.

Sass를 Next.js 프로젝트에서 제대로 세팅해두면 스타일 관리가 훨씬 깔끔해지고 유지보수도 쉬워지니 꼭 한번 시도해 보시길 추천해요!