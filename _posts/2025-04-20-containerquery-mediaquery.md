---
title: "미디어 쿼리 vs. 컨테이너 쿼리 차이점 비교"
description: ""
coverImage: "/TIL/assets/img/2025-04-20-containerquery-mediaquery.png"
date: 2025-04-20 00:06
ogImage:
  url: /TIL/assets/img/2024-07-15-TechStackForProduction-ReadyLLMApplicationsIn2024_0.png
tag: Tech
originalTitle: "Tech Stack For Production-Ready LLM Applications In 2024"
link: "https://medium.com/python-in-plain-english/tech-stack-for-production-ready-llm-applications-in-2024-5eb14105d1b4"
---

안녕하세요! 오늘은 **컨테이너 쿼리(Container Query)** 를 본격적으로 사용하기 전에 꼭 알고 있어야 할 개념인, **미디어 쿼리(Media Query)** 와의 차이점에 대해 이야기해볼게요. 🤔

둘 다 반응형 디자인에 사용하는 CSS 문법이지만, **어디에 적용되는지, 언제 쓰는 게 더 좋은지**에 따라 차이가 꽤 있습니다!

---

## ✅ 미디어 쿼리란?

미디어 쿼리는 우리가 이미 많이 써온 방식이에요.  
**브라우저 뷰포트(viewport)의 크기**에 따라 스타일을 다르게 적용하죠.

```css
@media (min-width: 768px) {
  .box {
    background-color: lightblue;
  }
}
```

- 위 예제는 화면의 너비가 **768px 이상**일 때 `.box`의 배경색을 파란색으로 바꿉니다.
- 주로 전체적인 **페이지 레이아웃**을 바꿀 때 많이 써요.

---

## ✅ 컨테이너 쿼리란?

컨테이너 쿼리는 조금 달라요!  
**뷰포트가 아니라, 특정 요소(부모 컨테이너)의 크기**를 기준으로 스타일을 바꿔줍니다. 🎯

```css
.container {
  container-type: inline-size;
}

@container (min-width: 400px) {
  .box {
    background-color: lightgreen;
  }
}
```

- `.container` 요소의 **가로 너비가 400px 이상**일 때, 내부 `.box` 요소에 스타일을 적용합니다.
- 그래서 페이지 크기와 상관없이, **컴포넌트의 위치나 구조에 따라 자동 반응**하게 만들 수 있어요!

---

## 📊 둘의 차이를 표로 한눈에 정리

| 비교 항목     | 미디어 쿼리 (Media Query)   | 컨테이너 쿼리 (Container Query)                                     |
| ------------- | --------------------------- | ------------------------------------------------------------------- |
| 기준          | **브라우저 뷰포트 크기**    | **부모 컨테이너 크기**                                              |
| 주 사용처     | 전체 페이지 레이아웃        | 개별 컴포넌트/요소 스타일링                                         |
| 유연성        | 한정적 (전체 페이지 기준)   | 매우 유연함 (컴포넌트 기반)                                         |
| 예시          | `@media (min-width: 768px)` | `@container (min-width: 400px)`                                     |
| 브라우저 지원 | 널리 지원                   | [최신 브라우저부터 지원](https://caniuse.com/css-container-queries) |

---

## 💡 간단한 비교 예제

### 💻 HTML

```html
<div class="container">
  <div class="box">나는 박스야</div>
</div>
```

### 🎨 CSS – 미디어 쿼리 버전

```css
.box {
  background-color: pink;
}

@media (min-width: 600px) {
  .box {
    background-color: skyblue;
  }
}
```

📸 **결과:** 화면 너비가 600px 이상이 되면 박스가 파란색으로 바뀜

---

### 🎨 CSS – 컨테이너 쿼리 버전

```css
.container {
  container-type: inline-size;
}

.box {
  background-color: pink;
}

@container (min-width: 400px) {
  .box {
    background-color: lightgreen;
  }
}
```

📸 **결과:** `.container`의 너비가 400px 이상일 때 박스가 연두색으로 바뀜  
👉 화면 크기가 아닌, **부모 요소 너비에 따라** 바뀌기 때문에 **레이아웃 내부에서 반응형 처리가 가능**해요!

---

## 🧠 언제 뭘 써야 할까?

- **미디어 쿼리**는 페이지 전체 레이아웃 변경에 여전히 좋습니다.
- **컨테이너 쿼리**는 컴포넌트 단위로 스타일을 바꿔야 할 때 아주 강력합니다!

🎯 특히 **디자인 시스템, 컴포넌트 기반 UI (예: React, Vue 등)** 을 쓸 때는 컨테이너 쿼리가 훨씬 유리해요.

---

## ⚠️ 사용 전 주의할 점

- **브라우저 지원**: 최신 버전의 크롬, 엣지, 파이어폭스에서는 지원하지만 Safari는 16.0 이상에서만 지원돼요.
- **`container-type` 설정 필수**: 부모 요소에 `container-type` 속성을 주지 않으면 동작하지 않아요!
- **성능 고려**: 너무 많은 요소에 컨테이너 쿼리를 쓰면 렌더링 비용이 올라갈 수 있어요.

---

## 📌 마무리

미디어 쿼리와 컨테이너 쿼리는 서로 경쟁하는 게 아니라 **상호 보완적**이에요.  
페이지 전체는 미디어 쿼리로, 세부 컴포넌트는 컨테이너 쿼리로 나눠서 쓰면 **더 유연하고 깔끔한 반응형 UI**를 만들 수 있어요. 🎉

---

> 다음 글에서는 컨테이너 쿼리 문법을 좀 더 자세히 다루고, 다양한 실전 예제도 소개할게요! 🙌
