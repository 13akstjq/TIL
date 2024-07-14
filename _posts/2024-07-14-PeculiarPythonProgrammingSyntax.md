---
title: "알아두면 유용한 파이썬 프로그래밍 문법 10가지"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-PeculiarPythonProgrammingSyntax_0.png"
date: 2024-07-14 20:37
ogImage: 
  url: /TIL/assets/img/2024-07-14-PeculiarPythonProgrammingSyntax_0.png
tag: Tech
originalTitle: "Peculiar Python Programming Syntax"
link: "https://medium.com/@elye-project/peculiar-python-programming-syntax-7b72083b7810"
---


## 프로그래밍 언어 배우기

![Peculiar Python Programming Syntax](/TIL/assets/img/2024-07-14-PeculiarPythonProgrammingSyntax_0.png)

만약 C++, Java 또는 Kotlin을 알고 있다면, Python을 배우는 것은 비교적 쉬울 것입니다. 그러나 Python에는 그 언어들에서는 만나지 못할 독특한 프로그래밍 규칙이 몇 가지 있습니다. 이를 공식적인 학습 없이 발견하기란 거의 불가능할 것입니다.

Python의 이러한 특징에 익숙해지는 것은 유익하지만, 모두를 발견하려면 공식 Python 3 튜토리얼 (글쓴 시점에서 Python 3.12.3 사용중)을 살펴보는 것은 꽤 지루할 수 있습니다.

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

이 프로세스를 편리하게 만들기 위해, C++, Java 또는 Kotlin을 배우며 접하지 못한 Python의 독특한 기능과 관용구 목록을 정리해왔어요. Python 프로그래밍의 독특한 측면을 이해하려는 누구에게나 이 개요가 도움이 되기를 바랍니다.

이 목록이 모두를 포함하지 않을 수 있고, 다른 언어에도 몇 가지 기능이 존재할 수 있음을 주의해 주세요. Python에만 해당되는 것이 아닐 수 있는 누락 사항이나 사항에 대한 피드백을 환영합니다.

# Python Repl 사용

## 마지막으로 출력된 값 할당

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

인터랙티브 모드에서는 마지막으로 출력된 표현식이 변수 `_`에 할당됩니다. 이는 Python을 계산기로 사용할 때 계산을 계속하기가 다소 쉽다는 것을 의미합니다. 예를 들어:

```js
>>> tax = 12.5 / 100
>>> price = 100.50
>>> price * tax
12.5625
>>> price + _
113.0625
>>> round(_, 2)
113.06
```

# 텍스트

## Raw String (이스케이프 문자 \ 사용 안 함)

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

만약 \로 시작하는 문자를 특수 문자로 해석하지 않으려면, 첫 따옴표 앞에 r을 추가하여 raw 문자열을 사용할 수 있어요:

```js
>>> print('C:\some\name')  # 여기서 \n은 새 줄을 의미해요!
C:\some
ame
>>> print(r'C:\some\name')  # 따옴표 앞의 r을 주목해 주세요
C:\some\name
```

## 문자열 곱하기

문자열은 + 연산자로 이어붙이거나 *를 통해 반복할 수 있어요:

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
>>> # 3번의 'un', 그리고 'ium'이 이어집니다
>>> 3 * 'un' + 'ium'
'unununium'
```

## 연산자를 사용한 문자열 결합

따옴표로 둘러싸인 두 개 이상의 문자열 리터럴은 자동으로 연결됩니다. 이 기능은 변수에는 적용되지 않습니다.

```js
>>> 'Py' 'thon'
'Python'
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

## 문자열 색인화

문자열이 할당되었을 때...

```js
>>> word = 'Python'
```

다음과 같이 색인화됩니다

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


 +---+---+---+---+---+---+
 | P | y | t | h | o | n |
 +---+---+---+---+---+---+
 0   1   2   3   4   5   6
-6  -5  -4  -3  -2  -1


다양한 흥미로운 예시들을 통해 접근하는 방법을 살펴보겠습니다.

```js
>>> word[0]  # 위치 0에 있는 문자
'P'
>>> word[5]  # 위치 5에 있는 문자
'n'
>>> word[-1]  # 마지막 문자
'n'
>>> word[-2]  # 끝에서 두 번째 문자
'o'
>>> word[-6]
'P'
>>> word[0:2]  # 위치 0부터 (포함) 2까지 (미포함)의 문자
'Py'
>>> word[2:5]  # 위치 2부터 (포함) 5까지 (미포함)의 문자
'tho'
>>> word[:2]   # 시작부터 위치 2까지 (미포함)의 문자
'Py'
>>> word[4:]   # 위치 4부터 (포함) 끝까지의 문자
'on'
>>> word[-2:]  # 끝에서 두 번째부터 (포함) 끝까지의 문자
'on'
>>> word[:2] + word[2:]
'Python'
>>> word[:4] + word[4:]
'Python'
>>> word[4:42]
'on'
>>> word[42:]
''
>>> word[42]  # 단어는 6개의 문자만 있습니다
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: string index out of range
```

## 참고 및 얕은 복사


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

보통의 할당은 참조를 할당합니다.

```js
>>> rgb = ["Red", "Green", "Blue"]
>>> rgba = rgb
>>> id(rgb) == id(rgba)  # 같은 객체를 참조합니다
True
>>> rgba.append("Alph")
>>> rgb
["Red", "Green", "Blue", "Alph"]
```

하지만 = 와 [:] 를 사용하면 얕은 복사가 됩니다.

```js
>>> correct_rgba = rgba[:]
>>> correct_rgba[-1] = "Alpha"
>>> correct_rgba
["Red", "Green", "Blue", "Alpha"]
>>> rgba
["Red", "Green", "Blue", "Alph"]
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

# 프로그래밍으로 나아가는 첫걸음

## 더 짧은 피보나치 알고리즘

동일한 라인 할당을 사용하면 피보나치 알고리즘을 훨씬 더 짧게 작성할 수 있습니다.

```js
>>> a, b = 0, 1
>>> while a < 1000:
...     print(a, end=',')
...     a, b = b, a+b
...
0,1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,
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

## print 문에서의 자동 공백 삽입

print 문을 사용할 때, 입력값들 사이에 자동으로 공백이 추가됩니다. 즉, '이것의 값은'과 'i' 사이에 자동으로 공백이 추가됩니다.

```js
>>> i = 256*256
>>> print('이것의 값은', i)
이것의 값은 65536
```

# 제어 흐름 도구 더 알아보기

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

## For - Else Flow Control

If-Else에 대해 들어보셨나요? Kotlin에서 소수를 확인하는 것과 같이, 찾아낸 소수가 아닌 경우 멈출 수 있는 방법이 있습니다. 불리언 변수 `isPrime`를 false로 설정할 수 있어요.

```js
for (n in 2..9) { 
    var isPrime = true
    for (x in 2 until n) { 
        if (n % x == 0) {
            println("$n equals $x * ${n / x}")
            isPrime = false
            break
        }
    }
    if (isPrime) {
        println("$n is a prime number")
    } 
}
```

하지만 Python에서는 For-Else를 사용해 isPrime 변수를 생략할 수 있어요.

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
>>> for n in range(2, 10):
...    for x in range(2, n):
...       if n % x == 0:
...          print(n, 'equals', x, '*', n//x)
...          break
...    else:
...       print(n, 'is a prime number')

2 is a prime number
3 is a prime number
4 equals 2 * 2
5 is a prime number
6 equals 2 * 3
7 is a prime number
8 equals 2 * 4
9 equals 3 * 3
```

## Pass Flow Control

The pass keyword is just to let Python know it’s doing nothing, than thinking it’s a syntax error

```js
>>> while True:
...    pass  # Busy-wait for keyboard interrupt (Ctrl+C)

>>> class MyEmptyClass:
...    pass

>>> def initlog(*args):
...    pass   # Remember to implement this!
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

## Match Statements

파이썬에서 `match`는 C++이나 Java에서의 `switch`와 Kotlin의 `when`과 유사합니다. 그러나 이는 패턴 매칭과 언패킹 할당에서 더 강력합니다. 주어진 사전의 패턴을 매칭하고 결과를 변수에 언패킹할 수 있습니다.

```js
def process_sequence(seq):
    match seq:
        case ["first", (left, right), _, *rest]:
            print(f"Matched pattern:")
            print(f"First element: 'first'")
            print(f"Tuple: ({left}, {right})")
            print(f"Rest of the list: {rest}")
        case _:
            print("No match found")

# Example sequences
sequence1 = ["first", (1, 2), 3, 4, 5, 6]
sequence2 = ["first", (10, 20), "x", "y", "z"]
sequence3 = ["second", (1, 2), 3, 4, 5]

# Process sequences
process_sequence(sequence1)
process_sequence(sequence2)
process_sequence(sequence3)
```

결과는 아래와 같습니다.

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
일치하는 패턴:
첫 번째 요소: 'first'
튜플: (1, 2)
나머지 목록: [4, 5, 6]
일치하는 패턴:
첫 번째 요소: 'first'
튜플: (10, 20)
나머지 목록: ['y', 'z']
일치하는 항목이 없습니다
```

이 스마트한 패턴 매칭은 데이터 추출 로직을 효과적으로 처리했습니다. 더 많은 통찰을 위해 PEP646 튜토리얼을 참조해보세요.

## 함수 매개변수 제어

이전 언어에서는 함수의 매개변수가 엄격히 위치에 따라 정해졌습니다. 최신 언어에서는 매개변수를 키워드로 할당하고 위치 요구 사항을 무시할 수 있습니다.

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

파이썬은 개발자들이 원하는 대로 선택할 수 있게 해줘요.

```js
#############################
## 기본값: 위치 및 키워드 모두 허용
#############################
>>> def standard_arg(arg):
...    print(arg)

>>> standard_arg(2)
2

>>> standard_arg(arg=2)
2

#############################
## 위치 전용
#############################
>>> def pos_only_arg(arg, /):
...    print(arg)

>>> pos_only_arg(1)
1

>>> pos_only_arg(arg=1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: pos_only_arg() got some positional-only arguments passed as keyword arguments: 'arg'

#############################
## 키워드 전용
#############################
>>> def kwd_only_arg(*, arg):
...    print(arg)

>>> kwd_only_arg(3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: kwd_only_arg() takes 0 positional arguments but 1 was given

>>> kwd_only_arg(arg=3)
3

#############################
## 결합된 키워드 및 위치
#############################

>>> def combined_example(pos_only, /, standard, *, kwd_only):
...    print(pos_only, standard, kwd_only)

>>> combined_example(1, 2, 3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: combined_example() takes 2 positional arguments but 3 were given

>>> combined_example(1, 2, kwd_only=3)
1 2 3

>>> combined_example(1, standard=2, kwd_only=3)
1 2 3

>>> combined_example(pos_only=1, standard=2, kwd_only=3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: combined_example() got some positional-only arguments passed as keyword arguments: 'pos_only'
```

## 임의 매개변수 뒤에 매개변수

C++, Java 및 Kotlin에서 임의 매개변수를 허용하지만, 그 뒤에 가능한 고정 매개변수를 누가 상상할까요?

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

하지만 Python으로는 아래와 같이 가능합니다. 여기서 sep은 임의의 매개변수 뒤에 고정된 매개변수입니다.

```python
>>> def concat(*args, sep="/"):
...    return sep.join(args)

>>> concat("earth", "mars", "venus")
'earth/mars/venus'
>>> concat("earth", "mars", "venus", sep=".")
'earth.mars.venus'
```

## JSON 처리 함수 매개변수

함수에 JSON을 제공하고 모든 것을 처리할 수 있다면 멋지지 않을까요?

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

파이썬을 사용하면 함수 매개변수로 보내는 변수에 **를 사용하여 가능합니다.

```python
>>> def testing(voltage, state='a stiff', action='voom'):
...    print("action =", action)
...    print("voltage =", voltage)
...    print("state =", state)

>>> d = {"voltage": "four million", "state": "bleedin' demised", "action": "VOOM"}
>>> testing(**d)

action = VOOM
voltage = four million
state = bleedin' demised
```

## 람다: 한 줄 함수

우리는 Java 8 및 Kotlin에서 Lambda에 대해 배웠지만, lambda 키워드는 사용되지 않았습니다. 대신 Lambda로 함수 포인터가 보내지고, 일반적으로 그 함수를 정의해야 했습니다.

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

파이썬에서는 lambda 키워드를 사용하여 해당 함수를 정의할 수 있어요

예시

```js
def getKey(pair):
    return pair[1]

# 같은 효과를 내는 코드입니다    
getKey_lambda = lambda pair: pair[1]
```

다음과 같이 간단한 사용 사례가 있어요. 람다 함수를 별도로 정의할 필요가 없는 경우입니다

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
>>> # 튜플 리스트
>>> pairs = [(1, 'one'), (2, 'two'), (3, 'three'), (4, 'four')]

>>> # 각 튜플의 두 번째 요소를 기준으로 리스트 정렬하기 (람다 사용)
>>> pairs.sort(key=lambda pair: pair[1])

>>> print(pairs)
[(4, 'four'), (1, 'one'), (3, 'three'), (2, 'two')]
```

## 함수 설명서 출력하기

일반적으로 주석은 프로그래밍 코드의 일부가 아닙니다. 그러나 Python에서는 함수.__doc__을 사용하여 함수 설명서를 출력할 수 있습니다.

```js
>>> def my_function():
...    """아무것도 하지 않지만 문서화합니다.
...
...    정말로, 아무것도 실행하지 않습니다.
...    """
...    pass

>>> print(my_function.__doc__)
아무것도 하지 않지만 문서화합니다.

    정말로, 아무것도 실행하지 않습니다.
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

하지만 매개변수만 알고 싶다면 .__annotations__를 사용할 수 있어요.

```python
>>> def f(ham: str, eggs: str = 'eggs') -> str:
...    print("Annotations:", f.__annotations__)
...    print("Arguments:", ham, eggs)
...    return ham + ' and ' + eggs

>>> f('spam')
Annotations: {'ham': <class 'str'>, 'return': <class 'str'>, 'eggs': <class 'str'>}
Arguments: spam eggs
'spam and eggs'
```

# 데이터 구조

## 리스트 내포

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

코드를 훨씬 간략하게 만들어주는 목록 내 항목을 이해해요.

```js
>>> squares = [x**2 for x in range(10)]
>>> print(squares)
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

>>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```

이를 통해 우리는 리스트를 변환하거나 필터링하는 등 알고리즘 함수를 수행할 수 있어요. 예를 들어,

```js
>>> vec = [-4, -2, 0, 2, 4]
>>> # 값이 두 배인 새 리스트 생성
>>> [x*2 for x in vec]
[-8, -4, 0, 4, 8]

>>> # 음수를 제외한 리스트 필터링
>>> [x for x in vec if x >= 0]
[0, 2, 4]
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

## 콤마 할당은 튜플을 만듭니다

할당 뒤에 쉼표(,)를 추가하면 전체 할당이 튜플 유형이 됩니다.

```js
>>> noComma = '안녕하세요'
>>> withComma = '안녕하세요',

>>> print(len(noComma))
5
>>> print(len(withComma))
1

>>> print(noComma)
안녕하세요
>>> print(withComma)
('안녕하세요',)
```

## 튜플 언패킹

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

튜플에서의 역 언패킹 할당은 가능합니다.

```js
>>> # 튜플 만들기
>>> t = 12345, 54321, 'hello!'

>>> # 튜플을 3개의 변수로 언패킹 (튜플의 크기와 정확히 일치해야 함)
>>> x, y, z = t
```

## 집합: 고유한 목록

중복을 없애는 리스트와 유사합니다. 꺽은 괄호 `{}`를 사용합니다.

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
>>> a = set('abracadabra')
>>> b = set('alacazam')
>>> a                                  # a 안의 고유한 문자
{'a', 'r', 'b', 'c', 'd'}
>>> a - b                              # a에만 있는 문자
{'r', 'd', 'b'}
>>> a | b                              # a나 b 또는 둘 다에 있는 문자
{'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
>>> a & b                              # a와 b에 모두 있는 문자
{'a', 'c'}
>>> a ^ b                              # a나 b에만 있는 문자
{'r', 'd', 'b', 'm', 'z', 'l'}
```

이것이 사전의 기초를 이룹니다.

## 문자열 불리언 비교

파이썬에서는 문자열을 비교하여 결과로 참/거짓 값을 얻을 수 있으며, 결과는 불리언으로 평가된 마지막 변수를 반환합니다.


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
>>> string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
>>> non_null = string1 or string2 or string3
>>> # 'Trondheim'가 출력됩니다. 첫 번째로 true를 반환한 OR 연산자
>>> print(non_null)
'Trondheim'


>>> string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
>>> non_null = string1 and string2 and string3
>>> # 빈 문자열이 하나라도 있으면 아무것도 출력되지 않습니다
>>> print(non_null) 


>>> string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
>>> non_null = string1 and string2 and string3
>>> # 'Hammer Dance'가 출력됩니다. true를 반환하는 마지막 AND 연산자
>>> print(non_null)
Hammer Dance
```

# 입력과 출력

## “=”를 사용하여 print 출력 결과 포맷팅하기

```js
>>> bugs = 'roaches'
>>> count = 13
>>> area = 'living room'
>>> print(f'Debugging {bugs=} {count=} {area=}')
Debugging bugs='roaches' count=13 area='living room'
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

# 클래스

## 데이터 멤버는 외부에서 생성할 수 있습니다

데이터 멤버는 클래스 외부에서 생성하고 삭제할 수 있습니다.

```js
>>> class MyClass:
...    """간단한 예제 클래스"""
...    def f(self):
...        return 'hello world'

>>> x = MyClass()        
>>> x.counter = 1
>>> print(x.counter)
1

>>> del x.counter

>>> try:
...    print(x.counter)
... except:
...    print("x.counter가 더 이상 존재하지 않습니다")
x.counter가 더 이상 존재하지 않습니다
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

이 도움이 되었으면 좋겣습니다! AI를 배우기 시작할 수 있는 프로그래밍 언어인 Python을 즐겁게 배우세요!