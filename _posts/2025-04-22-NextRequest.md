---
title: " Next.js 15에서 API 요청 쉽게 처리하는 방법(NextRequest)"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 12:41
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "NextRequest"
link: "https://nextjs.org/docs/app/api-reference/functions/next-request"
isUpdated: false
---


# NextRequest

NextRequest는 기존의 Web Request API를 확장해서 더 편리한 메서드들을 추가한 거예요.

## cookies

Request의 Set-Cookie 헤더를 읽거나 수정할 수 있어요.

---

여기서 NextRequest는 보통 웹에서 HTTP 요청을 다룰 때 기본적으로 제공하는 Request 객체보다 좀 더 쓰기 편하게 기능을 확장한 거라고 생각하면 돼요. 특히 쿠키 같은 걸 다룰 때 번거로운 부분이 있는데, NextRequest는 그런 부분을 깔끔하게 처리해줘서 개발할 때 훨씬 편리하답니다.

예를 들어, 서버에서 특정 쿠키 값을 쉽게 읽거나, 새 쿠키를 설정할 때 도움이 되죠. 서버 사이드 렌더링(SSR)이 많아지는 요즘, 이런 유틸리티가 있으면 작업 속도가 확실히 빨라져요.

쿠키를 직접 다룰 때 주의할 점도 하나 말씀드리자면, 보안 설정(예: HttpOnly, Secure, SameSite 등)을 꼼꼼히 관리해야 해요. NextRequest 같은 툴로 쿠키를 다룰 때도 이런 설정을 신경 써줘야 보안 사고를 미연에 방지할 수 있으니 참고하세요!

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

### set(name, value)

이 함수는 특정 이름(name)과 값(value)을 받아서 요청(request)에 해당하는 쿠키를 설정해줘요.

예를 들어, 사용자가 `/home` 페이지에 들어왔을 때 배너를 숨기고 싶다면 이렇게 작성할 수 있죠:

```js
// /home 페이지에 들어온 요청에서
// 'show-banner'라는 쿠키를 'false'로 설정해서 배너를 숨겨요
// 이 요청은 'Set-Cookie: show-banner=false; path=/home' 헤더를 가지게 돼요
request.cookies.set('show-banner', 'false')
```

즉, 브라우저가 다음 번에 `/home` 경로에 요청을 보낼 때 `show-banner=false`라는 쿠키를 함께 보내게 됩니다.

---

### get(name)

다음으로 `get(name)`은 설정된 쿠키 중에서 특정 이름을 가진 쿠키의 값을 가져오는 함수예요.

```js
// 예를 들어 쿠키 중 'show-banner'의 값을 가져와서
// 그 값에 따라 배너를 보이거나 숨길 수 있어요
const bannerStatus = request.cookies.get('show-banner')
if (bannerStatus === 'false') {
  // 배너 숨기기 로직
} else {
  // 배너 보여주기 로직
}
```

이렇게 `get`을 사용하면 쿠키에 저장된 상태 정보를 쉽게 읽어서 사용자 경험을 맞춤화할 수 있답니다.

---

쿠키를 다룰 때는 보안도 중요해요! 예를 들어,

- **HttpOnly** 속성을 사용하는 경우 자바스크립트에서는 쿠키를 읽지 못하도록 할 수 있어요.
- **Secure** 속성을 추가하면 HTTPS 연결에서만 쿠키가 전송돼요.
- **SameSite** 옵션으로 다른 사이트에서 쿠키를 보내는 걸 제한할 수도 있고요.

이런 옵션들도 필요에 따라 같이 설정하는 게 좋아요.

쿠키를 효과적으로 활용해서 사용자 경험을 개선해보세요!

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

쿠키 관련 작업을 할 때, 특정 쿠키의 값을 가져오거나 모든 쿠키를 확인하고 싶을 때가 많죠. 여기서는 두 가지 유용한 메서드에 대해 이야기해볼게요: `get()`과 `getAll()`입니다.

### get(cookieName)

- 주어진 쿠키 이름에 해당하는 쿠키 값을 반환해요.
- 만약 동일한 이름의 쿠키가 여러 개 있으면, 가장 먼저 찾은 쿠키 값을 반환합니다.
- 쿠키가 없으면 `undefined`를 반환해요.

예시를 들어볼게요.

```js
// 요청이 /home 경로로 들어왔다고 가정
// 쿠키 중에 { name: 'show-banner', value: 'false', Path: '/home' } 가 있다고 할 때
const bannerValue = request.cookies.get('show-banner');
console.log(bannerValue); // 'false'
```

여기서 기억할 점은, 도메인이나 경로와 같은 쿠키 속성 때문에 같은 이름의 쿠키가 여러 개 존재할 수 있다는 거예요. `get()`은 처음 발견한 값만 준다는 점도 참고하세요.

---

### getAll(cookieName)

- 특정 쿠키 이름을 주면, 그 이름을 가진 **모든** 쿠키 값을 배열로 반환합니다.
- 만약 이름 없이 호출하면, 요청에 포함된 모든 쿠키를 반환해요.

예를 들어:

```js
// 예를 들어 여러 개의 'theme' 쿠키가 있을 수 있어요
const themes = request.cookies.getAll('theme');
console.log(themes); // ['dark', 'light']

// 이름 없이 호출하면 모든 쿠키를 배열로 받아요
const allCookies = request.cookies.getAll();
console.log(allCookies);
// [ { name: 'show-banner', value: 'false' }, { name: 'theme', value: 'dark' }, ... ]
```

---

### 팁!

- 쿠키 이름은 보통 유니크해야 하지만, Path나 Domain 때문에 중복 있을 수 있어요. 그래서 꼭 필요할 때 `getAll()`을 이용해 여러 쿠키 값을 한 번에 알아보면 유용합니다.
- 쿠키 값은 문자열로 전달되니, JSON 형태 같은 데이터를 저장할 땐 `JSON.stringify`와 `JSON.parse`를 꼭 활용하세요.

---

이렇게 `get()`과 `getAll()`만 잘 활용해도 쿠키를 다루는 데 훨씬 편해질 거예요! 도움이 되셨나요? 궁금한 점 있으면 언제든 질문 주세요~

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

자, 오늘은 JavaScript에서 요청(request) 객체 내 쿠키를 다루는 방법에 대해 살펴볼게요. 쿠키를 읽고, 여러 개를 가져오고, 삭제하는 방법까지 간단하게 정리해볼게요.

---

### 쿠키 여러 개 가져오기

요청에 담긴 쿠키가 여러 개일 때 특정 이름을 가진 쿠키들 또는 모든 쿠키들을 가져오고 싶을 때가 있죠? `request.cookies.getAll()` 메서드를 사용하면 돼요.

```js
// 예를 들어 /home 경로에 이런 쿠키가 있을 때
[
  { name: 'experiments', value: 'new-pricing-page', Path: '/home' },
  { name: 'experiments', value: 'winter-launch', Path: '/home' },
]

// 특정 이름 'experiments' 쿠키 모두 가져오기
request.cookies.getAll('experiments')

// 아니면 그냥 모든 쿠키 가져오기
request.cookies.getAll()
```

이렇게 하면 해당 이름을 가진 모든 쿠키값 배열을 받을 수 있어서, 실험 기능 활성화 여부 등을 여러 개 동시 관리할 때 유용해요.

---

### 쿠키 삭제하기

쿠키를 없애고 싶을 때는 `delete(name)` 메서드를 사용하면 됩니다. 삭제 성공 여부도 boolean 형태로 알려줘서 관리하기 좋아요.

```js
// 'experiments' 쿠키를 삭제하려고 시도
const deleted = request.cookies.delete('experiments')
if(deleted) {
  console.log('쿠키가 정상적으로 삭제됐어요!')
} else {
  console.log('해당 이름의 쿠키가 없어서 삭제하지 못했어요.')
}
```

여기서 주의할 점! 쿠키 삭제는 클라이언트에서 관리되는 쿠키를 서버 사이드에서 다룰 때 잘 동작하는 경우가 많으니, 프론트엔드와 백엔드 쿠키 경로와 옵션이 일치하는지 확인하는 게 좋아요.

---

### 추가 팁: 쿠키 관리 시 체크할 점

- 쿠키 이름은 중복될 수 있으니 `getAll()`로 모두 가져오고 원하는 것을 필터링하는 게 안전합니다.
- `Path`, `Domain` 같은 속성이 다르면 동일 이름 쿠키 복수 존재 가능하니, 특정 경로나 조건을 꼭 확인하세요.
- 보안 설정(`HttpOnly`, `Secure`) 때문에 클라이언트에서 못 읽는 쿠키도 있으니 상황에 따라 백엔드에서만 처리할 수도 있답니다.

---

쿠키는 사용자 경험을 향상시키기 위한 중요한 도구인데요, 이렇게 JavaScript에서 요청 내 쿠키를 자유자재로 불러오고, 삭제할 수 있으면 훨씬 유연한 웹 서비스 운영이 가능해져요! 필요할 때 바로 꺼내 쓰는 방법, 오늘 꼭 알아가세요~

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

### has(name)

`has(name)`는 요청(request) 안에 특정 이름의 쿠키가 있는지 확인할 때 사용해요. 만약 해당 이름의 쿠키가 있으면 `true`를, 없으면 `false`를 반환하죠.

```js
// 'experiments'라는 이름의 쿠키가 요청에 있으면 true, 없으면 false 반환
request.cookies.has('experiments')
```

예를 들어 사용자 인증이나 실험 기능 토글 같은 상황에서 쿠키 존재 여부를 확인할 때 유용해요.

---

### clear()

`clear()` 메서드는 요청에 포함된 모든 쿠키를 한 번에 삭제해버릴 때 사용해요. 

이걸 쓰면 브라우저에 저장된 쿠키가 싹 사라지는 건 아니고, 현재 요청에서 다루는 쿠키 세트를 비우는 효과라고 보면 됩니다. 서버에서 받은 요청과 관련된 쿠키 데이터를 초기화할 때 좋죠.

---

#### 참고로!

쿠키는 HTTP 요청/응답의 중요한 부분이라서, 프론트엔드든 백엔드든 잘 다뤄야 해요. 예를 들어 보안 목적으로 HttpOnly, Secure, SameSite 속성을 잘 설정하거나, 쿠키 저장 용량과 만료 시간도 신경 써야 하죠.

마크다운에서 표로 쿠키 관련 주요 메서드 간단히 정리해봤어요.

| 메서드          | 설명                                          | 반환값            |
|-----------------|----------------------------------------------|-------------------|
| `has(name)`     | 요청에 특정 이름의 쿠키가 있는지 확인        | `true` or `false` |
| `clear()`       | 요청에 포함된 모든 쿠키 정보를 초기화         | 없음              |

이렇게 쿠키는 사용법이 간단해 보여도, 프로젝트에 따라 꼭 필요한 보안과 정책을 꼼꼼히 챙기는 게 중요해요!

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

요청(request)에서 Set-Cookie 헤더를 제거하고 싶을 때, 보통 클라이언트 측에서는 Set-Cookie를 직접 수정할 수 없어요. 왜냐하면 Set-Cookie는 서버에서 응답(response)에 포함하는 헤더라서, 요청(request)에서는 일반적으로 존재하지 않거든요.

하지만, 만약 요청에 포함된 쿠키들을 깔끔하게 초기화하거나 지우고 싶다면, `request.cookies.clear()` 같은 메서드를 사용할 수 있어요. 이 방법은 Next.js 13에서 도입된 새로운 서버 컴포넌트 혹은 API Route 핸들링에서 사용되는 쿠키 객체에 적용되는 방식입니다.

정리하면,

| 작업                  | 설명                                      |
|----------------------|-----------------------------------------|
| `request.cookies.clear()` | 요청에 포함된 모든 쿠키들을 지웁니다.           |
| Set-Cookie 헤더 변경     | Set-Cookie 헤더는 서버가 응답 시 설정하는 쿠키이므로, 요청에서 직접 제거 불가 |

또한, `nextUrl` 객체는 기본 URL API를 확장한 것으로, Next.js에서 유용하게 쓸 수 있는 여러 확장 기능이 포함되어 있어요. 예를 들어 URL의 쿼리 파라미터를 쉽게 다루거나, Next.js 라우팅에 특화된 프로퍼티들이 있죠.

간단하게 사용 예시를 보여드리자면:

```js
export function middleware(request) {
  console.log(request.nextUrl.pathname) // 현재 요청 경로
  request.cookies.clear() // 쿠키 초기화
}
```

참고로, `request.cookies`는 immutable 한 객체라서 바로 수정이 안 될 수도 있는데, Next.js에서는 이 부분을 직접 제어할 수 있도록 API를 제공합니다. 요청 쿠키를 다뤄야 할 때는 꼭 Next.js 문서를 참고해서 정확한 방법을 확인하는 것이 좋아요.

필요하시면 더 자세한 쿠키 다루기 방법이나 `nextUrl` 활용법도 알려드릴게요!

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
// /home 경로로 요청 시 pathname은 /home
request.nextUrl.pathname
// /home?name=lee 로 요청 시 searchParams는 { 'name': 'lee' }
request.nextUrl.searchParams
```

아래는 `request.nextUrl`에서 사용할 수 있는 주요 옵션들입니다:

| 속성           | 타입                     | 설명                                                                                  |
| -------------- | ------------------------ | ------------------------------------------------------------------------------------- |
| `basePath`     | `string`                 | URL의 [base path](https://nextjs.org/docs/app/api-reference/config/next-config-js/basePath)입니다.                     |
| `buildId`      | `string` \| `undefined`  | Next.js 앱의 빌드 식별자입니다. [커스터마이징](https://nextjs.org/docs/app/api-reference/config/next-config-js/generateBuildId)도 가능합니다. |
| `pathname`     | `string`                 | URL의 경로 부분입니다.                                                                |
| `searchParams` | `Object`                 | URL의 쿼리 파라미터들을 담고 있는 객체입니다.                                         |

> 참고) 페이지 라우터(Pages Router)에서 제공하던 국제화(i18n) 관련 속성들은 앱 라우터(App Router)에서는 사용할 수 없으니 참고하세요. 앱 라우터에서의 국제화는 [앱 라우터 문서](https://nextjs.org/docs/app/building-your-application/routing/internationalization)에서 더 자세히 확인할 수 있습니다.

---

덧붙이자면, `request.nextUrl.searchParams` 는 `URLSearchParams` 인터페이스를 기반으로 하므로, `.get()`, `.has()`, `.getAll()` 같은 메서드를 통해 쿼리 파라미터를 손쉽게 다룰 수 있다는 장점이 있어요. 예를 들어:

```js
const name = request.nextUrl.searchParams.get('name')
```

이렇게 하면 쿼리에서 'name' 값만 간단히 얻을 수 있으니 참고하세요! 😊

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

| 버전      | 변경 사항                  |
|-----------|---------------------------|
| `v15.0.0` | `ip`와 `geo` 기능이 제거됨 |

---

여기서 `ip`와 `geo`가 뭔지 궁금할 수 있는데요, 보통 `ip`는 IP 주소 관련 기능, `geo`는 지리 정보 관련 기능을 뜻해요. 그래서 이번 버전(v15.0.0)부터는 이 두 가지 기능이 빠졌다는 뜻이죠. 만약 프로젝트에서 IP나 위치 기반 기능을 쓴다면, 이번 업데이트 전후로 영향이 있는지 꼭 확인해보는 게 좋아요! 업데이트 후에는 이 기능들을 대신할 다른 라이브러리나 API를 찾아보는 것도 방법일 수 있겠네요.