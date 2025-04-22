---
title: "Next.js 15에서 urlImports 기능 활용하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:31
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "urlImports"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/urlImports"
isUpdated: false
---


# URL Imports

URL Imports는 아직 실험적인 기능이지만, 외부 서버에서 바로 모듈을 가져와서 사용할 수 있게 해줘요. 보통은 로컬 디스크에서 모듈을 불러오잖아요? 그런데 이 기능을 쓰면 웹 상에 있는 모듈을 직접 불러올 수 있다는 거죠.

> ⚠️ 주의!  
> 이 기능은 불러오는 도메인을 신뢰할 수 있을 때만 사용해야 해요. 악성 코드를 다운로드해서 내 컴퓨터에 실행할 수도 있으니까요. 정식 기능으로 안정화될 때까지는 조심해서 써야 해요.

사용하려면 `next.config.js` 파일에 허용할 URL 접두사를 설정해줘야 해요. 예를 들어:

```js
// next.config.js
module.exports = {
  experimental: {
    urlImports: [
      "https://cdn.skypack.dev/",
      "https://esm.sh/"
    ]
  }
}
```

위처럼 허용할 URL들을 배열로 적어주면, 해당 주소에서 모듈을 바로 불러올 수 있습니다.

---

## URL Imports의 매력 포인트!

- **빠르게 외부 라이브러리 디펜던시 추가**: `npm install` 없이도 외부 CDN에서 바로 모듈을 가져와 개발할 수 있어요.
- **버전 관리 손쉬움**: URL에 버전을 명시하면 특정 버전의 패키지를 즉시 사용 가능!
- **특정 모듈만 쓰고 싶을 때 편리**: 프로젝트에 모두 설치하지 않아도 필요한 모듈만 간편히 import 가능해요.

---

## 하지만, 이전에 꼭 알아둬야 할 점!

- 아직 실험적이라 예상치 못한 이슈가 발생할 수 있어요.
- 네트워크 상태에 따라 모듈 로딩 속도가 달라지고, 경우에 따라 실패할 수도 있죠.
- 보안 문제 때문에 신뢰할 수 없는 출처는 무조건 피하셔야 합니다.

---

URL Imports는 앞으로의 모듈 관리와 배포 방식을 바꿀 수 있는 흥미로운 기능이에요. 직접 써보면서 맛보기 좋으니, 작은 프로젝트부터 한번 시도해보세요!

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

이번에는 Next.js에서 실험적으로 지원하는 URL Imports 기능에 대해 이야기해볼게요. 이 기능을 활용하면, 원격 URL에서 직접 모듈을 불러와 사용할 수 있어요. 아주 신기하죠?

설정 방법부터 간단하게 설명해 드릴게요. `next.config.js` 파일에 다음과 같이 추가해 줍니다.

```js
module.exports = {
  experimental: {
    urlImports: ['https://example.com/assets/', 'https://cdn.skypack.dev'],
  },
}
```

여기서 `urlImports` 배열 안에는 직접 모듈을 불러오고 싶은 URL 주소의 베이스를 적어줘야 해요. 이렇게 설정한 후엔 아래처럼 URL 경로에서 바로 모듈을 import 할 수 있답니다.

```js
import { a, b, c } from 'https://example.com/assets/some/module.js'
```

이게 왜 좋냐고요? 보통은 npm 패키지를 설치하고 관리해야 하는데, 이렇게 하면 별도로 내려받지 않고도 외부 서버에서 최신 모듈을 바로 가져와 사용할 수 있어요. 특히 CDN에 호스팅된 유틸리티나 라이브러리를 가져오는 데 굉장히 편리하죠.

하지만 주의할 점도 있어요.  
- 외부 서버 상태에 따라 앱 로딩 속도나 안정성에 영향을 받을 수 있어요.  
- 보안상 신뢰할 수 있는 URL만 등록하는 게 중요해요.  
- 아직은 실험적인 기능이라 완전한 안정성은 보장되지 않으니 프로덕션에 바로 적용하기 전 충분히 테스트하는 걸 추천합니다.

참고로, 이 URL Imports는 일반적인 npm 패키지 import 할 수 있는 모든 곳에서 사용할 수 있어요. 예를 들어 components, utils 스크립트 안에서도 똑같이 쓸 수 있다는 뜻이에요.

마지막으로, 이 기능이 발전하면 패키지 설치와 관리가 훨씬 간편해질 수도 있으니 앞으로도 계속 관심 가져볼 만한 기능이랍니다!

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

## 보안 모델(Security Model)

이번 기능은 보안을 최우선으로 생각하며 설계되고 있어요. 우선, URL 임포트를 허용할 도메인을 명시적으로 지정하도록 강제하는 실험적 플래그(flag)를 추가했답니다. 이게 왜 중요하냐면, 내 코드가 외부에서 불러오는 리소스를 무분별하게 받아들이면 보안 취약점이 생길 수 있기 때문이죠. 앞으로는 URL 임포트가 브라우저 샌드박스 환경인 Edge Runtime에서만 실행되도록 범위를 제한해서, 더 안전하게 만들 계획입니다.

사실 이렇게 도메인 허용 리스트를 관리하는 건 좋은 습관이에요. 여러분도 꼭 쓰는 서드파티 라이브러리나 리소스의 출처를 명확히 하고, 필요 없는 외부 URL은 차단하는 걸 잊지 마세요.

## Lockfile

URL 임포트를 사용하면, Next.js는 `next.lock`이라는 디렉터리를 만들어서 그 안에 락파일(lockfile)과 함께 실제로 받아온 자산(asset)들을 저장해요. 중요한 점은, 이 `next.lock` 폴더를 `.gitignore`에 넣고 무시하지 말고 꼭 Git에 커밋해야 한다는 거예요.

왜 그럴까요? 바로 팀원들이나 CI/CD 환경에서 동일한 외부 의존성을 정확히 재현하기 위해서입니다. 만약 이 폴더를 깃에서 제외하면, 다른 개발자들이나 배포 파이프라인에서 같은 자산을 보장할 수 없게 돼서 문제를 일으킬 수 있죠.

---

추가로, 이런 락파일이나 다운로드한 의존성을 커밋하는 방식은 npm의 `package-lock.json`이나 Yarn의 `yarn.lock` 과 비슷한 역할을 해요. 외부 URL을 통한 의존성도 안전하게 관리하려면 이런 락 파일 관리는 필수겠죠? 이번 기회에 lockfile 관리에 신경 쓰시면 좋겠네요!

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

Next.js에서 URL Imports를 다룰 때 중요한 두 가지 동작 방식을 쉽게 설명해볼게요.

- **next dev 실행 시**: Next.js가 새로 발견한 URL Imports들을 모두 다운로드해서 `lockfile`에 추가해줘요.
- **next build 실행 시**: 실제 배포용 빌드를 할 때는 오직 `lockfile`에 기록된 URL Imports 만 사용해요.

한마디로, 개발 모드에서는 네트워크에서 해당 URL들을 받으면서 업데이트하고, 빌드할 때는 안정성을 위해 네트워크 요청 없이 `lockfile`에 있는 내용만 써서 빌드를 해요. 그래서 `lockfile`이 오래되면 빌드가 실패할 수도 있죠. 다만, 딱 한 가지 예외가 있는데요. 만약 URL이 `Cache-Control: no-cache` 헤더로 응답한다면, 이 리소스는 `lockfile`에 `no-cache` 항목으로 기록되고, 빌드할 때마다 네트워크에서 항상 최신 내용을 받아와요.

조금 더 감을 잡으라고 예제를 보여드릴게요.

## 예제

### Skypack

Skypack 같은 CDN에서 패키지를 URL Import로 불러올 때 이 동작 방식이 어떻게 적용되는지 확인할 수 있어요.

---

추가로, 만약 여러분이 Next.js에서 URL Import를 사용한다면 `lockfile` 관리를 꼼꼼히 하는 게 매우 중요하답니다. 이 파일은 프로젝트 루트에 보통 `.next/imports.lock` 형태로 존재하는데, 이걸 잘 확인하고 관리해야 나중에 빌드 에러를 피할 수 있어요.

그리고, 개발 중에 새로운 URL Import를 추가하거나, CDN에서 새로운 버전이 나왔을 때도 `next dev`를 한 번 돌려줘야 `lockfile`이 최신 상태로 유지된답니다. 빌드파이프라인에 넣는다면 캐시 관리 전략도 잘 세우는 것이 좋겠죠?

Markdown으로 표를 만들어 정리하면 아래와 같아요.

| 명령어       | 동작 설명                                            |
|--------------|-----------------------------------------------------|
| `next dev`   | 새로 발견된 URL Import를 다운로드하고 lockfile에 추가 |
| `next build` | lockfile에 기록된 URL Import만 사용해 빌드           |

이정도면 URL Import 관련 작업 흐름을 이해하는 데 도움이 됐길 바라요!

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

이번에는 React 코드 예제와 함께 이미지 import 방법, 그리고 CSS에서 URL을 사용하는 방법에 대해 살펴볼게요. Next.js나 React 프로젝트에서 이미지를 다룰 때 유용한 팁도 함께 알려드릴게요.

---

## React에서 confetti 효과 넣기

```js
import confetti from 'https://cdn.skypack.dev/canvas-confetti'
import { useEffect } from 'react'
 
export default () => {
  useEffect(() => {
    confetti()
  })
  return <p>Hello</p>
}
```

위 코드는 `canvas-confetti` 라이브러리를 사용해서 페이지가 로드될 때 폭죽 터지는 효과를 내는 예제입니다. `useEffect` 안에 `confetti()`를 호출하면 컴포넌트가 마운트될 때마다 confetti가 실행되죠.

> 참고로, `useEffect` 두 번째 파라미터로 빈 배열 `[]`을 넣으면 마운트 시에만 실행되어 성능상 더 좋습니다.

```js
useEffect(() => {
  confetti()
}, []) // 한 번만 실행
```

---

## 정적 이미지 import하기 (Static Image Imports)

```js
import Image from 'next/image'
import logo from 'https://example.com/assets/logo.png'
 
export default () => (
  <div>
    <Image src={logo} placeholder="blur" />
  </div>
)
```

Next.js에서 이미지 최적화를 위해 `next/image`를 많이 사용하죠? 위처럼 URL로 이미지를 직접 import하는 방법도 있지만, 사실 Next.js가 공식적으로 지원하는 `next/image`의 `src` prop에는 보통 로컬 파일 경로나 공개된 URL 문자열이 들어갑니다.

> 이렇게 `import logo from '...'` 방식은 보통 `next.config.js`에서 remote 패턴이 설정되어 있어야 동작합니다.  
> 그리고 `placeholder="blur"`를 사용하면 로딩 전 저해상도 블러 이미지가 잠깐 보이면서 UX가 향상됩니다.

---

## CSS의 URL 사용법

마지막으로 CSS에서 URL을 사용할 때는 보통 백그라운드 이미지 등으로 이렇게 쓰죠:

```css
.my-div {
  background-image: url('/images/bg.png');
}
```

CSS 파일 내에서 URL을 넣을 때 유의할 점!

- URL 경로가 올바른지 (특히 빌드 시 경로 변환 이슈 체크)
- Next.js같은 프레임워크에서는 public 폴더 하위에 이미지 넣고 `/images/bg.png`처럼 절대경로로 쓰는 게 편리

---

### 정리: 이미지 다룰 때 주의점

| 포인트                 | 설명                                                                    |
|------------------------|-------------------------------------------------------------------------|
| 이미지 import 방식      | 로컬 이미지 import는 `import img from '...'` 로, URL 문자열도 가능       |
| next/image 사용 시      | 최적화, lazy-loading, placeholder(blurry) 기능 활용                      |
| CSS url 경로           | public 폴더 내 이미지 절대경로로 지정하는 게 안전                         |
| confetti 효과 넣기     | `canvas-confetti` 라이브러리로 간단히 폭죽 효과 구현 가능                |

---

다양한 방식으로 이미지를 다루고 효과를 넣는 방법을 알아봤는데요, 프로젝트 상황에 따라 적절한 방법을 선택해서 더 멋진 UI/UX를 만들어 보시길 바랍니다! 궁금한 점 있으면 또 알려주세요 :)

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

JS에서 이미지나 파일 경로를 다룰 때, 그냥 문자열로 쓰는 것과 URL 객체로 처리하는 방식이 있어요. 예제 코드도 두 가지 상황을 보여주는데요, 쉽게 설명해드릴게요.

```css
.className {
  background: url('https://example.com/assets/hero.jpg');
}
```
이 코드는 CSS에서 배경 이미지로 직접 URL을 지정하는 경우예요. 이러면 그냥 해당 링크로 바로 이미지가 로드됩니다. 근데 이런 방식은 빌드나 배포 시 파일이 어떻게 관리되는지에 따라 문제가 생길 수 있어요.

---

```js
const logo = new URL('https://example.com/assets/file.txt', import.meta.url);

console.log(logo.pathname);

// 출력 예시: "/_next/static/media/file.a9727b5d.txt"
```

두 번째 코드는 JavaScript 모듈 안에서 `URL` 객체를 생성해서 파일 경로를 관리하는 예시예요. `import.meta.url`은 현재 모듈의 URL을 알려주고, 이걸 기준으로 상대경로나 절대경로를 처리할 수 있죠.

예를 들어 Next.js나 Vite 같은 최신 프레임워크들은 Assets(이미지, 폰트, 텍스트 파일 등)를 이렇게 import하거나 URL 객체화 해서 다뤄요. 이렇게 하면 빌드 시점에 최적화된 경로(해시가 붙거나 static 경로로 변경된 경로)로 변환되니까, 실제 배포된 파일 위치와 항상 맞게 관리할 수 있어요.

| 장점 | 설명 |
| --- | --- |
| 빌드 최적화 지원 | 파일명에 해시가 붙어 캐시 무효화 자동 처리 가능 |
| 경로 관리 용이 | 상대 패스 문제를 줄이고, 절대 경로 기반으로 안정적으로 자원 참조 |
| 타입 안정성 | 타입스크립트 사용 시, import 시점에 오류 잡기 용이 |

---

### 추가 팁

- CSS에서 URL 경로를 지정할 때, 로컬 파일이라면 웹팩이나 Vite 같은 빌드 도구가 알아서 경로를 처리하게끔 설정하는 게 좋아요.
- JS에서 URL 객체 쓸 때는, URL 생성자가 브라우저 환경과 Node 환경에서 다르게 동작할 수 있으니 환경도 확인하세요.
- Next.js 같은 경우는 `import`로 이미지, 텍스트 파일 등을 직접 불러와서 JSX에 쓸 수 있으니, 공식 문서 참고하는 걸 추천해요.

이렇게 파일 경로나 자원 관리를 조금만 신경 써도 배포 후 깨지는 이미지 문제나 경로 문제를 훨씬 줄일 수 있어요! 개발할 때 자주 보게 될 테니까 익숙해지면 좋습니다 :)