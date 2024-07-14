---
title: "당신이 놓쳤을 7가지 미친 파이썬 이스터 에그"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-7CrazyPythonEasterEggsYouProbablyHaventSeen_0.png"
date: 2024-07-14 20:36
ogImage: 
  url: /TIL/assets/img/2024-07-14-7CrazyPythonEasterEggsYouProbablyHaventSeen_0.png
tag: Tech
originalTitle: "7 Crazy Python Easter Eggs You Probably Havent Seen"
link: "https://medium.com/pythoneers/7-crazy-python-easter-eggs-you-probably-havent-seen-025f4e23c813"
---


![이미지](/TIL/assets/img/2024-07-14-7CrazyPythonEasterEggsYouProbablyHaventSeen_0.png)

Python은 기술 세계에서 가장 인기 있는 프로그래밍 언어 중 하나로, 요약하면 코딩 왕국의 석탄왕입니다. Python은 사용하기 쉬운 구문과 다재다능성으로 개발자들의 마음 속에 특별한 자리를 꽂아왔습니다. 그러나 실용적인 응용 분야를 넘어 숨겨진 재미있는 이스터 에그들로 가득 찬 비밀의 놀이터가 기다리고 있습니다. 이 블로그에서는 아마 본 적이 없는 몇 가지 미친 Python 이스터 에그를 공유하려고 합니다.

## 1. .py 확장자는 크게 상관이 없어요 🐍

Python에 관한 모든 책, 모든 튜토리얼, 그리고 모든 기사는 Python 프로그램을 저장할 때 항상 .py 확장자를 사용해야 한다고 말할 것입니다. 이것은 사실이고, 항상 이 윤리적으로 해야 하지만, 이 기사는 Python 윤리에 관한 것이 아니라 이스터 에그에 관한 것입니다.

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


![image](https://miro.medium.com/v2/resize:fit:996/1*s-gIoYmgwo4NNlTBsTniZw.gif)

파이썬 파일을 저장할 때는 .cow, .cat 또는 .mango와 같은 모든 종류의 확장자를 사용할 수 있습니다. 스크립트가 유효하다면 원하는 출력을 제공하고 실행될 것입니다.

```js
# 간단한 CLI 계산기
# cli_calc.cow

def add(x, y):
  return x + y
def subtract(x, y):
    return x - y
def multiply(x, y):
    return x * y
def divide(x, y):
    if y == 0:
        return "오류: 0으로 나눌 수 없습니다!"
    else:
        return x / y

print("연산을 선택하세요:")
print("1. 더하기")
print("2. 빼기")
print("3. 곱하기")
print("4. 나누기")

while True:
    choice = input("선택하세요 (1/2/3/4): ")
    if choice in ('1', '2', '3', '4'):
        num1 = float(input("첫 번째 숫자를 입력하세요: "))
        num2 = float(input("두 번째 숫자를 입력하세요: "))

        if choice == '1':
            print("결과:", add(num1, num2))
        elif choice == '2':
            print("결과:", subtract(num1, num2))
        elif choice == '3':
            print("결과:", multiply(num1, num2))
        elif choice == '4':
            print("결과:", divide(num1, num2))
        break
    else:
        print("유효하지 않은 입력입니다. 유효한 숫자를 입력하세요 (1/2/3/4).")
```

![image](https://miro.medium.com/v2/resize:fit:1200/1*bRH0ZnSKG-kaCv-gr8fonA.gif)


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

## 2. 파이썬에서 무한대는 도달 가능합니다

정말 그렇습니다.
파이썬에서는 float(`inf`)를 사용하여 양의 무한대를 나타내고, float(`-inf`)를 사용하여 음의 무한대를 나타낼 수 있습니다.

![이미지](https://miro.medium.com/v2/resize:fit:324/1*0KTkloaq_LLwl7esTS_dbQ.gif)

아래는 간단한 예제 사용 사례입니다...

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
def find_max(numbers):
    if not numbers:
        return float('-inf')  # 빈 리스트에는 음의 무한대를 반환합니다.
    else:
        max_value = float('-inf')  # max_value를 음의 무한대로 초기화합니다.
        for num in numbers:
            if num > max_value:
                max_value = num
        return max_value

# 예시 사용법:
numbers_list = [5, 9, 2, 11, 6]
print("최댓값:", find_max(numbers_list))

empty_list = []
print("빈 리스트의 최댓값:", find_max(empty_list))

----------------------------
최댓값: 11
```

## 3. The OG 😎

“Python의 기초”는 Python 프로그램을 작성하는 지침의 모음입니다. 이것들은 Tim Peters가 쓴 것으로 Python 해석기에 "이스터 에그"로 포함되어 있습니다. Python 인터프리터에서 import this를 입력하여 확인할 수 있습니다.

```js
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
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

PEP 20(Python Enhancement Proposal 20)은 팀 피터스가 저술한 Python의 철학인 Zen of Python을 처음 소개했습니다. 20개의 금언이라고 불리지만, 실제로는 PEP에 명시된 것은 19개 뿐입니다. 이 작은 차이는 적혀 있는 것과 인용된 것 사이의 흥미로운 측면 중 하나입니다. 

![image](https://miro.medium.com/v2/resize:fit:200/0*zT7m0MwnO2vf35uK.gif)

## 4. "한치의 여지도 없다"

Python에는 Python 2와 Python 3 모두와 호환성을 유지하기 위해 개발자가 사용하는 __future__ 패키지가 있습니다. 이를 통해 Python 2와 Python 3를 모두 지원하는 단일하고 깨끗한 Python 3.x 호환 코드베이스를 최소한의 오버헤드로 사용할 수 있습니다.

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

음... 좋아! 이 패키지에는 오류와 클래스에서 유머로 가득찬 은박이 숨겨져 있어요.

```js
from __future__ import braces

  File "<stdin>", line 1
SyntaxError: not a chance
```

미래에서 중괄호를 가져오려고 하면 '흥! 그럴 리가' 라는 재미있는 오류가 발생할 거에요. 이 부분은 Python이 C, Java, 또는 JavaScript와 같은 다른 프로그래밍 언어와 달리 중괄호 ''를 블록 구분에 사용하지 않는다는 사실을 유머스럽게 표현한 장난이나 은박이로 의도적으로 만들어졌어요.

## 5. Chinese As Var Names

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

네, 올바르게 이해하셨어요!!!

Python에서는 변수 이름으로 중국어 문자나 유니코드 문자를 사용할 수 있어요. Python은 언어 전반에 걸쳐 유니코드를 지원해요.

```js
# 변수 이름으로 중국어 문자 사용하기
大人弓戈 = "Hello"
print(大人弓戈)  # 출력: Hello

金竹戈女日 = "World"
print(金竹戈女日)  # 출력: World

# 함수 이름에 중국어 문자 사용하기
def 心口戈弓廿尸日廿日():
    print("心卜廿竹人弓戈尸中人女水")

心口戈弓廿尸日廿日()
```

## 6. Antigravity

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

파이썬의 재미있는 이스터에그 중 하나는 "Python"이라는 XKCD 만화를 참조한 것입니다. 이 만화에는 어떤 복잡한 작업도 쉽게 느껴지게 만들 수 있는 캐릭터 이야기가 포함되어 있어 파이썬의 간결함과 가독성을 강조합니다.

```js
import antigravity
```

## 7. 이상한 임포트

파이썬에는 30만 개 이상의 패키지가 있습니다. 그 중 일부는 내장되어 있고 놀라운 파이썬 커뮤니티가 외부에서 몇 가지 생성합니다. 각 패키지는 고유한 능력 집합을 갖고 있지만 일부는 파이썬의 재미를 한 단계 더 높여줍니다.

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
import __hello__

>> Hello World!
```

__hello__ 모듈은 파이썬에서 특별한 모듈로, 파이썬 패키지를 만들고 배포하는 간단한 데모로 사용됩니다. 실제 기능은 포함하고 있지 않지만, 파이썬 패키지의 구조와 레이아웃을 보여줍니다.

여기까지 읽어주셔서 감사합니다. 내 컨텐츠가 마음에 들고, 지원하고 싶다면 다음이 최고 방법입니다 —

- Clap👋을 남기고 생각을 💬 댓글로 남겨주세요.
- 제 Medium 팔로우하기.
- LinkedIn에서 나와 연결하기.
- 내 이메일 목록에 가입하여 다른 글을 놓치지 마세요.