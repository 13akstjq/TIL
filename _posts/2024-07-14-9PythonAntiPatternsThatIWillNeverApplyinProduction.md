---
title: "프로덕션에서 절대 사용하지 않을 9가지 Python 안티 패턴"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-9PythonAntiPatternsThatIWillNeverApplyinProduction_0.png"
date: 2024-07-14 20:45
ogImage: 
  url: /TIL/assets/img/2024-07-14-9PythonAntiPatternsThatIWillNeverApplyinProduction_0.png
tag: Tech
originalTitle: "9 Python Anti Patterns That I Will Never Apply in Production"
link: "https://medium.com/gitconnected/9-python-anti-patterns-that-i-will-never-apply-in-production-dd6f3d1e4b42"
---


## 파이썬

![Python Anti-Patterns](/TIL/assets/img/2024-07-14-9PythonAntiPatternsThatIWillNeverApplyinProduction_0.png)

어떤 언어든, 오래된 만큼 반드시 몇 가지 안티-패턴 기능과 이해하기 어려운 구문이 있습니다.

파이썬이 우아한 언어라고 하더라도 이 운명에서 벗어날 수 없습니다.

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

소프트웨어 개발자로서 우리가 할 수 있는 것은 지식을 최신 상태로 유지하고 오래된 또는 부적절한 기능을 사용하지 않는 것입니다.

이 기사는 가독성, 보안 및 디버깅에 해로운 9가지 Python 안티 패턴에 대해 상기시킬 것입니다. 경험 많은 Python 엔지니어는 제품 개발 시 이러한 패턴을 적용해서는 안 됩니다.

# 1. For-Else 구조

Python에는 매우 특별한 구문인 for-else가 있습니다.

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

if-else가 아니라 for 루프 후에 "else"가 있다는 거야!

```js
leaders = ["Elon", "Tim", "Warren"]

for i in leaders:
    if i == "Yang":
        print("Yang is a leader!")
        break
else:
    print("Yang을 찾을 수 없어요!")

# Yang을 찾을 수 없어요!
```

위의 코드에서 보듯이 leaders 리스트에는 Yang이 없지만 Yang 또한 지도자입니다. 따라서 for 루프에 break가 없어요.

이 경우, for 루프 뒤에 있는 "else" 블록이 실행되었어요.

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

그 로직은 간단합니다. "else" 블록은 루프에 중단점이 없을 때만 실행됩니다.

그러나 다른 프로그래밍 언어에서는 else 문을 이렇게 사용할 수 없기 때문에, 이 for-else 구조는 파이썬 원어민이 아닌 대부분의 동료들을 혼란스럽게 만들 수 있습니다.

동료들이 파이썬 마스터라 하더라도, 이 기능이 100% 필요하지 않고, 사용하는 사람이 많지 않으므로, 제품에 적용하는 것은 전혀 좋은 생각이 아닙니다. 그렇지 않으면, 코드베이스의 가독성이 극도로 떨어질 수 있습니다.

그렇다면, for-else 구조 없이 예시 코드를 다시 작성하는 방법은 무엇일까요?

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

테이블 태그를 마크다운 형식으로 변경해주세요:


leaders = ["Elon", "Tim", "Warren"]
found_yang = False

for i in leaders:
    if i == "Yang":
        print("Yang is a leader!")
        found_yang = True
        break

if not found_yang:
    print("Not found Yang!")


## 2. eval() 또는 exec() 함수

처음 보면, eval() 함수의 힘을 활용하면 일부 기능을 구현하는 것이 더 쉬워 보입니다.

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

예를 들어, 다음 함수는 eval() 함수의 도움을 받아 한 줄의 코드로 기본 계산기를 구현합니다:

```js
def calculator(a, b, op):
    return eval(f'{a} {op} {b}')

print(calculator(2, 3, '+'))
# 5
print(calculator(6, 5, '-'))
# 1
print(calculator(7, 8, '*'))
# 56
print(calculator(8, 2, '/'))
# 4.0
```

깔끔하고 간결하죠?

사이버 공격자들도 eval() 함수를 좋아합니다.

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

그 이름에서 알 수 있듯이, eval() 함수는 Python 식을 평가하는 데 사용됩니다.

그러나 입력이 시스템을 해킹하도록 특별히 설계된 경우, eval() 함수는 모든 악의의 근원이 될 수 있습니다.

예를 들어, 누군가 이전의 계산기를 다음과 같은 방식으로 사용한다면, 쉽게 현재 위치의 폴더와 파일 목록을 가져올 수 있습니다:

```js
def calculator(a, b, op):
    return eval(f'{a} {op} {b}')


print(calculator("__import__('os').system('ls')",'',''))
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

다음과 같이 입력을 변경해보는 건 어떨까요:

```js
print(calculator("__import__('os').system('rm -rf *')",'',''))
```

따라서, 제작 환경에서는 evel() 함수를 사용하지 않는 것이 가장 좋습니다.

evel() 함수와 유사하게, exec()는 Python 프로그램 코드를 동적으로 실행하는 또 다른 내장 함수입니다. 보안 문제로 인해 이 역시 제작 환경에서 사용하지 않는 것이 좋습니다.

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

# 3. 깊게 중첩된 루프

어떤 복잡한 경우에는 중첩된 루프를 사용할 수밖에 없는 것처럼 보입니다:

```js
list_a = [1, 2020, 70]
list_b = [2, 4, 7, 2000]
list_c = [3, 70, 7]

for a in list_a:
    for b in list_b:
        for c in list_c:
            if a + b + c == 2077:
                print(a, b, c)
# 70 2000 7
```

하지만 깊게 중첩된 루프는 읽기 어렵고 파이썬다운 방식이 아닙니다.

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

나쁜 코딩에서 나오는 안티 패턴입니다. 파이썬은 이 문제를 해결하기 위한 내장 도구를 제공합니다:

```python
from itertools import product

list_a = [1, 2020, 70]
list_b = [2, 4, 7, 2000]
list_c = [3, 70, 7]

for a, b, c in product(list_a, list_b, list_c):
    if a + b + c == 2077:
        print(a, b, c)
# 70 2000 7
```

위의 예시에서 보다시피, itertools.product() 함수의 도움을 받아 이전 프로그램은 중첩된 루프를 완전히 피할 수 있습니다. 이 함수는 입력 이터러블의 카테시안 곱을 얻기 위한 방법입니다.

# 4. 와일드카드로 모두 가져오기(import)

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

와일드카드 임포트(from module import *)는 이름 충돌과 코드 불명확성의 근본이 됩니다.

이것은 모듈에서 모든 객체를 현재 네임스페이스로 가져오기 때문에 어디서 변수 또는 함수가 정의되었는지 추적하기 어렵습니다. 모듈에 의해 소개된 모든 이름을 기억하기 어려울 뿐만 아니라, 특히 대규모 타사 모듈의 경우 더욱 문제입니다.

네임스페이스 오염을 피하기 위해, 우리가 명시적으로 필요한 것만 가져오는 것이 최선의 실천 방법입니다:

```js
from module import func_a, func_b
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

# 5. 멋진 한 줄 요약

한 줄 요약은 프로그래밍의 재미난 부분을 보여줍니다. 재미로 작성하는 건 괜찮아요.

하지만 제품용 코드에 넣는 건 재앙이 될 수 있습니다.

이것은 퀵 정렬의 파이썬 한 줄 요약입니다:

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
퀵정렬 = lambda l: 퀵정렬([i for i in l[1:] if i < l[0]]) + [l[0]] + 퀵정렬([j for j in l[1:] if j >= l[0]]) if l else []
```

이 코드는 잘 작동하지만 코드의 로직을 얼마나 빨리 이해할 수 있습니까?

당신만큼 빨리 동료들도 이해할 수 있을까요?

그리고 람다 함수는 많은 멋진 파이썬 한 줄짜리 코드의 기반입니다. 우리는 매우 간단한 함수에만 사용해야 합니다.

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

# 6. 깊은 상속 계층 구조와 복잡한 믹스인

객체 지향 프로그래밍은 어떤 면에서는 두 각도로 양날의 검입니다.

때로는 메서드의 목적을 이해하기 위해 매우 복잡한 상속 계층을 추적해야 할 수도 있습니다.

게다가 Python은 믹스인을 지원하는데, 이는 객체를 이해하기 어렵게 만들 수 있습니다.

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

동료들이 편리하게 일할 수 있도록 상속 체인을 간소화하는 것을 고려해보세요.

# 7. 변경 가능한 기본 인수

파이썬은 값에 의한 호출(call-by-name)도 아니고 참조에 의한 호출(call-by-reference)도 아니며, 공유에 의한 호출(call-by-sharing)이다.

이는 변경 가능한 기본 인수를 정의할 경우 혼란스러운 코드로 이어질 수 있습니다:

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
def get_leaders(element, leader_list=[]):
    leader_list.append(element)
    return leader_list


print(get_leaders('Elon'))
# ['Elon']
print(get_leaders('Yang'))
# ['Elon', 'Yang']
print(get_leaders('Warren'))
# ['Elon', 'Yang', 'Warren']
```

위의 함수는 함수를 3번 독립적으로 호출해도 항상 동일한 목록을 사용합니다.

이제 leader_list의 기본 값을 None으로 변경하고 결과를 확인해봅시다:

```js
def get_leaders(element, leader_list=None):
    if leader_list is None:
        leader_list = []
    leader_list.append(element)
    return leader_list


print(get_leaders('Elon'))
# ['Elon']
print(get_leaders('Yang'))
# ['Yang']
print(get_leaders('Warren'))
# ['Warren']
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

매번 함수 호출할 때마다 새로운 목록이 생성됩니다.

실제로 Python 함수에서 가변 기본 인수를 사용하는 것은 Python이 가변 및 불변 데이터 구조를 처리하는 방식으로 인해 안티 패턴으로 간주됩니다. 이 동작은 종종 예상치 못한 결과와 찾기 어려운 버그로 이어집니다.

# 8. If-Else가 모든 곳에서 등장합니다

Python 3.10 이전에는 때때로 많은 if-else 문을 작성해야 했습니다. 왜냐하면 Python은 아직 match-case 구문을 지원하지 않았기 때문입니다.

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

파이썬 3.10 이후로 구조적 패턴 매칭 기술이 소개되면서 if-else 문을 반복적으로 작성하는 불편을 피할 수 있게 되었습니다:

```js
def handle_http_status(status_code):
    match status_code:
        case 200:
            return "성공!"
        case 400:
            return "잘못된 요청"
        case 401:
            return "권한 없음"
        case 404:
            return "찾을 수 없음"
        case 500:
            return "내부 서버 오류"


print(handle_http_status(200))
# 성공!
print(handle_http_status(404))
# 찾을 수 없음
print(handle_http_status(2077))
# None
```

위 프로그램에서 보듯이, match-case 구문을 사용하여 각각 다른 입력을 처리할 수 있습니다. 모든 경우에 if-else 문이 필요하지 않다는 것을 알 수 있습니다.

# 9. C-스타일 문자열 포맷팅

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

파이썬의 매우 오래된 버전에서는 문자열 포맷팅 연산이 C 프로그래밍 언어와 같았습니다:

```js
name = 'Yang'
print("Hi, %s" % name)
# Hi, Yang
```

그러나 파이썬이 많이 발전함에 따라 이전 구문은 여전히 사용할 수 있지만, 그 낡은 면모를 보여주고 있습니다:

지금은 이 오래된 기술을 피하고 모든 문자열 포맷팅에 f-string 구문을 적용해야 합니다:

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


name = 'Yang'
desc = 'amazing'
print(f"Hi, {name}! You are {desc}!")
# Hi, Yang! You are amazing!


# 결론

어떤 프로그래밍 언어도 완벽하지 않습니다.

자바보다 오래된 파이썬은 불가피하게 몇 가지 나쁜 설계로 인해 안티 패턴을 유발합니다.


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

경험 많은 Python 개발자로서, 우리는 문제가 있는 곳을 파악하고 그것들을 피하는 것이 중요해요. 동시에, 최신 구문을 활용하기 위해 새로운 Python 버전의 업데이트를 주시하면 도움이 될 거예요.

읽어 주셔서 감사합니다. ❤️ 만약 마음에 드셨다면, 연락해 보아요:

- Linkedin
- Medium

제가 작성한 더 많은 Python 자습서들: