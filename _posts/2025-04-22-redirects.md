---
title: "Nextjs 15 웹사이트에서 다른 url로 보내는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:19
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "redirects"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/redirects"
isUpdated: false
---


# Redirects (리다이렉트)

리다이렉트는 사용자가 특정 URL로 접속했을 때, 다른 URL로 자동으로 이동시키는 기능이에요. 예를 들어, 예전 페이지 주소가 바뀌었거나, 좀 더 깔끔한 URL로 바꾸고 싶을 때 유용하게 쓰이죠.

Next.js에서는 `next.config.js` 파일 안에 `redirects`라는 키를 써서 쉽게 리다이렉트를 설정할 수 있어요.

```js
module.exports = {
  async redirects() {
    return [
      {
        source: '/about',       // 리다이렉트가 발생할 원본 경로
        destination: '/',       // 이동할 대상 경로
        permanent: true,        // 301 리다이렉트 여부 (true면 301, false면 302)
      },
    ]
  },
}
```

- **source**: 사용자가 접속할 원래 경로
- **destination**: 사용자를 보내고 싶은 새 경로
- **permanent**: 영구 리다이렉트인가? (true=301, false=302)

### 추가로 알고 가면 좋은 점

- **동적 라우팅도 가능해요.** 예를 들어 `/blog/:slug` 같은 패턴을 잡아서 리다이렉트할 수 있어요.
- **프론트엔드에서 직접 처리하는 리다이렉트와 다릅니다.** 이 설정은 빌드 타임에 Next.js 서버 레벨에서 처리하기 때문에 SEO에도 긍정적이에요.
- 만약 조건부로 리다이렉트를 하고 싶다면, 이 함수 내에서 자유롭게 로직을 짤 수도 있습니다.

리다이렉트를 적절히 활용하면 사용자의 URL 접근 경험도 깔끔해지고, 잘못된 주소로 들어오는 걸 막는 것도 가능해서 사이트 운영에 큰 도움이 돼요!

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

redirects는 비동기(async) 함수로, source, destination, permanent 속성을 가진 객체들을 요소로 하는 배열을 반환해야 합니다.

| 속성           | 설명                                                                                                   |
|--------------|------------------------------------------------------------------------------------------------------|
| **source**      | 들어오는 요청 경로 패턴입니다.                                                                           |
| **destination** | 라우팅하려는 경로입니다.                                                                                  |
| **permanent**   | true이면 308 상태 코드를 사용해 클라이언트/검색엔진이 영구적으로 리다이렉트를 캐싱하도록 하고, false이면 307 상태 코드를 사용해 임시 리다이렉트이며 캐싱하지 않습니다. |

### 왜 Next.js는 307과 308 상태 코드를 사용할까?

전통적으로 임시 리다이렉트는 302, 영구 리다이렉트는 301 상태 코드를 사용했는데요, 문제는 많은 브라우저들이 리다이렉트를 처리하면서 원래 요청 메서드(GET, POST 등)를 무시하고 무조건 GET으로 바꿔버리는 경우가 있었어요.

예를 들어, POST /v1/users 요청이 302 상태 코드와 Location 헤더(/v2/users)를 반환하면, 브라우저가 다음 요청을 POST가 아니라 GET /v2/users로 보낼 수도 있다는 거죠. 이게 의도와 다르니까 개발자 입장에서 곤란해질 수 있습니다.

그래서 Next.js는 이 문제를 해결하기 위해 **307 (임시 리다이렉트)** 과 **308 (영구 리다이렉트)** 상태 코드를 사용해, 리다이렉트 시에도 원래 HTTP 메서드를 꼭 유지하도록 명시해줍니다.

---

그 외에도 redirects 객체 안에 쓸 수 있는 속성들이 있어요:

| 속성       | 설명                                                                                               |
|----------|--------------------------------------------------------------------------------------------------|
| **basePath** | false 혹은 undefined. false면 basePath가 매칭 경로에 포함되지 않으며, 주로 외부 리다이렉트 시에 사용합니다.          |
| **locale**   | false 혹은 undefined. false면 locale이 매칭 경로에 포함되지 않습니다.                                      |
| **has**      | has 객체들의 배열입니다. 각 객체는 type, key, value 속성을 포함합니다. 추가 조건을 확인할 때 사용합니다.             |
| **missing**  | missing 객체들의 배열입니다. 각 객체는 type, key, value 속성을 포함하며, 특정 조건이 없을 때 적용됩니다.               |

---

### 개인적으로 팁 하나!

redirects를 잘 활용하면, 앱이나 웹사이트를 운영할 때 URL 구조가 바뀌더라도 깨지지 않는 깨끗한 리다이렉트를 만들 수 있어요. 예를 들어, 특정 구간에서 사용자 위치에 따라 다르게 리다이렉트한다거나, 인증 여부에 따른 분기처리를 쉽게 구현할 수 있죠.

여기에 `has` 와 `missing` 조건을 적절히 조합하면, 복잡한 조건별 리다이렉트도 깔끔하게 처리할 수 있으니 꼭 기억해두세요!

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

리다이렉트(Redirect)는 페이지와 /public 파일 등 실제 파일 시스템을 확인하기 전에 먼저 체크됩니다.

Pages Router를 사용할 때, 리다이렉트는 클라이언트 사이드 라우팅(예: Link 컴포넌트나 router.push)에는 적용되지 않아요. 단, Middleware가 존재하고 해당 경로와 일치하는 경우에만 예외입니다. 즉, 클라이언트 쪽에서 URL을 바꿀 때는 기본적으로 리다이렉트가 자동으로 작동하지 않는 거죠.

그리고 중요한 점! 리다이렉트가 발생하면, 원래 요청에 있던 쿼리(query) 값들은 그대로 리다이렉트 목적지로 전달됩니다. 예를 들어, 다음과 같은 리다이렉트 설정이 있다고 가정해 봅시다:


| 속성        | 설명                                                      |
|-------------|-----------------------------------------------------------|
| source      | '/old-blog/:path*'                                        |
| destination | '/blog/:path*'                                            |
| permanent   | false (임시 리다이렉트, 상태 코드 307/302로 처리됨)         |


이 설정은 `/old-blog/어떤경로`로 접속하면 `/blog/어떤경로`로 이동하라는 뜻이에요. 예를 들어, `/old-blog/post-1?ref=google` 로 접속하면, 이 쿼리 값 `?ref=google` 도 그대로 `/blog/post-1?ref=google`로 전달됩니다.

추가로, permanent 옵션을 true로 설정하면 301 영구 리다이렉트가 발생합니다. SEO에 민감하거나 URL 구조가 완전히 바뀐 경우 permanent를 true로 바꾸는 게 좋아요. 반대로, 아직 리다이렉트가 임시임을 알리고 싶다면 false로 두면 되구요.

요약하자면, 리다이렉트는 서버 쪽에서 우선 처리되고 쿼리는 유지되며, 클라이언트 사이드 라우팅에는 Middleware 없이는 기본으로 적용되지 않는다는 점 기억하세요! 개발하면서 경로 변경이나 사이트 구조를 리팩토링할 때 이 부분 꼭 챙기세요!

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

> 참고! 경로 파라미터에서 소스와 목적지 경로를 작성할 때는 콜론(:) 앞에 꼭 슬래시(/)를 붙여줘야 해요. 만약 슬래시 없이 콜론만 쓰면 그 경로가 단순한 문자열로 인식돼서 무한 리다이렉트 문제가 발생할 수 있답니다.

예를 들어, 클라이언트가 `/old-blog/post-1?hello=world`를 요청하면, `/blog/post-1?hello=world`로 리다이렉트된다는 걸 기억하세요.

## 경로 매칭 (Path Matching)

경로 매칭은 이렇게 부분적으로도 가능합니다. 예를 들어 `/old-blog/:slug`로 설정하면, `/old-blog/hello-world` 같은 경로와 매칭이 되구요. 단, 중첩 경로(예: `/old-blog/hello-world/extra`)는 매칭되지 않습니다.

아래 표로 예시를 정리해봤어요.

| 설정된 경로          | 매칭되는 요청 경로         | 설명                         |
|-----------------|---------------------|----------------------------|
| `/old-blog/:slug` | `/old-blog/hello-world` | 매칭됨 - 단일 경로 파라미터           |
| `/old-blog/:slug` | `/old-blog`          | 매칭 안됨 - 파라미터가 없기 때문           |
| `/old-blog/:slug` | `/old-blog/hello-world/extra` | 매칭 안됨 - 중첩 경로는 지원하지 않음 |

만약 더 복잡한 경로 매칭이 필요하면, 정규 표현식이나 다른 라우팅 라이브러리를 활용하는 방법도 있으니 참고하세요!

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

자, 이번에는 Next.js에서 리다이렉트(redirects)를 설정하는 방법에 대해 이야기해볼게요. 사실 개발하면서 기존 URL들을 새 URL로 깔끔하게 바꿔주고 싶을 때 리다이렉트 설정만 잘 해줘도 SEO나 사용자 경험에 큰 도움이 되거든요.

---

### 기본 리다이렉트 설정

```js
module.exports = {
  async redirects() {
    return [
      {
        source: '/old-blog/:slug',
        destination: '/news/:slug', // 동적으로 들어온 slug 값을 destination에서도 쓸 수 있어요
        permanent: true, // 301 영구 리다이렉트
      },
    ]
  },
}
```

위 코드처럼 `redirects` 함수 안에 리다이렉트 규칙을 배열로 리턴해주면 돼요.  
- `source`는 기존 URL 패턴,  
- `destination`은 새로운 URL 경로,  
- 그리고 `permanent: true`는 301 리다이렉트, 즉 영구 이동을 의미해요.

여기서 주목할 점은 `:slug`라고 쓴 부분인데요, URL에서 동적으로 변하는 값(예: 글의 고유 아이디, 제목 등)을 변수처럼 잡아낼 수 있어요. 그리고 그 변수를 바뀐 URL 쪽에서도 그대로 쓸 수 있다는 점!

---

### 와일드카드 경로 매칭하기

하나의 파라미터가 여러 단계의 하위 경로를 모두 커버해야 한다면 어떻게 할까요? 바로 `*`(별표)를 뒤에 붙여서 와일드카드로 사용할 수 있어요!

```js
module.exports = {
  async redirects() {
    return [
      {
        source: '/blog/:slug*',
        destination: '/news/:slug*', // 여러 단계 하위 경로도 그대로 넘겨줘요
        permanent: true,
      },
    ]
  },
}
```

위처럼 `:slug*`라고 하면 `/blog/a/b/c` 같은 여러 하위 경로까지 다 매칭해 줍니다. 그리고 destination에서도 똑같이 써서 경로 구성을 유지할 수 있죠.

---

### 추가 Tip!

- 리다이렉트 설정은 빌드 시에만 적용되기 때문에, 동적으로 변하는 값을 잘 조합해서 만들면 SEO에도 엄청 도움 됩니다.
- `permanent: false`로 설정하면 임시 리다이렉트(302)가 돼요. 테스트할 때 이 옵션을 잘 활용하면 좋겠죠?
- Next.js 리다이렉트 경로 설정은 서버나 클라이언트 쪽 둘 다에서 작동하니, 유저가 URL을 직접 접근해서 잘못된 페이지로 가지 않도록 꼼꼼히 설정해 주세요.

---

요약하자면, Next.js의 `redirects()`에서 `source` 경로에 파라미터(`:slug`)와 와일드카드(`*`)를 적절히 사용하면, 원하는 URL 구조를 자유롭게 리다이렉트 시킬 수 있다는 이야기에요.  
코드를 직접 만져보면서 이해하면 훨씬 쉬우니, 간단한 프로젝트에서 한번 바로 적용해보길 추천합니다!

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

### Regex 경로 매칭

간단히 말해, 경로에서 특정 패턴을 정규식으로 매칭하고 싶을 때는 파라미터 뒤에 소괄호로 정규식을 감싸주면 돼요. 예를 들어, `/post/:slug(\d{1,})` 이렇게 작성하면 `/post/123` 같은 숫자만 매칭되고 `/post/abc` 같은 문자는 매칭되지 않아요.

```js
module.exports = {
  async redirects() {
    return [
      {
        source: '/post/:slug(\\d{1,})',
        destination: '/news/:slug', // 매칭된 파라미터를 목적지 경로에서도 사용할 수 있어요
        permanent: false,
      },
    ]
  },
}
```

조금 설명을 덧붙이자면, 여기서 `:slug(\\d{1,})` 부분은 'slug'라는 이름의 파라미터가 숫자 1개 이상이어야 한다는 의미입니다.

추가로 참고하면 좋은 점! 정규식에 포함된 특수문자 중 `(`, `)`, `'`, `:`, `*`, `+`, `?` 같은 글자들은 일반 문자로 쓰고 싶을 땐 앞에 `\\`를 붙여서 이스케이프(escape) 처리를 해줘야 한다는 점 기억하세요.

| 특수문자 | 설명                         | 사용 시 주의사항       |
|----------|------------------------------|-----------------------|
| ( )      | 그룹핑 또는 매칭 범위 지정   | `\\(`, `\\)`로 이스케이프 |
| '        | 문자열 리터럴 등에서 사용    | `\\'`로 이스케이프           |
| :        | 파라미터 시작 표시            | `\\:`로 이스케이프           |
| *        | 0개 이상 반복                | `\\*`로 이스케이프           |
| +        | 1개 이상 반복                | `\\+`로 이스케이프           |
| ?        | 0개 또는 1개, 또는 선택적     | `\\?`로 이스케이프           |

만약 이들 문자를 이스케이프 하지 않으면 정규식이 제대로 동작하지 않거나 의도하지 않은 매칭이 발생할 수 있으니 꼭 기억하세요!

---

이 기능을 활용하면 특정 URL 패턴만 선별해서 리다이렉트하거나, 라우팅을 매우 세밀하게 제어할 수 있어요. 예를 들어 블로그 글 번호가 숫자로만 이루어져 있을 때, 해당 번호를 이용해 다른 경로로 넘긴다거나 할 때 유용하죠.

혹시 처음 써보는 분이라면 정규식 문법에 대해 기본적인 공부를 해보는 것도 추천합니다! 어렵게 느껴질 수 있지만, 익숙해지면 웹 개발에서 진짜 강력한 무기가 되니 꼭 한번 도전해보세요 :)

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
  async redirects() {
    return [
      {
        // `/english(default)/something` 경로로 요청이 들어올 때 매칭됩니다.
        source: '/english\\(default\\)/:slug',
        destination: '/en-us/:slug',
        permanent: false,  // 영구 리다이렉트가 아님을 의미
      },
    ]
  },
}
```

## Header, Cookie, 그리고 Query 값 조건에 따른 리다이렉트

위 예시처럼 단순한 URL 매칭 외에도, 요청 헤더(header), 쿠키(cookie), 쿼리(query) 값에 따라서만 리다이렉트를 적용할 수 있어요. 

이를 위해 `has` 필드와 `missing` 필드를 활용할 수 있는데요,  
- `has`: 지정한 조건들이 **모두** 일치해야 합니다. 예를 들어, 특정 쿠키가 있어야 한다거나, 쿼리 파라미터가 특정 값이어야 하죠.  
- `missing`: 지정한 조건들이 **모두** 존재하지 않아야 합니다.

즉, `source` 경로에 대한 매칭과 `has`의 모든 조건 만족, 그리고 `missing`의 모든 조건 불일치가 동시에 성립할 때만 해당 리다이렉트가 적용된답니다.

---

### has와 missing이 지원하는 필드 종류

| 필드명     | 설명                   | 비고                                 |
|------------|------------------------|------------------------------------|
| `type`     | 검사 대상 유형          | `header`, `cookie`, `query` 중 하나 |
| `key`      | 검사할 특정 헤더, 쿠키, 쿼리의 키 | 값이 없으면 전체 검사                  |
| `value`    | 키에 대응되는 값 일치 여부 | 정규 표현식도 가능                   |

예를 들어, `has`에 아래와 같은 형식으로 넣으면:

```js
has: [
  {
    type: 'cookie',
    key: 'user-preference',
    value: 'dark-mode',
  },
  {
    type: 'header',
    key: 'x-custom-header',
    value: '^special-value$',
  },
]
```

- 쿠키 `user-preference`가 `dark-mode`여야 하고,  
- 헤더 `x-custom-header`가 정확히 `special-value`여야  
리다이렉트가 동작합니다.

---

### 참고 팁!

- `value`에 정규표현식을 쓸 땐, 슬래시(`/`)로 감싸지 않아도 되고, 그냥 문자열로 입력 후 내부적으로 정규식 매칭이 됩니다.  
- 쿠키값이나 헤더값이 조금 복잡한 경우, 정규표현식의 패턴을 잘 작성해주면 원하는 조건만 정확히 걸러낼 수 있어요.  
- 여러 조건을 조합해 리다이렉트를 세밀하게 컨트롤할 수 있으니, 사용자 환경에 따른 맞춤 경로 안내 같은 데 활용해보세요!

---

다음은 기본적인 테이블 마크다운 형식으로 정리한 `has`와 `missing` 필드 지원 항목입니다:

| 필드      | 설명                               |
|-----------|----------------------------------|
| type      | 검사할 대상 타입 (header/cookie/query) |
| key       | 검색할 특정 키 이름                      |
| value     | 키에 매칭할 값 (정규표현식 가능)        |

이 내용을 바탕으로, 조건부 리다이렉트를 만들 때 깔끔하고 유용하게 활용해보시길 바랍니다!

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

이번에는 Next.js에서 제공하는 리디렉션(redirect) 설정 방법을 한 번 살펴볼게요. 특히 `has` 나 `missing` 속성으로 조건을 걸어 리퀘스트 헤더, 쿠키, 호스트, 쿼리 파라미터에 따라 리디렉션을 다르게 하는 예시인데요, 직접 써보면 확실히 서버단에서 세밀한 리디렉션 제어가 가능해서 아주 유용해요.

---

### 주요 옵션 설명

| 옵션명     | 타입       | 설명                                                                                         |
|------------|------------|----------------------------------------------------------------------------------------------|
| type       | String     | 조건 유형 지정 (header, cookie, host, query 중 하나)                                        |
| key        | String     | type에 따라 비교할 키 값을 지정                                                               |
| value      | String or undefined | 비교할 값, undefined면 key에 해당하는 아무 값이라도 OK. 정규표현식 형태로 사용 가능 (named capture group 포함 가능) |

- 예를 들어 `value: 'first-(?<paramName>.*)'` 이렇게 설정하면 실제로 `first-second`가 들어왔을 때 `:paramName` 으로 second 값을 받아서 destination에 활용할 수 있답니다.

---

### 예시 코드와 설명

```js
module.exports = {
  async redirects() {
    return [
      // 1. x-redirect-me 헤더가 있으면 /another-page로 리디렉션
      {
        source: '/:path((?!another-page$).*)',
        has: [
          {
            type: 'header',
            key: 'x-redirect-me',
          },
        ],
        permanent: false,
        destination: '/another-page',
      },
      // 2. x-do-not-redirect 헤더가 없으면 /another-page로 리디렉션
      {
        source: '/:path((?!another-page$).*)',
        missing: [
          {
            type: 'header',
            key: 'x-do-not-redirect',
          },
        ],
        permanent: false,
        destination: '/another-page',
      },
      // 3. 쿼리 ?page=home 와 쿠키 authorized=true 가 모두 만족할 때만 리디렉션
      {
        source: '/specific/:path*',
        has: [
          {
            type: 'query',
            key: 'page',
            value: 'home',
          },
          {
            type: 'cookie',
            key: 'authorized',
            value: 'true',
          },
        ],
        permanent: false,
        destination: '/another/:path*',
      },
      // 4. x-authorized 헤더가 yes 또는 true 라는 값을 포함하면 해당 값 받아와서 쿼리에 넣음
      {
        source: '/',
        has: [
          {
            type: 'header',
            key: 'x-authorized',
            value: '(?<authorized>yes|true)',
          },
        ],
        permanent: false,
        destination: '/home?authorized=:authorized',
      },
      // 5. 호스트가 example.com이면 리디렉션
      {
        source: '/:path((?!another-page$).*)',
        has: [
          {
            type: 'host',
            value: 'example.com',
          },
        ],
        permanent: false,
        destination: '/another-page',
      },
    ]
  },
}
```

---

### 중요한 팁 — basePath가 있을 때 리디렉션 설정

Next.js에서 `basePath`를 사용 중이라면 리디렉션 설정 시에도 `source`와 `destination` 경로 앞에 자동으로 `basePath`가 붙어요. 만약 특정 리디렉션에 대해 `basePath`를 붙이고 싶지 않으면 아래처럼 `basePath: false` 옵션을 추가하면 됩니다.

```js
{
  source: '/some-path',
  destination: '/other-path',
  basePath: false, // basePath 자동 추가 안함
  permanent: true,
}
```

이 점은 API 요청이나 다른 조건에 따라 `basePath` 적용 유무를 조절하고 싶을 때 매우 유용합니다.

---

### 정리하며...

- `has`와 `missing` 으로 헤더, 쿠키, 쿼리, 호스트 조건을 세밀하게 걸 수 있어요.
- 값에 정규식(named capture group)도 사용할 수 있어서 동적 리디렉션도 가능!
- basePath 쓰면 기본적으로 경로 앞에 붙으니 필요한 경우 옵션으로 조절 가능.

이 기능 하나만 잘 쓰면, 사용자 환경이나 상태에 따라 맞춤형 리디렉션 처리가 가능해서 리액트 앱 유저 경험 개선에 큰 도움이 돼요! 필요하신 분들은 꼭 활용해보시길 바랍니다 :)

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

Next.js에서 basePath와 i18n를 사용할 때 리다이렉트 설정을 해보신 적 있나요? 오늘은 이 부분을 쉽게 설명해드릴게요. 코드 예제를 보면서 어떻게 작동하는지 함께 공부해봐요!

---

### 1. basePath와 함께하는 리다이렉트

```js
module.exports = {
  basePath: '/docs',

  async redirects() {
    return [
      {
        source: '/with-basePath',      // /docs/with-basePath로 자동 변환
        destination: '/another',       // /docs/another로 자동 변환
        permanent: false,
      },
      {
        // basePath가 false로 설정되어 '/docs'가 붙지 않음
        source: '/without-basePath',
        destination: 'https://example.com',
        basePath: false,
        permanent: false,
      },
    ];
  },
}
```

- `basePath: '/docs'`를 설정하면 내 모든 경로에 `/docs`가 기본적으로 붙어요.
- 리다이렉트할 때도 그게 기본 적용되죠! 그래서 `/with-basePath`는 `/docs/with-basePath`로 해석돼요.
- 만약 특정 리다이렉트에서 `basePath: false`를 붙이면 `/docs`가 붙지 않고 원래 경로 그대로 사용해요.
- 외부 URL로 갈 때는 보통 이렇게 `basePath: false`를 많이 쓴답니다.

---

### 2. i18n 지원과 함께하는 리다이렉트

다국어 사이트를 만들 때 유용한 i18n 옵션이 있죠. 리다이렉트 설정도 이 부분과 맞추어 써야 해서 조금 헷갈릴 수 있는데, 여기서 핵심만 짚어드릴게요.

```js
module.exports = {
  i18n: {
    locales: ['en', 'fr', 'de'],
    defaultLocale: 'en',
  },

  async redirects() {
    return [
      {
        source: '/with-locale',  // 모든 locale 자동 대응: /en/with-locale, /fr/with-locale 등
        destination: '/another', // 이동할 때도 locale 자동 전달
        permanent: false,
      },
      {
        // locale 자동 처리 비활성화 (false)
        source: '/nl/with-locale-manual',
        destination: '/nl/another',
        locale: false,
        permanent: false,
      },
      {
        // 기본 locale(en) 경로를 직접 지정 (locale: false)
        source: '/en',
        destination: '/en/another',
        locale: false,
        permanent: false,
      },
      {
        // locale 변수를 직접 쓰면서 모든 로케일에 대응하기 (locale: false)
        source: '/:locale/page',
        destination: '/en/newpage',
        permanent: false,
        locale: false,
      },
      {
        // /(en|fr|de)/(.*)로 변환됨. 루트 경로나 /fr 같은 최상위 경로에는 매칭 안됨
        source: '/(.*)',
        destination: '/another',
        permanent: false,
      },
    ];
  },
}
```

- `locales` 배열에 우리가 지원하는 언어들이 있어요 (`en`, `fr`, `de`).
- 기본 언어는 `en`으로 설정되어 있어요.
- 보통 리다이렉트에서 `locale: false`를 안 달면, 설정된 모든 언어 경로에 알아서 적용되죠.
- 하지만 `locale: false`를 작성하면, 자동으로 언어 접두사를 붙이지 않아요. 이때는 `source`와 `destination` 경로에 직접 언어 접두사를 넣어줘야 해요.
- 예를 들어 `/en`으로 시작하는 경로나 `/nl` 같은 직접 작성한 경로에만 맞게 리다이렉트할 수 있다는 뜻입니다.

---

### 덧붙여서

- `basePath`와 `i18n`를 같이 사용하면 경로가 꽤 복잡해질 수 있어요. 항상 경로가 어떻게 변환될지 테스트해보는 걸 추천합니다.
- 특히 다국어 서버사이드 렌더링(SSR)을 할 때 경로가 제대로 처리되는지 확인하는 게 중요하답니다.

---

### 요약

| 설정 키           | 역할                                                      | 특징                                   |
|------------------|---------------------------------------------------------|--------------------------------------|
| `basePath`       | 모든 URL 앞에 붙는 기본 경로                                | 기본에는 모든 라우트에 적용, false 가능   |
| `basePath: false`| 해당 리다이렉트에만 basePath 적용 안 함                      | 외부 URL 리다이렉트에 주로 사용          |
| `i18n.locales`   | 지원하는 언어 리스트                                       | `['en', 'fr', 'de']` 같은 배열           |
| `i18n.defaultLocale`| 기본 언어                                                | 기본 경로에 적용되는 언어 지정           |
| `locale: false`  | 리다이렉트에 자동 언어 접두사 붙이는 걸 끔                     | 경로 직접 작성해야 함                    |

---

Next.js 리다이렉트를 설정하면서 `basePath`와 `i18n`를 함께 쓰는 상황, 이제 좀 더 이해되셨죠? 꼭 직접 적용해보고 다양한 상황에서 어떻게 변하는지 실험해보세요! 혹시 더 궁금한 점 있으면 언제든 물어봐 주세요~

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

가끔씩 아주 드물게, 오래된 HTTP 클라이언트에서 제대로 리다이렉트를 처리하려면 커스텀 상태 코드를 지정해야 할 때가 있어요. 이럴 때는 `permanent` 속성 대신 `statusCode` 속성을 사용할 수 있는데, 두 개를 동시에 사용하면 안 돼요. 참고로, IE11 같은 구형 브라우저 호환성을 위해 308 상태 코드를 사용할 때는 자동으로 `Refresh` 헤더가 추가된답니다.

## 리다이렉트 활용 팁

- API 라우트나 라우트 핸들러 안에서는 들어오는 요청에 따라 리다이렉트를 자유롭게 할 수 있어요.
- `getStaticProps`나 `getServerSideProps` 내부에서도 요청 시점에 특정 페이지로 리다이렉트하는 게 가능하답니다.

## 버전 히스토리

| 항목 | 내용 |
| --- | --- |
| 커스텀 상태 코드 | 오래된 HTTP 클라이언트 호환을 위해 statusCode 속성 사용 가능 (permanent와는 상호 배타적) |
| IE11 대응 | 308 상태 코드 사용 시 자동으로 Refresh 헤더 추가 |
| 리다이렉트 위치 | API 라우트, 라우트 핸들러, getStaticProps, getServerSideProps에서 가능 |

참고로, 최신 브라우저들은 대부분 3xx 상태 코드를 잘 처리하지만, 레거시 호환을 신경 써야 할 때 이렇게 상태 코드랑 헤더를 적절히 써주면 좋아요! 혹시 더 궁금한 리다이렉트 관련 팁이 있으면 언제든 질문 주세요 :)

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

아래는 버전별로 추가된 기능들을 정리한 표입니다. 

| Version   | Changes          |
|-----------|------------------|
| `v13.3.0` | `missing` added. |
| `v10.2.0` | `has` added.     |
| `v9.5.0`  | `redirects` added.|

이 표를 보시면 각 버전에 어떤 기능들이 새롭게 추가되었는지 한눈에 파악할 수 있죠. 특히 버전 업데이트 때마다 기능이 어떻게 점점 추가되고 개선되는지 살펴보면 프로젝트 관리나 코드 유지보수에 큰 도움이 됩니다. 만약 여러분이 특정 기능(`missing`, `has`, `redirects`)을 사용하려면 어느 버전부터 지원하는지 꼭 확인하세요!