---
title: "Next.js 15에서 레이아웃과 페이지 쉽게 만드는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 01:01
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "How to create layouts and pages"
link: "https://nextjs.org/docs/app/getting-started/layouts-and-pages"
isUpdated: false
---

# 레이아웃과 페이지 만드는 방법

Next.js는 파일 시스템 기반 라우팅을 사용해요. 즉, 폴더와 파일 구조를 활용해서 라우트를 정의할 수 있다는 거죠. 이번 글에서는 레이아웃과 페이지를 어떻게 만들고 서로 링크하는지 쉽게 알려드릴게요.

## 페이지 만들기

페이지란 특정 경로에서 렌더링되는 UI를 뜻해요. 페이지를 만들려면 `app` 디렉토리 안에 파일을 추가하고, 그 파일에서 React 컴포넌트를 기본 내보내기(default export) 하면 됩니다. 예를 들어, 인덱스 페이지(`/`)를 만들고 싶으면 아래처럼 하면 돼요:

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

아래 이미지는 Next.js에서 페이지를 간단하게 만드는 예시 코드와 관련된 내용입니다.

```js
export default function Page() {
  return <h1>Hello Next.js!</h1>;
}
```

위 예시처럼 기본적으로 Next.js에서는 함수를 작성해서 간단한 페이지를 만들 수 있어요.

---

## 레이아웃 만들기 (Creating a layout)

레이아웃(layout)이란 여러 페이지에서 공통으로 쓰이는 UI를 의미해요. 예를 들어 네비게이션 바, 사이드 메뉴, 푸터 같은 부분들이 여기에 해당하겠죠.

Next.js의 장점 중 하나는 네비게이션을 할 때 레이아웃이 다시 렌더링되지 않고, 상태를 유지하며 대화형을 계속 유지한다는 점이에요. 덕분에 페이지가 바뀌어도 빠르고 자연스러운 사용자 경험을 제공할 수 있어요.

> **추가 팁!**  
> 레이아웃을 만들 땐 `app/layout.js` 파일을 활용하면 좋아요. 이 파일에 공통 UI를 정의하면, 그 하위에 있는 모든 페이지에서 레이아웃이 자동으로 적용됩니다. 그래서 중복 코드를 줄이고, 전체 애플리케이션의 구조를 깔끔하게 관리할 수 있답니다.

---

여기까지 Next.js에서 어떻게 간단한 페이지를 만들고, 공통 UI를 위해 레이아웃을 활용하는지 살펴봤어요. 다음에는 레이아웃을 실제로 구현하는 방법에 대해 더 깊게 다뤄볼게요!

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

리액트에서 레이아웃(Layout)을 정의하는 방법에 대해 이야기해볼게요. 기본적으로 레이아웃 파일에서 React 컴포넌트를 default export 하면 레이아웃으로 사용할 수 있어요. 이 컴포넌트는 꼭 `children` prop을 받아야 하는데요, 이 `children`이 바로 각각의 페이지나 다른 레이아웃이 될 수 있답니다.

예를 들어, `app` 디렉토리 안에 레이아웃 파일을 만들어서, 그 안에 index 페이지를 자식으로 받는 레이아웃을 만들어보는 거죠.

아래 코드를 보면 어떻게 구성되는지 감이 잡히실 거예요.

```jsx
export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        {/* 레이아웃 UI 영역 */}
        {/* 여기에 children을 넣어서 페이지나 중첩된 레이아웃을 렌더링 */}
        <main>{children}</main>
      </body>
    </html>
  );
}
```

이렇게 레이아웃 컴포넌트를 만들면, `DashboardLayout`이 감싸는 모든 페이지는 이 레이아웃 UI 안에 렌더링됩니다. 즉, 공통적으로 적용하고 싶은 UI(헤더, 사이드바, 푸터 등)를 레이아웃 컴포넌트 안에 넣으면 유지보수가 훨씬 편해지죠.

더불어, Next.js 13 이상에서는 `app` 폴더 기반 라우팅 시스템에서 이런 레이아웃 정의가 기본으로 사용되니 꼭 익혀두면 좋아요!

참고로, `children`에 들어가는 값이 또 다른 레이아웃일 수도 있어서, 레이아웃을 중첩해서 사용할 수 있다는 점도 기억해두세요.

필요하다면, 레이아웃에 헤더, 네비게이션 바 등을 넣어 사용자 경험이 일관되게 만들어주는 용도로 아주 유용하답니다!

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

위에 나오는 레이아웃을 '루트 레이아웃(root layout)'이라고 부르는데요, 그 이유는 앱 디렉토리의 최상단(루트)에 정의되기 때문이에요. 루트 레이아웃은 앱에 꼭 필요하고, html 태그와 body 태그를 반드시 포함해야 한다는 점도 기억하세요.

## 중첩 네스트드 라우트(Nested Route) 만들기

네스트드 라우트는 여러 URL 세그먼트로 구성된 경로를 의미해요. 쉽게 말해, URL이 여러 부분으로 나뉘어 있는 구조죠. 예를 들어서, `/blog/[slug]` 라우트는 세 개의 세그먼트로 이루어져 있어요:

| 세그먼트 종류 | 설명                                | 예시     |
| ------------- | ----------------------------------- | -------- |
| 루트 세그먼트 | 최상위 경로                         | `/`      |
| 중간 세그먼트 | 중간 디렉토리경로                   | `blog`   |
| 리프 세그먼트 | 마지막 세그먼트(동적 세그먼트 포함) | `[slug]` |

여기서 `[slug]`는 동적 세그먼트를 의미하는데, 예를 들어 특정 블로그 글의 고유 주소 부분을 표현할 때 사용돼요.

추가로, 중첩 라우팅을 활용하면 UI를 더 효율적으로 구성할 수 있어요. 예를 들어 공통 레이아웃을 중간 세그먼트인 `blog` 아래에 두고, 각 글은 그 레이아웃 안에서 화면에 표시하는 식이죠. Next.js나 React 기반 프레임워크에서 이런 네스트드 라우팅을 지원해주니까 활용해보시면 좋아요!

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

Next.js에서 라우트를 어떻게 만들고 관리하는지 간단하게 알려줄게요!

---

### Next.js 라우팅 기본 개념

- **폴더**: URL 경로(segment)를 정의하는 역할을 해요.
- **파일**: 예를 들어 `page.tsx`나 `layout.tsx` 같은 파일은 해당 경로(segment)에 보여질 UI를 담당해요.

---

### 중첩 라우트 만드는 법

중첩 라우트를 만들고 싶으면 폴더 안에 폴더를 계속 넣으면 돼요.

예를 들어, `/blog` 라는 경로를 만들고 싶다고 하면,  
`app` 폴더 안에 `blog`라는 폴더를 만들면 됩니다.

그리고 `/blog` 경로에서 보여줄 페이지를 만들려면, `blog` 폴더 안에 `page.tsx` 파일을 만들어야 해요.  
이 `page.tsx`가 실제로 `/blog` 경로에서 렌더링될 UI를 담당하죠.

---

### 구조 예시

| 경로                | 설명                                  |
| ------------------- | ------------------------------------- |
| `app/blog/`         | `/blog` 경로(라우트)의 폴더           |
| `app/blog/page.tsx` | `/blog` 경로에 보여질 페이지 컴포넌트 |

---

### 추가 꿀팁!

- Next.js 13부터는 `app` 디렉토리 기반의 라우팅이 기본이에요. 이전의 `pages` 폴더 스타일보다 훨씬 직관적이고, 레이아웃 관리도 편해졌어요.
- `layout.tsx` 파일을 같은 위치 혹은 상위 폴더에 두면, 그 경로에 공통으로 적용되는 레이아웃을 만들 수 있답니다.
- 예를 들어, `app/blog/layout.tsx`를 만들면 `/blog` 아래 모든 페이지가 공통 레이아웃을 가지게 되죠.

---

이미지로 보면 이런 느낌이에요!

![폴더 구조와 page.tsx](/TIL/assets/img/2025-04-22-Howtocreatelayoutsandpages_2.png)  
(※ 실제 이미지 주소가 다르다면 알맞게 수정해 주세요.)

---

Next.js로 라우팅 구조 짤 때는 folder = URL segment, 그리고 그 segment에 보여줄 UI는 파일로 관리한다!  
이 공식만 기억하면 중첩 라우팅도 쉽게 할 수 있으니까 꼭 익혀두세요! 😊

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

계속해서 중첩 폴더를 만들어 경로를 중첩해서 관리할 수 있어요. 예를 들어, 블로그 개별 포스트 페이지를 만들고 싶다면 `blog` 폴더 안에 `[slug]`라는 이름의 폴더를 새로 만들고, 그 안에 `page.js` 파일을 추가하면 됩니다.

아래처럼 기본적으로 `generateStaticParams` 함수를 만들어서 어떤 슬러그들이 있을지 미리 정의해줄 수도 있어요.

```js
function generateStaticParams() {
  // 예: 블로그 글 슬러그들을 담아 리턴
  return [{ slug: "my-first-post" }, { slug: "hello-world" }];
}

export default function Page() {
  return <h1>Hello, Blog Post Page!</h1>;
}
```

이런 식으로 슬러그를 동적으로 받아서 각각의 블로그 포스트 페이지를 만들 수 있답니다.

---

📌 여기서 잠깐!  
`[slug]` 폴더명에서 대괄호(`[]`)는 동적 라우트를 의미해요. 즉, URL 경로에 따라 달라지는 부분을 변수처럼 처리한다는 거죠. 예를 들어 `/blog/my-first-post`나 `/blog/hello-world` 같은 주소가 들어오면 각각에 맞는 슬러그 값을 받아 페이지를 렌더링하는 식이에요.

정리하면, 폴더 구조가 URL 구조와 일치하므로 직관적으로 관리하고, 동적 라우팅 덕분에 다양한 경로를 손쉽게 처리할 수 있어요.

이걸 응용하면 포트폴리오, 쇼핑몰 상품 페이지, 유저 프로필 등 여러 상황에 딱 맞는 동적 페이지를 편하게 만들 수 있겠죠? 😄

---

### 추가 팁: `generateStaticParams` 활용하기

- 이 함수는 빌드 시점에 호출되어, 미리 생성할 정적 경로를 알려줍니다.
- 블로그 글처럼 내용이 자주 바뀌지 않는 경우 미리 정적으로 빌드해두면 성능이 엄청 좋아져요.
- 반대로 동적으로 내용을 자주 바꾸는 경우엔 서버 사이드 렌더링이나 ISR(Incremental Static Regeneration) 등의 기법도 고려해보세요!

이상으로 Next.js 앱 라우팅에서 중첩 폴더와 동적 라우트 설정하는 법을 간단히 알아봤어요. 궁금한 점 있으면 언제든 질문 주세요!

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

폴더 이름을 대괄호([])로 감싸면 예를 들어 [slug]처럼, 이 부분이 동적 라우트 세그먼트(dynamic route segment)가 돼서 데이터 기반으로 여러 페이지를 쉽게 만들어줄 수 있어요. 블로그 포스트나 상품 페이지처럼 같은 구조지만 내용이 다른 여러 페이지를 만들 때 정말 유용하답니다.

## 레이아웃 중첩(Nesting layouts)

기본적으로 폴더 구조에 따라 레이아웃도 중첩돼요. 즉, 부모 레이아웃이 자식 레이아웃을 children 프로퍼티로 감싸는 형태라는 뜻이죠. 이걸 활용해서 특정 라우트(폴더)마다 레이아웃을 따로 지정할 수 있어요.

예를 들어, `/blog` 경로를 위한 레이아웃을 만들고 싶다면 blog 폴더 안에 layout 파일을 추가하면 됩니다. 이렇게 하면 `/blog`와 그 하위 페이지에만 적용되는 레이아웃을 별도로 관리할 수 있어서 더 체계적이고 유지보수도 쉬워져요.

더 덧붙이자면, 이 방식은 여러 페이지의 공통 UI(예: 네비게이션 바, 푸터 등)를 재사용할 때 아주 효과적이에요. 레이아웃을 잘 설계해 놓으면 페이지 개발 속도가 확실히 빨라지고, UI 일관성도 자연스럽게 유지할 수 있답니다.

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

앱을 만들 때 레이아웃을 중첩해서 사용하는 경우가 많아요. 위 코드에서는 `BlogLayout`이라는 컴포넌트를 정의했는데요, 이 컴포넌트가 `children`을 받아서 `<section>` 태그로 감싸고 있죠. 이건 블로그 관련 페이지만 따로 꾸밀 때 유용해요.

예를 들어, 루트 레이아웃인 `app/layout.js`가 전체 앱을 감싸고, 그 안에 `app/blog/layout.js`(즉, 위에서 정의한 `BlogLayout`)가 블로그 관련 페이지를 감싸는 구조라고 생각하면 됩니다. 그리고 실제 블로그 메인 페이지(`app/blog/page.js`)와 개별 포스트 페이지(`app/blog/[slug]/page.js`)가 그 안에 들어가는 식이죠.

이렇게 하면 기본 레이아웃은 유지하면서도, 특정 섹션(여기서는 블로그)만 별도로 스타일이나 구성을 다르게 할 수 있어서 훨씬 관리하기 편해집니다.

---

## 페이지 간 링크 연결하기

페이지들을 만들었으면 이제 서로 연결해줘야겠죠? Next.js에서는 `<Link>` 컴포넌트를 사용해서 페이지 간 이동을 쉽게 할 수 있어요. 예를 들어 블로그 리스트에서 각각의 포스트 페이지로 이동하고 싶을 때 이렇게 할 수 있습니다:

```jsx
import Link from "next/link";

export default function BlogList({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.slug}>
          <Link href={`/blog/${post.slug}`}>{post.title}</Link>
        </li>
      ))}
    </ul>
  );
}
```

위 예제는 간단한 블로그 리스트 컴포넌트인데요, `posts` 배열을 받아서 각 포스트 제목에 맞게 링크를 걸어줍니다. 이렇게 하면 Next.js가 자동으로 사전 렌더링과 클라이언트 측 네비게이션을 최적화해줘서 사용자 경험이 훨씬 좋아져요.

---

### 팁!

- 중첩 레이아웃을 쓸 때는 각 레이아웃 파일 이름과 위치가 중요해요. 예를 들어 `app/blog/layout.js`는 `/blog` 경로 하위 모든 페이지에 자동으로 적용됩니다.
- 레이아웃 안에 헤더나 푸터를 넣어서 반복되는 UI를 한 번만 작성해도 효율적입니다.
- 링크를 걸 때는 가능한 `next/link`를 써주세요. `<a>` 태그만 쓰면 페이지 전환 시 전체가 새로고침돼서 느릴 수 있어요.

이제 여러분도 레이아웃을 중첩해서 더 구조화된 Next.js 앱을 쉽게 만들 수 있어요! 혹시 더 궁금한 점 있으면 언제든 질문해 주세요~

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

Next.js에서 페이지 간 이동을 할 때는 `Link` 컴포넌트를 사용하면 아주 편리해요. `Link`는 HTML의 기본 `a` 태그를 확장한 컴포넌트로, 미리 페이지를 불러오는(prefetching) 기능과 클라이언트 사이드 네비게이션을 지원해줘서 사용자 경험을 훨씬 부드럽게 만들어줍니다.

예를 들어, 블로그 글 목록을 만들고 싶다면, `next/link`에서 `Link`를 임포트(import)한 다음, 각 글의 경로를 `href` 속성에 넘겨주면 돼요.

```js
import Link from "next/link";

export default async function PostList() {
  const posts = await getPosts();

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.slug}>
          <Link href={`/posts/${post.slug}`}>{post.title}</Link>
        </li>
      ))}
    </ul>
  );
}
```

여기서 주목할 점은, `Link` 컴포넌트는 내부적으로 HTML의 `<a>` 태그를 사용하지만, Next.js가 최적화해주는 사전 로딩과 클라이언트 사이드 라우팅 기능을 추가로 제공한다는 것이에요.

> 참고로, Next.js의 공식 문서에서는 페이지 이동 시 `Link`를 쓰는 것을 권장합니다. 하지만 더 세밀한 라우팅 제어가 필요할 때는 `useRouter` 훅을 사용해 프로그래밍 방식으로 라우팅할 수도 있어요.

더불어 `Link` 내부에 `<a>` 태그를 직접 감싸는 예전 방식도 있었는데, Next.js 13버전부터는 `href`를 바로 `Link`에 넘겨주고 텍스트나 요소를 자식으로 넣는 방식이 표준이에요. 혹시 이전 문서를 참고한다면 이 부분 조금 헷갈릴 수 있으니 주의하세요!

간단하게 정리하면:

| 특징                         | 설명                                                   |
| ---------------------------- | ------------------------------------------------------ |
| Link 컴포넌트                | HTML `<a>` 태그에 라우팅 기능이 추가된 컴포넌트        |
| prefetching                  | 화면에 보이기 전에 미리 페이지를 불러와 빠른 이동 지원 |
| 클라이언트 사이드 네비게이션 | 전체 페이지 새로고침 없이 부드럽게 라우팅              |
| useRouter 훅 사용            | 프로그래밍 방식으로 라우팅을 제어할 때 활용 가능       |

이처럼 Next.js에서는 기본적으로 `Link`를 활용해 사용자에게 자연스러운 페이지 전환 경험을 제공하는 게 가장 깔끔한 방법입니다!
