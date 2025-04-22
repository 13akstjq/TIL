---
title: "Next.js 15에서 generateBuildId로 빌드 ID 커스텀하는 방법 완벽 가이드"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:08
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "generateBuildId"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/generateBuildId"
isUpdated: false
---


# generateBuildId

Next.js는 `next build`를 할 때 앱 버전을 구분하기 위해 고유한 빌드 ID(build ID)를 자동으로 생성해요. 이 빌드 ID는 여러 컨테이너가 같은 버전의 앱을 실행하도록 보장하는 역할을 하죠.

근데 환경마다(예: 개발, 스테이징, 프로덕션) 빌드를 다시 할 때마다 빌드 ID가 달라지면, 여러 컨테이너에서 같은 버전을 쓰는 게 어렵습니다. 그래서 이럴 땐 `generateBuildId` 함수를 사용해서 의도적으로 **일관된 빌드 ID**를 만들어주면 좋아요.

`next.config.js`에 다음처럼 작성하면, 환경 변수에 담긴 최신 Git 커밋 해시(GIT_HASH)를 빌드 ID로 사용할 수 있어요:

```js
module.exports = {
  generateBuildId: async () => {
    // 최신 git 커밋 해시를 빌드 ID로 사용
    return process.env.GIT_HASH
  },
}
```

### 추가 팁
- Git 해시 말고도 환경 변수, 날짜, 혹은 커스텀한 문자열을 조합해서 빌드 ID를 만들어도 좋아요.
- 이렇게 빌드 ID를 고정하면 CI/CD 파이프라인에서 배포 자동화할 때 같은 버전을 안전하게 여러 서버에 배포 가능하답니다.
- `generateBuildId` 함수는 비동기 함수로도 작성할 수 있어서, 필요하면 API 호출을 해서 받아오는 빌드 ID도 활용할 수 있어요.

Next.js 앱을 여러 환경에 안정적으로 배포할 때 꼭 기억해두세요!