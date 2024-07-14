---
title: "8-bit vs 16-bit 코드 유닛 프론트엔드와 백엔드를 위한 PHP Symfony와 TypeScript NextJS 비교 이야기"
description: ""
coverImage: "/TIL/assets/img/2024-07-15-8-bitvs16-bitcodeunitaPHPSymfonyvsTypeScriptNextJStaleforFrontendandBackend_0.png"
date: 2024-07-15 00:07
ogImage: 
  url: /TIL/assets/img/2024-07-15-8-bitvs16-bitcodeunitaPHPSymfonyvsTypeScriptNextJStaleforFrontendandBackend_0.png
tag: Tech
originalTitle: "8-bit vs 16-bit code unit a PHP Symfony vs TypeScript NextJS tale for Frontend and Backend"
link: "https://medium.com/stackademic/8-bit-vs-16-bit-code-unit-a-php-symfony-vs-typescript-nextjs-tale-for-frontend-and-backend-0f084d674c97"
---


<img src="/TIL/assets/img/2024-07-15-8-bitvs16-bitcodeunitaPHPSymfonyvsTypeScriptNextJStaleforFrontendandBackend_0.png" />

만약 국제화 및 번역이 필요한 제품에 작업했다면, 멀티바이트 문자열로 인한 문제에 직면한 적이 있을 것입니다.

게다가, 웹 개발자로 일하고 있다면 PHP 및 JavaScript (또는 TypeScript)에서 멀티바이트 문자열을 어떻게 확인할지 궁금해했을 것입니다.

저는 현재 유럽에 본사를 두고 아시아에 다른 사무실을 둔 회사에서 일하고 있습니다.

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

동아시아 지역에서 많은 사용자가 브라우징하고 있어서, 저희 시스템이 I18N 요구 사항을 충족할 준비가 되어 있는지 확인해야 했습니다.

우리는 먼저 PHP와 Symfony로 모놀리식 프로젝트를 개발했습니다. 그런 다음, 일부 작은 유틸리티를 사용하여 특정 작업을 처리하기 위해 JavaScript로 작성되었고 NodeJS로 구동되는 마이크로 서비스 기반 프로젝트로 전환했습니다.

지금은 동일한 프로젝트를 PHP와 Symfony를 사용하여 Backend API로 다시 만들고 있습니다. Frontend에서는 NextJS를 사용하여 PHP API 및 NextJS API로 HTTP 호출을 처리하며, Symfony, TypeScript 및 Golang 마이크로 서비스를 사용하고 있습니다.

다시 말해, 우리는 각 프로그래밍 언어의 가장 좋은 부분을 활용하고 있습니다.

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

다양한 프로그래밍 언어를 사용하면서 주목한 재미난 점 중 하나는 국제화(Internationalisation)가 다르게 처리된다는 것이에요.

저는 PHP와 TypeScript 두 가지 가장 주요한 프로그래밍 언어를 현재 사용하고 있기 때문에, 여러 바이트 문자열을 다룰 때 주요한 차이점을 강조하고 싶어요.

Symfony를 사용하면서, 제가 제출된 데이터의 백엔드 유효성 검사를 수행하기 위해 사용자 정의 Constraints를 만들었어요. 이 Constraints 중 하나는 주어진 문자열이 싱글 바이트(다중 바이트가 아닌)임을 보장하는 것인데, 특정 결제 공급업체 요구 사항 때문에 데이터를 "다루기 쉬운" 형식으로 필요하기 때문이에요.

아래는 PHP Symfony에서 백엔드 유효성 검사를 하는 예시 코드입니다.

```js
// PHP Symfony Backend

// src/Validator/IsSingleByteStringValidator.php

public function validate($value, Constraint $constraint): void
{
    // ...

    $singleByteLength = strlen($value);
    $multiByteLength = mb_strlen($value);

    if ($singleByteLength !== $multiByteLength) {
        $this->context->buildViolation($constraint->message)
            ->setParameter('{ string }', $value)
            ->addViolation();
    }
}
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

이전 유효성 검사를 실행하는 제약 조건을 사용하여 요청 프로세스에서 데이터를 유효성 검사했습니다. 이 모든 것이 #[MapRequestPayload] 속성을 통해 간단히 이루어졌어요.

하지만 JavaScript에 대한 유사한 접근 방식이 없다는 것을 알게 되었고, TypeScript 코드에서 비슷한 방식을 어떻게 달성할 수 있는지 궁금해졌습니다. 내가 발견한 것은 예상보다 더 복잡하면서 더 흥미로운 것이었어요.

또한 PHP와 JavaScript/TypeScript에는 동기적 및 비동기적 접근 방식 외에도 (개발 단계에 직접적으로 영향을 미치는 가장 관련성 있는 포인트 중 하나인) 문자열 처리 시 "코드 단위"에 대한 다른 차이점이 있다는 것을 배웠습니다.

```js
// 예시:

$str1 = "a";
$str2 = "円";
$str3 = "𠮷";

var_dump([
    $str1 => ['strlen' => strlen($str1), 'mb_strlen' => mb_strlen($str1)],
    $str2 => ['strlen' => strlen($str2), 'mb_strlen' => mb_strlen($str2)],
    $str3 => ['strlen' => strlen($str3), 'mb_strlen' => mb_strlen($str3)],
]);

// 이전 코드는 다음과 같은 출력 값을 반환합니다:

array(3) {
  'a' => array(2) {
    'strlen' => int(1)
    'mb_strlen' => int(1)
  }
  '円' => array(2) {
    'strlen' => int(3)
    'mb_strlen' => int(1)
  }
  '𠮷' => array(2) {
    'strlen' => int(4)
    'mb_strlen' => int(1)
  }
}
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

보시다시피, 두 개의 아시아 문자(일본어와 간단화된 중국어)를 처리할 때에도, 한 바이트 체크로 바이트 수를 확인하면 다른 결과를 얻지만, 멀티바이트 체크를 실행할 때에는 항상 같은 결과를 얻습니다.

이는 예상한 바와 같지만 여기서 강조해야 할 점은 문자에 따라 3바이트 또는 4바이트를 얻게 된다는 것입니다. 이 차이의 이유는 사용된 문자가 ASCII 관점뿐만 아니라 코드 유닛 관점에서도 다르기 때문입니다:

```javascript
$str1 = "a";    // ASCII 문자
$str2 = "円";   // BMP 내의 싱글 코드 유닛 문자
$str3 = "𠮷";   // 대리 서로쌍 (두 코드 유닛)
```

그러면 PHP가 이미 단일 문자 체크에서 다른 동작을 제공하는데, JavaScript나 TypeScript의 경우는 어떻게 될까요? 그 답은 서로 다른 시나리오를 직면할 때 더 간단한 작업을 수행한다는 점입니다:

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
const str1 = "a";    // ASCII character
const str2 = "円";   // Single code unit character in BMP
const str3 = "𠮷";   // Surrogate pair (two code units)

console.log(str1.length); // Output: 1
console.log(str2.length); // Output: 1
console.log(str3.length); // Output: 2
```

만약 PHP 예제를 다시 확인하면 각각의 경우에 바이트 수가 다른 것을 기억할 것입니다. 그런데 이게 왜 발생하는 걸까요?

- "a"는 PHP에서 1바이트이고 JavaScript / TypeScript에서도 1바이트입니다.
- "円"은 PHP에서 3바이트이고 JavaScript / TypeScript에서는 1바이트입니다.
- "𠮷"는 PHP에서 4바이트이고 JavaScript / TypeScript에서는 2바이트입니다.

답은 코드 단위에 있습니다:

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

- PHP에서 strlen("円")은 2를 반환합니다. 이는 PHP의 strlen() 함수가 바이트를 세기 때문에 "円"이 UTF-8로 표현될 때 8비트의 3바이트(E5 86 86)로 계산되기 때문입니다.
- JavaScript에서 "円".length는 16비트 코드 단위의 수를 반환합니다. 따라서 "円"에 대해 1을 반환합니다.
- 다시 말해, PHP는 8비트 논리를 사용하고, JavaScript는 16비트 논리를 사용합니다.

그래서 나는 각 케이스를 더 잘 이해하기 위해 편리하고 효율적으로 에러와 불일치의 개수를 최소화하려고, Frontend가 Backend API 호출을 방지할 수 있도록 초기 단계에서 이상한 것을 감지하면 됩니다.

```js
// 더 나은 이해를 위한 JavaScript 함수

function isSingleCodeUnit(char) {
    return char.length === 1;
}

function isSurrogatePair(char) {
    return char.length === 2 && char.codePointAt(0) > 0xFFFF;
}

function classifyCharacters(str) {
    let result = [];

    for (let char of Array.from(str)) {
        if (isSingleCodeUnit(char)) {
            result.push({ char, length: 1 });
        } else if (isSurrogatePair(char)) {
            result.push({ char, length: 2 });
        } else {
            result.push({ char, length: "Unknown" });
        }
    }

    return result;
}

// 예제 사용법:
let input1 = "a円𠮷";

console.log(classifyCharacters(input1));
// [
//     { char: 'a', length: 1 },
//     { char: '円', length: 1 },
//     { char: '𠮷', length: 2 }
// ]
```

이 케이스를 더 잘 이해하기 위해 유니코드와 기본 다국어 평면(BMP)과 UTF 인코딩에 대해 더 깊숙히 알아야 할 필요가 있습니다.

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

- Unicode은 모든 문자체계에서 모든 문자를 포함하고자 하는 표준입니다. 각 문자에는 고유한 코드 포인트가 할당됩니다.
- 기본 다국어 평면(BMP)은 Unicode 문자 집합의 첫 번째 평면으로, U+0000부터 U+FFFF까지의 코드 포인트를 가진 문자가 포함되어 있습니다. 이에는 라틴 문자, 숫자, 기호 및 다양한 문자 체계의 문자가 포함됩니다.
- UTF-8: 문자가 1~4바이트 길이일 수 있는 가변 길이 인코딩입니다. ASCII 문자(U+0000부터 U+007F)는 1바이트입니다. U+0080에서 U+FFFF까지는 2바이트 또는 3바이트이며, U+10000에서 U+10FFFF까지의 문자는 4바이트입니다.
- UTF-16: 문자가 일반적으로 2바이트인 가변 길이 인코딩입니다. 그러나 BMP 밖의 문자(U+10000에서 U+10FFFF)는 대리 쌍을 사용하여 4바이트로 인코딩됩니다.
- UTF-32: 각 문자가 코드 포인트에 관계없이 4바이트인 고정 길이 인코딩입니다.

BMP(다른 인코딩의 맥락에서의 다중 바이트 문자):

- UTF-8: BMP(U+0000에서 U+FFFF)의 문자는 1, 2 또는 3바이트로 인코딩될 수 있습니다. ASCII 문자(U+0000에서 U+007F)는 1바이트입니다. 기타 BMP 문자(U+0080에서 U+FFFF)는 2 또는 3바이트일 수 있습니다.
- UTF-16: BMP의 문자는 일반적으로 2바이트로 인코딩됩니다. BMP 밖의 문자만 대리 쌍을 사용하여 4바이트로 인코딩됩니다.

다음은 UTF-8 및 UTF-16에서 다양한 문자가 인코딩되는 방식을 설명하는 몇 가지 예시입니다:

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

[ UTF-8 ] ASCII 문자 (예: ‘A’):

- 코드 포인트: U+0041
- UTF-8 인코딩: 1바이트 (0x41)

[ UTF-8 ] Latin-1 보충 (예: ‘é’):

- 코드 포인트: U+00E9
- UTF-8 인코딩: 2바이트 (0xC3 0xA9)

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

[ UTF-8 ] CJK 통합 한자 (예: ‘円’):

- 코드 포인트: U+5186
- UTF-8 인코딩: 3바이트 (0xE5 0x86 0x86)

[ UTF-16 ] ASCII 문자 (예: ‘A’):

- 코드 포인트: U+0041
- UTF-16 인코딩: 2바이트 (0x0041)

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

[ UTF-16 ] 라틴-1 보충 (예 : ‘é’):

- 코드 포인트: U+00E9
- UTF-16 인코딩: 2바이트 (0x00E9)

[ UTF-16 ] CJK 통합 한자 (예 : ‘円’):

- 코드 포인트: U+5186
- UTF-16 인코딩: 2바이트 (0x5186)

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

[ UTF-16 ] 보충 문자 (예: '𠮷'):

- 코드 포인트: U+20BB7
- UTF-16 인코딩: 4바이트 (0xD842 0xDFB7) (대리 쌍)

다중 바이트 문자: UTF-8에서는 한 바이트보다 더 많은 바이트가 필요한 모든 문자를 의미합니다. UTF-16에서는 BMP(기본 다중 언어 평면) 내 문자들이 일반적으로 2바이트이지만, BMP 밖의 문자들은 대리 쌍으로 인코딩되어 4바이트를 차지합니다.

BMP 문자: 대부분 일반적인 문자와 기호를 포함하며, UTF-16에서는 보통 2바이트로 표현됩니다. UTF-8에서는 1에서 3바이트까지 길이가 다양합니다.

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

## 박수와 팔로우 부탁드려요!

👏 이 글이 마음에 드셨나요? 아래의 👏 버튼을 눌러 박수를 보내주세요. 여러분의 지원은 저에게 큰 힘이 됩니다!

📚 제 최신 게시물을 계속해서 받고 싶으신가요? "팔로우" 버튼을 눌러 저의 커뮤니티에 참여하고 놓치지 마세요.

읽어 주셔서 감사합니다! 여러분의 피드백과 지원이 저에게 더 많은 가치있는 정보를 공유하도록 격려해줍니다. 🙌

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

# 스택더믹 🎓

끝까지 읽어주셔서 감사합니다. 떠나시기 전에:

- 작가를 클래핑하고 팔로우해주시면 감사하겠습니다! 👏
- 저희를 팔로우해주세요 X | LinkedIn | YouTube | Discord
- 다른 플랫폼도 방문해보세요: In Plain English | CoFeed | Differ
- 더 많은 콘텐츠는 Stackademic.com에서 확인하세요