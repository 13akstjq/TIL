---
title: "모든 개발자가 알아야 할 고급 파이썬 개념 9가지"
description: ""
coverImage: "/TIL/assets/img/2024-07-15-9AdvancedPythonConceptsEveryDeveloperShouldUnderstand_0.png"
date: 2024-07-15 00:03
ogImage: 
  url: /TIL/assets/img/2024-07-15-9AdvancedPythonConceptsEveryDeveloperShouldUnderstand_0.png
tag: Tech
originalTitle: "9 Advanced Python Concepts Every Developer Should Understand"
link: "https://medium.com/@learntocodetoday/9-advanced-python-concepts-every-developer-should-understand-ffc59b0b9b69"
---



![이미지](/TIL/assets/img/2024-07-15-9AdvancedPythonConceptsEveryDeveloperShouldUnderstand_0.png)

Python은 간단하고 가독성이 좋아 초보자들 사이에서 인기가 높습니다. 그러나 Python의 전체 잠재력을 활용하기 위해 개발자들은 일부 고급 개념을 숙달해야 합니다. 여기 개발자들이 더 효율적이고 유지보수 가능하며 확장 가능한 코드를 작성하기 위해 이해해야 하는 9가지 고급 Python 개념이 소개됩니다.

# 1. 제너레이터와 이터레이터

제너레이터와 이터레이터를 사용하면 게으른(lazy) 평가가 가능하며, 이는 메모리를 절약하고 성능을 향상시킬 수 있습니다. 제너레이터는 함수와 yield 문을 사용하여 이터레이터를 만드는 간단한 방법입니다.


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

# 이터레이터

이터레이터는 이터레이터 프로토콜을 구현한 객체로, __iter__()와 __next__() 메서드로 구성됩니다.

```js
class MyIterator:
    def __init__(self, data):
        self.data = data
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index >= len(self.data):
            raise StopIteration
        value = self.data[self.index]
        self.index += 1
        return value

my_iter = MyIterator([1, 2, 3])
for item in my_iter:
    print(item)
```

# 제너레이터

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

생성기는 반복자를 쉽게 만드는 방법을 제공합니다.

```js
def my_generator():
    yield 1
    yield 2
    yield 3

gen = my_generator()
for item in gen:
    print(item)
```

# 2. 데코레이터

데코레이터는 함수 또는 클래스의 동작을 수정하는 강력한 기능입니다. 주로 로깅, 접근 제어, 측정 및 캐싱에 사용됩니다.

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
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("함수 호출 전")
        result = func(*args, **kwargs)
        print("함수 호출 후")
        return result
    return wrapper

@my_decorator
def say_hello():
    print("안녕!")

say_hello()
```

# 3. 컨텍스트 매니저

컨텍스트 매니저는 원하는 시점에 정확히 리소스를 할당하고 해제할 수 있도록 해줍니다. 가장 흔한 컨텍스트 매니저의 사용 방법은 `with` 문과 함께 사용하는 것입니다.

```python
class MyContextManager:
    def __enter__(self):
        print("컨텍스트 진입")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print("컨텍스트 종료")

with MyContextManager():
    print("컨텍스트 내부")
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

파일 처리는 일반적인 사용 사례입니다.

```python
with open('file.txt', 'w') as file:
    file.write('Hello, World!')
```

## 4. 메타클래스

메타클래스는 클래스의 동작을 정의하는 방법입니다. 클래스 속성이나 메서드를 정의할 때 클래스를 수정하여 클래스 생성을 사용자 정의할 수 있게 합니다.

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
class Meta(type):
    def __new__(cls, name, bases, attrs):
        attrs['greet'] = lambda self: f"Hello from {self.__class__.__name__}"
        return super().__new__(cls, name, bases, attrs)

class MyClass(metaclass=Meta):
    pass

obj = MyClass()
print(obj.greet())
```

# 5.  디스크립터

디스크립터는 클래스 내에서 관리 속성을 만드는 방법입니다. 속성에 대한 액세스, 설정 및 삭제 동작을 정의할 수 있도록 합니다.

```js
class MyDescriptor:
    def __init__(self, name=None):
        self.name = name

    def __get__(self, instance, owner):
        return instance.__dict__.get(self.name)

    def __set__(self, instance, value):
        instance.__dict__[self.name] = value

    def __delete__(self, instance):
        del instance.__dict__[self.name]

class MyClass:
    attr = MyDescriptor('attr')

obj = MyClass()
obj.attr = 42
print(obj.attr)
del obj.attr
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

# 6. 코루틴과 Asyncio

코루틴과 asyncio 모듈을 사용하면 Python에서 비동기 프로그래밍을 할 수 있어요. 이를 통해 블로킹 되지 않는 I/O 작업을 수행할 수 있는 코드를 작성할 수 있어요.

## 코루틴

코루틴은 실행을 일시 중지하고 다시 시작할 수 있는 함수들이에요.

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
import asyncio

async def my_coroutine():
    print("Start coroutine")
    await asyncio.sleep(1)
    print("End coroutine")

asyncio.run(my_coroutine())
```

# Asyncio

Asyncio는 async/await 구문을 사용하여 동시성 코드를 작성하기 위한 라이브러리입니다.

```js
async def main():
    await asyncio.gather(my_coroutine(), my_coroutine())

asyncio.run(main())
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

# 7. 타입 주석

타입 주석은 변수, 함수 매개변수 및 반환 값을 정의하여 코드 가독성을 향상시키고 오류를 초기에 잡아낼 수 있습니다.

```python
def greet(name: str) -> str:
    return f"Hello, {name}"

def add(a: int, b: int) -> int:
    return a + b

name: str = "Alice"
age: int = 30
```

# 8. 슬롯

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

인스턴스 속성을 미리 정의된 목록으로 제한하여 메모리를 절약하고 속성 액세스 속도를 높일 수 있는 슬롯은 유용합니다.

```python
class MyClass:
    __slots__ = ['name', 'age']

    def __init__(self, name, age):
        self.name = name
        self.age = age

obj = MyClass('Alice', 30)
print(obj.name, obj.age)
```

# 9. Multiprocessing

multiprocessing 모듈을 사용하면 별도의 프로세스를 생성하고 관리할 수 있어 Python 애플리케이션에서 진정한 병렬 처리를 수행할 수 있습니다.

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
from multiprocessing import Process

def worker(num):
    print(f'Worker: {num}')

if __name__ == '__main__':
    processes = []
    for i in range(5):
        p = Process(target=worker, args=(i,))
        processes.append(p)
        p.start()

    for p in processes:
        p.join()
```

이러한 고급 Python 개념을 습득함으로써 더 효율적이고 유지보수가 쉬운 확장 가능한 코드를 작성할 수 있습니다. 이러한 개념은 코딩 기술뿐만 아니라 더 복잡한 문제와 프로젝트에 자신감을 갖고 접근할 수 있도록 도와줍니다. 즐거운 코딩하세요!