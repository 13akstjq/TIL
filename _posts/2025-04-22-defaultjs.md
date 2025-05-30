---
title: "Next.js 15 Parallel Routes default.js 활용법 정리"
description: ""
coverImage: "/TIL/assets/img/nextjs.png"
date: 2025-04-22 02:35
ogImage:
  url: /TIL/assets/img/nextjs.png
tag: Tech
originalTitle: "default.js"
link: "https://nextjs.org/docs/app/api-reference/file-conventions/default"
isUpdated: false
---

# default.js 파일에 대해 알아보자

안녕하세요! 오늘은 Next.js에서 Parallel Routes를 사용할 때 나오는 `default.js` 파일에 대해 이야기해볼게요. Next.js를 쓰다 보면 우리가 원하는 화면(서브페이지)을 보여주다가도, 가끔 새로고침(하드 네비게이션)을 하면 그 상태를 잃어버리고 다시 렌더링해야 할 때가 있잖아요? 이 문제를 해결해주는 게 바로 `default.js` 파일이에요.

---

## default.js 파일, 왜 필요할까?

Next.js는 **소프트 네비게이션**(페이지를 새로고침하지 않고 이동할 때)에서는 각 슬롯(slot)의 액티브 상태를 잘 기억하고 있답니다. 그래서 사용자가 어디에 있었는지 그대로 보여줄 수 있죠.

그런데, 만약 **하드 네비게이션**(진짜 페이지를 새로 고침하거나 처음 방문하는 경우)이 발생하면 액티브 상태를 기억하지 못해요. 그래서 이때, 현재 URL과 맞지 않는 서브페이지가 있을 때 렌더링할 기본 화면을 제공해주기 위해 `default.js`가 필요하답니다.

---

## 예시 폴더 구조 살펴보기

아래처럼 폴더 구조를 보면, `@team` 슬롯에는 `settings` 페이지가 있는 반면, `@analytics`는 없어요.

app
├── layout.js
├── page.js
├── @team
│ ├── page.js
│ └── settings
│ └── page.js
└── @analytics
└── page.js

위처럼 되어있다면, `@analytics` 슬롯에 `settings` 페이지 같은 게 없어 하드 네비게이션 시 상태 복구가 힘들기 때문에, 이때 `default.js`가 빈 슬롯 내용이나 기본 안내 메시지를 보여주는 역할을 하게 되는 거죠.

---

## 조금 더 알아두면 좋은 점!

- `default.js`는 슬롯 내부의 서브페이지가 현재 URL과 맞지 않을 때 보여주는 **fallback UI**입니다.
- 이를 활용하면 페이지가 완전히 새로고침되어도, 깨지지 않고 친절한 기본 메시지를 띄울 수 있죠.
- 이 기능을 통해 탭이나 메뉴처럼 여러 슬롯을 가진 복잡한 UI를 만들 때 유저 경험이 훨씬 좋아져요.

---

궁금한 점 있으면 댓글로 남겨주세요! 다음에도 재미있고 유용한 Next.js 팁 가지고 올게요~ :)

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

위 내용을 쉽게 풀어서 설명해드릴게요.

---

Next.js에서 /settings 경로로 이동하면, `@team` 슬롯에는 설정 페이지가 보여지고, `@analytics` 슬롯은 현재 활성화된 페이지를 계속 유지해요.

**여기서 슬롯(@team, @analytics)이란?**  
페이지를 여러 영역으로 나누고, 각 영역에 다른 컴포넌트를 꽂아 넣을 수 있는 공간 같은 개념이에요. 그래서 한 페이지 안에서도 독립적으로 여러 컴포넌트가 관리됩니다.

---

그런데, 새로고침을 하게 되면 Next.js는 `@analytics` 슬롯에 대해 `default.js` 파일을 찾으려고 해요. 만약 `default.js`가 없다면 404 (페이지 없음) 오류를 띄웁니다.

또 하나 중요한 점!  
`children` 슬롯은 암묵적으로 존재하는 기본 슬롯인데요, 이 `children` 슬롯도 `default.js` 파일을 만들어 줘야 해요. 그래야 Next.js가 부모 페이지의 활성 상태를 복구하지 못할 때 대체할 컴포넌트를 보여줄 수 있어요.

---

### 요약하자면

| 상황                        | 해야할 일                           | 이유                                                              |
| --------------------------- | ----------------------------------- | ----------------------------------------------------------------- |
| /settings 접근 시           | @team 슬롯에 설정 페이지 렌더링     | 해당 슬롯에 지정한 컴포넌트만 바꾸고 다른 슬롯은 유지하기 위해    |
| 새로고침할 때               | `@analytics/default.js` 만들어 두기 | 새로고침 시 기본으로 보여줄 컴포넌트가 필요, 없으면 404 오류 발생 |
| 암묵적 기본 슬롯 `children` | `children/default.js`도 만들어 주기 | 부모 페이지 상태 복구 실패 시 대체 컴포넌트를 보여주기 위해       |

---

### 추가 팁!

- 이렇게 슬롯별로 `default.js`를 준비해두면 사용자 경험이 한층 좋아져요. 새로고침 후에도 오류 대신 기본 페이지가 적절히 렌더링 되거든요.
- 만약 `default.js`가 없으면, 사용자가 새로고침할 때 꽤 당황스러운 404를 보게 되니 꼭 챙겨주세요!

---

이 정도만 알고 있어도 Next.js의 복잡한 경로 관리와 슬롯 구조를 조금 더 편하게 다룰 수 있을 거예요. 다음에도 이런 실용적인 Next.js 팁 있으면 공유해 드릴게요! :)

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

## 참고 자료

### params (선택 사항)

`params`는 루트 경로부터 슬롯 내부의 하위 페이지까지의 동적 라우트 매개변수를 담고 있는 객체를 반환하는 **Promise**입니다. 쉽게 말해, 서버나 페이지가 렌더링될 때 URL 경로에서 변하는 부분들을 비동기적으로 받아오는데 사용돼요.

예를 들어 아래 코드를 보면:

```js
export default async function Default({ params }: { params: Promise<{ artist: string }> }) {
  const { artist } = await params;
}
```

`params`는 `{ artist: string }` 타입을 가진 Promise를 받는데, 여기서 `artist`는 URL 경로의 동적 부분입니다 (`/artist/:artist` 같은 라우트).

---

### 좀 더 쉽게 풀어볼까요?

- 동적 라우팅(dynamic routing)은 URL 경로가 고정된 게 아니라, 변수처럼 변하는 부분을 말해요. 예를 들면, 프로필 페이지 URL이 `/profile/john`과 `/profile/jane`처럼 각각 다를 때, `john`이나 `jane`이 동적 매개변수가 되는 거죠.
- Next.js 13의 App 디렉토리에서는 이런 동적 매개변수를 `params` Promise로 받아 사용할 수 있게 해줍니다.
- 그리고 `params`는 Promise 타입이기 때문에, async/await을 써서 값을 받아와야 하는 점 꼭 기억하세요!

---

### 팁

- 서버 컴포넌트는 기본적으로 async 함수로 만들 수 있으니, 동적으로 데이터를 불러와야 할 때는 이 `params`를 활용하면 아주 편리해요.
- 만약 여러 개의 동적 파라미터가 있다면, `params` 객체에 여러 키가 포함돼서 옵니다. 예를 들어, `/artist/:artist/album/:albumId` 라우트의 경우 `{ artist: string; albumId: string }` 형태로 받겠죠.
- 이 `params`를 잘 활용하면 클라이언트에서 URL을 참고해 요청을 전달하거나 데이터를 동적으로 렌더링하기 훨씬 수월해집니다.

이렇게 `params` Promise를 통해 쉽고 똑똑하게 동적 라우팅 데이터를 다룰 수 있다는 점 기억해두세요!

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

다음은 Next.js에서 동적 라우팅 시 `params`가 어떻게 전달되는지 예제를 통해 정리한 내용이에요.

| Example                                    | URL          | `params`                                     |
| ------------------------------------------ | ------------ | -------------------------------------------- |
| `app/[artist]/@sidebar/default.js`         | `/zack`      | `Promise<{ artist: 'zack' }>`                |
| `app/[artist]/[album]/@sidebar/default.js` | `/zack/next` | `Promise<{ artist: 'zack', album: 'next' }>` |

여기서 중요한 점은, `params`가 Promise 형태라는 거죠. 그래서 `params` 값을 실제로 사용하기 위해서는 `async/await` 또는 React의 `use` 함수를 써야 해요. 예를 들어, 이렇게 쓸 수 있죠:

```jsx
const params = await props.params; // 또는 React 18의 use 함수 사용
```

버전 14 이하에서는 `params`가 동기적으로 바로 사용 가능했는데, Next.js 15부터는 비동기로 변경되면서 사용법도 달라졌답니다. 다만, 지금 당장은 이전 방식도 호환을 위해 지원하긴 하지만 곧 deprecated될 예정이에요. 그러니까 코드를 새로 쓸 때는 이 부분 꼭 염두에 두는 게 좋아요.

추가로, `params`가 Promise라는 점은 뭔가 데이터가 비동기적으로 준비된다는 걸 의미하기도 해요. 따라서 페이지의 서버사이드 렌더링이나 데이터 페칭 로직과 잘 맞물려서 설계해야 부드러운 UX를 만들 수 있습니다.

이 부분 혼동되기 쉬운데, 꼼꼼히 체크해서 다음 프로젝트에 제대로 적용해보세요!
