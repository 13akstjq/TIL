---
title: "코드 효율성을 높이는 7가지 파이썬 메모리 최적화 비법"
description: ""
coverImage: "/TIL/assets/no-image.jpg"
date: 2024-07-14 23:44
ogImage: 
  url: /TIL/assets/no-image.jpg
tag: Tech
originalTitle: "7 Python Memory Optimization Tricks To Enhance Your Codes Efficiency"
link: "https://medium.com/techtofreedom/7-python-memory-optimization-tricks-to-enhance-your-codes-efficiency-5ef65bf415e7"
---


## 파이썬

프로젝트가 점점 커지면서, 컴퓨팅 자원을 효율적으로 관리하는 것은 불가피한 요구사항입니다.

안타깝게도, 특히 C나 C++과 같은 저수준 언어와 비교할 때 파이썬은 메모리 효율적이지 못한 것으로 보입니다.

지금 프로그래밍 언어를 바꾸는 것이 좋을까요?

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

물론이죠.

사실, 파이썬 프로그램의 메모리 사용량을 최적화하는 많은 방법이 있습니다. 훌륭한 모듈과 도구부터 고급 데이터 구조와 알고리즘까지 다양한 방법이 있어요.

이 기사는 파이썬의 내장 메커니즘에 초점을 맞추고 7가지 기본적이지만 효과적인 메모리 최적화 요령을 소개할 거에요. 이들을 숙달하면 파이썬 프로그래밍 스킬이 상당히 향상될 거예요.

# 1. 클래스 정의에서 __slots__ 사용하기

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

파이썬은 동적 타이핑 언어로 객체 지향 프로그래밍에 있어 유연성을 가지고 있습니다. 좋은 예시로 파이썬 클래스에 런타임에서 추가적인 속성과 메서드를 넣는 기능이 있습니다.

예를 들어, 아래 코드는 'Author'라는 클래스를 정의합니다. 원래 'name'과 'age'라는 두 속성이 있었지만 나중에 손쉽게 하나를 더 추가할 수 있습니다:

```python
class Author:
    def __init__(self, name, age):
        self.name = name
        self.age = age

me = Author('Yang Zhou', 30)
me.job = 'Software Engineer'
print(me.job)
# Software Engineer
```

그러나 동전에는 양면이 있다고 하죠. 이 유연성은 내부에서 더 많은 메모리를 낭비합니다.

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

파이썬의 클래스의 모든 인스턴스는 인스턴스 변수를 저장하는 특별한 사전(__dict__)을 유지합니다. 이 사전은 해시 테이블 기반의 구현 때문에 메모리 효율이 좋지 않기 때문에 상당한 양의 메모리를 소비합니다.

대부분의 경우에는 인스턴스의 변수나 메서드를 런타임에 변경할 필요가 없으며, 클래스 정의 이후에는 __dict__가 변경되지 않습니다. 따라서 __dict__ 사전을 유지하는 것을 피하는 것이 좋습니다.

파이썬은 이를 위해 마법처럼 동작하는 속성을 제공합니다: __slots__.

이는 클래스의 모든 유효한 속성 이름을 지정하여 화이트리스트로 작동합니다:

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

```python
class Author:
    __slots__ = ('name', 'age')

    def __init__(self, name, age):
        self.name = name
        self.age = age

me = Author('양주', 30)
me.job = '소프트웨어 엔지니어'
print(me.job)
# AttributeError: 'Author' object has no attribute 'job'
```

위의 코드에서 보듯이, 런타임 중에 job 속성을 더 이상 추가할 수 없습니다. 왜냐하면 __slots__ 화이트리스트에서는 name과 age 속성만을 정의했기 때문입니다.

이론적으로 속성이 지금 고정되어 있기 때문에, Python은 더 이상 이를 위한 사전을 유지할 필요가 없습니다. __slots__에서 정의된 속성에 필요한 메모리 공간만 할당하면 됩니다.

이것이 실제로 작동하는지 여부를 확인하기 위해 간단한 비교 프로그램을 작성해 봅시다:


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
import sys

class Author:
    def __init__(self, name, age):
        self.name = name
        self.age = age

class AuthorWithSlots:
    __slots__ = ['name', 'age']

    def __init__(self, name, age):
        self.name = name
        self.age = age

# Creating instances
me = Author('Yang', 30)
me_with_slots = AuthorWithSlots('Yang', 30)

# Comparing memory usage
memory_without_slots = sys.getsizeof(me) + sys.getsizeof(me.__dict__)
memory_with_slots = sys.getsizeof(me_with_slots)  # __slots__ classes don't have __dict__

print(memory_without_slots, memory_with_slots)
# 152 48
print(me.__dict__)
# {'name': 'Yang', 'age': 30}
print(me_with_slots.__dict__)
# AttributeError: 'AuthorWithSlots' object has no attribute '__dict__'
```

위 코드에서 볼 수 있듯이, __slots__를 사용하기 때문에 me_with_slots 인스턴스는 __dict__ 사전이 없습니다. 이는 me 인스턴스와 비교하여 추가 사전을 유지해야 하는 리소스를 효과적으로 절약합니다.

# 2. Generators 사용

Generators는 Python에서 리스트의 게으른 평가 버전입니다.


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

그들은 요소를 생성하는 공장처럼 작동합니다: 모든 항목을 한꺼번에 계산하는 대신 next() 메서드가 호출될 때마다 항목을 생성합니다.

그렇기 때문에 대규모 데이터셋을 처리할 때 매우 메모리를 효율적으로 사용합니다.

```js
def number_generator():
    for i in range(100):
        yield i

numbers = number_generator()
print(numbers)
# <generator object number_generator at 0x104a57e40>
print(next(numbers))
# 0
print(next(numbers))
# 1
```

위 코드는 제너레이터를 작성하고 사용하는 기본적인 예제를 보여줍니다. yield 키워드가 제너레이터의 정의 핵심입니다. 이를 적용하면 항목 i는 next() 메서드가 호출될 때에만 생성됩니다.

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

이제 제너레이터와 리스트를 비교해보면 어떤 쪽이 더 메모리를 효율적으로 사용하는지 살펴봅시다:

```python
import sys

numbers = []
for i in range(100):
    numbers.append(i)

def number_generator():
    for i in range(100):
        yield i

numbers_generator = number_generator()
print(sys.getsizeof(numbers_generator))
# 112
print(sys.getsizeof(numbers))
# 920
```

위 프로그램의 결과는 제너레이터를 사용하면 메모리 사용량을 크게 절약할 수 있다는 사실을 입증합니다.

그런데, 리스트 컴프리헨션의 대괄호를 괄호로 변환하면 제너레이터 표현식이 됩니다. 이것이 파이썬에서 제너레이터를 정의하는 더 간단한 방법입니다:

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

```python
import sys

numbers = [i for i in range(100)]
numbers_generator = (i for i in range(100))

print(sys.getsizeof(numbers_generator))
# 112
print(sys.getsizeof(numbers))
# 920
```

# 3. 대용량 파일 처리를 위한 메모리 맵 파일 지원 활용

메모리 맵 파일 I/O, 줄여서 "mmap"이라고도 불리는 것은 OS 수준의 최적화입니다.

간단히 말해서, mmap 기술을 사용하여 파일을 메모리 맵핑하면 현재 프로세스의 가상 메모리 공간에 파일의 맵핑이 직접 생성되어 파일 전체를 메모리에 로드하는 대신 처리합니다.


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

테이블 태그를 Markdown 형식으로 변경할 수 있습니다.

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

```python
import mmap

with open('test.txt', "r+b") as f:
    # 파일을 메모리 매핑하여 사용할 수 있습니다. 크기 0은 전체 파일을 의미합니다.
    with mmap.mmap(f.fileno(), 0) as mm:
        # 표준 파일 메서드를 사용하여 내용 읽기
        print(mm.read())
        # 슬라이스 표기법을 사용하여 내용 읽기
        snippet = mm[0:10]
        print(snippet.decode('utf-8'))
```

위에서 보여진 대로, Python은 메모리 매핑된 파일 I/O 기술의 사용을 편리하게 만듭니다. 우리가 해야 할 일은 단지 mmap.mmap() 메서드를 적용하고, 표준 파일 메서드나 슬라이싱 표기법을 통해 개방된 객체를 처리하는 것뿐입니다.

# 4. 전역 변수 사용 최소화

전역 변수는 글로벌 스코프를 갖기 때문에 프로그램이 실행되는 동안 메모리에 유지됩니다.


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

전역 변수가 큰 데이터 구조를 보유하는 경우 프로그램 수명 전체 동안 메모리를 점유하므로 비효율적인 메모리 사용으로 이어질 수 있습니다.

파이썬 코드에서 전역 변수 사용을 최소화해야 합니다.

# 5. 논리 연산자의 단축 평가 활용

이 꿀팁은 섬세해 보일 수 있지만, 스마트하게 사용하면 프로그램의 메모리 사용량을 중대하게 절약할 수 있습니다.

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

예를 들어, 반환된 boolean 값에 따라 최종 결과를 얻는 간단한 코드 스니펫이 있습니다:

```js
result_a = expensive_function_a()
result_b = expensive_function_b()
result = result_a if result_a else result_b
```

위의 코드는 동작하지만 실제로 두 가지 메모리 비효율적인 함수를 실행합니다.

동일한 결과를 얻는 더 스마트한 방법은 다음과 같습니다:

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
result = expensive_function1() or expensive_function2()
```

위 코드의 논리 연산자는 단락 평가 규칙을 따르기 때문에, expensive_function1()이 True 인 경우, 위 코드의 expensive_function2()는 실행되지 않습니다. 이는 불필요한 메모리 사용을 절약할 수 있습니다.

# 6. 데이터 유형 신중히 선택하기

고급 Python 개발자는 데이터 유형을 신중하고 정확하게 선택할 것입니다.

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

특정 상황에서는 다른 데이터 유형을 사용하는 것이 다른 것보다 더 메모리를 효율적으로 사용합니다.

## 튜플은 리스트보다 메모리를 효율적으로 사용합니다

튜플은 변경할 수 없는(생성 후에 변경할 수 없음) 특성 때문에 Python은 메모리 할당 측면에서 최적화를 할 수 있습니다.

하지만, 리스트는 변경 가능하기 때문에 잠재적인 수정 사항을 수용하기 위해 추가 공간이 필요합니다.

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
import sys

my_tuple = (1, 2, 3, 4, 5)
my_list = [1, 2, 3, 4, 5]

print(sys.getsizeof(my_tuple))
# 80
print(sys.getsizeof(my_list)) 
# 120
```

위 코드 조각에서 보듯이, 같은 요소를 포함하고 있어도 튜플인 my_tuple이 리스트보다 적은 메모리를 사용합니다.

따라서, 데이터를 생성한 후에 변경할 필요가 없다면 리스트 대신 튜플을 사용하는 것이 좋습니다.

## 배열은 리스트보다 메모리를 더 효율적으로 사용합니다.

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

파이썬의 배열은 요소들이 동일한 데이터 유형이어야 합니다(예: 모두 정수 또는 모든 부동 소수점 수), 그러나 리스트는 다른 유형의 객체를 저장할 수 있기 때문에 더 많은 메모리가 필요합니다.

따라서 배열을 사용하면 리스트 요소가 모두 동일한 유형인 경우 더 메모리 효율적입니다:

```python
import sys
import array

my_list = [i for i in range(1000)]

my_array = array.array('i', [i for i in range(1000)])

print(sys.getsizeof(my_list))  
# 8856
print(sys.getsizeof(my_array)) 
# 4064
```

## 우수한 데이터 과학 모듈이 내장 데이터 유형보다 효율적입니다

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

파이썬은 데이터 과학의 주요 언어입니다. NumPy와 Pandas와 같은 강력한 타사 모듈과 도구들이 더 많은 데이터 유형을 제공합니다.

만약 NumPy가 제공하는 많은 기능이 필요하지 않고 단순한 1차원 숫자 배열만 필요하다면, 파이썬의 내장 배열이 좋은 선택일 수 있습니다.

하지만 복잡한 행렬 조작이 필요한 경우에는, NumPy가 제공하는 배열을 사용하는 것이 모든 데이터 과학자에게 첫 번째이자 아마도 최상의 선택일 것입니다.

# 7. 동일한 문자열에 문자열 인터닝 기술을 적용

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

많은 개발자들을 혼란스럽게 만들 수 있는 코드는 다음과 같습니다:

```js
a = 'Y'*4096
b = 'Y'*4096
a is b
True
c = 'Y'*4097
d = 'Y'*4097
c is d
False
```

is 연산자는 두 변수가 메모리에서 동일한 객체를 참조하는지 확인하는 데 사용됩니다. 이는 == 연산자와는 다릅니다. == 연산자는 두 객체의 값을 비교합니다.

그래서 a is b는 True를 반환하지만, c is d는 False를 반환하는 이유는 무엇인가요?

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

파이썬에는 숨겨진 보석이 있습니다 — 문자열 인터닝 기술이 그것입니다.

값이 같은 몇 개의 작은 크기 문자열들이 있다면, 파이썬은 암묵적으로 그들을 인터닝하고 메모리에서 동일한 객체를 참조합니다.

작은 문자열을 정의하는 마법 숫자는 4096입니다.

c와 d의 길이가 4097이므로, 두 개의 객체가 메모리에 있기 때문에, 암묵적인 문자열 인터닝은 일어나지 않습니다. 따라서 c is d를 실행하면 False를 얻습니다.

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

문자열 인터닝은 메모리 사용량을 최적화하는 강력한 기술입니다. 명시적으로 수행하려면 sys.intern() 메서드를 활용해야 합니다:

```js
>>> import sys
>>> c = sys.intern('Y'*4097)
>>> d = sys.intern('Y'*4097)
>>> c is d
True
```

그리고, 문자열 인터닝 외에도 Python은 작은 정수에 대해서도 인터닝 기법을 적용합니다. 이를 메모리 최적화 목적으로 활용할 수도 있습니다.

독서해 주셔서 감사합니다. ❤️ 마음에 드셨다면, 연결하고 싶습니다:

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

X | Linkedin | Medium 

파이썬 메모리 관리 메커니즘에 관한 인터뷰 질문: