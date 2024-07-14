---
title: "파이썬으로 밀리초를 시간, 분, 초로 변환하는 방법"
description: ""
coverImage: "/TIL/assets/img/2024-07-15-ConvertingMillisecondstoHoursMinutesandSecondsinPython_0.png"
date: 2024-07-15 00:01
ogImage: 
  url: /TIL/assets/img/2024-07-15-ConvertingMillisecondstoHoursMinutesandSecondsinPython_0.png
tag: Tech
originalTitle: "Converting Milliseconds to Hours, Minutes, and Seconds in Python"
link: "https://medium.com/@adnanturgayaydin/converting-milliseconds-to-hours-minutes-and-seconds-in-python-2aa041ea3c31"
---


안녕하세요!

시간 변환은 프로그래밍에서 흔한 작업 중 하나로, 기본 산술 연산과 문자열 처리에 대한 기본적인 이해를 개발하는 데 도움이 됩니다. 이 글에서는 Python을 사용하여 밀리초를 시간, 분, 초로 변환하는 방법을 살펴보겠습니다. 이 작업을 통해 루프, 조건문 및 함수 설계에 대한 지식을 확립할 수 있을 겁니다.

변환 이해하기

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

코드를 시작하기 전에 기본적인 시간 단위를 이해해보겠습니다:

- 1 시간 = 3600000 밀리초
- 1 분 = 60000 밀리초
- 1 초 = 1000 밀리초

솔루션 디자인

주어진 밀리초 수를 시간, 분 및 초로 변환하는 것이 목표입니다. 다음은 단계별 접근 방식입니다:

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

- 입력 유효성 검사: 입력이 0보다 큰 유효한 숫자인지 확인합니다.
- 변환 로직: 밀리초를 시간, 분 및 초로 분해합니다.
- 출력 형식 지정: 결과를 읽기 쉬운 형식으로 표시하며, 0이 아닌 시간 단위만 표시합니다.
- 사용자 상호 작용: 사용자가 "exit"을 입력할 때까지 입력을 요청하는 루프를 구현합니다.

구현

시간 변환기 코드를 시작해보겠습니다:

```js
def convert_milliseconds(ms):
    if ms < 1000:
        return f"방금 {ms} 밀리초"
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

```python
    hours = ms // 3600000
    ms %= 3600000
    minutes = ms // 60000
    ms %= 60000
    seconds = ms // 1000
    result = []
    if hours > 0:
        result.append(f"{hours} hour/s")
    if minutes > 0:
        result.append(f"{minutes} minute/s")
    if seconds > 0:
        result.append(f"{seconds} second/s")
    return ' '.join(result)
def main():
    print("### 이 프로그램은 밀리초를 시간, 분, 초로 변환합니다 ###")
    print('(프로그램을 종료하려면 "exit"을 입력하세요)')
    while True:
        user_input = input("밀리초를 입력하세요 (0보다 커야 합니다): ")
        if user_input.lower() == "exit":
            print("프로그램 종료... 안녕히 가세요")
            break
        try:
            ms = int(user_input)
            if ms > 0:
                print(convert_milliseconds(ms))
            else:
                print("올바르지 않은 입력입니다 !!!")
        except ValueError:
            print("올바르지 않은 입력입니다 !!!")
if __name__ == "__main__":
    main()
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

- 사용자 상호 작용:

  - 주요 기능은 사용자 입력을 처리하고 유효성을 검사합니다.
  - 사용자에게 밀리초를 요청하고 유효한지 확인한 후 convert_milliseconds 함수를 사용하여 변환합니다.
  - 사용자가 "exit"을 입력할 때까지 루프가 계속됩니다.