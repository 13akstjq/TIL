---
title: "nextjs 15 접근 금지 페이지 forbidden.js로 쉽게 만드는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:37
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "forbidden.js"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/forbidden"
isUpdated: false
---


# forbidden.js 파일 설명

Next.js에서 인증 과정 중 forbidden 함수가 호출될 때 표시되는 UI를 커스터마이징할 수 있는 파일이 바로 forbidden.js예요. 이 파일을 통해 사용자가 권한이 없을 때 어떤 화면을 보여줄지 자유롭게 디자인할 수 있답니다. 그리고 중요한 점은, 이 컴포넌트를 렌더링하면 Next.js가 자동으로 HTTP 상태 코드 403(Forbidden)을 반환한다는 거예요.

예를 들어, 기본적인 forbidden.js는 아래처럼 작성할 수 있어요:

```js
import Link from 'next/link'

export default function Forbidden() {
  return (
    <div>
      <h2>Forbidden</h2>
      <p>You are not authorized to access this resource.</p>
      <Link href="/">Return Home</Link>
    </div>
  )
}
```

조금 더 친근하게 메시지를 바꾸거나, 추가적인 안내 문구를 넣어 사용자 경험을 개선할 수도 있겠죠? 예를 들어, 로그인이 필요한 경우라면 로그인 페이지로 바로 가는 버튼을 추가하는 것도 좋은 방법이에요.

```js
import Link from 'next/link'

export default function Forbidden() {
  return (
    <div style={{ textAlign: 'center', margin: '2rem' }}>
      <h2>접근 금지</h2>
      <p>이 페이지에 접근할 권한이 없어요.</p>
      <p>계속하려면 로그인이 필요합니다.</p>
      <Link href="/login">
        <a style={{ color: 'blue', textDecoration: 'underline' }}>로그인 하러 가기</a>
      </Link>
      <br />
      <Link href="/">
        <a style={{ color: 'gray', marginTop: '1rem', display: 'inline-block' }}>홈으로 돌아가기</a>
      </Link>
    </div>
  )
}
```

> 참고로, 이런 방식으로 UI를 커스텀하면 사용자에게 명확한 안내를 제공할 수 있고, 더 나은 사용자 경험(UX)을 만들 수 있다는 점 기억하세요!

## 참고자료
- Next.js 공식 문서: [Error Handling](https://nextjs.org/docs/advanced-features/custom-error-page)
- HTTP 상태 코드 403: [MDN - 403 Forbidden](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/403)

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

### Props

forbidden.js 컴포넌트는 어떤 props도 받지 않습니다.

## 버전 히스토리

| 버전       | 변경사항                   |
|------------|---------------------------|
| `v15.1.0`  | forbidden.js가 새로 추가됨 |

버전 히스토리를 보면 forbidden.js는 v15.1.0에서 처음 등장했네요. 새로운 컴포넌트를 도입할 때는 이렇게 변경사항을 꼼꼼히 챙겨두면 나중에 관리할 때 정말 도움이 됩니다. 개발하면서 버전 관리는 꼭 습관처럼 하시길 추천드려요!