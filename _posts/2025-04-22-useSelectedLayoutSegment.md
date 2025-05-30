---
title: "Nextjs 15에서 useSelectedLayoutSegment 훅을 활용하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:56
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "useSelectedLayoutSegment"
link: "https://nextjs.org/docs/app/api-reference/functions/use-selected-layout-segment"
isUpdated: false
---


# useSelectedLayoutSegment 훅이란?

`useSelectedLayoutSegment`는 클라이언트 컴포넌트에서 사용할 수 있는 훅으로, 호출한 레이아웃 바로 아래에 있는 활성화된(활성 상태인) 라우트 세그먼트를 읽을 수 있게 도와줘요.

### 언제 쓰면 좋을까?

예를 들면, 부모 레이아웃 안에 여러 자식 탭이 있을 때 각 탭의 활성화 여부에 따라 스타일을 바꾸고 싶을 때 유용해요. 즉, 현재 어떤 세그먼트(탭 혹은 페이지의 구간)가 활성화 되어 있는지 쉽게 알 수 있어서, 그 정보를 UI에 반영하면 됩니다.

### 코드 예시

```jsx
'use client'

import { useSelectedLayoutSegment } from 'next/navigation'

export default function ExampleClientComponent() {
  const segment = useSelectedLayoutSegment()

  return <p>Active segment: {segment}</p>
}
```

이렇게 하면 현재 활성화된 세그먼트 이름이 화면에 출력되죠.

---

### 추가 팁!

- `useSelectedLayoutSegment`는 부모 레이아웃 아래 한 단계의 세그먼트만 읽어올 수 있다는 점을 기억하세요. 예를 들어 `/dashboard/settings/profile` 같은 경로에서, `dashboard` 레이아웃 내에서 이 훅을 쓰면 `settings` 세그먼트만 받아옵니다. `profile`까지 깔끔하게 가져오려면 다른 방식이 필요해요!
- 복수 개의 세그먼트가 활성화되는 상황(중첩 라우팅 등)에서는 `useSelectedLayoutSegments`라는 복수형 훅도 있어서, 필요하면 함께 살펴보시는 것도 좋습니다.
- 네비게이션 UI를 만들 때 이 훅 덕분에 상태 관리나 URL 파싱 없이도 현재 위치를 쉽게 파악할 수 있어 개발 효율이 훨씬 올라가요!

Next.js의 App Router와 함께 쓸 때 특히 훌륭한 툴이니 다음 프로젝트에 바로 활용해보세요!

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
useSelectedLayoutSegment 훅은 클라이언트 컴포넌트에서 사용해야 합니다. 왜냐하면 레이아웃(Layout)은 기본적으로 서버 컴포넌트이기 때문이죠. 그래서 보통 useSelectedLayoutSegment는 레이아웃 안에 임포트된 클라이언트 컴포넌트에서 호출하게 됩니다.  
그리고 이 훅은 현재 활성화된 세그먼트 중 한 단계 아래(자식 레벨) 세그먼트만 반환해요. 만약 활성화된 모든 세그먼트를 한꺼번에 받고 싶다면 useSelectedLayoutSegments라는 훅을 참고하세요.

## 파라미터

| 파라미터명          | 타입         | 설명                                                                                  |
|------------------|------------|-------------------------------------------------------------------------------------|
| parallelRoutesKey | string (옵션) | 특정 슬롯 내에서 활성화된 경로 세그먼트를 읽고 싶을 때 사용합니다. 예를 들어 병렬 라우트가 여러 개인 경우 해당 슬롯을 지정할 수 있어요. |

```js
const segment = useSelectedLayoutSegment(parallelRoutesKey?: string)
```

---

### 추가로 알려드릴 팁!  
Next.js의 앱 디렉토리 구조에서 라우트 세그먼트(segment)는 URL 경로의 일부를 의미합니다. 예를 들어 `/dashboard/settings`라면 `dashboard`와 `settings`가 각각 하나의 세그먼트죠.  
useSelectedLayoutSegment 훅을 활용하면 현재 화면에서 어느 경로 세그먼트가 활성화되어 있는지를 쉽게 알 수 있어서, 조건부 렌더링이나 UI 상태 관리에 굉장히 유용합니다.

또한, parallelRoutesKey를 지정하는 경우는 조금 특별한데요, 예를 들어 하나의 레이아웃에 여러 병렬 경로 슬롯이 있다면, 어느 슬롯의 세그먼트를 조회할지 명확히 할 수 있어 좋습니다. 예전에는 복잡한 라우팅 정보를 관리하기 위해 별도의 로직을 짜야 했던 점을 생각하면, 이 훅이 확실히 편리함을 더해주죠!

꼭 클라이언트 컴포넌트 안에서만 쓸 수 있다는 점 기억하시고, 서버 컴포넌트라면 클라이언트 컴포넌트를 분리해서 함께 사용하는 구조로 짜시면 됩니다. 이 부분은 Next.js 13 이후 앱 디렉토리 사용 시 자주 보게 될 패턴이라 익숙해지면 개발이 한결 수월해질 거예요!

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

## Returns

`useSelectedLayoutSegment`은 현재 활성화된 세그먼트의 문자열을 반환하며, 만약 활성 세그먼트가 없다면 `null`을 반환합니다.

아래 표는 다양한 레이아웃과 방문한 URL에 따른 반환값 예시를 보여주는데요, 한눈에 이해하기 쉽게 정리해봤어요.

| Layout                    | 방문한 URL              | 반환 값          |
|---------------------------|-------------------------|------------------|
| `app/layout.js`            | `/`                     | `null`           |
| `app/layout.js`            | `/dashboard`            | `'dashboard'`    |
| `app/dashboard/layout.js`  | `/dashboard`            | `null`           |
| `app/dashboard/layout.js`  | `/dashboard/settings`   | `'settings'`     |
| `app/dashboard/layout.js`  | `/dashboard/analytics`  | `'analytics'`    |
| `app/dashboard/layout.js`  | `/dashboard/analytics/monthly` | `'analytics'` |

여기서 중요한 점은, 상위 레이아웃에서 `/dashboard` 같은 경로를 방문했을 때는 세그먼트가 바로 반환되지만, 하위 레이아웃에서는 경로에 따라 조금 다르게 동작한다는 거예요.

예를 들어, `app/dashboard/layout.js` 안에서 `/dashboard/analytics/monthly`를 방문하면 가장 가까운 하위 세그먼트인 `'analytics'`를 반환하죠. 이 부분은 레이아웃과 라우팅 계층 구조를 잘 이해하면 훨씬 더 효과적으로 활용할 수 있을 거예요.

추가 팁을 드리자면, 이 훅은 주로 동적으로 현재 경로에 따라 UI를 변경하거나, 세그먼트 기반으로 조건부 렌더링 해야 할 때 유용하게 사용할 수 있어요. React 내비게이션 상태 관리와 잘 결합하면 복잡한 네비게이션 로직을 훨씬 간결하고 명확하게 만들 수 있답니다!

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

## 예제

### 활성화된 링크 컴포넌트 만들기

`useSelectedLayoutSegment` 훅을 활용해서 현재 활성화된 세그먼트에 따라 스타일이 바뀌는 활성 링크 컴포넌트를 만들 수 있어요. 예를 들어, 블로그 사이드바에 자주 보여주는 게시글 리스트를 구현할 때 유용하죠.

```js
'use client'

import Link from 'next/link'
import { useSelectedLayoutSegment } from 'next/navigation'

// 이 *클라이언트* 컴포넌트는 블로그 레이아웃에 포함돼서 사용됩니다.
export default function BlogNavLink({
  slug,
  children,
}: {
  slug: string
  children: React.ReactNode
}) {
  // 예를 들어 `/blog/hello-world` 에서 현재 선택된 레이아웃 세그먼트는 'hello-world'가 됩니다.
  const segment = useSelectedLayoutSegment()
  const isActive = slug === segment

  return (
    <Link
      href={`/blog/${slug}`}
      // 링크가 활성화 됐을 때는 폰트가 굵게, 그렇지 않으면 기본 스타일로 표시합니다.
      style={{ fontWeight: isActive ? 'bold' : 'normal' }}
    >
      {children}
    </Link>
  )
}
```

---

여기서 살짝 더 알려드리자면, `useSelectedLayoutSegment`는 현재 URL 경로 중 해당 레이아웃의 세그먼트를 반환해주기 때문에, 특정 경로가 선택됐는지 쉽게 알 수 있어요. 이걸 활용해 사이드바 메뉴, 탭 네비게이션 등에서 '현재 위치' 표시를 깔끔하게 할 수 있답니다.

또한, 스타일뿐만 아니라 클래스명을 바꾸거나 아이콘을 변경하는 방식 등으로도 확장할 수 있으니 필요한 UI에 맞게 조절해보세요! Next.js 13의 레이아웃과 내비게이션 시스템을 활용하면 이런 사용자 경험이 훨씬 자연스럽고 간편해집니다.

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
// 클라이언트 컴포넌트인 BlogNavLink를 부모 레이아웃(Server Component)에 임포트하는 예제입니다.
import { BlogNavLink } from './blog-nav-link'
import getFeaturedPosts from './get-featured-posts'

export default async function Layout({
  children,
}: {
  children: React.ReactNode
}) {
  // 비동기로 추천 포스트를 받아옵니다.
  const featuredPosts = await getFeaturedPosts()

  return (
    <div>
      {/* 받아온 추천 포스트들을 하나씩 돌면서 BlogNavLink 컴포넌트에 slug와 제목을 넘겨 렌더링합니다. */}
      {featuredPosts.map((post) => (
        <div key={post.id}>
          <BlogNavLink slug={post.slug}>{post.title}</BlogNavLink>
        </div>
      ))}
      {/* 하위 컴포넌트(children)를 렌더링 합니다. */}
      <div>{children}</div>
    </div>
  )
}
```

---

### Version History

| Version   | Changes                      |
|-----------|------------------------------|
| `v13.0.0` | `useSelectedLayoutSegment` 훅이 도입되었습니다. |

---

#### 간단 팁!

- `Layout` 컴포넌트는 React의 **서버 컴포넌트(Server Component)**로 설계되어 있습니다. 서버에서 데이터를 받아오고 렌더링하는 데 최적화되어 있죠.
- 클라이언트 컴포넌트를 렌더링하려면(예: 여기서 `BlogNavLink`) 반드시 클라이언트 컴포넌트로 만들어야 하며, 이 부분은 "use client" 지시어를 파일 상단에 넣어야 합니다.
- Next.js 13의 새로운 라우팅 시스템에서 레이아웃은 중첩 구조를 가질 수 있어, 이처럼 하위 컴포넌트(children)를 받는 패턴이 일반적입니다.
- `useSelectedLayoutSegment`는 라우트가 활성화되었는지 알 수 있게 해줘서, 네비게이션 메뉴 활성화 등에 자주 사용되는 커스텀 훅이에요!

이 코드를 활용해서 동적인 네비게이션이나 블로그 포스트 목록 등을 서버 측에서 불러와서 쉽게 만들 수 있다는 점, 꼭 기억하세요 :)