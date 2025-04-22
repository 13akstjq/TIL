---
title: "nextjs 15에서 template.js 사용하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:47
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "template.js"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/template"
isUpdated: false
---

# template.js 파일 이해하기

이번에는 `template.js` 파일에 대해 이야기해볼게요. 템플릿 파일은 레이아웃(layout)과 비슷한 역할을 하지만, 조금 다릅니다.

레이아웃은 여러 라우트(route)에서 유지되면서 상태(state)를 유지하는 반면, 템플릿은 고유한 키(unique key)를 가지고 있어서 자식 클라이언트 컴포넌트(Client Components)의 상태가 페이지가 이동할 때마다 초기화(reset)된다는 차이가 있어요.

예를 들어, 다음과 같은 템플릿 컴포넌트가 있다고 가정해볼게요.

```js
export default function Template({ children }: { children: React.ReactNode }) {
  return <div>{children}</div>;
}
```

이 코드에서는 `children`을 받아서 `<div>`로 감싸기만 했죠. 이 템플릿을 사용하면, 페이지를 이동할 때마다 자식 컴포넌트들의 상태가 초기화됩니다.

---

### 상태 유지 vs 초기화: 왜 중요할까?

- **레이아웃**은 여러 페이지 사이를 오가더라도 상태가 유지돼요. 예를 들어, 사이드바가 열려있는 상태를 유지하고 싶다면 레이아웃이 맞아요.
- **템플릿**은 페이지 또는 특정 라우트마다 상태를 초기화해야 할 때 씁니다. 예를 들어, 폼 입력값을 페이지별로 완전히 새로 시작하고 싶을 때요.

---

### 꿀팁: 템플릿과 레이아웃, 언제 써야 할까?

- 상태 유지가 필요하면 → 레이아웃을 사용하세요.
- 상태 초기화가 필요하면 → 템플릿을 사용하세요.

i이해를 돕기 위해 이미지도 참고하면 좋아요!
t
![template 개념 이미지](/TIL/assets/img/2025-04-22-templatejs_0.png)

---

요약하자면, `template.js`는 레이아웃과 비슷하지만, 페이지가 바뀔 때마다 자식 컴포넌트 상태가 초기화 된다는 점에서 다릅니다. 프로젝트에서 상태 관리가 필요한 부분을 명확히 구분해서 템플릿과 레이아웃을 적절히 활용해 보세요!

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

가끔은 레이아웃(Layout) 대신에 템플릿(Template)을 선택하는 게 더 나은 경우도 있어요. 예를 들면:

- useEffect (예: 페이지 뷰 로깅)나 useState (예: 각 페이지별 피드백 폼) 같은 훅을 활용하는 기능이 필요할 때.
- 기본 프레임워크 동작 방식을 바꾸고 싶을 때. 예를 들어, 레이아웃 내의 Suspense 경계는 레이아웃이 처음 로드될 때만 fallback UI를 보여주고 페이지 전환 시에는 보여주지 않지만, 템플릿에서는 페이지가 바뀔 때마다 fallback UI가 매번 표시됩니다.

## Props

### children (필수)

children은 템플릿 내부에 렌더링될 자식 요소들을 의미합니다. 이걸 사용해서 부모 컴포넌트에서 자식 컴포넌트를 감싸거나 그 안에 배치할 수 있어요. React에서 자식 요소를 다룰 때 거의 항상 사용하니까 꼭 기억해두세요!

---

추가로, 레이아웃과 템플릿 선택 기준에 대해 조금 더 설명하자면, 레이아웃은 기본적으로 페이지 전환 시 지속되는 공통 UI나 구조를 만드는 데 쓰이고, 템플릿은 각 페이지가 로드될 때마다 새로 렌더링되어야 하는 동적인 UI나 상태 관리가 필요할 때 적합해요. 개발하는 서비스의 특성과 UX 목표에 따라 적절히 선택하면 됩니다!

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

Template 컴포넌트는 children prop을 받아 사용할 수 있어요. 예를 들어 이렇게 작성할 수 있죠:

```jsx
<Layout>
  {/* 템플릿에는 자동으로 고유한 key가 주어진다는 점 참고하세요. */}
  <Template key={routeParam}>{children}</Template>
</Layout>
```

> 알아두면 좋은 점:
> 기본적으로 템플릿은 서버 컴포넌트(Server Component)로 동작하지만, "use client" 지시어를 붙이면 클라이언트 컴포넌트(Client Component)로도 사용할 수 있습니다.
> 사용자가 템플릿을 공유하는 여러 라우트 사이를 이동할 때는 컴포넌트가 새로 마운트되고, DOM 요소들도 재생성돼요. 그래서 클라이언트 컴포넌트의 상태는 유지되지 않고, effect도 다시 실행됩니다.

즉, 템플릿을 클라이언트 측에서 사용한다면 상태 관리 혹은 사이드 이펙트 처리에 유의해야 한다는 점 기억하세요!

## Version History

(이후 버전 히스토리 내용이 이어지는 부분이라면 여기에 추가 정보를 넣을 수 있어요.)

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

아래는 표를 마크다운 형식으로 변환한 내용이에요.

| Version   | Changes                |
| --------- | ---------------------- |
| `v13.0.0` | `template` introduced. |

여기서 'template'이 새로 도입되었다고 하네요. 보통 이런 변경 사항은 라이브러리나 프레임워크의 중요한 기능 추가나 개선일 가능성이 높아요. 만약 여러분이 사용 중인 버전이 13.0.0이라면 이 'template' 기능을 꼭 한번 살펴보는 걸 추천해요. 새로운 기능이 어떤 식으로 코드를 더 깔끔하게 해주거나, 생산성을 높여줄지 직접 써보면서 경험해보는 게 가장 좋아요!
