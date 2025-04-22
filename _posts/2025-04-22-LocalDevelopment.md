---
title: "Nextjs15에서 로컬 개발 환경 구축하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:05
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Local Development"
link: "https://nextjs.org/docs/app/building-your-application/optimizing/local-development"
isUpdated: false
---


# 로컬 개발 환경

Next.js는 개발자 경험을 최우선으로 설계되었어요. 그런데 프로젝트가 커질수록 로컬에서 개발할 때 컴파일 속도가 느려지는 걸 경험할 수 있답니다. 그래서 이번 글에서는 자주 발생하는 컴파일 시간 지연 문제를 어떻게 발견하고 해결할 수 있는지 알려드릴게요.

## 로컬 개발과 프로덕션 빌드의 차이

Next.js에서 `next dev` 명령어로 로컬 개발을 할 때와, `next build` + `next start`로 프로덕션 모드로 실행하는 과정은 서로 다릅니다.

로컬 개발(`next dev`)에서는 개발 편의성을 위해 핫 리로딩, 소스맵 제공, 다양한 검사 기능들이 활성화되어 있어요. 그래서 빌드가 좀 더 느릴 수밖에 없죠. 반면 프로덕션 빌드(`next build`)는 최적화를 최대한 해서 빠른 페이지 로딩과 작은 번들 크기를 목표로 합니다.

여기서 중요한 점! 로컬 개발 속도가 너무 느리다면, 단순히 production 빌드 환경으로 바꿔서 해결하려고 하기보다는, 개발 중에만 발생하는 문제들이 없는지 한번 점검해 보는 게 좋아요.

---

추가로! Next.js 13부터는 새로운 앱 디렉터리와 React Server Components가 도입되면서 빌드와 컴파일 패턴이 조금 달라졌어요. 그래서 앱 디렉터리를 사용하는 경우에도 성능 최적화 방법이 약간 다를 수 있다는 점, 참고해주세요!

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

Next.js 개발할 때 'next dev' 명령어를 쓰면, 애플리케이션의 각 라우트를 실제로 열거나 이동할 때 그 라우트를 컴파일해요. 덕분에 처음 'next dev' 서버를 켤 때 모든 라우트를 한꺼번에 빌드하느라 오래 기다릴 필요가 없죠. 이것도 훨씬 빠르고, 메모리도 덜 먹으니 좋고요.

반면 프로덕션 빌드(운영용 빌드)는 파일 압축(minify)이나 콘텐츠 해시 생성 같은 최적화 작업들이 추가로 들어가서, 개발환경보다는 시간이 더 걸리고 무겁습니다. 그래서 로컬 개발할 때는 'next dev'를 쓰는 게 훨씬 효율적이죠.

---

## 로컬 개발 속도 높이는 방법

### 1. 내 컴퓨터의 백신 프로그램, 체크해봤나요?

의외로 많은 사람들이 간과하는 포인트인데요, 백신 프로그램이 파일 접근 속도를 확 느리게 할 수 있답니다. 개발 도중 수시로 파일을 읽고 쓰는데, 백신이 실시간 감시한다고 생각해보세요. 파일 입출력(I/O) 작업이 지연되니 개발 속도에도 영향이 가는 건 당연하죠.

아래 팁들 참고해보세요:

- 개발 폴더를 백신 검사 제외 대상으로 등록하기  
- 혹은 백신을 잠시 꺼두고 개발하기 (안전한 환경이라면)  
- 백신을 경량화하거나 개발 환경에 맞는 설정을 찾아보기  

간단하면서도 효과 볼 수 있는 방법이니 꼭 테스트해보시길 바랍니다!

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

프로젝트 폴더를 안티바이러스 예외 목록에 추가해보세요. 보통 윈도우 환경에서 많이 하는 방법이지만, 안티바이러스가 설치된 모든 시스템에서 추천하는 팁입니다. 안티바이러스가 파일을 검사하면서 개발 서버 성능이 떨어질 수 있기 때문이에요.

### 2. Next.js 업데이트 및 Turbopack 활성화

최신 버전의 Next.js를 사용하고 있는지 확인해보세요. Next.js는 버전이 올라갈 때마다 성능 개선이 자주 포함된답니다.

그리고 Turbopack이라는 새로운 번들러가 Next.js에 통합되었어요. Turbopack은 로컬 개발 환경에서 빌드와 번들링 속도를 크게 높여주는 역할을 하니 꼭 활용해보시길 추천합니다!

> 참고로, Turbopack은 아직 완전히 안정화된 단계는 아니지만, 빠른 속도를 경험하고 싶다면 충분히 시도해볼 만해요. 설정 방법이나 지원하는 기능도 계속 업데이트 중이라 공식 문서를 수시로 확인하는 게 좋아요.

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

```bash
npm install next@latest
npm run dev --turbopack
```

Turbopack에 대해 더 알고 싶다면 공식 문서와 업그레이드 가이드, 코드를 자동 변환해주는 codemods도 참고해보세요.

### 3. import 구문 점검하기

코드를 어떻게 import하느냐에 따라 컴파일 속도와 번들 크기에 큰 영향을 줄 수 있어요. 패키지 번들링 최적화 방법에 대해 배우고 싶다면 Dependency Cruiser, Madge 같은 도구도 한 번 사용해보시길 추천합니다. 

이런 도구들은 의존성 관계를 시각화하고 불필요한 중복 import를 찾는 데 도움을 줘서, 코드가 더 깔끔하고 빠르게 빌드되도록 도와줘요. 특히 큰 프로젝트에서는 이런 최적화가 빛을 발한답니다!

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

### 아이콘 라이브러리 활용 팁

@material-ui/icons나 react-icons 같은 라이브러리는 무려 수천 개의 아이콘을 한꺼번에 가져올 수 있어요. 그런데 실제로는 그중 몇 개만 쓸 때가 많죠. 이럴 때는 꼭 필요한 아이콘만 딱딱 가져오는 게 좋아요!

```js
// 이렇게 한꺼번에 불러오기보다는:
import { Icon1, Icon2 } from 'react-icons/md'

// 이렇게 개별로 불러오는 게 더 효율적이에요:
import Icon1 from 'react-icons/md/Icon1'
import Icon2 from 'react-icons/md/Icon2'
```

이렇게 하면 불필요한 아이콘들이 번들에 포함되는 걸 막을 수 있어서 앱의 용량을 줄이고, 로딩 속도를 빠르게 할 수 있답니다.

그리고 react-icons처럼 여러 아이콘 세트를 한 자리에서 제공하는 라이브러리는 한 가지 세트만 선택해서 통일감 있게 사용하는 걸 추천해요. 그러면 디자인도 깔끔해 보이고, 코드 관리도 훨씬 쉬워집니다!

추가로, 만약 프로젝트가 커져서 아이콘이 많아지면 커스텀 아이콘 컴포넌트를 만들어서 재사용하는 것도 좋은 방법이에요. 작지만 효과적인 성능 최적화 팁 중 하나랍니다!

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

예를 들어, 만약 여러분의 애플리케이션에서 react-icons를 사용하면서 다음과 같이 여러 아이콘 세트에서 모두 다 임포트한다고 해볼게요:

- pi (Phosphor Icons)
- md (Material Design Icons)
- tb (tabler-icons)
- cg (cssgg)

이렇게 하면, 실제로는 한 개씩 아이콘만 가져와서 사용하더라도, 컴파일러는 수만 개에 달하는 모듈을 처리해야 하거든요. 이게 빌드 시간은 물론 최종 번들 크기에도 꽤 큰 영향을 미쳐요.

### Barrel files (배럴 파일)

여기서 배럴 파일 개념을 활용할 수 있어요. 배럴 파일이란 여러 개의 모듈을 한 군데서 모아두는 일종의 '중간 집합' 파일인데, 이렇게 하면 필요한 아이콘만 깔끔하게 임포트할 수 있어요. 예를 들어, `icons/index.js` 같은 파일을 만들어서 자주 쓰는 아이콘들만 골라서 한군데 내보내기(export) 하는 거죠.

```js
// icons/index.js
export { PiSomeIcon } from 'react-icons/pi';
export { MdAnotherIcon } from 'react-icons/md';
export { TbSampleIcon } from 'react-icons/tb';
export { CgExampleIcon } from 'react-icons/cg';
```

그리고 실제 컴포넌트에서는 이렇게 쓰면 돼요:

```js
import { PiSomeIcon, MdAnotherIcon } from './icons';
```

이렇게 하면 필요 없는 아이콘 모듈들은 빌드 과정에서 제외될 가능성이 높아지고, 코드 관리도 훨씬 수월해져요. 

뿐만 아니라, 이것 말고도 react-icons에서 지원하는 트리 쉐이킹(Tree shaking) 기능을 최대한 활용하는 방법도 고려해보면 좋아요. 예를 들어 아이콘을 직접 필요한 것만 개별 임포트하거나, 번들러 설정(Webpack, Vite 등)에서 최적화를 제대로 해줘야 하죠.

정리하자면,

| 문제점                    | 해결 방법               | 추가 팁                         |
|---------------------------|------------------------|--------------------------------|
| 수만 개 모듈 임포트       | 배럴 파일로 모듈 한데 묶기 | 트리쉐이킹 지원 활성화           |
| 빌드 속도와 번들 크기 증가 | 필요 아이콘 개별 임포트   | 번들러 최적화 설정 함께 챙기기   |

조금만 신경 써도 아이콘 사용이 훨씬 간편해지고 앱이 가벼워질 수 있으니 꼭 한 번 시도해 보시길 추천드려요!

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

"Barrel 파일"이라는 개념 들어보셨나요? 간단히 말하면, 여러 개의 모듈이나 컴포넌트를 한 곳에서 한 번에 export해주는 파일을 뜻해요. 예를 들어, 여러 컴포넌트를 각각 import하지 않고, barrel 파일 하나만 import하면 편리하죠.

하지만 이런 barrel 파일이 오히려 빌드 속도를 늦출 수 있어요. 그 이유는 컴파일러가 해당 파일을 파싱하면서 모듈 스코프 내에 부작용(side-effects)이 있는지 확인해야 하기 때문이거든요. 그래서 가능하면 각 컴포넌트나 모듈을 직접 import하는 게 좋아요.

Next.js에서는 이런 barrel 파일과 관련된 최적화 기능도 제공하고 있으니 참고하시면 좋아요.

---

### 패키지 import 최적화하기

Next.js는 특정 패키지들에 대해 자동으로 import 최적화를 해준답니다. 만약 barrel 파일 방식을 사용하는 패키지를 쓴다면, `next.config.js` 파일에 해당 패키지를 아래와 같이 추가해 주면 돼요:

```js
// next.config.js
module.exports = {
  experimental: {
    optimizePackageImports: true,
  },
  optimizePackageImports: {
    'some-package': {
      transform: 'some-package/lib/{{member}}', // 실제 import 경로를 지정
    },
  },
};
```

이렇게 설정해 놓으면 Next.js가 필요한 모듈만 쏙쏙 골라서 import하기 때문에 빌드 속도와 번들 크기 측면에서 이점이 생기죠.

---

### 추가 팁!

만약 여러분이 직접 패키지를 만들거나 관리하는 입장이라면, 가능하면 barrel 파일 사용을 최소화하고, 패키지 내부에서 개별 모듈을 명확하게 export하는 방식을 추천해요. 그래야 사용자 입장에서도 임포트가 최적화되고, 빌드 속도도 빨라지니까요.

또한, import할 때 필요 없는 항목까지 한꺼번에 가져오지 말고, 코드에서 실제 필요한 것만 골라 import하는 습관을 들이면 번들 크기를 작게 유지하는 데 큰 도움이 됩니다!

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

```js
module.exports = {
  experimental: {
    optimizePackageImports: ['package-name'],
  },
}
```

위 코드는 Turbopack에서 특정 패키지의 import를 최적화하려고 설정한 예시입니다. 하지만 Turbopack은 이미 자동으로 import를 분석하고 최적화해주기 때문에, 이 설정은 사실 필요 없습니다. 번거롭게 직접 설정하지 않아도 Turbopack이 알아서 똑똑하게 처리해줘요.

---

### 4. Tailwind CSS 설정 확인하기

만약 Tailwind CSS를 사용 중이라면, 제대로 설정되어 있는지 꼭 확인해보세요. Tailwind는 써보면 정말 편리한 CSS 유틸리티 프레임워크인데, 설정이 조금만 틀리면 스타일이 제대로 적용되지 않을 수 있거든요.

예를 들어, `tailwind.config.js`에서 콘텐츠 경로(`content` 또는 `purge`)에 프로젝트 내 사용하는 컴포넌트 경로나 파일이 모두 포함되어 있는지 확인해야 합니다. 빠뜨리면 Tailwind가 불필요한 CSS를 제거하면서 실제 필요한 스타일까지 지워버릴 수 있어요.

또한 PostCSS나 Next.js 같은 빌드 도구 설정에서 Tailwind 플러그인이 정확히 추가되어 있는지도 체크해보세요. 잘못 연결되어 있으면 컴파일 오류나 스타일 적용 문제로 이어질 수 있답니다.

혹시 Tailwind CSS 설정이 처음이라면 공식 문서 한 번 정독해보는 걸 추천드리고, 설정 한 번 끝내고 나면 CSS 작성이 훨씬 간결해지고 관리도 수월해질 거예요!

---

추가로 궁금한 점이나 설정 관련해서 더 도움 필요하면 언제든 말씀해 주세요!

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

Tailwind CSS 설정할 때 자주 하는 실수 중 하나가, content 배열에 node_modules 같은 불필요하게 큰 폴더들이 포함되어 빌드 속도를 느리게 만드는 경우예요.

Tailwind CSS 버전 3.4.8 이상부터는, 이런 설정이 빌드를 느리게 할 수 있다는 경고를 알려줘서 옵니다.

### 주의할 점과 좋은 설정 예시

- tailwind.config.js에서 어떤 파일을 스캔할지 구체적으로 명시하세요:

```js
module.exports = {
  content: [
    './src/**/*.{js,ts,jsx,tsx}', // 좋은 예시
    // 아래 설정은 너무 범위가 넓어서 node_modules까지 포함될 수 있음
    // '../../packages/**/*.{js,ts,jsx,tsx}',
  ],
}
```

- 불필요한 파일 스캔을 피하세요:

```js
module.exports = {
  content: [
    // 훨씬 좋은 방법 - 오직 src 폴더만 스캔합니다.
    '../../packages/ui/src/**/*.{js,ts,jsx,tsx}',
  ],
}
```

이렇게 하면 Tailwind가 진짜 필요한 파일만 훑어보게 되어 빌드 시간이 확 줄어들고, 불필요한 경고나 오류도 줄어듭니다.

---

### 추가 팁!

만약 프로젝트에 여러 패키지가 묶여있거나, 모노레포 구조라면 특히 어디를 포함시키고 어디를 제외할지 꼼꼼하게 체크해야 해요. node_modules, 빌드 결과물(dist), 그리고 테스트 결과물 같은 폴더는 꼭 제외하는 게 좋습니다.

---

### 다음 내용 살펴보기

5. 커스텀 Webpack 설정 확인하기

Webpack이나 다른 번들러와 함께 Tailwind를 쓸 때, 설정이 꼬이지 않았는지 꼭 한 번 확인하세요. 특히 경로나 플러그인 충돌을 점검하면 빌드 문제를 미리 예방할 수 있습니다!

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

만약에 여러분이 커스텀 웹팩 설정을 추가했다면, 그 설정 때문에 컴파일 속도가 느려질 수도 있어요.

로컬 개발 환경에서 정말 그 설정이 필요한지 한 번 생각해 보세요. 필요 없다면, 특정 툴이나 설정은 프로덕션 빌드 때만 포함시키는 걸 고려해 보는 것도 좋아요. 그리고 요즘은 Turbopack 같은 좀 더 빠른 빌드 도구도 많이 떠오르고 있으니 이런 새로운 도구와 로더(loader) 사용도 한번 조사해 보세요!

### 6. 메모리 사용 최적화

앱이 엄청 크다면, 메모리가 더 많이 필요할 수 있어요. 빌드나 실행 중 메모리 부족으로 속도가 느려지는 경우가 종종 있거든요. 개발 환경에서 메모리 설정을 늘리거나, 불필요한 모듈을 정리해서 메모리 사용량을 줄여보는 게 도움이 될 수 있습니다.

---

> 팁: Node.js를 쓰신다면, `--max-old-space-size` 옵션으로 메모리 제한을 늘려줄 수 있어요. 예를 들어 `node --max-old-space-size=4096`은 최대 힙 메모리를 4GB로 확장하니, 빌드가 훨씬 여유로워질 수 있습니다.

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

메모리 사용 최적화에 관한 공부를 계속해보면서, 이번에는 서버 컴포넌트와 데이터 패칭에 대해 이야기해볼게요.

### 7. 서버 컴포넌트와 데이터 가져오기

서버 컴포넌트가 변경되면 그 페이지 전체가 다시 렌더링돼요. 이 과정에서 해당 컴포넌트에 필요한 새로운 데이터를 다시 가져오게 되죠. 이게 로컬 개발 시에는 꽤 번거롭고, 자칫하면 불필요한 API 호출이 많아져 비용이 증가할 수 있어요.

여기서 실험적으로 제공되는 옵션인 `serverComponentsHmrCache`를 활용하면, 서버 컴포넌트가 변경되어도 핫 모듈 교체(HMR) 과정 중에 fetch한 응답을 캐싱해둘 수 있어요. 즉, 다시 똑같은 데이터를 요청하지 않고 캐시된 데이터를 재사용함으로써, 훨씬 빠르게 페이지를 업데이트할 수 있고, API 호출 횟수도 줄일 수 있답니다.

---

사실 이런 캐싱 옵션을 잘 활용하면 개발 속도가 눈에 띄게 빨라지고, 특히 외부 API 호출에 비용이 붙는 상황에서는 큰 절약 효과를 볼 수 있어요. 다만, 이 기능이 아직 실험 단계라는 점은 참고하세요. 즉, 실제 운영 환경에서는 예상치 못한 이슈가 발생할 수 있으니 로컬 개발용으로만 활용하는 걸 추천합니다.

추가로, 서버 컴포넌트에서 데이터 패칭을 최적화할 때는 캐시 적중률을 높이도록 데이터 요청을 일관성 있게 만드는 것도 중요해요. 예를 들어, 쿼리 파라미터를 변동 없이 사용하거나, 필요한 데이터만 정확히 요청하면 캐싱 효율이 확 올라갑니다.

---

요약해서 핵심 내용만 짚어드리면 아래 표처럼 정리할 수 있어요.

| 개념                          | 설명                                               | 장점                                                     | 주의사항                      |
|-----------------------------|--------------------------------------------------|--------------------------------------------------------|----------------------------|
| 서버 컴포넌트 렌더링 변경           | 컴포넌트 변경 시 페이지 전체가 다시 렌더링되고 데이터 재요청 발생       | 최신 데이터를 반영하는 깨끗한 렌더링                            | 과도한 API 호출 발생 가능           |
| serverComponentsHmrCache 옵션 | HMR 동안 서버 컴포넌트 데이터 fetch 결과를 캐싱                     | 빠른 반응 속도, API 호출 비용 절감                              | 아직 실험 기능, 로컬 개발용 추천       |

앞으로도 메모리뿐 아니라 개발 효율과 비용 절감에 도움되는 이런 팁들을 계속 소개할게요. 혹시 서버 컴포넌트 관련해서 더 궁금한 점 있으면 알려주세요!

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

실험적인 옵션에 대해 더 알아보기

## 문제를 찾는 데 도움이 되는 도구들

### 자세한 fetch 로그 기록

개발 중에 무슨 일이 일어나고 있는지 더 자세히 보고 싶을 때는 이 명령어를 사용해 보세요:

```bash
FETCH_LOG_LEVEL=debug npm start
```

(예: FETCH_LOG_LEVEL 환경 변수를 debug로 설정하고 앱을 실행하는 방법)

이렇게 하면 fetch 요청과 응답에 관련된 상세한 정보가 로그로 출력돼서, 네트워크 문제나 데이터 처리 과정에서 무슨 일이 일어나는지 더 쉽게 파악할 수 있어요.

---

추가로, 개발할 때 이런 로그를 꼼꼼히 살펴보는 습관을 들이면 문제를 빨리 찾고 해결하는 데 큰 도움이 됩니다. 특히 API 호출이 많거나 비동기 처리 로직이 복잡한 프로젝트일수록요.

그리고 fetch 관련 로그뿐만 아니라, 다른 요청이나 상태 변화를 추적할 수 있는 도구(예: 브라우저의 개발자 도구 네트워크 탭, Redux DevTools 등)도 같이 활용하면 문제 파악이 훨씬 수월합니다!

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

```bash
next dev --verbose
```

## Turbopack 추적 기능 활용하기

Turbopack tracing은 로컬 개발 중에 애플리케이션의 성능을 분석하는 데 도움을 주는 도구예요. 각 모듈이 컴파일되는 데 걸리는 시간과 모듈 간의 연관성을 자세히 보여주기 때문에, 느린 부분이나 문제점을 찾아내는 데 유용하답니다.

### Turbopack 추적을 시작하는 방법

1. **Next.js 최신 버전 확인**  
   Turbopack tracing은 최신 Next.js 버전에서 제대로 동작하니, 먼저 `next` 패키지가 최신인지 확인해주세요.

2. **추적 파일 생성하기**  
   다음 명령어를 통해 개발 서버를 켜면서 추적을 활성화할 수 있어요:

   bash
   NEXT_TURBOPACK_TRACING=1 npm run dev
   
   
   개발하면서 페이지를 이동하거나 파일을 수정하면서 문제를 재현해 보세요.

3. **서버 종료 후 추적 파일 확인**  
   개발 서버를 끄면 `.next` 폴더 안에 `trace-turbopack`이라는 추적 파일이 생성됩니다.

4. **추적 파일 해석하기**  
   생성된 파일을 분석하려면 아래 커맨드를 실행하세요:

   bash
   next internal trace .next/trace-turbopack
   

   만약 `trace` 명령어가 없으면, 구버전에서는 `turbo-trace-server`라는 이름이었으니 아래 명령어를 써보시면 돼요:

   bash
   next internal turbo-trace-server .next/trace-turbopack
   

5. **웹에서 추적 결과 보기**  
   명령어를 실행해 추적 서버가 켜지면, 브라우저에서 [https://trace.nextjs.org/](https://trace.nextjs.org/)로 접속해 결과를 확인할 수 있습니다.

### 추가 팁 - 타이밍 정보 보는 법

기본적으로 추적 뷰어는 타이밍을 집계해서 보여주는데요, 전체 타이밍 뿐 아니라 각 작업별 상세 타이밍을 보고 싶을 땐 오른쪽 위에서 "Aggregated in order"를 "Spans in order"로 변경해 주세요. 그러면 모듈별 구체적인 컴파일 시간을 살펴볼 수 있답니다.

---

Turbopack tracing은 복잡한 의존성이나 느린 모듈 문제를 잡을 때 정말 강력한 도구예요. 특히 대규모 프로젝트를 다룰 때 성능 병목을 쉽게 발견할 수 있어 개발 생산성을 크게 높여주죠. Next.js를 사용하면서 느려짐을 체감한다면 꼭 한 번 활용해 보시길 추천드립니다!

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

## 아직 문제 해결이 안 되셨나요?

Turbopack 트레이싱 섹션에서 생성된 트레이스 파일을 공유해 주세요.  
그 파일을 GitHub Discussions나 Discord에 올리면 더 빠르게 도움을 받을 수 있어요!

추가 팁을 드리자면, 트레이스 파일에는 실행 흐름에 대한 정보가 담겨 있어서 문제를 분석하는 데 큰 도움이 됩니다.  
그러니 가능하면 문제 상황을 최대한 자세히 적어 함께 올려 주세요. 그러면 개발자나 커뮤니티 멤버들이 더 정확하게 도움을 줄 수 있답니다!