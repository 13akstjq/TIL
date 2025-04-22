---
title: "Node.js httpAgentOptions 완벽 가이드"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:12
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "httpAgentOptions"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/httpAgentOptions"
isUpdated: false
---


# httpAgentOptions 설정하기

안녕하세요! 오늘은 Next.js에서 `httpAgentOptions` 옵션을 활용해 서버 사이드에서 `fetch()` 호출 시 HTTP Keep-Alive를 제어하는 방법에 대해 이야기해볼게요.

## 왜 이 설정이 필요할까?

Node.js 18 버전 이전에서는 Next.js가 내부적으로 `fetch()` 함수를 사용할 때 `undici` 라이브러리를 통해 자동으로 polyfill(대체)해주는데요, 이 과정에서 기본적으로 HTTP Keep-Alive 설정이 켜져 있습니다. Keep-Alive는 같은 서버로 여러 요청을 보낼 때 TCP 연결을 유지해서 성능을 높여주지만, 특정 상황에서는 연결을 계속 유지하는 게 오히려 문제가 될 수도 있어요.

예를 들어, 짧게 끊는 요청이 많거나 프록시 서버를 사용 중이라면 Keep-Alive를 꺼서 연결이 오래 유지되는 걸 방지하는 게 유리할 수 있어요.

## Next.js에서 HTTP Keep-Alive 끄는 방법

`next.config.js` 파일에 간단히 다음과 같은 옵션을 추가하면, 서버 쪽에서 호출하는 모든 `fetch()`에 대해 Keep-Alive를 끌 수 있습니다:

```js
module.exports = {
  httpAgentOptions: {
    keepAlive: false,
  },
}
```

이걸 넣으면 Next.js가 내부에서 사용하는 HTTP 에이전트에 `keepAlive: false`가 적용돼서, 연결이 요청마다 끊기게 돼요.

## 참고로 알아두면 좋은 점!

- 이 설정은 서버사이드에서만 적용되는 점, 클라이언트 사이드 `fetch()`에는 영향을 미치지 않는 점 기억하세요.
- Node.js 18 이상에서는 기본적으로 글로벌 `fetch`가 내장되어 있고, Next.js에서도 undici보다 내장 fetch를 사용할 수 있기 때문에 이 설정이 영향을 미치지 않을 수 있습니다.
- 개발 환경이나 서버 환경에 따라 Keep-Alive 연결 유지 여부에 따라 성능이나 네트워크 자원 활용도가 달라지기 때문에, 꼭 상황에 맞게 설정해주시면 좋아요!

이렇게 간단히 `httpAgentOptions`를 활용해서 서버 fetch의 동작을 제어할 수 있으니, 참고해보시고 꼭 필요한 경우에 맞춰 설정해보세요. 오늘도 좋은 개발 하세요! 🚀