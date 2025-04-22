---
title: "Next.js 15에서 서버 외부 패키지 관리하는 방법(serverExternalPackages)"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 13:26
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "serverExternalPackages"
link: "https://nextjs.org/docs/app/api-reference/config/next-config-js/serverExternalPackages"
isUpdated: false
---


# serverExternalPackages

Next.js 13부터 Server Components와 Route Handlers 내부에서 사용하는 의존성들은 기본적으로 Next.js가 자동으로 번들링해줘요. 덕분에 서버에서 필요한 패키지를 따로 신경 쓸 필요가 줄어들죠.

그런데 만약 어떤 의존성이 Node.js의 특정 기능들(예: 파일 시스템 접근, 네이티브 모듈 등)을 사용하고 있어서, Next.js가 번들링하는 과정에서 문제가 생긴다면? 이때는 serverExternalPackages라는 옵션을 활용할 수 있어요. 이 옵션에 해당 패키지를 적으면 Next.js가 그 패키지를 번들링하지 않고, 대신 Node.js의 `require()`를 이용해 네이티브 방식으로 불러오게 됩니다.

예를 들어, '@acme/ui'라는 패키지를 이렇게 설정할 수 있어요.

```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  serverExternalPackages: ['@acme/ui'],
}
 
module.exports = nextConfig
```

이 설정은 '@acme/ui'를 서버 번들링에서 제외하고, Node.js 환경에서 직접 불러오도록 지시하는 거예요.

---

### 추가 팁!

- 대부분의 패키지는 굳이 이 옵션에 넣지 않아도 잘 작동해요. 서버 번들링이 문제를 일으키는 경우에만 선별적으로 사용하세요.
- `serverExternalPackages`에 너무 많은 패키지를 넣으면 오히려 빌드 성능에 영향을 줄 수 있으니 꼭 필요한 것만 넣는 게 좋아요.
- 이 기능은 주로 네이티브 모듈이나, 빌드 타임에 처리하기 어려운 Node.js 환경 전용 라이브러리에서 유용합니다.

서버 컴포넌트와 라우트 핸들러를 활용할 때, 이런 설정 하나만으로도 라이브러리 호환성을 좀 더 쉽게 맞출 수 있으니 참고하세요!

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

Next.js에서 현재 호환성을 작업 중이고 자동으로 제외 처리되는 인기 패키지 리스트가 있어요. 이 리스트에 포함된 패키지들은 Next.js 환경에서 문제없이 사용할 수 있도록 특별히 관리되고 있답니다.

아래는 그 인기 패키지 리스트예요:

- @appsignal/nodejs
- @aws-sdk/client-s3
- @aws-sdk/s3-presigned-post
- @blockfrost/blockfrost-js
- @highlight-run/node
- @huggingface/transformers
- @jpg-store/lucid-cardano
- @libsql/client
- @mikro-orm/core
- @mikro-orm/knex
- @node-rs/argon2
- @node-rs/bcrypt
- @prisma/client
- @react-pdf/renderer
- @sentry/profiling-node
- @sparticuz/chromium
- @swc/core
- @xenova/transformers
- argon2
- autoprefixer
- aws-crt
- bcrypt
- better-sqlite3
- canvas
- chromadb-default-embed
- cpu-features
- cypress
- dd-trace
- eslint
- express
- firebase-admin
- import-in-the-middle
- isolated-vm
- jest
- jsdom
- keyv
- libsql
- mdx-bundler
- mongodb
- mongoose
- newrelic
- next-mdx-remote
- next-seo
- node-cron
- node-pty
- node-web-audio-api
- onnxruntime-node
- oslo
- pg
- playwright
- playwright-core
- postcss
- prettier
- prisma
- puppeteer-core
- puppeteer
- require-in-the-middle
- rimraf
- sharp
- shiki
- sqlite3
- ts-node
- ts-morph
- typescript
- vscode-oniguruma
- webpack
- websocket
- zeromq

이런 패키지들은 Next.js가 서버 컴포넌트에서 처리할 때 자동으로 제외 처리되어 호환성이나 빌드 오류 걱정을 줄여줍니다. 즉, 앞으로 직접 호환성을 설정하는 대신 이 리스트에 포함되어 있으면 알아서 잘 작동할 거라는 뜻이죠.

그리고 빼놓으면 안 될 업데이트 소식! 

| 버전      | 변경 사항                                                                            |
|-----------|-------------------------------------------------------------------------------------|
| v15.0.0   | 기능이 experimental 단계에서 안정화(stable) 단계로 변경되었어요.  그리고 설정 이름이 `<code>serverComponentsExternalPackages</code>`에서 `<code>serverExternalPackages</code>`로 바뀌었답니다. |

이 부분은 Next.js가 서버 컴포넌트 관련 설정을 좀더 명확하게 정리한 거예요. 만약 기존에 experimental 옵션을 써서 외부 패키지를 관리하고 있었다면, 이 점을 참고해서 설정을 고쳐야 해요.

---

추가로 조금 더 알려드리자면, Next.js는 점점 서버 컴포넌트(Server Components) 지원을 강화하면서 다양한 외부 패키지와의 호환성을 신경 쓰고 있어요. 서버 컴포넌트는 클라이언트에 전달하지 않고 서버에서 렌더링하는 컴포넌트로, 서버-클라이언트 경계를 나누는 데 혁신적인 역할을 하고 있죠.

하지만 서버 컴포넌트에서는 모든 패키지가 안전하게 동작하지 않을 수 있어서, 지금처럼 '자동 opt-out' 리스트를 만들어 관리하는 거랍니다. 앞으로 여기에 올라오는 패키지가 점점 많아질 거라서 개발자 입장에서 점점 더 편리해지겠죠?

필요하다면 이 리스트를 참고해서 내 프로젝트에서 사용하는 패키지가 지원되는지 꼭 확인해 보세요! 그렇게 하면 Next.js 최신 기능들을 더 안정적으로 쓸 수 있어요 :)