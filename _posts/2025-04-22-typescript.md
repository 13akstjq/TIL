---
title: "Nextjs 15에서 typescript 제대로 사용하기 "
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:31
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "typescript"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/typescript"
isUpdated: false
---


# TypeScript와 Next.js: production 빌드 시 타입 에러 처리하기

Next.js를 사용하다 보면 `next build` 명령어로 프로덕션 빌드를 만들 때, 프로젝트 내에 TypeScript 에러가 있으면 빌드 자체가 실패하는 경우가 있어요. 이는 Next.js가 타입 안정성을 매우 중요하게 여기기 때문인데요.

하지만 만약 여러분이 타입 에러가 있더라도, 빌드를 강제로 진행하고 싶을 때도 있죠. 예를 들어, 배포 파이프라인 안에서 별도로 타입 검사를 돌리고 있다거나, 빌드 중간에 타입 에러가 있어도 어플리케이션을 일단 빌드해봐야 할 때가 있을 수 있어요.

이럴 때는 Next.js의 내장 타입체크 단계를 비활성화하는 방법이 있는데, 이렇게 하면 타입 에러가 있어도 빌드가 계속 진행됩니다. 하지만 이 방법은 정말 "위험한(dangerous)" 방법인 걸 명심해야 해요. 타입 검사를 비활성화하면 의도하지 않은 버그가 프로덕션에 들어갈 수도 있기 때문에, 별도로 타입 체크를 빌드 프로세스나 배포 프로세스에서 꼭 돌려줘야 합니다.

---

## Next.js에서 타입체크 비활성화 방법

`next.config.js`에 아래 설정을 추가해 보세요:

```js
module.exports = {
  typescript: {
    ignoreBuildErrors: true, // 타입 오류가 있어도 빌드를 무시하고 진행
  },
};
```

이 설정을 추가하면 빌드 시 타입 에러가 있어도 에러를 무시하고 빌드가 완료됩니다.

---

## 주의사항

- 위 설정은 개발할 때 임시로 쓰는 용도지, 프로덕션에선 정말 신중하게 사용하세요.
- 무심코 타입 체크를 끄면, 예상치 못한 런타임 오류가 발생할 수 있으니, 별도 CI/CD 파이프라인에서 `tsc --noEmit` 같은 명령으로 타입 검사하는 게 필수입니다.
- 팀 협업 시 타입 검사 설정을 명확히 공유하고, 빌드 실패가 바로 코드 품질 문제로 이어진다는 점을 인지시켜 주세요.

---

## 추가 팁: 타입 체크만 따로 돌리기

Next.js는 `next build`에서 타입 체크와 빌드를 같이 하지만, 타입 체크만 별도로 돌리고 싶으면 명령어를 활용할 수 있어요.

```bash
tsc --noEmit
```

이 명령은 타입 검사만 수행하고, 컴파일 결과물은 만들지 않습니다. 그래서 CI 과정에서 타입 에러를 걸러내는 데 딱 좋아요.

---

### 정리

| 상황                      | 조치 방법                            | 주의사항                            |
|-------------------------|--------------------------------|----------------------------------|
| 타입 에러 있으면 빌드 실패          | 기본 설정 유지                       | 문제가 있으면 빌드 실패로 알려줌             |
| 타입 에러 있어도 빌드 진행하고 싶을 때  | `typescript.ignoreBuildErrors = true` 설정 | 타입 에러 감춰지므로 별도 타입 체크 꼭 할 것   |
| 타입만 별도로 체크하고 싶을 때       | `tsc --noEmit` 명령어 사용            | 빌드 전에 타입 체크를 반드시 수행             |

---

Next.js와 TypeScript를 함께 쓰다 보면 의외로 이런 타입 체크 관련 설정이 꽤 중요한 역할을 해요. 빌드를 무조건 통과시키는 게 좋은 건 아니지만, 상황에 맞게 잘 활용하면 개발 편의성도 올릴 수 있으니 참고하세요!

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

next.config.js 파일을 열어서 typescript 설정에서 ignoreBuildErrors 옵션을 활성화해봅시다:

```js
module.exports = {
  typescript: {
    // !! 주의 !!
    // 타입 에러가 있어도 프로덕션 빌드를 강제로 완료하게 합니다.
    // 실수로 에러를 놓치는 상황이 발생할 수 있으니 신중히 사용하세요!
    ignoreBuildErrors: true,
  },
}
```

---

여기서 잠깐! 이 옵션은 타입스크립트 에러를 무시하고 빌드를 통과시키는 거라, 개발 초기나 빠르게 테스트할 때는 편리하지만, 실제 서비스에 배포할 때는 위험할 수 있어요. 에러가 있는 상태로 배포하면 예상치 못한 버그가 발생할 가능성이 커지니까, 꼭 필요할 때만 사용하고 나중에 에러를 수정하는 걸 추천합니다!

또한, 만약 타입 에러가 계속 발생해서 막히는 상황이라면, 이 옵션 대신 에러를 하나씩 수정하거나 타입 검사 설정을 좀 더 유연하게 조정해보는 것이 더 좋은 방법입니다.