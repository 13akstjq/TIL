---
title: "Next.js 15에서 instrumentation-client.js 로 웹사이트 분석 도구 사용하기"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:38
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "instrumentation-client.js"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/instrumentation-client"
isUpdated: false
---


# instrumentation-client.js

instrumentation-client.js|ts 파일은 여러분의 애플리케이션 프런트엔드 코드가 실행되기 전에 모니터링이나 분석 코드를 추가할 수 있게 해줍니다. 이를 통해 성능 추적, 오류 감지 같은 클라이언트 측 관찰성 도구들을 쉽고 빠르게 셋업할 수 있죠.

사용 방법도 아주 간단해요! 파일을 애플리케이션 루트 폴더나 `src` 폴더 안에 넣기만 하면 준비 완료입니다.

## 사용법

(여기서부터 구체적인 사용법을 이어서 작성하면 됩니다.)  

이렇게 미리 모니터링 코드를 두면, 현장에서 발생하는 문제를 더 빨리 발견할 수 있고, 서비스 품질을 높이는데 큰 도움이 되니 꼭 활용해보세요!

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

서버 사이드 계측(instrumentation)과 달리, 클라이언트 쪽에서는 특정 함수를 따로 export할 필요가 없어요. 그냥 원하는 파일 안에 모니터링 코드를 바로 작성해주시면 됩니다.

```js
// 퍼포먼스 모니터링 시작 시점 찍기
performance.mark('app-init')
 
// 애널리틱스 초기화
console.log('Analytics initialized')
 
// 에러 추적 세팅
window.addEventListener('error', (event) => {
  // 에러 추적 서비스로 에러 데이터 전송
  reportError(event.error)
})
```

이렇게 간단히 작성할 수 있죠. 

참고로 `performance.mark()`는 브라우저의 성능 측정 API로, 특정 시점에 표시를 찍어서 나중에 성능 분석 자료로 활용할 수 있어요. 그리고 `window.addEventListener('error', ...)`를 사용하면 페이지 내에서 발생하는 자바스크립트 에러를 잡아내서 별도의 추적 시스템에 전송할 수 있어서 디버깅에 매우 유용합니다.

---

## 버전 히스토리

| Version | Changes                         |
|---------|--------------------------------|
| v15.3   | `instrumentation-client` 도입  |

`instrumentation-client`를 도입하면서 클라이언트 쪽에서도 더 쉽게 모니터링 코드 작성이 가능해졌어요. 이 부분 활용하면 애플리케이션 상태를 실시간으로 추적하거나 성능 병목을 빠르게 찾는 데 큰 도움이 됩니다.