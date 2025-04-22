---
title: "Next.js 15 디버깅 가이드 - 오류 추적과 해결 방법"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:08
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Debugging"
link: "https://nextjs.org/docs/app/building-your-application/configuring/debugging"
isUpdated: false
---

# 디버깅하기

이번 글에서는 Next.js 애플리케이션의 프론트엔드와 백엔드 코드를 어떻게 디버깅할 수 있는지 알려드릴게요. 특히 소스맵(source maps) 지원을 완벽하게 활용해서, VS Code 디버거, 크롬 개발자 도구, 파이어폭스 개발자 도구 같은 도구로 손쉽게 디버깅하는 방법을 다룹니다.

사실 Node.js에 붙일 수 있는 디버거라면 Next.js 앱 디버깅에도 다 쓸 수 있어요. 더 자세한 내용은 [Node.js 디버깅 가이드](https://nodejs.org/en/docs/guides/debugging-getting-started)를 참고해 보시면 큰 도움이 됩니다.

---

## VS Code로 디버깅하기

VS Code는 웹 개발자들 사이에서 가장 많이 쓰는 에디터 중 하나인데요, Next.js 개발 환경에서도 디버거와 연동이 정말 편리해요. 혹시 아직 VS Code에 디버거 익스텐션 설치가 안 되어 있다면, 꼭 설치해두세요!

VS Code에서 Next.js 앱을 디버깅하기 위한 기본적인 설정은 `launch.json` 파일에 디버깅 구성을 추가하는 거예요. 이 파일은 `.vscode` 폴더 안에 위치해 있습니다.

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Next.js: Launch Server",
      "runtimeExecutable": "npm",
      "runtimeArgs": ["run", "dev"],
      "port": 9229,
      "console": "integratedTerminal",
      "cwd": "${workspaceFolder}"
    },
    {
      "type": "chrome",
      "request": "launch",
      "name": "Next.js: Debug Frontend",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}",
      "sourceMaps": true,
      "sourceMapPathOverrides": {
        "webpack:///./*": "${workspaceFolder}/*"
      }
    }
  ]
}
```

- 첫 번째 구성은 Next.js 개발 서버를 디버깅 모드로 실행하는 설정이에요. `npm run dev` 명령어를 디버깅 모드로 실행하며, 백엔드 서버에 연결할 수 있도록 포트 9229를 엽니다.
- 두 번째 구성은 크롬 브라우저를 띄워서 프론트엔드 코드를 디버깅하는 설정이에요. 로컬호스트 3000번 포트(Next.js 기본 포트)에 접속하고, 소스맵을 활용해서 실제 작성한 코드를 보고 디버깅할 수 있도록 해줍니다.

> **팁!** 디버깅을 하다 보면 브레이크포인트가 제대로 안 걸리거나, 소스맵 때문에 헤매는 경우가 있는데요. 위 설정에서 `sourceMapPathOverrides` 부분이 실제 코드 파일 위치와 소스맵 경로를 잘 연결해주니까 꼭 확인하세요.

---

추가로, VS Code는 디버깅할 때 Call Stack, 변수, 워치(Watch), 콘솔 등 유용한 도구들을 한 화면에서 보여주니 디버깅 경험이 훨씬 쾌적해집니다. 특히 서버와 클라이언트 코드가 섞여 있을 때 각각 따로 디버깅할 수 있어서 편리합니다.

다음에는 크롬 개발자 도구나 파이어폭스 개발자 도구로 디버깅하는 방법도 알려드릴게요! 질문이 있으면 댓글에 남겨주세요. 😊

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

프로젝트 루트에 `.vscode/launch.json` 파일을 만들어서 다음 내용을 넣어주세요!

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Next.js: debug server-side",
      "type": "node-terminal",
      "request": "launch",
      "command": "npm run dev"
    },
    {
      "name": "Next.js: debug client-side",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:3000"
    },
    {
      "name": "Next.js: debug client-side (Firefox)",
      "type": "firefox",
      "request": "launch",
      "url": "http://localhost:3000",
      "reAttach": true,
      "pathMappings": [
        {
          "url": "webpack://_N_E",
          "path": "${workspaceFolder}"
        }
      ]
    },
    {
      "name": "Next.js: debug full stack",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/node_modules/.bin/next",
      "runtimeArgs": ["--inspect"],
      "skipFiles": ["<node_internals>/**"],
      "serverReadyAction": {
        "action": "debugWithEdge",
        "killOnServerStop": true,
        "pattern": "- Local:.+(https?://.+)",
        "uriFormat": "%s",
        "webRoot": "${workspaceFolder}"
      }
    }
  ]
}
```

> 참고로, VS Code에서 Firefox로 디버깅하려면 `Firefox Debugger` 확장 프로그램을 꼭 설치해야 해요!

그리고 `npm run dev` 명령어는 사용하는 패키지 매니저에 따라 바꿔서 써도 돼요.  
예를 들어 Yarn을 쓴다면 `yarn dev`, pnpm을 쓴다면 `pnpm dev`로 변경하면 됩니다.

---

### 조금 더 팁을 드리자면!

- **서버 사이드 디버깅**: `"Next.js: debug server-side"` 설정은 Next.js 서버 쪽 코드를 디버깅할 때 좋아요. 코드를 수정하고 바로 디버깅할 수 있죠.
- **클라이언트 사이드 디버깅**: `"Next.js: debug client-side"`와 Firefox 버전은 브라우저에서 실행되는 React 컴포넌트 쪽 문제를 잡는 데 유용해요.
- **풀스택 디버깅**: `"Next.js: debug full stack"`은 서버와 클라이언트를 동시에 디버깅 하고 싶을 때 최적이에요. 특히 복잡한 버그를 추적할 때 아주 도움이 됩니다.

VS Code 디버그 설정을 잘 해두면, 브레이크포인트도 걸고 변수도 바로바로 보면서 개발할 수 있어서 디버깅 생산성을 확 올릴 수 있으니 꼭 활용해보세요!

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

"Next.js: debug full stack" 설정에서 serverReadyAction.action은 서버가 준비됐을 때 어떤 브라우저를 열지 지정해줘요. 예를 들어 debugWithEdge는 Edge 브라우저를 실행하겠다는 뜻이고, 만약 Chrome을 쓰신다면 이 값을 debugWithChrome으로 바꿔주면 됩니다.

또, 애플리케이션이 시작하는 포트 번호를 바꾸셨다면 http://localhost:3000 중 3000을 새로 쓰는 포트 번호로 교체해 주세요.

그리고 Next.js를 루트 디렉터리가 아닌 다른 경로에서 실행 중이라면(예를 들어 Turborepo를 사용하는 경우), 서버 쪽과 full stack 디버깅 작업에 cwd 옵션도 꼭 추가해야 해요. 예를 들면 이렇게요: `"cwd": "${workspaceFolder}/apps/web"`.

마지막으로 디버그 패널(Windows/Linux는 Ctrl+Shift+D, macOS는 ⇧+⌘+D)로 가서 실행할 launch 구성을 선택한 뒤, F5를 누르거나 커맨드 팔레트에서 Debug: Start Debugging을 선택하면 디버깅 세션이 시작됩니다.

---

### 추가 팁!

- `cwd` 설정은 현재 작업 디렉토리를 지정하는 건데요, 이게 없으면 디버거가 소스 파일을 제대로 찾지 못해서 오류가 날 수 있으니 꼭 신경 써주세요.
- 혹시 브라우저가 자동으로 열리지 않는다면, 브라우저 쪽 설정과 VSCode의 디버깅 콘솔 로그를 한 번씩 확인해보는 게 좋아요.
- 만약 다른 브라우저(예: Firefox)를 쓰고 싶다면, 그에 맞는 `debugWithFirefox` 같은 값이 있는지 플러그인 문서나 설정 참고해보세요.

작게 하나 더! VSCode에서 Next.js 디버깅할 때는 `launch.json` 파일에 이런 설정이 들어가 있어야 하는데, 이를 잘 만들어 놓으면 매번 복잡한 명령어 없이도 쉽게 디버깅할 수 있어요. 초반에 설정이 좀 귀찮긴 하지만, 한 번 해두면 앞으로 엄청 편합니다!

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

## JetBrains WebStorm에서 디버거 사용하기

WebStorm에서 디버깅을 시작하려면, 먼저 실행 구성(runtime configuration) 드롭다운 메뉴를 클릭하고, **Edit Configurations...**를 선택하세요. 여기서 JavaScript Debug 구성 하나를 새로 만듭니다. URL은 보통 `http://localhost:3000` 같은 로컬 서버 주소로 설정해주고, 필요에 따라 디버깅할 브라우저 종류도 선택할 수 있어요. 또, 이 설정을 프로젝트 파일로 저장할지 여부도 결정할 수 있습니다. 구성 설정이 끝나면 **OK**를 눌러 저장하세요.

이제 이 디버깅 구성을 실행하면, WebStorm이 자동으로 선택한 브라우저를 열어줍니다. 이 상태에서 두 앱이 디버깅 모드로 실행 중입니다: 하나는 Next.js가 실행되는 Node.js 백엔드 애플리케이션, 그리고 다른 하나는 클라이언트 쪽(브라우저) 애플리케이션이죠.

이렇게 하면 백엔드와 프론트엔드를 한곳에서 편리하게 디버깅할 수 있어서, 문제를 더 빠르게 잡아낼 수 있습니다.

---

## 브라우저 개발자 도구(DevTools)를 활용한 디버깅

### 클라이언트 사이드 코드

브라우저 내장 개발자 도구는 여러분이 가장 쉽게 접할 수 있는 디버깅 도구 중 하나예요. WebStorm의 디버그 세션과 함께 사용하면 훨씬 강력하죠.

예를 들어, React 같은 클라이언트 프레임워크를 사용한다면, 개발자 도구에서 `Sources` 탭을 통해 코드에 대한 중단점(breakpoint)을 직접 걸 수 있고, 상태 변수나 호출 스택(call stack)을 살펴볼 수 있습니다. 또한, 콘솔(Console) 탭을 활용해 `console.log` 이상의 상호작용을 하면서, 변수를 실시간으로 확인하거나 함수 실행 결과를 테스트할 수도 있어요.

한 가지 팁을 드리자면, 브라우저의 개발자 도구에서 네트워크(Network) 탭을 통해 API 요청과 응답을 모니터링하면, 백엔드와 클라이언트 간 통신 문제를 빠르게 진단할 수 있습니다. 예를 들어, 요청이 제대로 보내졌는지, 응답 시간이 지연되거나 에러는 없는지 등을 실시간으로 확인할 수 있죠.

또한, React 개발자라면 Chrome 확장 프로그램인 React Developer Tools를 설치해서 컴포넌트 트리 상태를 직접 들여다보는 것도 추천합니다.

---

### 정리하자면

- WebStorm 내에서 JavaScript Debug 구성을 만들어 사용하면, 브라우저를 자동으로 열면서 코드를 단계별로 실행해볼 수 있어서 편리합니다.
- 동시에 백엔드와 프론트엔드 모두를 디버깅해야 하는 환경에 매우 적합하죠.
- 브라우저 개발자 도구는 클라이언트 쪽 코드를 빠르게 점검하고 문제를 분석하는 데 필수 도구입니다.
- 네트워크 요청 모니터링과 다양한 콘솔 기능도 적극 활용해보세요.

다음에는 Next.js 백엔드 코드나 프론트엔드 코드에 실제로 중단점을 걸고, 어떤 방식으로 디버깅하면 좋은지 더 자세히 파헤쳐볼게요!

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

개발 서버를 시작할 때는 보통 next dev, npm run dev, 혹은 yarn dev 명령어를 사용하면 돼요. 서버가 켜지면 http://localhost:3000 (혹은 설정한 다른 URL)을 브라우저에서 열어보세요.

크롬(Chrome)을 사용할 때는 이렇게 하면 됩니다:

- 크롬 개발자 도구 열기 (윈도우/리눅스: Ctrl+Shift+J, macOS: ⌥+⌘+I)
- 상단 탭에서 Sources 탭 클릭

파이어폭스(Firefox)를 사용할 때도 비슷한 방식이에요. 다음에 이어서 설명할게요!  
(필요하면 알려주세요.)

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

파이어폭스 개발자 도구 열기 (Windows/Linux는 Ctrl+Shift+I, macOS는 ⌥+⌘+I)  
그리고 나서 디버거(Debugger) 탭으로 이동하세요.

여기서 중요한 팁! 클라이언트 쪽 코드에 `debugger` 문이 나오면 코드 실행이 잠시 멈추고, 그 파일이 디버그 영역에 나타납니다. 직접 브레이크포인트를 설정하고 싶을 때는 파일을 검색해서 찾으면 되는데요.

- 크롬에서는 Windows/Linux는 Ctrl+P, macOS는 ⌘+P를 누르면 돼요.
- 파이어폭스에서는 같은 단축키로 검색하거나 왼쪽 패널에 있는 파일 트리를 이용해도 됩니다.

참고로, 파일을 검색할 때 소스 파일 경로가 `webpack://_N_E/./`로 시작하는 경우가 많으니 이 점 명심하세요.

추가로, 디버깅할 때 이 파일 경로가 조금 헷갈릴 수 있는데, 이 경로는 웹팩이 번들링하면서 붙인 경로입니다. 만약 소스맵(Source Maps)이 잘 연결되어 있다면, 실제 당신이 작성한 원본 코드 위치로 바로 이동할 수 있으니 소스맵 설정도 미리 확인해 두는 게 좋아요!

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

### 서버 사이드 코드 디버깅하기

Next.js로 서버 사이드 코드를 디버깅할 때, 브라우저 개발자 도구를 사용하려면 Node.js 프로세스에 `--inspect` 플래그를 전달해줘야 해요. 간단하게 이렇게 실행하면 됩니다:

```bash
NODE_OPTIONS='--inspect' next dev
```

이렇게 하면 서버 코드가 디버깅 모드로 실행되면서, 크롬 같은 브라우저 개발자 도구에서 디버깅이 가능해져요.

> 참고로, 만약 도커 같은 환경에서 로컬호스트가 아닌 외부에서 디버깅 접근을 하려면, 플래그를 조금 바꿔야 해요:
>
> bash
> NODE_OPTIONS='--inspect=0.0.0.0' next dev
>
> 이렇게 하면 외부 IP에서도 디버깅 포트에 접속할 수 있어서 원격 디버깅이 가능해집니다. 개발할 때 정말 유용하니 기억해두세요!

덧붙여, 디버깅을 좀 더 효율적으로 하려면 VSCode 같은 에디터의 디버깅 기능과도 연동해서 사용해보는 걸 추천해요. `launch.json` 설정을 통해서 바로 디버깅 세션을 시작할 수 있거든요!

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

만약에 `npm run dev` 또는 `yarn dev`로 개발 서버를 실행하고 있다면, package.json 안에 dev 스크립트를 이렇게 수정해줘야 해요:

```json
{
  "scripts": {
    "dev": "NODE_OPTIONS='--inspect' next dev"
  }
}
```

이렇게 하면 Next.js 개발 서버를 띄울 때 `--inspect` 옵션도 같이 붙어서 디버깅이 가능해집니다. 실행하면 터미널에 이런 메시지가 나올 거예요:

Debugger listening on ws://127.0.0.1:9229/0cf90313-350d-4466-a748-cd60f4e47c95
For help, see: https://nodejs.org/en/docs/inspector
ready - started server on 0.0.0.0:3000, url: http://localhost:3000

이 메시지는 Node.js의 디버깅 프로토콜이 활성화돼서 디버거가 연결 대기 중이라는 뜻이에요. 크롬 개발자 도구나 VSCode 같은 에디터에서 이 포트를 연결해서 코드를 단계별로 디버깅할 수 있답니다!

추가로, 윈도우 환경에서는 `NODE_OPTIONS='--inspect'` 부분을 이렇게 바꿔줘야 할 수도 있어요:

```json
"dev": "set NODE_OPTIONS=--inspect&& next dev"
```

운영체제별로 환경 변수 설정법이 다르니 참고하세요! 이렇게 하면 Next.js 개발할 때 버그 찾기가 훨씬 편해져요. 기억해두시면 좋겠죠?

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

크롬(Chrome)에서 디버깅하기:

1. 새 탭을 열고 `chrome://inspect`로 이동하세요.
2. 오른쪽 상단의 **Configure...** 버튼을 클릭해 디버깅 포트가 제대로 등록되어 있는지 확인합니다.
3. `localhost:9229`와 `localhost:9230`이 목록에 없다면 추가해 주세요.
4. **Remote Target** 섹션에서 여러분의 Next.js 애플리케이션을 찾습니다.
5. **Inspect** 버튼을 클릭하면 별도의 DevTools 창이 열립니다.
6. **Sources** 탭으로 이동해 코드를 살펴보면서 디버깅을 시작하세요.

Firefox에서 디버깅하기:

1. 새 탭을 열고 `about:debugging` 페이지로 이동합니다.
2. 왼쪽 사이드바에서 **This Firefox**를 클릭합니다.
3. **Remote Targets** 아래에서 Next.js 애플리케이션을 찾습니다.
4. **Inspect** 버튼을 눌러 디버거 창을 엽니다.
5. **Debugger** 탭으로 가서 디버깅할 코드를 확인해 보세요.

---

추가 팁!

- Next.js 앱을 디버깅할 때 Node.js 서버 쪽 디버깅도 중요해요. 보통 `nodemon --inspect` 같은 옵션으로 서버를 실행한 뒤 위에서 설명한 포트를 확인하면 됩니다.
- 크롬과 파이어폭스 둘 다, 디버깅 포트가 다르면 각각 추가해야 하니 꼭 확인하세요.
- DevTools에서는 브라우저에서 실행되는 클라이언트 사이드 코드 뿐 아니라, 서버 사이드 렌더링 함수들도 디버깅할 수 있어 아주 유용해요.
- 혹시 Next.js에서 제공하는 기본 디버깅 툴 외에 더 전문적인 도구를 찾는다면 VS Code의 디버거도 추천합니다. 요즘은 VS Code가 Next.js와 Node.js 디버깅 모두 잘 지원해줘서 편리하게 사용할 수 있어요!

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

서버 사이드 코드를 디버깅하는 방법은 클라이언트 사이드 디버깅과 굉장히 비슷해요. 예를 들어 파일을 검색할 때 (Ctrl+P 또는 ⌘+P) 소스 파일 경로가 webpack://'application-name'/./ 로 시작하는 걸 볼 수 있는데, 여기서 'application-name'은 package.json에 설정한 여러분의 애플리케이션 이름으로 자동으로 바뀌어요.

### 브라우저 개발자 도구로 서버 에러 살펴보기

에러가 발생했을 때, 소스 코드를 직접 확인하면 원인 찾기가 훨씬 수월해요. Next.js에서는 에러 화면에 Node.js 아이콘이 Next.js 버전 표시 아래에 나타납니다. 이 아이콘을 클릭하면 개발자 도구(DevTools) URL이 클립보드에 복사돼요. 이 URL을 새 탭에 붙여넣으면 Next.js 서버 프로세스를 직접 들여다볼 수 있답니다.

참고로, 이렇게 서버 프로세스를 브라우저에서 디버깅할 수 있다는 점이 정말 편리한데요, 예전에는 터미널에서만 로그를 보거나 복잡한 디버깅 과정을 거쳐야 했거든요. 이 기능 덕분에 코드 흐름을 한눈에 확인하고, 오류 위치를 빨리 찾아낼 수 있어 개발 속도가 확실히 빨라진답니다. 혹시 더 깊이 있는 디버깅이 필요하면, Node.js 전용 디버거(예: VSCode의 디버깅 툴)와 병행해서 사용해보는 것도 추천해요!

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

### Windows에서 디버깅하기

Windows에서 개발하다 보면 `NODE_OPTIONS='--inspect'` 이런 식으로 설정할 때 문제가 생길 수 있어요. 윈도우는 이 문법을 그대로 지원하지 않거든요. 그래서 윈도우, 맥, 리눅스 어디서든 똑같이 작동하도록 하려면 `cross-env`라는 패키지를 활용하는 걸 추천해요.

`cross-env`는 다양한 운영체제 환경에 맞게 환경 변수를 설정해주는 역할을 하는데요, 덕분에 같은 설정으로 모든 플랫폼에서 디버깅이 가능해집니다.

먼저, `cross-env`를 개발 의존성으로 설치해줍니다:

```bash
npm install -D cross-env
# 혹은
yarn add -D cross-env
```

그 다음 `package.json`의 `dev` 스크립트를 이렇게 바꿔주세요:

```json
{
  "scripts": {
    "dev": "cross-env NODE_OPTIONS='--inspect' next dev"
  }
}
```

이렇게 설정하면 이제 `npm run dev` 또는 `yarn dev`로 실행할 때 디버거가 잘 붙을 거예요.

> **추가 팁!**  
> 만약 Windows PowerShell에서 터미널을 사용한다면, `'--inspect'` 대신 `"--inspect"`로 바꿔서 실행할 수도 있으니 환경에 따라 테스트해보세요.

개발 환경을 맞추는 건 가끔 깔끔하지 않을 때가 많은데요, `cross-env` 같은 도구를 잘 활용하면 이런 골치 아픈 부분을 많이 덜 수 있어서 강력 추천합니다!

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

> 참고하세요: Windows Defender를 내 컴퓨터에서 꼭 비활성화하세요. 이 외부 서비스는 읽는 모든 파일을 검사하는데, 이로 인해 Next.js 개발 시 Fast Refresh 속도가 크게 느려질 수 있다고 알려져 있어요. 이 문제는 Next.js 자체와 관련된 게 아니지만, 개발 환경에는 영향을 미치니까 꼭 체크해두는 게 좋아요.

## 추가 정보

자바스크립트 디버거를 어떻게 사용하는지 더 배우고 싶다면, 아래 문서들을 참고해보세요:

- VS Code에서 Node.js 디버깅: Breakpoints
- Chrome DevTools: 자바스크립트 디버깅
- Firefox DevTools: 디버거

디버깅 기능을 잘 활용하면 개발할 때 훨씬 편리하고 효율적으로 오류를 잡을 수 있으니, 시간이 날 때 꼭 한번 익혀보는 걸 추천합니다!
