---
title: "CSS :has() 선택자 사용 방법 정리"
description: ""
coverImage: "/TIL/assets/img/2025-04-20-css-has-selector.png"
date: 2025-04-20 00:06
ogImage:
  url: /TIL/assets/img/2024-07-15-TechStackForProduction-ReadyLLMApplicationsIn2024_0.png
tag: Tech
originalTitle: "Tech Stack For Production-Ready LLM Applications In 2024"
link: "https://medium.com/python-in-plain-english/tech-stack-for-production-ready-llm-applications-in-2024-5eb14105d1b4"
---

안녕하세요! 오늘은 CSS에서 최근에 도입된 강력한 기능인 `:has()` 선택자에 대해 알아보겠습니다. 이 선택자는 부모 요소가 특정 자식 요소를 포함하고 있는지 여부에 따라 스타일을 적용할 수 있게 해줍니다. 자바스크립트 없이도 동적인 스타일링이 가능해졌다는 점에서 매우 유용합니다.

## `:has()` 선택자란?

`:has()`는 CSS의 관계형 의사 클래스(relational pseudo-class)로, 특정 자식 요소를 포함하는 부모 요소를 선택할 수 있게 해줍니다. 예를 들어, `<div>` 요소 중에서 `<p>` 요소를 포함하는 것만 선택하고 싶다면 다음과 같이 작성할 수 있습니다:

```css
div:has(p) {
  background-color: #f0f0f0;
}
```

이렇게 하면 `<p>` 요소를 포함한 `<div>`에만 배경색이 적용됩니다.

## 실전 예제들

### 1. 이미지가 있는 아티클 강조하기

```html
<article>
  <h2>첫 번째 아티클</h2>
  <p>내용이 여기에 있습니다.</p>
</article>

<article>
  <h2>두 번째 아티클</h2>
  <img src="image.jpg" alt="이미지 설명" />
  <p>이미지가 포함된 아티클입니다.</p>
</article>
```

```css
article:has(img) {
  border: 2px solid #007acc;
  padding: 10px;
}
```

**설명**: 이미지가 포함된 아티클에만 테두리와 패딩을 적용하여 시각적으로 강조합니다.

### 2. 서브메뉴가 있는 네비게이션 항목 표시하기

```html
<ul class="main-menu">
  <li>홈</li>
  <li>
    서비스
    <ul class="sub-menu">
      <li>웹 개발</li>
      <li>모바일 앱</li>
    </ul>
  </li>
  <li>문의하기</li>
</ul>
```

```css
.main-menu > li:has(.sub-menu)::after {
  content: " ▼";
  font-size: 0.8em;
  color: #555;
}
```

**설명**: 서브메뉴가 있는 항목에만 화살표 기호를 추가하여 사용자가 하위 메뉴가 있음을 알 수 있도록 합니다.

### 3. 입력 필드가 비어 있을 때 버튼 비활성화하기

```html
<form>
  <input type="text" id="username" placeholder="사용자 이름" />
  <button type="submit">제출</button>
</form>
```

```css
form:has(input:placeholder-shown) button {
  background-color: #ccc;
  cursor: not-allowed;
}
```

**설명**: 입력 필드가 비어 있을 때 버튼의 배경색을 변경하고 커서를 비활성화 상태로 표시하여 제출을 방지합니다.

## 주의할 점 및 참고 사항

- **브라우저 지원**: `:has()` 선택자는 최신 브라우저에서 지원됩니다. [Can I use](https://caniuse.com/css-has)에 따르면, 대부분의 최신 브라우저에서 사용할 수 있지만, 일부 구형 브라우저에서는 지원되지 않을 수 있습니다.

- **성능 고려**: 복잡한 선택자나 많은 요소에 `:has()`를 사용할 경우 성능에 영향을 줄 수 있으므로 주의해야 합니다.

- **중첩 사용 제한**: `:has()` 선택자 안에 또 다른 `:has()`를 중첩해서 사용하는 것은 현재 지원되지 않습니다.

## 마무리

`:has()` 선택자는 CSS에서 부모 요소를 자식 요소의 상태에 따라 스타일링할 수 있게 해주는 강력한 도구입니다. 자바스크립트 없이도 동적인 스타일링이 가능해져 코드의 간결성과 유지보수성이 향상됩니다. 실무에서도 다양한 상황에 활용할 수 있으니, 적극적으로 사용해보시기 바랍니다!

더 궁금한 점이나 공유하고 싶은 예제가 있다면 댓글로 남겨주세요. 😊
