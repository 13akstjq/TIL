---
title: "Next.js 15로 만드는 2025년 최신 Progressive Web Application (PWA)"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:09
ogImage: 
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "Progressive Web Applications (PWA)"
link: "https://nextjs.org/docs/app/building-your-application/configuring/progressive-web-apps"
isUpdated: false
---


# 프로그레시브 웹 앱 (PWA)란?

프로그레시브 웹 앱(PWA)은 웹 애플리케이션의 접근성과 범위를 그대로 가지면서, 네이티브 모바일 앱처럼 부드럽고 풍부한 사용자 경험을 제공하는 기술이에요. Next.js 같은 프레임워크와 함께 사용하면, 여러 플랫폼에서 작동하는 앱을 단일 코드베이스로 만들 수 있고, 별도의 앱 스토어 심사 과정을 거치지 않아도 되죠.

PWA의 장점은 크게 세 가지예요:

| 장점 | 설명 |
|---|---|
| 즉시 업데이트 배포 | 앱 스토어 승인 없이 웹사이트 업데이트하듯 바로 적용할 수 있어요. 사용자는 항상 최신 버전을 바로 받아볼 수 있죠. |
| 단일 코드베이스로 크로스 플랫폼 지원 | 안드로이드, iOS, 데스크탑 등 다양한 기기에서 같은 코드를 사용해 앱을 만들 수 있어 개발 속도가 빨라져요. |
| 네이티브와 같은 기능 제공 | 홈 화면에 설치하기, 푸시 알림 같은 기능을 지원해 앱 같은 직관적인 경험을 제공할 수 있어요. |

사실 PWA는 요즘 모바일 환경에서 사용자 접근성을 높이고, 유지보수를 간편하게 하는 데 매우 유용해요. 특히, 네이티브 앱 개발 비용이나 복잡성을 줄이고 싶을 때 강력한 대안이 될 수 있으니, Next.js로 시작해보는 걸 추천드립니다!

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

## Next.js로 PWA 만들기

### 1. 웹 앱 매니페스트(Manifest) 만들기

Next.js에서는 App Router를 활용해 웹 앱 매니페스트를 쉽게 생성할 수 있도록 기본 지원을 제공하고 있어요. 여기서 웹 앱 매니페스트란, PWA(Progressive Web App)의 아이콘, 앱 이름, 시작 URL 등 기본 정보를 담는 JSON 파일을 말하죠.

Next.js에서는 정적인 파일로 만들 수도 있고, 필요에 따라 동적으로 매니페스트를 생성할 수도 있어요.

예를 들어 `app/manifest.ts` 혹은 `app/manifest.json` 파일을 이렇게 만들어볼 수 있습니다:

| 파일명          | 설명                          |
|-----------------|-------------------------------|
| `app/manifest.ts`  | TypeScript로 동적 매니페스트 생성  |
| `app/manifest.json` | 정적인 JSON 파일로 매니페스트 작성  |

---

> 참고로, 동적 매니페스트를 만들면 사용자 설정이나 환경 변수에 따라 앱 이름이나 아이콘을 다르게 보여줄 수 있어 유용해요.  
> 요즘은 PWA가 일반 웹앱보다 앱처럼 동작하는 경험을 제공하기 때문에, 앱처럼 보이게 하는 매니페스트 설정이 중요하답니다!

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

이번에는 Next.js에서 PWA(Progressive Web App)를 만들 때 사용되는 manifest 파일에 대해 이야기해볼게요.

위 코드에서 보시는 manifest 함수는 PWA의 메타 정보를 담고 있어요. 간단히 말하면, 이 파일은 여러분의 앱이 스마트폰 홈 화면에 아이콘처럼 설치될 때 필요한 정보들을 담고 있죠. 앱 이름, 아이콘 이미지, 시작 경로, 화면 표시 방식, 배경색과 테마 색 등이 들어갑니다.

```ts
| 키             | 설명                                               |
|----------------|----------------------------------------------------|
| name           | 앱의 전체 이름                                           |
| short_name     | 아이콘 아래에 표시될 짧은 이름                         |
| description    | 앱에 대한 설명                                          |
| start_url      | 앱이 시작될 때의 URL (보통 루트 경로 '/'를 넣음)          |
| display        | 앱 실행 시 화면 표시 방식 (standalone이면 독립 실행 앱처럼 보임) |
| background_color | 앱 실행 시 배경색                                         |
| theme_color    | 모바일 브라우저의 주소창 색상 등 테마 색상                      |
| icons          | 앱 아이콘 이미지 파일들 (여러 사이즈를 넣어야함)                   |
```

여기서 중요한 건, 아이콘을 여러 해상도별로 준비해야 한다는 점이에요. 왜냐면 기기별로 해상도가 다르기 때문에 적절한 아이콘이 필요하거든요. 그래서 보통 favicon 생성기 같은 온라인 툴을 이용해서 아이콘 세트를 만들고, public 폴더에 넣는 방식을 많이 씁니다.

팁을 하나 드리자면, 아이콘 만들 때 단순히 크기만 맞춘다고 끝나는 게 아니고, 투명 배경(PNG)이나 마스크 아이콘을 따로 준비해야 완성도 높은 PWA가 될 수 있어요. 특히 iOS는 조금 까다로워서 별도의 아이콘 설정이 필요할 때도 있으니 참고하세요!

---

### 다음으로: 웹 푸시 알림(Web Push Notifications) 구현하기

PWA라고 해서 앱 실행 중에만 알림을 받을 수 있는 건 아니에요. 웹 푸시 알림을 구현해서 사용자가 앱을 닫았어도 중요한 소식이 있으면 알림을 띄울 수 있답니다. 이 기능은 사용자 참여도를 크게 높여주니 꼭 챙겨보세요!

그럼 웹 푸시 알림 구현에 대해서도 곧 다뤄보겠습니다!

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

웹 푸시 알림(Web Push Notifications)은 최신 브라우저 대부분에서 지원되고 있어요. 특히 아래 환경에서 쓸 수 있습니다:

| 플랫폼 / 브라우저                | 지원 버전                             |
|---------------------------|----------------------------------|
| iOS                         | 16.4 이상, 홈 화면에 설치된 앱         |
| Safari                      | 16, macOS 13 이상                     |
| 크로미엄 기반 브라우저(Chrome, Edge 등) | 최신 버전                           |
| Firefox                     | 최신 버전                           |

덕분에 PWA(Progressive Web Apps)가 네이티브 앱 못지않은 훌륭한 대안으로 자리 잡았죠. 심지어 오프라인 기능 없이도 설치 유도(install prompt)를 띄울 수 있어서 더 편리해졌답니다.

웹 푸시 알림의 가장 큰 장점은 사용자가 앱을 적극적으로 사용하지 않을 때도 다시 관심을 끌어올 수 있다는 점인데요, 결국 사용자 리텐션 향상에 아주 효과적이에요.

자, 이제 Next.js 앱에 웹 푸시 알림을 어떻게 적용할 수 있는지 간단히 소개할게요!

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

자, 이번에 Next.js 앱의 메인 페이지 컴포넌트 app/page.tsx를 만들어 볼게요. 코드가 길어질 수 있어서 이해하기 쉽게 나눠서 설명할 예정이에요.

먼저 필요한 import와 유틸 함수를 추가해줍니다. 여기서 Server Actions라고 하는 subscribeUser, unsubscribeUser, sendNotification 함수는 아직 구현 안 돼 있어도 일단 그냥 둬도 괜찮아요. 차근차근 만들어가면 되니까요.

```tsx
'use client'

import { useState, useEffect } from 'react'
import { subscribeUser, unsubscribeUser, sendNotification } from './actions'

// VAPID 키를 Push 구독에 사용할 수 있는 Uint8Array로 변환하는 함수
function urlBase64ToUint8Array(base64String: string) {
  const padding = '='.repeat((4 - (base64String.length % 4)) % 4)
  const base64 = (base64String + padding).replace(/-/g, '+').replace(/_/g, '/')

  const rawData = window.atob(base64)
  const outputArray = new Uint8Array(rawData.length)

  for (let i = 0; i < rawData.length; ++i) {
    outputArray[i] = rawData.charCodeAt(i)
  }
  return outputArray
}
```

위 함수는 Push 알림 등록 시 서버의 VAPID 공개키(보통 환경변수로 관리)를 브라우저에서 사용할 수 있는 형식으로 바꿔주기 위한 거예요. 이 부분은 조금 어렵게 느껴질 수도 있지만, 푸시 알림을 웹 표준에 맞게 구현하는 데 꼭 필요합니다.

다음은 실제로 사용자가 푸시 알림을 구독(subscribe), 구독 해제(unsubscribe), 그리고 테스트 알림을 보내는 UI 및 로직을 담은 컴포넌트입니다.

```tsx
function PushNotificationManager() {
  const [isSupported, setIsSupported] = useState(false)
  const [subscription, setSubscription] = useState<PushSubscription | null>(null)
  const [message, setMessage] = useState('')

  useEffect(() => {
    // Push API와 Service Worker 지원 여부 체크
    if ('serviceWorker' in navigator && 'PushManager' in window) {
      setIsSupported(true)
      registerServiceWorker()
    }
  }, [])

  // Service Worker 등록 함수
  async function registerServiceWorker() {
    const registration = await navigator.serviceWorker.register('/sw.js', {
      scope: '/',
      updateViaCache: 'none',
    })
    const sub = await registration.pushManager.getSubscription()
    setSubscription(sub)
  }

  // 푸시 알림 구독 함수
  async function subscribeToPush() {
    const registration = await navigator.serviceWorker.ready
    const sub = await registration.pushManager.subscribe({
      userVisibleOnly: true,
      applicationServerKey: urlBase64ToUint8Array(
        process.env.NEXT_PUBLIC_VAPID_PUBLIC_KEY!
      ),
    })
    setSubscription(sub)

    // 푸시 구독 정보를 서버에 전송 (서버 액션 호출)
    const serializedSub = JSON.parse(JSON.stringify(sub))
    await subscribeUser(serializedSub)
  }

  // 푸시 알림 구독 해제 함수
  async function unsubscribeFromPush() {
    await subscription?.unsubscribe()
    setSubscription(null)
    await unsubscribeUser()
  }

  // 테스트 알림 전송 함수
  async function sendTestNotification() {
    if (subscription) {
      await sendNotification(message)
      setMessage('')
    }
  }

  if (!isSupported) {
    return <p>Push notifications are not supported in this browser.</p>
  }

  return (
    <div>
      <h3>Push Notifications</h3>
      {subscription ? (
        <>
          <p>You are subscribed to push notifications.</p>
          <button onClick={unsubscribeFromPush}>Unsubscribe</button>
          <input
            type="text"
            placeholder="Enter notification message"
            value={message}
            onChange={(e) => setMessage(e.target.value)}
          />
          <button onClick={sendTestNotification}>Send Test</button>
        </>
      ) : (
        <>
          <p>You are not subscribed to push notifications.</p>
          <button onClick={subscribeToPush}>Subscribe</button>
        </>
      )}
    </div>
  )
}
```

### 짧게 정리해볼게요!

| 기능 | 설명 |
| -------- | ------ |
| `isSupported` | 이 브라우저가 푸시 알림과 service worker를 지원하는지 여부를 알려줘요 |
| `subscription` | 현재 푸시 알림 구독 상태를 저장해요 |
| `registerServiceWorker()` | 서비스 워커를 등록하고 기존 구독이 있다면 불러와요 |
| `subscribeToPush()` | 새 구독을 만들고 서버에 구독 정보를 보내요 |
| `unsubscribeFromPush()` | 구독해제하고 서버에도 알려줘요 |
| `sendTestNotification()` | 입력한 메시지로 테스트 푸시 알림을 보내요 |

---

사실 이 기능을 완성하려면 `./actions`에 있는 서버쪽 함수들도 만들어야 하고, service worker 파일(sw.js)도 준비해야 해요. 다음 글에서 이어서 어떻게 Service Worker를 구성하는지, 그리고 서버 액션을 작성하는 방법도 소개할게요.

웹 푸시 알림은 사용자 경험을 한층 업그레이드시켜주지만 구현 난이도가 약간 있는 편이니까, 천천히 따라오세요! 필요하면 언제든 질문해 주세요.

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

마지막으로, iOS 기기 사용자들에게 홈 화면에 앱을 설치하도록 안내하는 메시지를 띄우는 컴포넌트를 만들어볼게요. 그리고 이 메시지는 앱이 이미 설치되어 있을 때는 보이지 않도록 설정했어요.

```jsx
function InstallPrompt() {
  const [isIOS, setIsIOS] = useState(false);
  const [isStandalone, setIsStandalone] = useState(false);

  useEffect(() => {
    // iOS 기기인지 확인 (iPad, iPhone, iPod)
    setIsIOS(
      /iPad|iPhone|iPod/.test(navigator.userAgent) && !(window as any).MSStream
    );

    // 앱이 독립 실행 모드(홈 화면에 설치된 상태)인지 체크
    setIsStandalone(window.matchMedia('(display-mode: standalone)').matches);
  }, []);

  // 이미 설치되어 있으면 설치 안내 UI를 보여주지 않음
  if (isStandalone) {
    return null;
  }

  return (
    <div>
      <h3>앱 설치하기</h3>
      <button>홈 화면에 추가</button>
      {isIOS && (
        <p>
          iOS 기기에서는 아래 방법으로 앱을 설치할 수 있어요. <br />
          공유 버튼{' '}
          <span role="img" aria-label="share icon">
            ⎋
          </span>{' '}
          을 누른 후 "홈 화면에 추가" <span role="img" aria-label="plus icon">➕</span> 를 선택하세요.
        </p>
      )}
    </div>
  );
}

export default function Page() {
  return (
    <div>
      <PushNotificationManager />
      <InstallPrompt />
    </div>
  );
}
```

---

위 코드에서 재미있는 점은 iOS에서는 아직 자동으로 홈 화면에 설치하는 방법이 없다는 거예요. 그래서 직접 사용자가 공유 버튼을 눌러서 '홈 화면에 추가'를 선택하도록 안내해야 하죠. 그리고 `window.matchMedia('(display-mode: standalone)')`를 활용하면, 현재 앱이 이미 독립 실행형 앱처럼 실행 중인지 쉽게 알 수 있어요.

> 참고로, Android 같은 경우 PWA 설치를 위한 브라우저 이벤트를 이용해서 자동 설치 UI를 보여줄 수 있는데, iOS는 아직 그런 기능이 없답니다. 그래서 iOS 사용자에게는 이처럼 직접적인 설명이 꼭 필요해요.

---

자, 이제 이 컴포넌트에서 호출할 Server Actions를 만들어봅시다!

### 3. Server Actions 구현하기

(다음에 이어서 Server Actions를 어떻게 작성할지 소개할게요.)

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

새로운 파일 app/actions.ts를 만들어서 구독 생성, 구독 삭제, 알림 전송 기능을 구현하는 코드를 작성해볼게요.

```ts
'use server'

import webpush from 'web-push'

// VAPID 키 정보 설정 — 알림 전송 시 인증을 위해 필요해요.
webpush.setVapidDetails(
  'mailto:your-email@example.com',
  process.env.NEXT_PUBLIC_VAPID_PUBLIC_KEY!,
  process.env.VAPID_PRIVATE_KEY!
)

// 간단하게 메모리 내에 구독을 저장하도록 했지만,
// 실제 서비스 환경에서는 DB에 저장해야 영속성을 확보할 수 있어요.
let subscription: PushSubscription | null = null

// 구독 정보 등록 함수
export async function subscribeUser(sub: PushSubscription) {
  subscription = sub
  // TODO: 실제 서비스라면 DB에 저장하는 로직 추가 필요
  // 예: await db.subscriptions.create({ data: sub })
  return { success: true }
}

// 구독 정보 삭제 함수
export async function unsubscribeUser() {
  subscription = null
  // TODO: DB에서 구독정보 삭제하는 로직 추가 필요
  // 예: await db.subscriptions.delete({ where: { ... } })
  return { success: true }
}

// 알림 전송 함수
export async function sendNotification(message: string) {
  if (!subscription) {
    throw new Error('구독 정보가 없습니다')
  }

  try {
    await webpush.sendNotification(
      subscription,
      JSON.stringify({
        title: 'Test Notification',
        body: message,
        icon: '/icon.png',
      })
    )
    return { success: true }
  } catch (error) {
    console.error('푸시 알림 전송 중 오류 발생:', error)
    return { success: false, error: '알림 전송 실패' }
  }
}
```

여기서 핵심은 web-push 라이브러리를 사용해서 서버에서 푸시 알림을 보내는 거예요. 그리고 VAPID 키는 푸시 알림 서비스 제공자에게 내 API 권한을 증명하는 데 필요하니 꼭 환경변수로 설정해줘야 해요.

또, subscribeUser 함수로 구독 정보를 저장하긴 하는데, 이 예제는 간단하게 메모리에 저장하는 구조라서 서버가 재시작하면 초기화돼 버립니다. 실제 서비스라면 데이터베이스에 저장해서 여러 사용자의 구독 상태를 관리할 수 있도록 해야 해요.

마지막으로 알림을 실제로 받는 쪽은 서비스 워커(Service Worker)가 담당하는데, 이 부분은 다음 단계에서 구현합니다. 그러니까 서버가 알림 요청을 보내면, 클라이언트 서비스 워커가 백그라운드에서 알림을 보여주게 되죠.

친절하게 환경설정과 구독 데이터를 다루는 부분을 잘 챙겨서 완성하면, 꽤 훌륭한 푸시 알림 시스템을 갖출 수 있을 거예요!

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

### 4. VAPID 키 생성하기

Web Push API를 사용하기 위해서는 VAPID 키를 만들어야 해요. 이 키는 푸시 서버와 브라우저 간의 신뢰를 보장해주는 역할을 하죠. 가장 간단한 방법은 `web-push`라는 CLI(Command Line Interface)를 사용하는 건데요,

먼저, `web-push`를 전역으로 설치해봅시다!

```bash
npm install -g web-push
```

> npm에 익숙하지 않다면, `npm`은 Node.js 환경에서 패키지를 설치하고 관리할 수 있는 도구예요.  
> - `-g` 옵션은 전역(global) 설치로, 어떤 프로젝트에서도 CLI 명령어를 바로 사용할 수 있게 해줍니다.

이제 `web-push` 명령어를 사용해서 VAPID 키를 쉽게 만들 수 있답니다! 앞으로 푸시 알림을 구현하면서 꼭 필요한 작업이니 차근차근 따라해보세요.

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

VAPID 키를 생성하려면 아래 명령어를 터미널에 입력해 주세요:

```bash
web-push generate-vapid-keys
```

그럼 public key와 private key가 출력될 텐데, 이 키들을 복사해서 프로젝트 루트에 있는 `.env` 파일에 붙여 넣으면 됩니다. 예시는 다음과 같아요:

```js
NEXT_PUBLIC_VAPID_PUBLIC_KEY=your_public_key_here
VAPID_PRIVATE_KEY=your_private_key_here
```

여기서 `NEXT_PUBLIC_VAPID_PUBLIC_KEY`는 클라이언트 쪽에서도 접근할 수 있도록 `NEXT_PUBLIC_` 접두어를 붙인 환경 변수입니다. 보통은 public key만 클라이언트에서 필요하고, private key는 서버에서만 안전하게 사용하니 꼭 구분해서 사용하세요.

추가로, `.env` 파일을 깃허브 같은 원격 저장소에 올리지 않도록 `.gitignore`에 포함시키는 것도 잊지 마시고요! 보안에 매우 중요한 키들이니까요.

만약 아직 `web-push` 패키지를 설치하지 않았다면, 다음 명령어로 설치해 주세요:

```bash
npm install -g web-push
```

그러면 전역 명령어로 `web-push`를 사용할 수 있습니다.

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

### 5. 서비스 워커 만들기

서비스 워커는 웹 알림(push notification)이나 백그라운드 동작 등에서 중요한 역할을 하는데요, 이번에는 `public/sw.js` 파일을 만들어서 푸시 알림을 받아 처리하는 예제를 살펴볼게요.

```js
self.addEventListener('push', function (event) {
  if (event.data) {
    const data = event.data.json();
    const options = {
      body: data.body,
      icon: data.icon || '/icon.png',
      badge: '/badge.png',
      vibrate: [100, 50, 100],
      data: {
        dateOfArrival: Date.now(),
        primaryKey: '2',
      },
    };
    event.waitUntil(self.registration.showNotification(data.title, options));
  }
});

self.addEventListener('notificationclick', function (event) {
  console.log('Notification click received.');
  event.notification.close();
  event.waitUntil(clients.openWindow('https://your-website.com'));
});
```

여기서 핵심을 몇 가지 짚어볼게요:

- `push` 이벤트 리스너는 서버에서 푸시 메시지를 받으면 실행됩니다. `event.data.json()`으로 전달받은 데이터를 파싱해서 알림에 필요한 내용(제목, 내용, 아이콘 등)을 설정해요.
- 알림 옵션에 `vibrate`를 넣어 진동 패턴도 줄 수 있어서 사용자 경험을 더 좋게 할 수 있답니다.
- `notificationclick` 이벤트는 사용자가 알림을 클릭했을 때 발생해요. 여기서는 알림을 닫고 지정한 웹사이트를 새 탭에서 열도록 처리하고 있죠.

### 추가 팁!

- 아이콘 이미지 경로는 실제 프로젝트 구조에 맞게 조정해 주세요.
- 보통 알림에 `tag`나 `actions` 같은 속성을 더 넣어서 알림 그룹핑이나 버튼 클릭도 처리할 수 있어요.
- 서비스 워커 파일은 HTTPS 환경에서만 작동하니, 개발 시 로컬 호스트(127.0.0.1 등)나 HTTPS 서버를 사용해야 합니다.
- 푸시 알림을 잘 구현하려면 서비스 워커 등록과 푸시 구독(subscription) 과정도 같이 구현해야 해요. 다음에는 그 부분에 대해 다뤄볼게요!

서비스 워커가 익숙하지 않다면, 기본적인 흐름은 웹 브라우저에서 백그라운드 이벤트를 받고 처리할 수 있는 스크립트라고 생각하면 편해요. 우리의 앱이 더 똑똑해지는 순간이죠!

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

- 알림 아이콘을 커스텀하고 싶을 때는 `icon`과 `badge` 속성을 사용하면 돼요. 예를 들어, 작은 배지 아이콘이나 알림 아이콘을 따로 지정할 수 있어서 더 직관적인 알림을 만들 수 있답니다.
- 진동 패턴(`vibrate`)도 조절할 수 있어서 지원하는 기기에서 다양한 진동 알림을 설정할 수 있어요. 예를 들어 `[200, 100, 200]` 같은 배열로 진동-멈춤-진동 패턴을 만들 수 있죠.
- `data` 속성을 활용하면 알림에 추가 데이터를 붙여서, 클릭했을 때 그 데이터를 참고할 수도 있어요. 예를 들어, 특정 게시물 ID나 사용자 정보를 넘겨서 알림 클릭 시 상세 페이지로 바로 이동하게 할 수 있죠.

그리고 꼭 서비스 워커를 여러 기기와 브라우저 환경에서 잘 동작하는지 꼼꼼하게 테스트해 보세요. 또, 알림 클릭 이벤트(`notificationclick`) 안에서 사용하는 URL(`https://your-website.com`)은 꼭 여러분 앱에 맞게 실제 주소로 바꿔줘야 해요.

### 6. 홈 화면에 추가하기

2단계에서 만든 InstallPrompt 컴포넌트는 iOS 기기 사용자들에게 홈 화면에 앱을 설치하라는 안내 메시지를 보여줘요. iOS는 안드로이드처럼 일반적인 PWA 설치 프로세스가 다르기 때문에, 이렇게 직접 메시지를 띄워 알려주는 게 필요합니다. 

사용자가 홈 화면에 앱을 추가하면 더 네이티브 앱 같은 경험을 할 수 있어서, 사용자 참여도와 편리함이 확 올라가게 됩니다. 만약 여러분이 PWA를 개발 중이라면, 이 부분도 꼭 신경 쓰니까 참고하세요!

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

모바일 홈 화면에 앱을 설치할 수 있게 하려면 꼭 챙겨야 할 두 가지가 있어요:

- 유효한 웹 앱 매니페스트 (1단계에서 만들었죠)
- 웹사이트가 HTTPS로 제공되어야 함

요즘 브라우저들은 위 조건만 충족하면 자동으로 설치 안내 팝업을 띄워줘요. 물론 직접 커스텀 설치 버튼을 만들고 싶으면 `beforeinstallprompt` 이벤트를 활용할 수도 있는데요, 이 방법은 크로스 브라우저, 크로스 플랫폼 지원이 완벽하지 않아서(특히 iOS 사파리에서는 작동 안 함) 추천하지는 않아요.

### 7. 로컬 환경에서 테스트하기

앱 배포 전에 로컬 환경에서 제대로 작동하는지 꼭 확인해야 하겠죠? HTTPS가 필수이기 때문에 로컬에서 바로 테스트하려면 약간 손이 필요해요. 보통은 로컬 서버에 SSL 인증서를 적용하거나, 혹은 `localhost`가 HTTPS로 작동하는 개발 도구를 활용하는 방법이 있어요.

예를 들어, [Visual Studio Code의 Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) 같은 플러그인을 사용하면 간단하게 로컬 서버를 띄울 수 있고, `mkcert` 같은 툴로 로컬용 SSL 인증서를 만들어 HTTPS 환경도 만들어 볼 수 있답니다.

그 외에도 크롬 브라우저에서는 `chrome://flags`에서 `Insecure origins treated as secure` 옵션에 로컬 주소를 추가해 HTTPS 없이도 설치 테스트를 해볼 수 있으니, 환경에 맞게 골라서 시도해보세요!

로컬에서 테스트할 때 꼭 스마트폰에서 직접 시도해보고, PWA가 제대로 홈 화면에 추가되고 작동하는지 확인하는 걸 잊지 마세요. 나중에 실제 사용자 환경에서의 문제를 미리 막을 수 있답니다!

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

로컬에서 알림을 제대로 확인하려면 다음 사항들을 꼭 챙겨주세요!

- 로컬 환경에서 HTTPS로 실행 중인지 확인하기  
  테스트할 때는 `next dev --experimental-https` 명령어를 사용하면 HTTPS 환경에서 개발할 수 있어요.  
- 사용 중인 브라우저(크롬, 사파리, 파이어폭스 등)에서 알림 권한이 활성화되어 있는지 확인  
  로컬에서 알림 권한을 요청하면, 꼭 ‘허용’ 버튼을 눌러주세요!  
- 브라우저 전체에서 알림이 꺼져 있지 않은지도 체크하기  
- 그래도 알림이 안 뜨면, 다른 브라우저에서 한 번 실행해 보면서 문제를 찾아보세요  

---

### 8. 애플리케이션 보안 강화하기

웹 애플리케이션에서 보안은 정말 중요한 부분이에요. 특히 PWA(Progressive Web App)를 다룰 때는 더더욱 신경 써야 하죠. Next.js를 사용한다면, `next.config.js` 파일을 통해 보안 관련 헤더를 쉽게 설정할 수 있어요.

예를 들어, 아래처럼 보안 헤더를 추가해서 보안을 강화할 수 있답니다:

```js
// next.config.js 예제
module.exports = {
  async headers() {
    return [
      {
        source: '/(.*)', // 모든 경로에 대해 적용
        headers: [
          {
            key: 'Content-Security-Policy',
            value: "default-src 'self'; img-src *; media-src media1.com media2.com; script-src 'none';", // Content Security Policy 설정
          },
          {
            key: 'X-Frame-Options',
            value: 'DENY',  // 클릭재킹 공격 방지
          },
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff',  // MIME 타입 스니핑 방지
          },
          {
            key: 'Referrer-Policy',
            value: 'origin-when-cross-origin', // 리퍼러 정보 제한
          },
          {
            key: 'Permissions-Policy',
            value: 'camera=(), microphone=(), geolocation=()', // 권한 제한
          },
        ],
      },
    ];
  },
};
```

이렇게 하면 브라우저가 사이트를 더 안전하게 인식할 뿐 아니라, 악성 스크립트나 공격을 예방하는 데 도움이 돼요.  
사실 보안 설정은 프로젝트마다 요구사항이 조금씩 다르니, 위 예시를 참고해서 필요에 맞게 조절하는 게 좋아요!

그리고 참고로, HTTPS가 기본적으로 보안을 지켜주는 핵심 요소 중 하나이니 꼭 로컬에서도 HTTPS 환경을 구축해서 테스트하세요.요즘은 `next dev --experimental-https` 옵션으로 간단하게 적용할 수 있으니 너무 부담 갖지 않으셔도 됩니다~

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

Next.js에서 HTTP 헤더를 설정하는 코드를 보면, 보안과 관련된 중요한 부분들을 꼼꼼히 챙기고 있네요. 블로그에서 쉽게 설명해 볼게요!

```js
module.exports = {
  async headers() {
    return [
      {
        source: '/(.*)', // 모든 경로에 적용되는 글로벌 헤더
        headers: [
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff',
          },
          {
            key: 'X-Frame-Options',
            value: 'DENY',
          },
          {
            key: 'Referrer-Policy',
            value: 'strict-origin-when-cross-origin',
          },
        ],
      },
      {
        source: '/sw.js', // 서비스 워커에만 적용되는 헤더
        headers: [
          {
            key: 'Content-Type',
            value: 'application/javascript; charset=utf-8',
          },
          {
            key: 'Cache-Control',
            value: 'no-cache, no-store, must-revalidate',
          },
          {
            key: 'Content-Security-Policy',
            value: "default-src 'self'; script-src 'self'",
          },
        ],
      },
    ]
  },
}
```

### 각 헤더가 무슨 역할을 할까?

| 키                                          | 설명                                                                                       |
|--------------------------------------------|--------------------------------------------------------------------------------------------|
| **X-Content-Type-Options: nosniff**        | 브라우저가 MIME 타입을 임의로 추측하지 못하게 막아요. 악성 파일 실행 위험을 줄일 수 있어요.               |
| **X-Frame-Options: DENY**                   | 내 사이트가 iframe 안에 들어가는 걸 막아서, 클릭재킹 공격(clickjacking)을 방어해요.                    |
| **Referrer-Policy: strict-origin-when-cross-origin** | 다른 도메인으로 요청 보낼 때 referrer 정보를 최소화해서 개인정보 노출을 줄이고, 적당히 기능도 유지해요.      |
| **Content-Type: application/javascript; charset=utf-8 (서비스 워커)** | 서비스 워커(sw.js)를 자바스크립트로 올바르게 해석하게 해줘요.                               |
| **Cache-Control: no-cache, no-store, must-revalidate (서비스 워커)** | 서비스 워커가 캐싱되지 않도록 해서 사용자가 항상 최신 버전을 받도록 만들어요.                        |
| **Content-Security-Policy: default-src 'self'; script-src 'self' (서비스 워커)** | 콘텐츠 보안 정책을 엄격히 해서, 같은 출처에서만 스크립트를 불러오도록 제한해요.                       |

---

서비스 워커(sw.js)는 PWA(Progressive Web App)의 핵심인데, 캐싱 관리를 제대로 안 하면 업데이트가 제대로 안 될 수도 있어요. 그래서 `Cache-Control` 헤더를 꼼꼼하게 관리하는 게 중요합니다.

그리고 `Content-Security-Policy` 는 사이트를 해킹하려는 악성 스크립트를 막는 데 큰 도움이 돼서, 특히 서비스 워커처럼 중요한 스크립트 파일에 적용하는 게 좋아요.

---

이런 헤더들을 Next.js에서 어떻게 설정하느냐고 어렵게 생각할 수 있지만, `next.config.js`에 위처럼 `headers` 메서드를 추가해 주면 끝! Next.js가 빌드할 때 알아서 적용해 줍니다.

더 자세한 내용은 Next.js 공식 문서에서 [Content Security Policy](https://nextjs.org/docs/advanced-features/security-headers) 부분을 참고하면 좋아요.

---

💡 **추가 팁**  
- 보안 헤더는 사이트 신뢰도 향상에도 도움돼서, SEO에도 긍정적 영향을 줘요!  
- HTTPS 환경이라면 `Strict-Transport-Security` 헤더도 꼭 넣어 보세요! (HSTS는 사이트를 HTTPS로만 접속하도록 강제합니다)  

필요한 보안 헤더들을 한 번에 정리해서 적용하니 훨씬 편하고 안전한 사이트 운영, 어렵지 않죠? ;)

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

## 다음 단계: PWA와 Next.js 활용 팁

여러분이 Next.js로 앱을 만들고 있다면, 한 번쯤 고려해볼 만한 내용들을 정리해봤어요. 특히 PWA(Progressive Web App) 기능을 강화하거나, 오프라인 지원, 보안 문제 등을 챙기면서 사용자 경험을 더 끌어올리는 방법들을 소개할게요.

- **PWA 기능 탐색하기**  
  PWA는 다양한 웹 API를 활용해 더 멋진 기능들을 구현할 수 있어요. 예를 들면, 백그라운드에서 동기화하거나, 주기적으로 데이터를 새로 고치는 ‘periodic background sync’, 또는 파일 시스템 접근이 가능한 ‘File System Access API’ 같은 것들이 있어요. 이런 최신 기능들을 잘 살리면 앱이 훨씬 더 강력해지고 편리해지죠. 참고할 만한 좋은 자료는 [What PWA Can Do Today](https://whatpwacando.today/)라는 사이트가 있는데, 최신 지원 기능과 사례를 잘 정리해뒀으니 꼭 한 번 들여다보세요.

- **정적 내보내기(Static Exports)**  
  만약 서버를 직접 운영하지 않고, 정적 파일만 배포해서 앱을 운영하고 싶다면 Next.js 설정을 통해 정적 내보내기 기능을 활성화할 수 있어요. 이 경우엔 서버 액션(Server Actions) 대신 외부 API를 호출하는 구조로 바꿔야 하고, 커스텀 헤더도 프록시쪽으로 옮겨야 해요. 관련 내용은 [Next.js Static Export 문서](https://nextjs.org/docs/advanced-features/static-html-export)를 참고하시면 도움이 됩니다.

- **오프라인 지원**  
  오프라인에서도 앱이 동작하도록 만들고 싶다면 ‘Serwist’라는 플러그인을 고려해볼 수 있어요. Next.js와의 통합 예제가 [Serwist 문서](https://serwist.dev/)에 잘 나와 있으니 참고하세요. 다만, 이 플러그인은 현재 webpack 설정을 직접 만져줘야 하니까 사전에 빌드 시스템을 이해하고 있어야 부담이 적겠죠.

- **보안 고려사항**  
  서비스 워커(Service Worker)를 사용할 때는 반드시 보안을 신경 써야 해요. HTTPS 프로토콜을 사용하고, 푸시 메시지 출처를 검증하며, 에러 처리도 꼼꼼히 해야 합니다. 혹시라도 보안 취약점이 생기면 사용자의 데이터를 위험하게 만들 수 있으니까요.

- **사용자 경험(UX) 개선**  
  모든 브라우저가 최신 PWA 기능을 지원하는 건 아니에요. 그래서 프로그레시브 향상(progressive enhancement) 기법을 적용해, 지원하지 않는 환경에서도 기본 기능은 잘 동작하도록 하는 것이 중요해요. 예를 들어, 서비스 워커가 없으면 네트워크 기반 방식으로 fallback 하도록 하는 등의 설계가 필요하겠죠.

---

이렇게 조금씩 PWA 기능을 더하고, 오프라인 지원이나 보안까지 챙기면서 앱을 다듬다 보면 사용자에게 더 좋은 경험을 줄 수 있을 거예요. 혹시 Next.js와 PWA 연동 관련해서 더 궁금한 점 있으면 언제든 질문 주세요!