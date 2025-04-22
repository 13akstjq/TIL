---
title: "Nextjs15에서 manifest.json 설정하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:50
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "manifest.json"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/metadata/manifest"
isUpdated: false
---


# manifest.json이란?

웹 앱을 만들 때 'manifest.json' 또는 'manifest.webmanifest' 파일을 루트 디렉토리에 넣어주는 게 좋아요. 이 파일은 웹 앱이 어떻게 동작하는지, 어떤 이름을 가졌는지, 아이콘은 무엇인지 등 브라우저가 알아야 할 정보를 담고 있답니다. 쉽게 말해, 내 웹 앱의 소개서를 만들어서 브라우저에게 알려주는 역할이에요.

## 고정된(static) manifest 파일 예시

아래는 간단한 manifest.json 예시입니다. 주로 이런 형식으로 만들어서 프로젝트 루트에 넣어주면 되죠.

```json
{
  "name": "My Next.js Application",
  "short_name": "Next.js App",
  "description": "An application built with Next.js",
  "start_url": "/"
  // 기타 설정들...
}
```

여기서 주요 속성들을 짚어볼게요.

| 속성명       | 설명                                             |
|--------------|--------------------------------------------------|
| name         | 웹 앱의 전체 이름, 기기나 설치 시 표시됨         |
| short_name   | 이름 대신 사용할 짧은 이름, 아이콘 밑 등에서 표시 |
| description  | 앱에 대한 간단한 설명                             |
| start_url    | 앱을 실행할 때 열릴 기본 URL                      |

---

### manifest.json을 왜 넣어야 할까?

* **PWA 지원**: Progressive Web App을 지원하려면 필수! 홈 화면에 아이콘을 추가하거나 오프라인 모드 같은 기능을 쓸 때 manifest가 필요해요.

* **탭 이름 및 아이콘 설정**: 브라우저 탭이나 북마크에 나타나는 이름과 아이콘을 설정할 수 있어요.

* **앱처럼 보이게 하기**: 풀스크린 모드, 방향 고정 등 다양한 옵션도 manifest에서 설정 가능하니 더 앱다운 느낌을 줄 수 있죠.

---

### 참고! manifest.json에 자주 쓰이는 주요 옵션들

| 속성명          | 설명                                                          |
|-----------------|---------------------------------------------------------------|
| icons           | 앱 아이콘 목록, 크기별 이미지를 배열로 넣음                     |
| display         | 앱이 실행될 때 UI 모드 (예: standalone, fullscreen, minimal-ui) |
| background_color| 앱 시작화면 배경색                                           |
| theme_color     | 브라우저 툴바 색상 등, 테마 컬러                               |
| scope           | 앱의 유효 범위 URL, 이 범위 내에서만 앱으로 인식됨             |

---

manifest.json 설정만 잘해줘도 사용자 경험이 크게 향상되니, Next.js 같은 프레임워크에서 PWA를 고민한다면 꼭 챙겨보세요!

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

## 매니페스트 파일 생성하기

이번에는 `manifest.js` 또는 `manifest.ts` 파일을 만들어서 `Manifest` 객체를 반환하는 방법을 알려드릴게요.

> 참고로 `manifest.js`는 Next.js의 특별한 Route Handler 중 하나예요. 기본적으로 캐시가 되지만, 만약 동적 API나 동적 설정 옵션을 사용하면 캐시되지 않아요.

```js
import type { MetadataRoute } from 'next'
 
export default function manifest(): MetadataRoute.Manifest {
  return {
    name: 'Next.js App',
    short_name: 'Next.js App',
    description: 'Next.js App',
    start_url: '/',
    display: 'standalone',
    background_color: '#fff',
    theme_color: '#fff',
    icons: [
      {
        src: '/favicon.ico',
        sizes: 'any',
        type: 'image/x-icon',
      },
    ],
  }
}
```

### 더 알아두면 좋은 팁!
- `start_url`은 사용자가 앱을 실행했을 때 기본으로 열리는 경로를 설정하는 거예요. 보통 루트(`/`)로 지정하지만 필요에 따라 다르게 할 수 있어요.
- `display` 옵션에는 `'standalone'` 외에도 `'fullscreen'`, `'minimal-ui'`, `'browser'` 등이 있어요. 각 옵션마다 앱을 어떻게 보여줄지 달라지니 원하는 사용자 경험에 맞게 선택하세요.
- `icons` 배열에는 여러 크기의 아이콘을 넣어두는 게 좋아요. 그래야 다양한 기기와 해상도에서 깔끔하게 앱 아이콘이 보여지거든요.

덤으로 매니페스트 파일은 PWA(Progressive Web App)를 만들 때 꼭 필요한 파일이니, 만약 여러분이 웹 앱을 좀 더 앱처럼 보이고 싶다면 꼭 만들어 보세요!

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

### Manifest Object

Manifest Object는 웹 표준이 새로워짐에 따라 업데이트될 수 있는 다양한 옵션들을 담고 있는 객체예요. 최신 옵션들에 대해 자세히 알고 싶다면, TypeScript를 사용 중이라면 코드 에디터 안에서 `MetadataRoute.Manifest` 타입을 참고하는 게 가장 확실하고요. 그렇지 않으면 [MDN 문서](https://developer.mozilla.org/en-US/docs/Web/Manifest)를 보는 것도 좋은 방법이에요.

참고로, Manifest Object는 주로 웹 앱이 어떻게 보이고 동작할지를 정의하는데 쓰여서, 푸시 알림, 아이콘, 시작 URL, 테마 컬러 같은 설정들을 포함하죠. 요즘에는 PWA(Progressive Web App)를 만들 때 꼭 필요한 요소 중 하나랍니다. 그래서 웹앱 개발자라면 한 번쯤 꼼꼼히 살펴볼 만한 부분이에요!