---
title: "2025년 Next.js 15 서버 액션(Server Actions) 사용 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:25
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "serverActions"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/serverActions"
isUpdated: false
---


# serverActions

Next.js에서 Server Actions(서버 액션)의 동작 방식을 설정할 수 있는 옵션들에 대해 이야기해볼게요.

## allowedOrigins

서버 액션이 호출될 수 있는 안전한 출처(origin) 도메인을 추가로 설정할 수 있는 옵션입니다. 이게 왜 필요하냐면, 서버 액션 요청이 들어올 때 그 요청의 출처(origin)를 호스트 도메인과 비교해서 일치하는지 확인해요. 이렇게 해서 CSRF(사이트 간 요청 위조) 공격을 막을 수 있죠.

만약 이 옵션을 설정하지 않으면 기본적으로 ‘같은 출처’(same-origin)에서만 서버 액션이 허용됩니다. 즉, 내 도메인에서만 서버 액션을 호출할 수 있어요.

**추가로 알아두면 좋은 점!**  
- allowedOrigins를 적절히 설정하지 않으면, 의도한 외부 도메인에서 서버 액션을 호출하지 못할 수 있어요.  
- 하지만 너무 많은 도메인을 허용하면 보안상 위험이 커질 수 있으니 꼭 필요한 도메인만 추가하는 게 좋아요.

아래는 allowedOrigins 옵션을 설정하는 예시입니다.

```js
export const serverActions = {
  allowedOrigins: ['https://mytrusted.com', 'https://another-trusted-site.com'],
};
```

이렇게 하면 `mytrusted.com`, `another-trusted-site.com`에서 오는 요청도 서버 액션을 실행할 수 있게 되겠죠!

---

Next.js의 Server Actions는 점점 더 중요해지고 있으니, 이런 보안 설정 하나하나 신경 써서 안전한 서비스를 만들어 봅시다!

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
/** @type {import('next').NextConfig} */
 
module.exports = {
  experimental: {
    serverActions: {
      allowedOrigins: ['my-proxy.com', '*.my-proxy.com'],
      bodySizeLimit: '3mb', // 예시로 3MB로 제한 설정
    },
  },
}
```

---

## bodySizeLimit 옵션 알아보기

Next.js에서 Server Actions(서버 액션)을 사용할 때, 기본적으로 한 번에 보낼 수 있는 요청 바디(request body)의 최대 크기가 1MB로 제한되어 있어요. 이 제한은 서버 리소스가 과도하게 사용되는 걸 막고, 때로는 DDoS 공격 같은 악의적인 대량 요청으로부터 보호하기 위한 거랍니다.

근데 가끔 데이터 양이 많아 1MB 제한을 넘겨야 할 때가 있잖아요? 예를 들어 이미지 같이 비교적 큰 파일을 보내거나, 여러 데이터를 한 번에 전송해야 할 때 말이죠. 이럴 때는 `serverActions.bodySizeLimit` 옵션을 통해 이 제한을 조정할 수 있어요.

### 설정 방법

- 바이트 단위 숫자 (예: 1000)
- 문자열로 크기 표현 (예: `500kb`, `3mb` 등) — `bytes`라는 패키지가 지원하는 형식이에요.

```js
module.exports = {
  experimental: {
    serverActions: {
      bodySizeLimit: '5mb', // 최대 5MB까지 허용
    },
  },
}
```

이렇게 설정하면, 5MB까지 데이터 전송이 가능해져서 보다 큰 요청도 서버 액션에서 처리할 수 있어요.

### 주의할 점

무턱대고 크기 제한을 늘리는 건 피하는 게 좋아요. 너무 크게 하면 서버가 무거워지고, 악성 의도한 대용량 요청에 취약해 질 수 있거든요. 꼭 필요한 경우에만 적절히 조절하고, 필요한 경우 요청을 여러 번에 나눠서 보내는 방법도 고려해 보세요.

---

서버 요청 크기 제한에 관해 종종 헷갈릴 수 있는데, 이렇게 설정해 두면 서버 액션이 좀 더 유연하게 데이터를 처리할 수 있어서 편리하답니다! 다음에는 이런 experimental 옵션 외에, 프로덕션 환경에서 안전성과 성능을 지키는 팁도 알려드릴게요~

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

Next.js에서 Server Actions 설정하는 방법을 알려드릴게요!

---

### Server Actions 설정 (Next.js 13 버전 기준)

최근 Next.js 14부터 Server Actions가 정식 기능으로 기본 활성화 되어 있는데요, 아직 13 버전을 사용 중이라면 직접 설정을 해줘야 합니다.

예를 들어, `next.config.js` 파일에 아래처럼 `experimental.serverActions` 옵션을 `true`로 설정해주시면 돼요.

```js
/** @type {import('next').NextConfig} */
const config = {
  experimental: {
    serverActions: true,
  },
}

module.exports = config
```

이 설정을 한 후에는 Server Actions를 사용할 수 있습니다.

---

### 참고: Next.js 14 이상에서는?

Next.js 14부터는 Server Actions가 기본적으로 활성화 되어 별도의 설정이 필요 없답니다. 만약 기본 바디 사이즈 제한을 변경하고 싶다면 아래처럼 설정할 수 있어요.

```js
/** @type {import('next').NextConfig} */

module.exports = {
  experimental: {
    serverActions: {
      bodySizeLimit: '2mb',
    },
  },
}
```

이렇게 하면 서버 액션의 요청 바디 크기 제한을 조절할 수 있습니다.

---

### 제가 추가로 알려드릴 팁!

- **Server Actions 란?**

  서버 액션은 클라이언트에서 서버 함수 호출을 훨씬 쉽게 만들어주는 Next.js의 강력한 기능입니다.

- **bodySizeLimit 설정이 중요한 이유**

  서버에 보내는 데이터 양이 많아지면 기본 제한 때문에 오류가 발생할 수 있어요. 따라서 요청 크기에 따라 적절히 조절해 주는 게 좋아요.

- **Next.js 버전 꼭 확인하기**

  프로젝트 버전에 따라 설정 방법이 다르니, `package.json`에서 Next.js 버전을 꼭 확인하시고 알맞게 설정하세요.

---

필요한 설정을 빠르게 찾는 데 도움이 되었으면 좋겠습니다! Next.js Server Actions, 꼭 직접 사용해보시고 개발 효율을 올려보세요 :)