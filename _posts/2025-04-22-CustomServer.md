---
title: "Next.js 15 커스텀 서버 구축하는 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:07
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Custom Server"
link: "https://nextjs.org/docs/app/building-your-application/configuring/custom-server"
isUpdated: false
---

# 커스텀 서버(Custom Server) 이해하기

Next.js를 사용하면 기본적으로 `next start` 명령어로 내장된 서버가 바로 실행돼요. 그런데 만약 이미 백엔드 서버가 따로 있다면, 그걸 그대로 유지하면서 Next.js와 함께 사용할 수도 있답니다. 이 경우를 ‘커스텀 서버(Custom Server)’라고 부르지 않아요.

커스텀 Next.js 서버는 필요에 따라 내 프로그램에서 직접 서버를 띄우면서 라우팅이나 요청 처리 방식을 자유롭게 바꾸고 싶을 때 사용해요. 하지만 대부분의 경우에는 Next.js가 제공하는 기본 라우터만으로도 충분하기 때문에 굳이 커스텀 서버를 만들 필요가 없어요.

> **여기서 주의!**
>
> - 커스텀 서버는 Next.js의 자동 정적 최적화(Automatic Static Optimization) 같은 중요한 성능 개선 기능들을 포기하는 대가를 치러야 해요.
> - 그리고 Vercel 같은 Next.js 공식 배포 플랫폼에서는 커스텀 서버를 사용할 수 없어요.
> - `standalone` 출력 모드를 사용할 때는 커스텀 서버 파일을 함께 추적하지 않고, 대신 최소한의 `server.js` 파일만 따로 만들어내요. 그래서 둘을 동시에 사용할 수도 없으니 주의가 필요해요.

아래는 커스텀 서버의 간단한 예시를 보여드릴게요. 예전에는 Express.js 같은 외부 서버 라이브러리와 함께 Next.js를 이렇게 쓰는 경우가 많았는데, 최근 Next.js가 점점 더 자체 기능을 강화하면서 점차 필요성이 줄고 있죠.

```js
const express = require("express");
const next = require("next");

const port = 3000;
const app = next({ dev: false });
const handle = app.getRequestHandler();

app.prepare().then(() => {
  const server = express();

  // 커스텀 라우트 예시
  server.get("/p/:id", (req, res) => {
    const actualPage = "/post";
    const queryParams = { id: req.params.id };
    app.render(req, res, actualPage, queryParams);
  });

  // 나머지는 Next.js 기본 처리
  server.all("*", (req, res) => {
    return handle(req, res);
  });

  server.listen(port, (err) => {
    if (err) throw err;
    console.log(`> Ready on http://localhost:${port}`);
  });
});
```

---

### 꼭 알아두면 좋은 팁

- 커스텀 서버는 정말 필요할 때에만 써요! 기본 Next.js 라우팅으로 충분하면 굳이 복잡하게 서버를 직접 관리할 필요 없어요.
- Vercel 배포에서는 사용 불가라는 점, 잊지 마세요.
- 서버 없는 환경(serverless)이나 요즘 많이 쓰는 ISR(Incremental Static Regeneration) 기능이랑도 잘 맞지 않아요.
- 많이 쓰이는 기능은 공식 Next.js 라우터 기능이나 API 라우트를 활용하는 게 더 깔끔하고 이후 유지보수도 편해요.

만약 커스텀 서버와 관련해서 더 궁금한 점 있으면 언제든 질문해 주세요! 혹은 Next.js 기본 라우터들의 여러 팁도 공유해 드릴게요. 😊

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

Next.js 프로젝트에서 커스텀 서버를 직접 만들고 싶을 때, 보통 `server.js` 파일을 만들어서 Node.js의 HTTP 서버를 활용하곤 해요. 오늘은 간단한 커스텀 서버 예제와, 이걸 어떻게 실행할지 package.json에 스크립트를 어떻게 추가하는지 알려드릴게요.

먼저, 이 코드를 보시면 서버를 직접 생성하고 Next.js 앱을 `next()` 함수로 감싼 뒤에 준비가 끝나면 HTTP 서버를 띄우는 구조예요.

```js
import { createServer } from 'http'
import { parse } from 'url'
import next from 'next'

const port = parseInt(process.env.PORT || '3000', 10)
const dev = process.env.NODE_ENV !== 'production'
const app = next({ dev })
const handle = app.getRequestHandler()

app.prepare().then(() => {
  createServer((req, res) => {
    const parsedUrl = parse(req.url!, true)
    handle(req, res, parsedUrl)
  }).listen(port)

  console.log(
    `> Server listening at http://localhost:${port} as ${
      dev ? 'development' : process.env.NODE_ENV
    }`
  )
})
```

이 코드에서 핵심은 `app.prepare()`가 끝나면 HTTP 서버를 열며, 들어오는 요청을 `handle` 함수로 넘겨서 Next.js가 페이지를 렌더링하도록 한다는 점이에요.

### 그런데 여기서 중요한 점!

`server.js`는 Next.js 컴파일러를 거치지 않고 그냥 Node.js에서 직접 실행되기 때문에, 최신 문법(예: ES 모듈 `import` 구문)을 쓸 때 Node.js 버전과 호환되는지 반드시 확인해야 합니다. 만약 호환 안 되면 직접 `require()`로 바꾸거나, Babel 같은 트랜스파일러 사용을 고려해야 해요.

---

### package.json에 커스텀 서버 실행 스크립트 추가하기

커스텀 서버를 실행하려면 `package.json`의 `scripts`를 이렇게 바꿔줘야 해요.

```json
{
  "scripts": {
    "dev": "node server.js",
    "build": "next build",
    "start": "NODE_ENV=production node server.js"
  }
}
```

- `dev`: 개발 모드에서 커스텀 서버 바로 실행
- `build`: Next.js 빌드 수행
- `start`: 프로덕션 환경에서 커스텀 서버 실행

---

### 덧붙여서…

커스텀 서버를 만들면 유연하게 서버 로직 제어가 가능하지만, Next.js의 기본 서버 기능과 자동 최적화, 배포 플랫폼(예: Vercel) 지원을 덜 받게 돼요. 그러니 정말 필요한 경우가 아니라면 기본 Next.js 빌트인 서버를 쓰는 걸 권장합니다!

그럼, 여러분만의 커스텀 서버 만들어서 원하는 기능 자유롭게 구현해보세요! 궁금한 점 있으면 언제든 질문 남겨주세요. 😄

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

대신에 nodemon을 설정해서 사용할 수도 있어요(예시 참고). 커스텀 서버는 Next.js 애플리케이션과 연결하기 위해 아래와 같은 import 문을 사용합니다:

```js
import next from "next";

const app = next({});
```

여기서 next 함수는 옵션들을 담은 객체를 인자로 받는데요, 주요 옵션들은 다음과 같습니다:

| 옵션         | 타입               | 설명                                                                                |
| ------------ | ------------------ | ----------------------------------------------------------------------------------- |
| `conf`       | `Object`           | `next.config.js` 에서 사용하는 설정 객체와 동일해요. 기본값은 `{}` 입니다.          |
| `dev`        | `Boolean`          | (선택) Next.js를 개발 모드로 실행할지 여부입니다. 기본값은 `false` 입니다.          |
| `dir`        | `String`           | (선택) Next.js 프로젝트 위치를 지정합니다. 기본값은 현재 폴더 `'.'` 입니다.         |
| `quiet`      | `Boolean`          | (선택) 서버 정보가 포함된 에러 메시지를 숨길지 결정합니다. 기본값은 `false` 입니다. |
| `hostname`   | `String`           | (선택) 서버가 실행되는 호스트 이름을 지정합니다.                                    |
| `port`       | `Number`           | (선택) 서버가 실행되는 포트를 지정합니다.                                           |
| `httpServer` | `node:http#Server` | (선택) Next.js가 작동 중인 HTTP 서버 객체를 넘겨줄 수 있습니다.                     |
| `turbo`      | `Boolean`          | (선택) Turbopack을 활성화할지 여부입니다.                                           |

---

**간단 팁!**

- 개발할 때 `dev` 옵션을 `true`로 주면 핫 리로딩도 잡아주고 디버깅이 편해져요.
- 프로젝트 경로가 현재 폴더가 아니라면 `dir` 옵션으로 정확히 지정해주는 게 중요합니다.
- `quiet` 옵션을 켜면 서버 에러 로그가 줄어들어 콘솔이 깔끔해져서 가끔 쓰면 좋아요.
- 그리고 마지막으로 `turbo` 옵션은 Next.js의 차세대 번들러인 Turbopack을 활성화하는건데, 아직 실험적인 기능이라 프로젝트 특성에 따라 테스트해보시는 걸 추천해요.

이렇게 다양한 옵션을 활용해서 커스텀 서버 환경을 자신의 필요에 맞게 세팅해보세요! 개발할 때 내 서버 환경을 좀 더 세밀하게 컨트롤할 수 있어서 편리합니다.

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

반환된 앱은 Next.js가 요청을 필요한 대로 처리할 수 있도록 사용하는 데 활용할 수 있어요.

여기서 중요한 점은, Next.js가 기본적으로 서버 사이드 렌더링(SSR)과 정적 생성(SSG) 모두를 지원하기 때문에, 반환된 앱으로 다양한 요청 처리 방식을 유연하게 다룰 수 있다는 거예요.

예를 들어, API 라우트나 커스텀 서버를 구축할 때 조금 더 세밀한 제어가 필요하면 이렇게 반환된 앱을 통해 요청을 직접 처리할 수 있고, 그렇지 않으면 Next.js의 기본 라우팅에 맡길 수도 있죠.

이런 방식을 활용하면 복잡한 서버 로직을 구현할 때도 Next.js의 장점을 최대한 살리면서 개발할 수 있어요!
