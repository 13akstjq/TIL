---
title: "초고속 언어 Mojo 간단 소개  Python의 슈퍼셋"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-AQuickIntroductiontoMojoaSupersetofPythonThatIsSuperFast_0.png"
date: 2024-07-14 20:18
ogImage: 
  url: /TIL/assets/img/2024-07-14-AQuickIntroductiontoMojoaSupersetofPythonThatIsSuperFast_0.png
tag: Tech
originalTitle: "A Quick Introduction to Mojo  a Superset of Python That Is Super Fast"
link: "https://medium.com/towards-data-science/a-quick-introduction-to-mojo-a-superset-of-python-that-is-super-fast-079c619036a7"
---



![Mojo Language](/TIL/assets/img/2024-07-14-AQuickIntroductiontoMojoaSupersetofPythonThatIsSuperFast_0.png)

2023년에 등장한 Mojo라는 새로운 언어가 눈에 띄게 화제가 되었습니다. Swift 프로그래밍 언어의 원조 아키텍트 인 크리스 라트너에 의해 만들어졌습니다. Mojo는 Python의 상위 집합으로 설계되었습니다. 구문은 Python과 매우 유사하여 Python 개발자들에게 매우 친숙합니다.

Mojo는 컴파일 언어이며 Python보다 훨씬 빠릅니다. 특히 ML/AI 관련 계산을 위해 최적화되어 있어 일반 Python 개발자들에게 흥미롭고 도움이 될 수 있습니다.

이 게시물에서는 Mojo의 매우 기본 개념을 소개하고 Python과 비교합니다. Python과 비교했을 때 Mojo의 구문과 효율성에 놀라실 것입니다.


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

## 왜 모조?

Mojo는 현재 AI 회사 모듈러(Modular)에서 개발한 프로프리어터리(소유권이 있는) 프로그래밍 언어로, 복잡성을 제거하고 AI 개발에 유연성과 속도를 더하는 확장 가능하고 통합된 AI 플랫폼을 구축하는 것을 목표로 합니다. 그리고 이 플랫폼을 위해 개발된 새로운 언어인 Mojo는 AI 인프라 확장 및 가속화 문제를 해결하기 위해 개발되었습니다. 이 언어는 현대 CPU 및 GPU를 포함한 이종 하드웨어에 이상적인 컴파일러 인프라인 MLIR로 구축된 첫 번째 프로그래밍 언어입니다. 요약하자면, Mojo는 AI를 위한 새로운 언어입니다.

## Mojo와 Python

Mojo는 Python의 슈퍼셋(또는 고급 버전)으로 디자인되었으며, TypeScript가 JavaScript의 슈퍼셋인 것과 유사합니다. 이것은 JavaScript를 크게 향상시키는 것처럼 Python 개발자들이 Mojo를 시작하는 것이 매우 쉽다는 것을 의미합니다.

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

파이썬은 머신 러닝/인공지능 및 기타 많은 분야에서 주목받는 언어입니다. 특히 데이터 관련 분야에서는 파이썬이 많이 사용됩니다. 그러나 파이썬은 저수준 성능과 동시성을 위한 글로벌 인터프리터 잠금(GIL) 등의 문제가 있습니다. 이러한 문제들은 오랫동안 파이썬 개발자들에게 머리아픈 문제였으며, 사람들이 파이썬이 느리고 동시성이 떨어진다고 말하는 부끄러운 이유입니다. 이는 또한 파이썬 개발을 제한시키는 중요한 사실이며, 이로 인해 파이썬이 모든 핵심 기능이 C와 같은 더 효율적인 컴파일된 언어로 개발된 '접착제 언어'로 끝나게 되었습니다.

Mojo는 파이썬의 인기와 간편함과 C 및 Rust와 같은 컴파일된 언어의 효율성 사이의 간극을 메우기 위해 디자인되었습니다. Mojo는 활발하게 파이썬 커뮤니티를 수용하고, 컴파일 및 메모리 관리와 같은 모든 복잡성을 숨겨 효율적인 코드 실행을 위해 배경에서 발생하는 작업을 쉽고 쉽게 만들려고 노력합니다.

이 게시물에서는 Mojo의 기본 사항을 직접 Python과 비교하여 안내합니다. 변수, 함수 및 구조체와 같은 언어 기본 사항을 소개할 것입니다. 마지막으로, 동일한 코드 조각에 대한 Mojo와 Python의 효율성을 비교하고 Python 코드를 Mojo에서 직접 실행하는 방법을 알려드릴 것입니다. 그러면 이 새로운 언어에 대해 더 나은 이해를 얻을 수 있고, 귀하의 특정 영역에서의 사용 가능성을 확인할 수 있을 것입니다.

## Mojo 설치

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

이 명령어를 사용하여 Linux 컴퓨터에 Mojo를 설치할 수 있어요:

```js
curl https://get.modular.com | sh - && \
modular auth mut_73b76eabd7a04555be4daa751d3e7088

modular install mojo
```

다른 플랫폼에 대한 지침은 여기서 찾을 수 있어요.

설치가 완료되면 콘솔에서 Mojo를 설정하는 지침이 표시될 거예요:

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


![Mojo Introduction 1](/TIL/assets/img/2024-07-14-AQuickIntroductiontoMojoaSupersetofPythonThatIsSuperFast_1.png)

Copy and run the corresponding commands for your shell so you can start using Mojo:

![Mojo Introduction 2](/TIL/assets/img/2024-07-14-AQuickIntroductiontoMojoaSupersetofPythonThatIsSuperFast_2.png)

The `print` command works the same as in Python, and we've just written our first "Hello World" code in Mojo!


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

모조 REPL을 종료하려면 Python의 quit()이나 iPython의 exit과 달리 콜론(:)을 누르고 quit를 입력해야 합니다.

## 변수

Python과 비슷하게, Mojo에서도 이름과 값을 가진 변수를 만들 수 있습니다:

```js
  1> x = 1 
  2.  
(Int) x = 1
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

다음과 같이 선언된 변수는 변할 수 있으므로, mutable합니다:

```js
  2> x = 2 
  3. print(x) 
  4.  
2
```

그러나 Mojo는 엄격한 유형을 가지고 있기 때문에 변수의 유형을 변경할 수 없습니다:

```js
  4> x = "a" 
  5.  
[사용자] 오류: 표현식 [2]:1:5: 대입 중에 'StringLiteral' 값을 'Int'으로 암시적으로 변환할 수 없습니다
x = "a"
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

테이블 태그를 마크다운 형식으로 변경하십시오.

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
  1> let x = 1 
  2. x = 2 
  3. print(x) 
  4.  
[User] error: Expression [0]:2:1: expression must be mutable in assignment
x = 2
```

Mojo는 현재 Python의 "상위집합"으로 설계되었지만 아직 완전히 그렇지는 않습니다. 따라서 Python처럼 보이지만 때로는 문법이 다를 수 있습니다. 예를 들어, 유형 주석을 위한 키워드가 다릅니다:

```js
  1> var x: Int = 1 
  2. var y: String = "a" 
  3. var z: Bool = True
```

## 함수들


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

Mojo 함수는 fn 또는 def 키워드로 선언할 수 있습니다.

함수를 선언할 때 def를 사용하면 인수 유형과 반환 유형을 지정할 필요가 없고 이름과 값만으로 변수를 생성할 수 있습니다. 이것은 파이썬에서 할 수 있는 것과 정확히 동일합니다:

```js
def sum_up(n):
    sum = 0
    for i in range(1, n+1):
        sum += i

    return sum
```

fn 키워드를 사용하여 정의된 동일한 함수는:

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
fn sum_up(n: Int) -> Int:
    var sum: Int = 0
    for i in range(1, n+1):
        sum += i
    
    return sum
```

fn으로 정의된 함수는 엄격한 타입을 갖고 있으며 컴파일 시간에 함수가 올바른 타입을 입력 받고 반환하는지 확인합니다. def 키워드를 지원하는 것은 Python 사용자들이 Mojo의 구문을 이해하고 시작하기 쉽게 만들기 위한 것일 뿐입니다. Mojo 코드를 작성할 때 좋은 습관으로 간주되지는 않습니다. Mojo 코드를 더 읽어보면 대부분의 함수가 fn 키워드로 정의되어 있는 것을 보실 수 있습니다.

## Mojo 구조체

Mojo의 구조체는 Python의 클래스와 유사합니다. 둘 다 프로퍼티, 메서드, 데코레이터 등을 지원합니다.


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

파이썬에서 간단한 클래스를 정의해 봅시다:

```python
class Dog:
    def __init__(self, name):
        self.name = name

    def run(self):
        print(f"{self.name} is running.")

dog = Dog('Teddy')
dog.run()
```

Mojo에서 해당하는 구조체는 다음과 같습니다:

```python
struct Dog:
    var name: String

    fn __init__(inout self, name: String):
        self.name = name

    fn run(self):
        print(self.name, " is running.")

let dog = Dog('Teddy')
dog.run()
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

여기 몇 가지 주요 차이점이 있어요:

- 속성을 먼저 정의하고 생성자(__init__())에서 값을 할당해야 해요.
- __init__()의 첫 번째 인자는 관례상 self로 불리며, Python에서는 inout 키워드로 정의되어 있어요. 이는 self가 가변 참조임을 의미해요. 이것은 복잡한 개념이지만, 이 글에서는 그 정도까지 깊게 파고들 필요는 없어요.
- 아직 Mojo에서 f-string은 지원되지 않으므로 일반적인 프린팅을 사용해야 해요.

변수와 함수의 기본을 알면, 두 요소의 조합인 구조체의 구문을 배우는 것은 쉬워요. 그러나 Mojo의 구조체는 복잡한 개념이며, 더 깊이 파고들고 싶다면 Mojo의 모든 것을 다루고 있어요. 심지어 Int, String, Bool과 같은 기본 유형도 내부적으로는 구조체이며, 이것이 클래스/구조체와 같은 대문자로 시작하는 이유에요.

## Python과 Mojo 코드의 효율성 비교

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

알겠어요, 이제 Mojo의 매우 기본적인 구문을 알았으니, Python과 Mojo 코드의 효율성을 비교하고 최근 왜 많은 관심을 받고 있는지 살펴봅시다.

다음 코드를 sum_up.py라는 Python 파일에 넣어봅시다:

```python
def sum_up(n):
    sum = 0
    for i in range(1, n+1):
        sum += i
    
    return sum

def main():
    print(sum_up(1000_000_000))

if __name__ == "__main__":
    main()
```

그리고 이 코드를 sum_up.mojo라는 Mojo 파일에 넣어봅시다:

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
fn sum_up(n: Int) -> Int:
    var sum: Int = 0
    for i in range(1, n+1):
        sum += i
    
    return sum

fn main():
    print(sum_up(1000_000_000))
```

Mojo 파일의 경우, 진입 지점으로 main() 함수를 정의해야 하지만 호출할 필요는 없습니다.

Linux에서는 time 명령어를 사용하여 명령어의 실행 시간을 측정할 수 있습니다. 먼저 Mojo 파일을 실행해봅시다. 그렇지 않으면 Python 파일을 먼저 실행하면 인내심을 잃을 수 있습니다 😅:

```js
$ time mojo sum_up.mojo 
500000000500000000

real    0m0,134s
user    0m0,161s
sys     0m0,004s
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

테이블 태그를 Markdown 형식으로 변경하였습니다.

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

Mojo는 컴파일된 언어이고 Python은 해석된 언어이기 때문에 이유가 달라요. 컴파일된 언어에서 동일한 코드는 Python에서보다 100배 빠를 수 있다는 것은 놀라운 일이 아닙니다. 그러나 Python과 구문이 매우 유사하고 Python 사용자에게 친숙하기 때문에 꽤 놀라운 것입니다. 만약 우리 모든 데이터 처리 및 ML/AI 코드가 코드를 최소한으로 변경하면서 100배 빠르다면 환상적일 것입니다.

## Mojo에서 Python 코드 직접 실행

장기적으로, Mojo가 성숙해지고 Python의 실제 상위 집합이되면 바로 Mojo를 사용하고 Python 코드를 Mojo에서 직접 실행할 수 있어야 합니다. 마치 TypeScript에서 유효한 JavaScript 코드를 작성할 수 있는 것처럼요.

그러나 현재는 Mojo에서 Python 모듈을 가져와서 Mojo 코드에서 Python 함수를 호출하고 Python 객체와 상호 작용할 수 있습니다. 위의 sum_up.py 모듈을 생성하고 Mojo에서 바로 실행하는 call_python_in_mojo.mojo라는 새 파일을 만들어 보죠.

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
Python 모듈이 Mojo에서 찾을 수 있도록 경로를 추가해야합니다. 다음 명령어로 이 코드를 실행할 수 있습니다:

time mojo call_python_in_mojo.mojo

Python을 직접 실행하는 것과 속도가 같다는 것을 알 수 있습니다. 조금 실망스러운 일이죠. 하지만 Mojo가 아직 초기 개발 중이고 많은 기능이 아직 구현되지 않았다는 것을 감안해야 합니다. 시간이 흐를수록 Python과의 사용성 및 통합이 훨씬 개선될 것으로 기대됩니다.
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

이 게시물에서는 Mojo라는 새로운 프로그래밍 언어의 매우 기초적인 부분을 소개했습니다. Mojo는 Python의 상위 집합으로 설계되어 Python과 매우 유사한 구문을 가지고 있습니다. 우리는 Mojo 코드의 간단한 구문과 놀라운 속도를 경험했는데, 이는 데이터 처리와 기계 학습에 매우 매력적일 수 있습니다.

그러나 Mojo는 아직 매우 새로운 언어이며 아직 완전히 성숙해지지 않았습니다. 보편적인 개발자들이 일할 준비가 되기까지 어느 정도 시간이 필요합니다. 그럼에도 불구하고, 이 언어를 계속 주시하고 AI의 새 시대를 놓치지 않기 위해 시간을 할애하는 가치가 있습니다. 그리고 준비가 되면 우리는 효율성을 향상시키기 위해 빠르게 전환할 수 있으며, 이는 게임 체인저 이벤트가 될 수도 있습니다.

## 관련 게시물:

- Python typing and validation with mypy and pedantic
- Python에서 쉘 명령어를 올바르게 실행하는 방법