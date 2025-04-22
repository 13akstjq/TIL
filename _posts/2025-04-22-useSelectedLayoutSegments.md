---
title: "Next.js 15에서 useSelectedLayoutSegments로 레이아웃 세그먼트 선택하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:57
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "useSelectedLayoutSegments"
link: "https://nextjs.org/docs/app/api-reference/functions/use-selected-layout-segments"
isUpdated: false
---


# useSelectedLayoutSegments 훅 쉽게 이해하기

`useSelectedLayoutSegments`는 Next.js에서 클라이언트 컴포넌트 안에서 사용할 수 있는 아주 유용한 훅이에요. 이 훅은 호출된 Layout 컴포넌트 아래에서 활성화된(즉, 현재 URL 경로에서 사용 중인) 라우트 세그먼트(segment)를 읽어올 수 있어요.

예를 들어, 부모 Layout에서 자식 라우트가 어떤 상태인지 알아야 할 때, 특히 현재 페이지 경로를 기반으로 빵 부스러기(브레드크럼) 같은 UI를 만들 때 유용하답니다!

---

## 어떻게 쓰는지 간단한 예시

```js
'use client'
 
import { useSelectedLayoutSegments } from 'next/navigation'
 
export default function ExampleClientComponent() {
  const segments = useSelectedLayoutSegments()
 
  return (
    <ul>
      {segments.map((segment, index) => (
        <li key={index}>{segment}</li>
      ))}
    </ul>
  )
}
```

위의 코드는 현재 활성화된 라우트 세그먼트들을 배열로 받아와서 `<ul>`로 나열해주는 아주 기본적인 사용법입니다. 예를 들어 현재 경로가 `/products/electronics` 라면, `segments`는 `['products', 'electronics']` 형태가 될 거에요.

---

## 참고하면 좋은 점!

- **클라이언트 컴포넌트에서만 작동해요**  
  당연히 내부에 `'use client'` 지시자가 꼭 있어야 하고, 서버 컴포넌트에선 사용할 수 없어요.

- **Breadcrumbs 만들기에 매우 좋아요!**  
  부모 레이아웃에서 자식 경로를 확인해 빵 부스러기 네비게이션 UI를 만들거나, 현재 위치에 따라 다른 스타일을 적용하는 상황에 활용하기 딱이죠.

- **동적 라우트도 문제없어요!**  
  예를 들어 `/blog/[slug]` 같은 동적 경로를 사용할 때 현재 어떤 슬러그를 보고 있는지 쉽게 알 수 있어요.

- **배열 형태 반환**  
  항상 배열로 반환되니까 각 세그먼트를 간단히 `.map()` 해서 처리할 수 있답니다.

---

### 조금 더 발전된 활용 팁

```js
'use client'

import { useSelectedLayoutSegments } from 'next/navigation'

export default function Breadcrumbs() {
  const segments = useSelectedLayoutSegments()

  // 각 세그먼트를 적절히 대문자로 변환해서 보여주기
  const formattedSegments = segments.map(seg => seg.charAt(0).toUpperCase() + seg.slice(1))

  // 링크 처리를 할 때는, 현재까지의 경로를 누적해서 만들어줍니다.
  let path = ''
  return (
    <nav aria-label="breadcrumb">
      <ul style={{ display: 'flex', gap: '8px' }}>
        {formattedSegments.map((segment, idx) => {
          path += '/' + segments[idx]
          return (
            <li key={idx}>
              <a href={path}>{segment}</a>
              {idx < segments.length - 1 && ' / '}
            </li>
          )
        })}
      </ul>
    </nav>
  )
}
```

이렇게 하면 현재 위치에 따른 링크가 포함된 빵 부스러기 네비게이션을 만들 수 있답니다. 정말 간단하죠?

---

이 훅을 잘 활용하면 레이아웃 안에서 현재 경로 정보를 쉽게 읽어와 다양한 UI 변화를 줄 수 있으니 꼭 한 번 사용해보세요!

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

> 알아두면 좋은 점:  
useSelectedLayoutSegments는 클라이언트 컴포넌트 훅이고, 레이아웃(Layout)은 기본적으로 서버 컴포넌트라서, 보통 useSelectedLayoutSegments는 레이아웃 안에 임포트된 클라이언트 컴포넌트를 통해 호출됩니다.  
그리고 이 훅이 반환하는 segments 배열에는 경로 그룹(Route Groups)도 포함되는데, UI에 굳이 표시하고 싶지 않은 경우가 많아요. 그럴 땐 filter() 배열 메서드를 써서 대괄호로 시작하는 항목들(예: [group])을 제외하면 됩니다.

## 파라미터

| 파라미터          | 설명                                                      |
|------------------|---------------------------------------------------------|
| parallelRoutesKey | 선택적으로 사용 가능한 문자열로, 활성화된 특정 슬롯 내 경로 세그먼트를 읽을 때 사용 |


```js
const segments = useSelectedLayoutSegments(parallelRoutesKey?: string)
```

useSelectedLayoutSegments는 parallelRoutesKey를 인자로 받을 수 있는데, 이 키를 넘기면 해당 슬롯의 활성화된 라우트 세그먼트를 읽어올 수 있어요. 예를 들어, 하나의 레이아웃 안에 여러 병렬 라우트가 있을 때 유용하죠.

---

### 추가 팁!  
경로 그룹(Route Groups)은 파일이나 폴더 이름에 대괄호([])를 써서 만든 가상의 라우트 구획입니다. 보통 URL 경로에는 표시되지 않지만, 내부적으로 라우팅에 도움이 되죠. 그래서 UI에 표시하려면 걸러내는 게 좋고, 예를 들어 이렇게 쓰면 됩니다:

```js
const filteredSegments = segments.filter(segment => !segment.startsWith('['));
```

이런 세세한 관리가 가능해서, 내비게이션 UI나 Breadcrum 같은 컴포넌트를 만들 때 훨씬 깔끔한 결과물을 얻을 수 있답니다!

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

## 반환값 (Returns)

`useSelectedLayoutSegments` 훅은 호출된 레이아웃으로부터 **한 단계 아래에 있는 활성화된 세그먼트들**을 문자열 배열로 반환해줘요. 만약 그런 세그먼트가 전혀 없다면, 빈 배열을 반환합니다.

조금 더 쉽게 예를 들어볼게요. 다음과 같은 레이아웃 구조와 방문 URL이 있다고 할 때, 반환되는 세그먼트는 아래 표와 같이 나옵니다:

| Layout             | 방문한 URL           | 반환되는 세그먼트          |
|--------------------|----------------------|----------------------------|
| `app/layout.js`     | `/`                  | `[]`                        |
| `app/layout.js`     | `/dashboard`          | `['dashboard']`             |
| `app/layout.js`     | `/dashboard/settings` | `['dashboard', 'settings']` |
| `app/dashboard/layout.js` | `/dashboard`      | `[]`                        |
| `app/dashboard/layout.js` | `/dashboard/settings` | `['settings']`              |

---

### 좀 더 설명을 덧붙이자면

- 이 훅은 **현재 레이아웃 위치(폴더) 기준으로 다음 하위 경로들을 배열로 반환**하는 기능이에요. 예를 들어, `app/layout.js` 아래에서 호출하면 전체 경로의 첫 세그먼트부터 하나씩 배열로 주고, `app/dashboard/layout.js` 아래에서 호출하면 그 기준 폴더 바로 아래의 세그먼트들만 반환하는 거죠.
- 그래서 URL 경로가 길더라도 각 레이아웃별로 자신과 바로 아래 경로 구간을 정확하게 인지할 수 있어서, 복잡한 네스팅 구조에서도 유용하게 쓸 수 있어요.

> 만약 Next.js 같은 프레임워크를 쓰고 있고, 다이나믹 라우팅을 하거나 레이아웃별로 상태관리를 하고 싶다면 `useSelectedLayoutSegments`를 잘 활용해보세요. 현재 위치한 레이아웃 기준으로 어떤 세그먼트가 활성화되어 있는지 쉽게 파악할 수 있답니다!

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

## 버전 히스토리

| 버전       | 변경 사항                     |
|------------|------------------------------|
| `v13.0.0`  | `useSelectedLayoutSegments`가 새롭게 추가됨. |

---

이렇게 간단하게 버전별로 변경된 내용을 정리해두면, 나중에 업데이트 내역을 한눈에 파악하기 너무 편해요. 특히 라이브러리나 프레임워크를 사용할 때는 어떤 기능이 새로 생겼는지, 어떤 버그가 고쳐졌는지 알아두는 게 중요하거든요.

참고로, 여기서 소개한 `useSelectedLayoutSegments`는 특정 레이아웃 세그먼트(구간)를 선택하는 데 도움을 주는 hook인데요, React 같은 환경에서 라우팅이나 UI를 좀 더 세밀하게 제어할 때 유용하게 쓸 수 있답니다. 만약 더 궁금하면 관련 예제도 알려드릴게요!