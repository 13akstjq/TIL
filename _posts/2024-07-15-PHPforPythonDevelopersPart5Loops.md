---
title: "파이썬 개발자를 위한 PHP  5편  반복문 사용하는 방법"
description: ""
coverImage: "/TIL/assets/img/2024-07-15-PHPforPythonDevelopersPart5Loops_0.png"
date: 2024-07-15 00:02
ogImage: 
  url: /TIL/assets/img/2024-07-15-PHPforPythonDevelopersPart5Loops_0.png
tag: Tech
originalTitle: "PHP for Python Developers  Part 5  Loops"
link: "https://medium.com/@tihomir.manushev/php-for-python-developers-part-5-loops-87b8a9fe0959"
---


![image](/TIL/assets/img/2024-07-15-PHPforPythonDevelopersPart5Loops_0.png)

# 시리즈의 목적

이 시리즈의 목적은 PHP 8.x를 배우려는 Python 3.x 개발자들에게 체계적인 안내를 제공하는 것입니다. 두 언어 사이의 유사점과 차이점을 강조하며, 원활한 전환을 보장하기 위해 실용적인 예제와 비교를 제공할 것입니다.

본 시리즈는 이미 Python과 핵심 프로그래밍 개념에 익숙하다고 가정합니다.

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

# 소개

반복문은 프로그래밍에 중요한 요소로, 특정 조건에 따라 코드가 반복적으로 실행될 수 있습니다. PHP에서는 다양한 종류의 반복문을 제공하여 더 많은 유연성을 제공합니다.

본 문서에서는 다음을 비교할 것입니다:

- while 루프
- for 루프
- foreach 루프
- do-while 루프
- break / continue 문장

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

# While loop

while 루프는 지정된 조건이 참인 경우에만 코드 블록을 반복하고 싶을 때 사용됩니다.

파이썬에서는 while 루프를 다음과 같이 사용합니다:

```python
count = 0
while count < 5:
    print(f"Count: {count}")
    count += 1
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

- count은 0부터 시작합니다
- 루프가 count가 5보다 작은 동안 계속 실행됩니다
- 루프 내에서 count가 출력되고 증가합니다

PHP에서는 다음과 같이 while 루프를 사용합니다:

```php
<?php
$count = 0;
while ($count < 5) {
    echo "Count: $count\n";
    $count++;
}
?>
```

- count가 0으로 초기화됩니다
- 루프는 $count가 5보다 작은 동안 계속 실행됩니다
- 루프 내에서 count가 출력되고 1씩 증가합니다

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

구문은 매우 유사하며, 로직은 정확히 같습니다.

# for 루프

for 루프는 일반적으로 반복 횟수가 미리 알려진 경우에 사용됩니다. 두 언어 모두 구별되는 구문을 갖고 있습니다.

파이썬에서:

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
for i in range(5):
    print(f"Iteration: {i}")
```

- range(5) 함수는 0부터 4까지의 숫자를 생성합니다.
- 이 범위 내 각 숫자마다 i가 출력됩니다.

PHP에서:

```php
<?php
for ($i = 0; $i < 5; $i++) {
    echo "Iteration: $i\n";
}
?>
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

- 루프는 $i를 0으로 초기화합니다.
- $i가 5보다 작은 경우에만 실행됩니다.
- 각 반복 후에 $i는 1씩 증가합니다.

PHP는 for 루프에 대해 C 스타일 구문을 사용합니다. $i = 0은 for 루프가 실행하는 반복 횟수와 관계없이 한 번만 계산됩니다. $i ` 5는 각 반복의 시작부에서 계산되고, $i++은 각 반복의 끝에서 계산됩니다.

재미있게도, 한 줄로 for 루프를 작성할 수 있지만, 하지 말아주세요. 코드는 실행되겠지만, 모든 해석기가 에러를 발생시킵니다.

파이썬에서:

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

위의 표를 마크다운 형식으로 변경해보겠습니다.


| Header One | Header Two |
|------------|------------|
| Row 1, Col 1 | Row 1, Col 2 |
| Row 2, Col 1 | Row 2, Col 2 |



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
<?php
for ($i = 1, $j = 0; $i <= 10; $j += $i, print $i, $i++);
?>
```

# Foreach Loop

The foreach loop is used to iterate over arrays or collections only.

In Python, we don't have foreach loops, but we can mimic it using the in membership operator and a for loop like this:

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

```javascript
colors = ["red", "green", "blue"];
for color in colors:
    print(f"Color: {color}")
```

- colors는 리스트입니다.
- for 루프는 각 요소를 순회하며 color에 할당하고 콘솔에 인쇄합니다.

PHP는 foreach 루프를 지원하며 for 루프와는 매우 다른 구문을 사용합니다.

```php
<?php
$colors = ["red", "green", "blue"];
foreach ($colors as $color) {
    echo "Color: $color\n";
}
?>
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

- `$colors`은 배열입니다
- 루프는 `$colors` 배열의 각 요소를 순회하며 현재 요소를 `$color`에 할당하고 콘솔에 출력합니다.

만약 연관 배열(파이썬의 딕셔너리)을 순회하려면 다른 구문을 사용할 수 있습니다:

```js
<?php
$color = [
    "r" => "red",
    "g" => "green",
    "b" => "blue",
];

foreach ($color as $key => $value) {
    echo "The $key is $value\n";
}
?>

// 결과:
// The r is red
// The g is green
// The b is blue
```

# Do-While 루프

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

PHP에는 코드 블록이 한 번 이상 실행되도록 보장하는 do-while 루프가 있습니다. Python에는 내장 do-while 루프가 없지만 while 루프와 break 조건을 사용하여 유사한 효과를 얻을 수 있습니다.

Python에서:

```python
count = 0
while True:
    print(f"Count: {count}")
    count += 1
    if count >= 5:
        break
```

- while True 룹은 무한히 실행됩니다.
- count가 5에 도달하면 break 문으로 루프를 수동으로 중단합니다.

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

PHP에서:

```js
<?php
$count = 0;
do {
    echo "Count: $count\n";
    $count++;
} while ($count < 5);
?>
```

- 코드 블록이 먼저 실행됩니다
- 그 후 $count < 5 조건이 확인됩니다
- 조건이 참인 동안 반복 실행됩니다

while 및 do-while 루프의 주요 차이점은 각 반복에서 진실 식이 시작이 아닌 끝에서 확인된다는 점입니다. 이것이 첫 반복이 항상 보장되는 이유입니다.

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

# break / continue 문

PHP와 Python 모두 break 및 continue 문을 제공하여 반복문의 흐름을 제어할 수 있습니다.

## break

Python에서는 반복문을 종료하는 방법은 다음과 같습니다:

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
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

for number in numbers:
    if number > 5:
        break # 숫자가 5보다 크면 룹을 종료합니다.
    print(f"Number: {number}")
```

PHP에서:

```js
<?php
$numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

foreach ($numbers as $number) {
    if ($number > 5) {
        break; // 숫자가 5보다 크면 룹을 종료합니다.
    }
    echo "Number: $number\n";
}
?>
```

추가로, PHP에서의 break는 선택적으로 숫자 인수를 허용하며, 이는 몇 개의 중첩된 룹을 탈출할지를 나타냅니다. 기본값은 1입니다. 두 룹을 동시에 탈출하는 방법에 대한 예시는 다음과 같습니다:

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
<?php
$matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

$target = 5;
$found = false;

foreach ($matrix as $row) {
    foreach ($row as $value) {
        if ($value == $target) {
            echo "Found target value $target. Breaking out of both loops.\n";
            $found = true;
            break 2; // Break out of both loops
        }
    }
}

if (!$found) {
    echo "Target value $target not found.\n";
}
?>
```

이 코드는 매우 유용하네요! Python에 이 기능을 추가해야 할까요?

## continue

Python에서는 다음 반복으로 건너뛰는 방법이 있습니다:

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
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

for number in numbers:
    if number % 2 == 0:
        continue # 짝수를 건너뛰기
    print(f"Number: {number}")
```

PHP에서 동일한 기능:

```js
<?php
$numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

foreach ($numbers as $number) {
    if ($number % 2 == 0) {
        continue; // 짝수를 건너뛰기
    }
    echo "Number: $number\n";
}
?>
```

continue 문은 중첩된 루프의 끝까지 건너뛰어야 하는 수준을 나타내는 선택적 숫자 인수를 받을 수도 있습니다. 기본 값은 1이며, 따라서 현재 루프의 끝으로 건너뛰게 됩니다.

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
<?php
$matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

foreach ($matrix as $row) {
    foreach ($row as $value) {
        if ($value % 2 == 0) {
            echo "짝수 $value를 만나면 해당 행 건너뛰기.\n";
            continue 2; // 해당 행의 나머지 부분을 건너뛰고 외부 루프의 다음 행으로 이동
        }
        echo "값: $value\n";
    }
}
?>

// 출력:
// 값: 1
// 짝수 2를 만나면 해당 행 건너뛰기.
// 짝수 4를 만나면 해당 행 건너뛰기.
// 값: 7
// 짝수 8를 만나면 해당 행 건너뛰기.
```

# 결론

루프는 프로그래밍에서 반복 작업에 필수적입니다. PHP 8 및 Python 3은 각기 다른 문법과 흐름을 가진 다양한 루프 구조를 제공합니다:

- while 루프: 두 언어에서 유사하며 조건에 기반한 반복에 사용됩니다.
- for 루프: Python에서는 range 함수를 사용하여 더 간단하며, PHP에서는 초기화자, 조건 및 증가자를 사용합니다.
- foreach 루프: PHP에서 직접 배열에 사용 가능하며, Python에서는 for 루프 기능의 일부입니다.
- do-while 루프: PHP에서 기본적으로 제공되며, Python에서는 while True 루프와 중단 조건을 사용하여 시뮬레이션할 수 있습니다.


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

# 만약 이 기사를 읽는 것을 즐기셨거나 도움이 되었다면, 아래와 같이 할 수 있어요:

- 20번 클랩을 눌러주세요
- 어떻게 도움이 되었는지 댓글을 남겨주세요
- 파이썬, PHP 및 AI 기사를 더 보려면 팔로우해주세요

여러분의 피드백과 참여가 저에게 글쓰고 가치 있는 콘텐츠를 공유하도록 격려해줍니다. 이 여정의 한 부분이 되어 주셔서 감사합니다!

또한 저를 LinkedIn에서도 만날 수 있어요.