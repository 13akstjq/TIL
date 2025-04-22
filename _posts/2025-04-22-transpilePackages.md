---
title: "Next.js 15에서 transpilePackages 설정으로 패키지 트랜스파일 쉽게 하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:29
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "transpilePackages"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/transpilePackages"
isUpdated: false
---


# transpilePackages

Next.js에서는 로컬 패키지(예: 모노레포 안의 패키지들)나 외부 의존성(node_modules)에 있는 패키지들을 자동으로 트랜스파일(transpile)하고 번들링할 수 있는 기능을 제공합니다. 이 기능 덕분에 예전에는 별도의 라이브러리인 `next-transpile-modules`를 사용해야 했지만, 이제는 Next.js 내장 기능으로 훨씬 간편하게 처리할 수 있게 되었어요.

사용법도 아주 간단합니다. `next.config.js` 파일에 아래처럼 `transpilePackages` 옵션에 트랜스파일하고 싶은 패키지 이름을 배열로 넣기만 하면 됩니다:

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  transpilePackages: ['package-name'],
}

module.exports = nextConfig
```

### 왜 이 기능이 필요할까?

일반적으로 Next.js 프로젝트에서 외부 라이브러리를 사용할 때는 이미 컴파일된 상태라서 별도의 트랜스파일 과정이 필요 없는데요. 하지만 모노레포(monorepo)처럼 내 프로젝트 안에 여러 패키지가 공존하거나, 외부 라이브러리 중에 최신 문법(예: ES6 이상 모듈이나 JSX)이 포함되어 있는 경우 바로 사용할 수 없을 때가 있어요. 이럴 때 `transpilePackages`를 이용하면 Next.js가 빌드 시 해당 패키지들을 자동으로 트랜스파일해줘서, 별도의 설정 없이 신경 쓰지 않고도 최신 문법을 사용할 수 있게 해줍니다.

### 참고로 알아두면 좋은 팁

- 여러 패키지를 한꺼번에 트랜스파일하려면 배열에 계속 추가하면 됩니다: `transpilePackages: ['package-a', 'package-b']`
- 모노레포 구조에서 `packages/` 폴더 아래에 있는 라이브러리들을 배포 없이 바로 개발 서버에서 테스트할 때 특히 유용해요.
- Next.js 12 버전 이상에서 공식 지원되니, 최신 버전 사용을 권장합니다.

---

## 버전 히스토리 (Version History)

| 버전 | 변경사항                              |
|-------|------------------------------------|
| 12.x  | `transpilePackages` 기능 최초 도입 |
| 최신  | 외부 라이브러리 트랜스파일 지원 확장 | 

Next.js를 사용하면서 모노레포나 커스텀 패키지 관리가 고민이었다면, 이 기능을 꼭 활용해보세요. 설정 단순하고 성능도 좋으니 작업 효율이 확실히 올라갈 거예요!

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

아래는 표를 마크다운 형식으로 바꾼 모습이에요. 실제로 개발 관련 문서나 블로그 글 쓸 때 이렇게 마크다운으로 정리해두면 훨씬 깔끔하고 보기 좋답니다!

| Version  | Changes                   |
|----------|---------------------------|
| `v13.0.0` | `transpilePackages` added.|

특히 이번 `v13.0.0` 버전에서는 `transpilePackages`라는 기능이 추가되었는데요, 이건 번들링이나 빌드할 때 특정 패키지를 트랜스파일할 수 있게 해주는 옵션이에요. 예를 들어, 프로젝트에서 일부 외부 패키지가 최신 자바스크립트 문법을 사용해서 브라우저 호환성을 위해 트랜스파일이 필요할 때 유용하죠.

더 자세한 내용이나 사용법 궁금하면 언제든 알려주세요!