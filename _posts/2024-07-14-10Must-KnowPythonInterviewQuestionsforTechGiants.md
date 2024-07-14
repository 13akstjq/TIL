---
title: "대기업 취업을 위한 필수 Python 면접 질문 10선"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-10Must-KnowPythonInterviewQuestionsforTechGiants_0.png"
date: 2024-07-14 23:47
ogImage: 
  url: /TIL/assets/img/2024-07-14-10Must-KnowPythonInterviewQuestionsforTechGiants_0.png
tag: Tech
originalTitle: "10 Must-Know Python Interview Questions for Tech Giants"
link: "https://medium.com/top-python-libraries/10-must-know-python-interview-questions-for-tech-giants-7b0ff126d3a2"
---


## TOP 10 PYTHON INTERVIEW QUESTIONS

회사나 역할에 따라 자주 나오는 인터뷰 질문이 다양하지만, 일반적으로 큰 기술 회사의 인터뷰에서는 기본 지식, 알고리즘, 시스템 디자인 질문이 흔합니다. 이 블로그에서는 대형 기술 회사 인터뷰에서 자주 나오는 10가지 질문을 다루고 있습니다.

# 1. Python의 특징은 무엇인가요?

- 배우기 쉽다
- 객체지향 프로그래밍
- 해석형 언어
- 강력한 표준 및 서드 파티 라이브러리
- 크로스 플랫폼

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

# 2. Python 코드는 어떻게 실행되나요?

- 해석기는 Python 코드를 읽고 구문 또는 형식 오류를 확인합니다.
- 오류가 없다면 해석기는 코드를 바이트 코드로 변환합니다.
- 이 바이트 코드는 Python 가상 머신에 실행을 위임합니다. 실행 중 오류가 발생하면 프로세스가 중단됩니다; 그렇지 않으면 결과가 표시됩니다.

![](/TIL/assets/img/2024-07-14-10Must-KnowPythonInterviewQuestionsforTechGiants_0.png)

# 3. 이터레이터와 제너레이터의 차이점은 무엇인가요?

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

- 반복자(Iterator): `next()` 작업을 지원하는 객체로, 일련의 요소를 포함합니다. `next()` 메서드는 요소를 하나씩 반환하고 모든 요소가 반환된 경우 `StopIteration`을 발생시킵니다. 반복자는 `__iter__()`와 `__next__()` 메서드를 구현합니다.
- 생성자(Generator): `yield` 키워드를 사용하여 생성자 객체를 반환하는 함수입니다. 이 객체는 실행을 일시 중단하고 다시 시작할 수 있습니다. 생성자에서 `next()`를 호출하면 `yield` 문에 도달할 때까지 실행이 계속되며, 생성자는 요청 시 값을 생성하여 메모리를 절약하고 효율성을 높일 수 있습니다.
- 차이점: 생성자는 함수 및 `yield` 키워드를 사용하여 정의된 특별한 반복자로, 내장 `iter()` 함수로 생성된 표준 반복자와 달리 호출 가능한 일반 함수이며 값들을 순차적으로 생성합니다.

# 4. `yield`를 사용하는 방법

- `yield`는 함수에서 생성자를 반환하는 Python 키워드입니다. 생성자는 모든 데이터를 한 번에 메모리에 저장하지 않기 때문에 대규모 데이터나 복잡한 구조에 유용한 지연 계산을 허용합니다.
- `yield`를 포함하는 함수를 **생성자 함수(Generator function)**라고 합니다. 값의 시리즈를 생성하고 각 호출 시 이전 상태에서 실행을 계속합니다.

```js
# 원래 방법: 대형 목록은 많은 메모리를 사용합니다
def create_numbers(n):
    num_list = []
    for i in range(n):
        num_list.append(i)
    return num_list

# yield 사용: 메모리를 효율적으로 사용하는 데이터 스트림 생성
def create_numbers(n):
    for i in range(n):
        yield i

nums = create_numbers(5)
for num in nums:
    print(num)
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

# 5. 파이썬2와 파이썬3의 차이점은 무엇인가요?

- 정수 타입: 파이썬2는 int와 long을 구분하지만, 파이썬3는 구분하지 않습니다.
- 문자열 포매팅: 파이썬3는 포매팅을 위해 f-문자열을 도입했습니다.
- 정수 나눗셈: 파이썬2는 정수 나눗셈을 수행하고, 파이썬3는 부동 소수점을 반환합니다.
- 인코딩: 파이썬2는 기본적으로 ASCII를 사용하며, 파이썬3는 유니코드를 사용합니다.
- 출력문: 파이썬2는 `print`를 사용하고, 파이썬3는 `print()`를 사용합니다.
- `range()` 함수: 파이썬2는 리스트를 반환하지만, 파이썬3는 이터레이터를 반환합니다.
- `input()`과 `raw_input()`: 파이썬2는 문자열에 대해 `raw_input()`을 사용하고, 파이썬3는 `input()`을 사용합니다.

# 6. 파이썬은 어떻게 메모리를 관리하나요?

참조 카운팅과 가비지 콜렉션:

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

- 참조 카운팅은 객체 참조를 추적합니다. 카운트가 제로에 도달하면 객체가 삭제됩니다.
- 가비지 컬렉션은 순환 참조를 처리하며, 객체를 수명에 따라 세대로 나누고 그에 따라 쓰레기를 수거합니다.

이러한 메커니즘들은 Python이 메모리를 자동으로 관리할 수 있게 하며, 개발자들이 메모리 세부사항에 집중하는 대신 논리에 집중할 수 있게 합니다.

# 7. 깊은 복사와 얕은 복사의 차이는 무엇인가요?

- 얕은 복사: 객체의 외부 레이어 멤버(기본 데이터 유형 및 문자열)를 복사합니다. 내부 멤버(중첩된 객체 또는 참조)는 참조에 의해 복사되어 새로운 인스턴스를 생성하지 않습니다. 내부 멤버의 변경은 원본 및 복사된 객체 양쪽에 영향을 줍니다.
- 깊은 복사: 전체 객체를 재귀적으로 복사하며, 모든 내부 멤버와 중첩된 객체를 포함한 새로운 메모리에 새 인스턴스를 생성합니다. 원본 객체의 내부 멤버를 변경해도 복사된 객체에 영향을 미치지 않습니다.

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

# 8. 클로저란 무엇인가요?

클로저는 내부 함수가 외부 함수의 변수에 의존하고, 외부 함수가 내부 함수를 반환할 때 생성됩니다. 이는 외부 함수가 실행을 마친 후에도 내부 함수가 이러한 변수에 액세스를 유지할 수 있게 합니다.

```js
def out_function(y):
    def in_function(x):
        return x + y
    return in_function

f = out_function(5)
print(f(3))  # 8
```

# 9. `is not`과 `!=`의 차이점은 무엇인가요?

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

- `!=`: 두 객체가 서로 다른 값을 가지는지 확인합니다.
- `is not`: 두 객체가 메모리에서 서로 다른지 확인합니다.

# 10. 람다 함수란 무엇인가요?

람다 함수 또는 익명 함수는 이름이 없는 한 줄짜리 함수입니다. 여러 매개변수를 가질 수 있지만 표현식은 하나뿐입니다.

최신 AI 이야기를 탑재하려면 Substack에서 저희와 연락을 유지하세요. 함께 AI의 미래를 함께 만들어 봅시다!

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

파이썬 이야기를 업데이트 받으려면 Substack에서 우리와 함께하세요. 함께 파이썬을 배워봐요!