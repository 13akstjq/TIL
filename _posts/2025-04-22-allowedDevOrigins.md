---
title: "Next.js 15에서 allowedDevOrigins 설정하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:00
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "allowedDevOrigins"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/allowedDevOrigins"
isUpdated: false
---


# allowedDevOrigins

Next.js는 개발 중에 기본적으로 크로스 도메인 요청(cross-origin requests)을 차단하지 않습니다. 하지만 앞으로 나올 주요 버전에서는 개발 모드에서 내부 자원이나 엔드포인트에 대한 무단 요청을 막기 위해 기본적으로 차단 설정이 될 예정이에요.

그래서 개발 중에 서버가 시작된 호스트 이름(기본값은 localhost)이 아닌 다른 출처(origin)에서도 요청을 허용하고 싶다면, `allowedDevOrigins` 옵션을 활용할 수 있습니다.

### allowedDevOrigins란?

`allowedDevOrigins`는 개발 모드에서 허용할 추가 출처(origin)를 설정하는 옵션이에요. 예를 들어, localhost뿐만 아니라 `local-origin.dev` 같은 다른 도메인에서도 요청을 허용하고 싶다면, `next.config.js` 파일에 아래처럼 설정하면 됩니다:

```js
// next.config.js
module.exports = {
  allowedDevOrigins: ['http://local-origin.dev'],
}
```

이렇게 설정하면 개발 환경에서 `http://local-origin.dev` 도메인도 Next.js 서버에 요청을 보낼 수 있게 됩니다.

---

### 참고로 더 알려드릴게요!

- 이 설정은 오직 **개발 모드**에서만 영향을 미칩니다. 프로덕션에서는 CORS 설정을 별도로 관리하는 게 좋습니다.
- 만약 여러 개의 출처를 허용하고 싶다면 배열 안에 여러 URL을 넣으면 됩니다.

예시:

```js
module.exports = {
  allowedDevOrigins: ['http://local-origin.dev', 'http://another-origin.dev'],
}
```

- 내부 API나 자원에 대해 외부에서 자유롭게 접근하지 못하게 하려면, 개발이 완료된 후엔 이 옵션을 비워 두거나 제거해서 보안을 강화하는 것을 잊지 마세요!

Next.js 계속 발전하는 만큼 이런 보안 설정도 업데이트되고 있으니, 공식 문서나 릴리즈 노트를 자주 확인하는 습관을 들이면 좋아요!

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

안녕하세요! 오늘은 Node.js 환경에서 사용되는 설정 예제 하나를 간단히 살펴볼게요.

```js
module.exports = {
  allowedDevOrigins: ['local-origin.dev', '*.local-origin.dev'],
}
```

이 코드는 `module.exports`를 통해 외부에서 사용할 수 있는 설정 객체를 내보내고 있어요. 여기서 `allowedDevOrigins`라는 속성은 개발환경에서 허용되는 출처(origin) 목록을 담고 있죠.

- `'local-origin.dev'`는 정확히 이 도메인에서 오는 요청을 허용한다는 의미고,
- `'*.local-origin.dev'`는 서브도메인 전체를 허용한다는 뜻입니다. 예를 들면, `app.local-origin.dev`, `test.local-origin.dev` 같은 도메인들이죠.

이런 식으로 origin을 명시하면, 보안이나 CORS 설정에서 특정 출처만 허용할 때 매우 유용해요. 다만, 와일드카드(*)를 사용할 때는 보안상 주의가 필요합니다. 개발용이라면 괜찮지만, 프로덕션 환경에서는 허용 범위를 최대한 좁히는 게 좋아요.

> 참고로, Node.js나 JavaScript 환경에서 `module.exports`는 해당 파일의 기능이나 데이터를 다른 파일에서 불러다 쓸 수 있게 해주는 역할을 합니다. 예를 들어, 이 설정을 다른 파일에서 이렇게 불러올 수 있죠:

```js
const config = require('./config');
console.log(config.allowedDevOrigins);
```

마지막으로, 만약 여러 도메인을 관리할 때는 배열로 깔끔하게 관리하는 것도 좋은 팁이에요. 필요하면 정규식이나 다른 로직으로 확장도 가능하고요.

궁금한 점 있으면 언제든지 댓글로 남겨주세요!