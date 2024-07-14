---
title: "TypeScript 코드를 더 잘 쓰는 방법"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-HowtowritebetterTypescriptcodes_0.png"
date: 2024-07-14 20:46
ogImage: 
  url: /TIL/assets/img/2024-07-14-HowtowritebetterTypescriptcodes_0.png
tag: Tech
originalTitle: "How to write better Typescript codes"
link: "https://medium.com/@technicadil_001/how-to-write-better-typescript-codes-dbfe43d85103"
---


## TYPESCRIPT

<img src="/TIL/assets/img/2024-07-14-HowtowritebetterTypescriptcodes_0.png" />

## #1 Optional Chaining (?.):

Optional chaining(?.)은 중첩된 프로퍼티 또는 메소드에 안전하게 접근할 수 있게 해줍니다. null 또는 undefined 값에 대해 걱정할 필요가 없습니다. 중간 프로퍼티 중 하나라도 null 또는 undefined이면 평가가 중단됩니다.

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

```js
const user = {
  name: 'Piotr',
  address: {
    city: 'Warsaw',
    postalCode: '00-240'
  }
};


const postalCode = user.address?.postalCode;
console.log(postalCode); // 00-240

const invalidCode = user.address?.postalCode?.toLowerCase();
console.log(invalidCode); // Output: undefined
```

## #2 Use Mapped Types for Transformation

Mapped types allow you to create new types by transforming properties of existing types.

```js
type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};
type Partial<T> = {
  [P in keyof T]?: T[P];
};
```

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

## #3 유틸리티 유형

TypeScript는 일반적인 유형 변환을 돕기 위해 여러 유틸리티 유형을 제공합니다.

i) Partial`T`: 모든 속성을 선택적으로 만듭니다.
ii) Required`T`: 모든 속성을 필수로 만듭니다.
iii) Readonly`T`: 모든 속성을 읽기 전용으로 만듭니다.
iv) Record`K, T`: 키가 K이고 유형이 T인 유형을 생성합니다.

```js
type Person = {
  name: string;
  age: number;
};
type PartialPerson = Partial<Person>;
type ReadonlyPerson = Readonly<Person>;
```

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

## #4 타입 가드

타입 가드를 사용하여 조건 블록 내에서 타입을 좁힐 수 있습니다.

```js
function isString(value: unknown): value is string {
  return typeof value === 'string';
}
```

## #5 템플릿 리터럴 타입

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

표 태그를 마크다운 형식으로 변경해주세요.


Tese allow you to create new string types by combining string literals.

```js
type EventName = 'click' | 'hover';
type EventHandlerName = `${EventName}Handler`; // 'clickHandler' | 'hoverHandler'
```

## #6 Indexed Access Types

Use indexed access types to extract the type of a property.


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

```js
유형 Person = { 이름: string; 나이: number };
유형 NameType = Person['name']; // string
```

## #7 키 리매핑을 통한 Mapped Types

새로운 유형을 생성하는 동안 키를 변환합니다.

```js
유형 PrefixKeys<T, P extends string> = {
  [K in keyof T as `${P}${K & string}`]: T[K]
};
유형 PrefixedPerson = PrefixKeys<{ 이름: string; 나이: number }, 'prefix_'>;
// { prefix_name: string; prefix_age: number }
```

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

## #8 공용체

이들은 서로 다른 유형의 조합을 만들고 유형 안전한(타입 세이프) 공용체를 만드는 데 도움을 줍니다.

```js
type Shape =
  | { kind: 'circle'; radius: number }
  | { kind: 'square'; side: number };

function getArea(shape: Shape) {
  switch (shape.kind) {
    case 'circle': return Math.PI * shape.radius ** 2;
    case 'square': return shape.side ** 2;
  }
}
```

## #9 제네릭에서 추론된 유형

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

`infer`를 사용하여 조건형 내에서 타입을 추출하고 사용하세요.

```js
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : any;
type Fn = () => number;
type Result = ReturnType<Fn>; // number
```

## #10 모듈 확장

기존 모듈에 새로운 기능을 추가하세요.

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

```typescript
// augmentations.ts
import 'express';
declare module 'express' {
  interface Request {
    user?: { id: string; role: string };
  }
}
```

## #11 Declare Merged Interfaces

Merge interfaces to extend types, especially useful with third-party libraries.

```typescript
interface Window {
  myCustomProperty: string;
}
window.myCustomProperty = 'Hello!';
```

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

## #12 기능 오버로딩

더 나은 유형 확인을 위해 여러 함수 시그니처를 제공합니다.

```js
function createDate(timestamp: number): Date;
function createDate(year: number, month: number, day: number): Date;
function createDate(x: number, y?: number, z?: number): Date {
  return y !== undefined && z !== undefined ? new Date(x, y, z) : new Date(x);
}
```

## #13 브랜드된 타입

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

브랜드화된 타입을 사용하여 명목적 타이핑을 만들어봐요.

```js
type UserId = string & { _brand: 'UserId' };
function createUserId(id: string): UserId {
  return id as UserId;
}
```

## #14 템플릿 리터럴 타입과 컨디셔널 타입

템플릿 리터럴 타입과 컨디셔널 타입을 결합하여 고급 문자열 조작을 해보세요.

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

```js
type ExtractRouteParams<T extends string> = T extends `/${infer Param}/${infer Rest}`
  ? { param: Param } & ExtractRouteParams<`/${Rest}`>
  : {};

type Params = ExtractRouteParams<'/user/:id/posts/:postId'>;
// { param: 'user' } & { param: 'posts' }
```

#15 Variadic Tuple Types

Typescript 4+는 가변 튜플 타입을 지원하여 튜플이 배열의 나머지 부분을 캡처할 수 있도록 합니다.

```js
type Push<T extends any[], V> = [...T, V];
type Result = Push<[1, 2, 3], 4]; // [1, 2, 3, 4]
```