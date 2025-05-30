---
title: "nextjs 15에서 meta data 사용하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:48
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Metadata Files API Reference"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/metadata"
isUpdated: false
---


# 메타데이터 파일 API 가이드

이번 글에서는 메타데이터 파일에 관한 규칙과 사용법에 대해 다뤄볼게요. Next.js에선 "파일 기반 메타데이터"를 사용해서 각 경로(route)별로 메타 정보를 관리할 수 있어요.

기본 아이디어는 특별한 메타데이터 파일들을 경로 폴더 안에 넣는 건데요, 크게 두 가지 방식으로 만들 수 있어요.

- 정적 파일: 예를 들어 `opengraph-image.jpg` 같은 이미지 파일을 그대로 넣는 경우
- 동적 파일: 예를 들어 `opengraph-image.js` 같이 코드를 실행해서 파일을 생성하는 경우

파일을 이렇게 정의하면 Next.js가 알아서 이 파일들을 서빙해주고, 프로덕션 환경에선 캐싱을 위해 파일명에 해시 값을 붙여 관리해줘요. 그리고 페이지의 `<head>` 태그에 이 메타데이터에 맞게 URL, 파일 타입, 이미지 크기 같은 정보들을 자동으로 업데이트해줍니다.

---

이걸 이용하면 여러분이 직접 `<meta>` 태그나 `<link>` 태그를 일일이 관리하는 수고를 줄일 수 있어요. 특히 SEO 최적화나 소셜 미디어 같은 외부 서비스에 정확한 정보를 보여줘야 할 때 매우 유용하답니다. 

그리고 동적 파일이 지원된다는 건, 상황에 따라 이미지나 메타 정보를 프로그래밍적으로 생성할 수 있다는 뜻이에요. 예를 들어 사용자 맞춤형 오픈그래프 이미지 만들기 같은 작업도 할 수 있겠죠.

다음에는 실제 예시를 통해 어떻게 파일을 만들고 구성하는지 구체적으로 살펴볼게요!

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

> 알아두면 좋은 팁:
특별한 라우트 핸들러들 — 예를 들어 sitemap.ts, opengraph-image.tsx, icon.tsx 같은 메타데이터 파일들은 기본적으로 캐싱이 됩니다.  
middleware.ts와 함께 사용할 때는, 메타데이터 파일들이 캐싱 문제로 꼬이지 않도록 matcher 설정에서 이 파일들을 제외해 주는 게 좋아요.

사실 이런 메타데이터 파일들은 자주 변경되지 않기 때문에 캐싱하는 게 성능 측면에서 유리하지만, 만약 미들웨어에서 특정 로직으로 캐싱을 우회해야 한다면 꼭 matcher를 잘 설정해 주세요. 그러면 불필요한 리퀘스트 차단이나 잘못된 데이터 제공 문제를 예방할 수 있거든요.

혹시 middleware.ts를 처음 사용하신다면, matcher 옵션에 대해 간단히 설명하자면, 이 옵션을 통해 미들웨어가 적용될 URL 패턴을 정할 수 있는데, 여기서 제외 패턴을 설정하면 특정 파일이나 경로는 미들웨어의 적용 대상에서 빠집니다. 아주 유용하니 꼭 기억해두세요!