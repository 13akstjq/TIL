---
title: "Next.js 15에서 optimizePackageImports로 불필요한 패키지 자동 최적화하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:16
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "optimizePackageImports"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/optimizePackageImports"
isUpdated: false
---


# optimizePackageImports

개발하다 보면 어떤 패키지들은 수백 개, 많게는 수천 개의 모듈을 내보내기도 하죠. 이런 경우, 필요하지 않은 모듈들까지 다 불러오면 개발 속도가 느려지고, 최종 빌드 결과물도 커져서 성능에 악영향을 줄 수 있어요.

그래서 Next.js에서는 `experimental.optimizePackageImports`라는 실험적인 기능을 제공하는데요. 여기에 패키지 이름을 등록해두면, 실제로 사용하는 모듈들만 딱 로드해줘서 부담을 확 줄일 수 있어요. 그렇지만, 평소처럼 여러 named export를 한꺼번에 import하는 작성 방식은 그대로 유지할 수 있어서 편리하답니다.

사용법은 아주 간단해요. `next.config.js`에 아래처럼 추가하면 됩니다:

```js
module.exports = {
  experimental: {
    optimizePackageImports: ['package-name'],
  },
}
```

이렇게 하면 `package-name` 내에서 내가 쓰는 부분만 덜 불러오니까, 개발 속도도 더 빠르고 빌드 사이즈도 더 작아질 수 있어요.

---

### 조금 더 추가로!

- 아직 실험적인 기능이라서, 모든 패키지에서 완벽하게 동작하지 않을 수 있으니 주의하세요.
- 보통 대용량 UI 라이브러리(e.g., lodash, moment, antd 등)에서 특히 효과가 좋습니다.
- 추후 정식 기능으로 자리잡으면 더 안정적으로 지원될 가능성이 높으니 깃허브나 공식 문서에서 업데이트를 계속 체크하는 게 좋아요.

복잡한 임포트 덕분에 빌드 시간이 느려지거나, 최종 번들 파일이 너무 커지는 분들에게는 꽤 유용한 옵션이니 꼭 기억해두면 좋아요!

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

안녕하세요 여러분! 오늘은 웹 개발할 때 자주 쓰는 라이브러리들 중에서 **기본적으로 최적화가 적용되는 라이브러리들**을 소개해드릴게요. 최적화가 미리 되어있는 라이브러리를 사용하면 번들 사이즈가 줄어들고, 성능도 더 좋아져서 프로젝트에 큰 도움이 된답니다.

아래는 기본적으로 최적화가 되어 있는 라이브러리 목록이에요:

| 라이브러리명                     | 설명 및 특징                                      |
|---------------------------------|------------------------------------------------|
| lucide-react                    | 가벼운 SVG 아이콘 라이브러리                     |
| date-fns                       | 날짜/시간 다루기 편한 함수 모음                   |
| lodash-es                      | 함수형 프로그래밍 기반 유틸리티 라이브러리         |
| ramda                         | 함수형 프로그래밍 도구, 불변성 유지에 도움         |
| antd                          | 기업용 UI 컴포넌트 라이브러리                        |
| react-bootstrap               | Bootstrap을 React 전용으로 재구현한 컴포넌트 모음    |
| ahooks                        | React 훅(Hooks) 유틸리티                            |
| @ant-design/icons             | Ant Design 아이콘 모음                              |
| @headlessui/react            | UI 로직만 제공하고 스타일링은 자유롭게 할 수 있는 컴포넌트 |
| @headlessui-float/react      | Headless UI 기능 중 'float' 관련 컴포넌트            |
| @heroicons/react/20/solid    | 깔끔한 20px 사이즈의 Heroicons 솔리드 버전          |
| @heroicons/react/24/solid    | 깔끔한 24px 사이즈의 Heroicons 솔리드 버전          |
| @heroicons/react/24/outline  | 깔끔한 24px 사이즈의 Heroicons 아웃라인 버전        |
| @visx/visx                   | 데이터 시각화용 React 컴포넌트                        |
| @tremor/react                | 대시보드 UI 컴포넌트 라이브러리                      |
| rxjs                         | 반응형 프로그래밍 라이브러리                          |
| @mui/material                | 머티리얼 UI 핵심 컴포넌트                             |
| @mui/icons-material          | 머티리얼 UI 아이콘 모음                               |
| recharts                     | 그래프 시각화용 React 컴포넌트                         |
| react-use                    | 유용한 React 훅들 모음                               |
| @material-ui/core            | 구버전 머티리얼 UI의 핵심 컴포넌트                     |
| @material-ui/icons           | 구버전 머티리얼 UI 아이콘 모음                         |
| @tabler/icons-react          | SVG 아이콘 라이브러리                                  |
| mui-core                     | MUI의 코어 컴포넌트 패키지                            |
| react-icons/*                | 다양한 아이콘을 쉽게 불러올 수 있는 아이콘 라이브러리     |

---

### 왜 최적화된 라이브러리를 써야 할까?

웹 앱 성능이 점점 중요해지면서, **번들 사이즈 줄이기**와 **필요한 코드만 가져오기(tree-shaking)**가 필수가 되었어요. 만약 최적화가 안 되어있는 라이브러리를 쓰면, 불필요한 코드까지 함께 번들에 포함돼서 로딩 속도가 느려질 수 있어요.

그래서 React 프로젝트에서는 위와 같은 미리 최적화가 잘 된 라이브러리를 많이 추천하죠. 특히 아이콘이나 UI 컴포넌트 같이 많이 쓰이는 라이브러리라면 이런 점이 더 중요해요.

---

### 팁!

- 라이브러리 import할 때 **ES 모듈 방식을 사용하면** 트리쉐이킹에 더 유리해요.
- 같은 종류의 아이콘이라도 `react-icons` 같은 라이브러리는 필요한 아이콘만 골라서 import 할 수 있어서 번들 사이즈 절감에 좋죠.
- MUI 같은 경우도 최신 버전에서는 tree-shaking에 꽤 신경을 많이 쓰고 있으니 최신 버전을 사용하는 걸 추천해요.

---

### 마무리

어떤 라이브러리를 쓰느냐에 따라서 프로젝트 성능이 달라질 수 있으니, 꼭 위 라이브러리들을 참고해서 최적화를 신경써보세요. 😊  
앞으로도 유용한 정보와 꿀팁들 많이 공유할게요!

궁금한 점 있으면 댓글로 남겨주세요~!  
좋은 하루 보내세요!🚀