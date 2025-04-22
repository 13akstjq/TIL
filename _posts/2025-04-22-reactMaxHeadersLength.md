---
title: "ReactJS에서 maxHeadersLength 설정하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:19
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "reactMaxHeadersLength"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/reactMaxHeadersLength"
isUpdated: false
---


# reactMaxHeadersLength 설정 알아보기

리액트에서 정적 렌더링(static rendering)을 할 때, 응답 헤더에 추가할 수 있는 여러 헤더들을 만들어낼 수 있어요. 이 헤더들은 브라우저가 폰트, 스크립트, 스타일시트 같은 리소스를 미리 불러오게(preload) 도와서 페이지 로딩 속도를 쑥~ 올려주는 역할을 해주죠.

디폴트 값은 6000인데, 환경에 따라 이 크기를 조절하고 싶다면 `next.config.js` 파일에서 `reactMaxHeadersLength` 옵션을 직접 설정할 수 있어요. 예를 들어, 헤더 총 길이를 1000으로 제한하려면 이렇게 작성하면 됩니다:

```js
module.exports = {
  reactMaxHeadersLength: 1000,
}
```

> 참고! 이 옵션은 App Router를 사용할 때만 적용 가능하니, 기존 Pages Router를 이용 중이라면 신경쓰지 않아도 됩니다.

추가로, 이 옵션을 너무 낮게 설정하면 preload 헤더가 잘려서 필요한 리소스가 미리 로드되지 않을 수 있으니 주의하세요. 반대로 너무 높게 설정하면 헤더 크기가 너무 커져서 네트워크 효율이 떨어질 수 있으니 적당한 균형을 찾는 게 중요합니다.

요약하자면, 정적 렌더링 성능 최적화를 위해 헤더 길이를 적절히 설정하는 `reactMaxHeadersLength` 옵션을 활용해보세요!

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

브라우저와 서버 사이에 어떤 종류의 프록시가 있느냐에 따라 HTTP 헤더가 잘려버릴 수 있어요. 예를 들어, 리버스 프록시(reverse proxy)를 사용하는데 이 프록시가 긴 헤더를 제대로 처리하지 못한다면, 헤더가 중간에 잘릴 위험이 있답니다. 그래서 이럴 땐 헤더 길이 제한 값을 낮춰서 잘리지 않도록 세팅하는 게 좋아요.

사실 이런 문제는 특히 대용량 쿠키나 토큰을 헤더에 넣을 때 종종 발생하곤 해요. 리버스 프록시뿐만 아니라, 네트워크 장비나 보안 솔루션에도 헤더 길이 제한이 있기 때문에 어디서든 문제가 될 수 있죠. 그래서 개발할 때 이 점을 염두에 두고, 너무 큰 헤더를 보내지 않도록 주의하는 게 좋습니다.

간단히 정리해보면:

| 상황                 | 해결 방법                             |
|--------------------|---------------------------------|
| 프록시가 긴 헤더를 못 처리할 때 | 헤더 길이 제한 값을 낮춰 설정                   |
| 대용량 쿠키나 토큰 사용 시       | 필요 없는 데이터 제거 또는 쿠키 분할 등 최적화 작업 |
| 헤더 길이 제한으로 인한 문제 발견  | 프록시 및 네트워크 장비 설정 점검 및 조정          |

이런 이슈들은 특히 트래픽이 많은 서비스에서 쉽게 지나치기 쉬운데, 한 번 겪으면 서비스 안정성에 큰 영향을 미치니 꼭 체크해보시길 바랍니다!