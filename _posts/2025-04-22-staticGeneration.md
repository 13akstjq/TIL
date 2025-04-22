---
title: "Next.js 15 정적 생성(staticGeneration) 사용 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:28
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "staticGeneration*"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/staticGeneration"
isUpdated: false
---


# staticGeneration*

Next.js의 staticGeneration* 옵션은 정적 생성(Static Generation) 과정에서 조금 더 세밀하게 설정하고 싶을 때 사용해요. 특히 프로젝트가 커지거나 빌드 환경이 복잡할 때 도움 되죠.

아래는 간단한 예시 코드입니다:

```js
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  experimental: {
    staticGenerationRetryCount: 1,
    staticGenerationMaxConcurrency: 8,
    staticGenerationMinPagesPerWorker: 25,
  },
}

export default nextConfig
```

이 옵션들은 `experimental` 아래에 넣어야 하는데, 아직 실험 단계임을 기억해 주세요.

## Config Options

| 옵션 이름                      | 설명                                                                                     | 기본값        |
|-----------------------------|----------------------------------------------------------------------------------------|------------|
| `staticGenerationRetryCount` | 정적 생성 중 실패한 페이지에 대해 재시도하는 횟수를 설정합니다. (예: 네트워크 문제 등)                  | 0          |
| `staticGenerationMaxConcurrency` | 정적 생성 작업을 동시에 처리할 최대 작업 수를 결정합니다. 빌드 시간이 중요할 때 조절해 성능 최적화 가능. | CPU 코어 수 기준 기본값 |
| `staticGenerationMinPagesPerWorker` | 각 워커(프로세스)에 할당되는 최소 페이지 수를 설정해서 과도한 스레드 분할을 막습니다.                 | 10         |

### 좀 더 부연 설명하면…

- `staticGenerationRetryCount`는 네트워크 문제나 외부 API 호출 실패 등으로 인해 생성이 실패한 페이지를 몇 번 더 시도할지 정하는 옵션이에요. 실패하는 페이지가 많거나, 간헐적으로 외부 서비스가 불안정할 경우 여길 올려보면 좋아요.

- `staticGenerationMaxConcurrency`는 동시에 실행하는 페이지 생성 수를 조절합니다. 너무 큰 값으로 설정하면 CPU나 메모리 리소스 과다 사용이 우려되고, 너무 작으면 빌드 속도가 느려지니 적당히 조절해야 해요.

- `staticGenerationMinPagesPerWorker`는 워커 단위로 할당하는 페이지 수를 최소치로 지정해 불필요한 워커 쓰레드 생성으로 인해 리소스를 낭비하지 않도록 도와줍니다.

---

Next.js가 엄청난 속도로 발전하는 만큼 이런 실험적 옵션도 계속 업데이트되고 있어요. 프로젝트 상황에 맞게 다양한 설정을 시도해보고, 공식 문서나 커뮤니티 동향도 꾸준히 체크하는 걸 추천합니다! 

만약 빌드 시간이 너무 길어 고민이라면, 이런 옵션으로 병렬 처리와 리소스 사용 방식에 변화를 주는 것이 첫걸음일 수 있어요. 정적 사이트로 사용자에게 빠른 경험을 제공하려면, 빌드 과정 최적화가 굉장히 중요하거든요!

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

이번에는 페이지를 정적으로 생성할 때 유용하게 쓸 수 있는 설정 옵션들을 한 번 정리해볼게요. 개발할 때 빌드 성능이나 안정성을 높이고 싶을 때 이런 옵션들이 도움이 되니까 잘 참고해봐요!

| 옵션 이름                      | 설명                                             |
|------------------------------|------------------------------------------------|
| staticGenerationRetryCount    | 페이지 생성 실패 시 빌드가 실패하기 전에 재시도하는 횟수 |
| staticGenerationMaxConcurrency | 각 워커(worker)가 동시에 처리할 수 있는 최대 페이지 수   |
| staticGenerationMinPagesPerWorker | 새 워커를 시작하기 전에 최소로 처리해야 하는 페이지 수    |

간단하게 말하면, **staticGenerationRetryCount**는 페이지를 만들다가 실패했을 때 몇 번 더 시도할지를 정해줘요. 예를 들어 네트워크 문제나 외부 API가 잠시 오류가 났을 때 유용하겠죠?

그리고 **staticGenerationMaxConcurrency**는 한 워커가 얼마나 많은 페이지를 동시에 처리할지를 조절해요. 값이 너무 크면 워커가 과부하 될 수 있고, 너무 작으면 빌드가 느려질 수 있으니 적절히 조절하는 게 중요해요.

마지막으로 **staticGenerationMinPagesPerWorker**는 새로운 워커를 시작할 때 최소할당 페이지 수를 정해줍니다. 한마디로 워커를 너무 자주 생성하는 걸 막아서 자원 낭비를 줄이는 역할을 하죠.

이 옵션들을 잘 활용하면 빌드 안정성과 속도 면에서 꽤 큰 도움을 받을 수 있으니, 프로젝트 상황에 맞게 조절해 보시길 추천드려요!