---
title: "1만 5천 달러로 트레이딩 봇 운영하기 단계별 가이드"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-RunningATradingBotWith15000Step-By-StepGuideOnHowToDoIt_0.png"
date: 2024-07-14 20:24
ogImage: 
  url: /TIL/assets/img/2024-07-14-RunningATradingBotWith15000Step-By-StepGuideOnHowToDoIt_0.png
tag: Tech
originalTitle: "Running A Trading Bot With 15,000 Step-By-Step Guide On How To Do It"
link: "https://medium.com/@konstantinmb/running-a-trading-bot-with-15-000-step-by-step-guide-on-how-to-do-it-27c8416a3432"
---


![Binance Testnet](/TIL/assets/img/2024-07-14-RunningATradingBotWith15000Step-By-StepGuideOnHowToDoIt_0.png)

얼마 전부터 거래 봇을 만들고 작업하는 것을 즐기고 있어요. 거의 1.5년 정도예요. 그 과정에서 저는 계정을 활용하면서 Python과 거래 지식을 비동기적으로 향상시켰어요.

오늘의 블로그 포스트에서는 여러분도 자신만의 거래 봇을 개발하고 테스트하는 여정을 시작할 수 있는 방법에 대해 알아볼 거에요! 🤩

자, 시작해봅시다... 🥁... 바이낸스 테스트넷!

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

# 트레이딩 봇을 사용하는 이유

주식 거래란 회사의 주식을 사고 팔아서 가격 변동에 기반해 돈을 벌려고 하는 것을 의미합니다.

![Trading Bot](/TIL/assets/img/2024-07-14-RunningATradingBotWith15000Step-By-StepGuideOnHowToDoIt_1.png)

주식을 암호화폐로 바꾸면 동일한 정의가 됩니다.

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

거래 전략은 분당 또는 심지어 초당으로 여러 거래를 배치하고 구성할 수 있기 때문에 거래 봇을 활용하여이 프로세스를 자동화하는 것은 많은 사람들에게 매우 유용합니다!

어느 정도까지는 거래의 감정적인 부분을 제거하기도하며, 이것이 많은 경우에 훌륭한 거래자와 나쁜 거래자를 구분하는 중요한 부분입니다!

# 테스트넷이 무엇인가요?

이것은 여러분의 테스트 환경입니다. 바이낸스와 같은 대형 암호 화폐 거래소들은 이를 우리에게 우리의 전략을 테스트하고 수익을 잃지 않도록 도와주는 도구로 제공합니다!

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

그 목표는 실제 거래 환경을 모방하지만 `푸가지`(가짜) 돈을 사용하는 것입니다. 💸

![이미지](https://miro.medium.com/v2/resize:fit:490/1*WNIJgycm4pTBkdcFU5Pb5w.gif)

# 환경 설정하기

이제 첫 번째 거래 봇을 개발하고, 바이낸스 테스트넷에서 실행하여 15,000달러의 암호화폐 지갑으로 무엇을 할 수 있는지 살펴봅시다! 🤑

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

## IDE 설정

이 프로젝트에서는 VisualStudio Code를 IDE로 사용할 것입니다.

PyCharm이나 다른 개발 플랫폼을 자유롭게 사용해주세요!

## 바이낸스 테스트넷 계정

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

다음으로, Binance 테스트넷 계정을 설정하세요. 아래는 그 방법을 단계별로 안내해드립니다!👇

마지막에는 테스트넷 계정용 API_KEY 및 API_SECRET이 있어야 합니다.

위 과정을 진행하는 데 어려움을 겪는다면 메시지나 댓글을 남겨주세요. 도와드릴 시간을 내어 도와드리겠습니다! 💪

# 우리의 트레이딩 봇 만들기

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

우리는 미래에 이동 평균을 사용하여 추세를 식별하는데 트레이더 사이에서 인기 있는 도구 중 하나인 바이낸스 선물 거래 봇을 만들 것입니다.

우리는 단순 이동 평균(SMA) 교차 전략을 사용할 것인데, 이는 단기 이동 평균이 장기 이동 평균을 꺾을 때 매수(하이 포지션)하고, 그 반대의 경우에 매도(숏 포지션)하는 전략입니다.

![image](/TIL/assets/img/2024-07-14-RunningATradingBotWith15000Step-By-StepGuideOnHowToDoIt_2.png)

## 1. 이 봇에 사용할 라이브러리를 정의합시다

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
from binance.client import Client # pip install python-binance
from binance.enums import *
import pandas as pd # pip install pandas
import numpy as np # pip install numpy
```

모두 `라이브러리 이름`을 이용해서 pip install 명령어로 설치할 수 있어요.

## 2. 이제, 우리의 바이낸스 클라이언트를 정의해봅시다.

여기서 매우 중요한 부분인데, `메인 계정이 아닌 Testnet API_KEY 및 API_SECRET을 사용하세요. 메인 계정과는 호환되지 않습니다!


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
API_KEY = '<상기 테스트넷 설정 과정에서 생성한 키>'
API_SECRET = '<상기 테스트넷 설정 과정에서 생성한 시크릿>'

client = Client(api_key=API_KEY, api_secret=API_SECRET, testnet=True)
```

‼️ API 키와 시크릿을 저장하는 위치와 방법에 주의하세요. 심지어 테스트넷 계정의 경우에도 dotenv와 같은 도구를 사용하여 로컬에 안전하게 저장하고 GitHub에 공개하지 말아주세요! 😅

## 3. 전략 및 함수 정의

```js
# 캔들스틱 데이터를 가져오는 함수:
def fetch_data(symbol, interval, lookback):
    bars = client.futures_historical_klines(symbol, interval, lookback)
    df = pd.DataFrame(bars, columns=['timestamp', 'open', 'high', 'low', 'close'])
    df['close'] = pd.to_numeric(df['close'])
    df['timestamp'] = pd.to_datetime(df['timestamp'], unit='ms')
    return df[['timestamp', 'close']] # 데이터 중 필요한 부분만 반환

# 주요 전략 로직 및 실행부 👇
def sma_strategy(symbol='BTCUSDT', interval='1h', short_window=50, long_window=200, lookback='30 days ago UTC'):
    data = fetch_data(symbol, interval, lookback)
    
    data['short_sma'] = data['close'].rolling(window=short_window).mean()
    data['long_sma'] = data['close'].rolling(window=long_window).mean()
    
    # 포지션 없이 시작한다고 가정
    in_position = False

    # SMA 교차 확인
    # SMA가 LMA를 아래로 교차할 때 (암호화폐 매수)👇
    if data['short_sma'].iloc[-2] < data['long_sma'].iloc[-2] and data['short_sma'].iloc[-1] > data['long_sma'].iloc[-1]:

        if not in_position:
            print("매수 신호!")
            order = client.futures_create_order(symbol=symbol, side='BUY', type='MARKET', quantity=0.01)
            in_position = True
            print(order)

    # LMA가 SMA를 아래로 교차할 때 (암호화폐 매도) 👇  
    elif data['short_sma'].iloc[-2] > data['long_sma'].iloc[-2] and data['short_sma'].iloc[-1] < data['long_sma'].iloc[-1]:

        if in_position:
            print("매도 신호!")
            order = client.futures_create_order(symbol=symbol, side='SELL', type='MARKET', quantity=0.01)
            in_position = False
            

if __name__ == '__main__':
    sma_strategy()
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

너무 세부적으로 언급하지 않겠습니다. 이것은 기본 전략이고 코드는 복잡해 보일지 몰라도 이해하기 어렵지 않습니다!

가장 중요한 점은 이동평균간의 교차점이 발생할 때, 우리는 바이낸스 테스트넷에서 숏 또는 롱 선물 주문을 실행합니다!

## 4. 거래 모니터링

![거래 모니터링](/TIL/assets/img/2024-07-14-RunningATradingBotWith15000Step-By-StepGuideOnHowToDoIt_3.png)

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

아래에서 아직 실행되지 않은 오픈 주문들을 볼 수 있습니다. 이것은 내가 설정한 가격에 충족되지 않았기 때문에 실행되지 않았습니다.

오른쪽에는 제 계정 잔액을 보실 수 있어요. 현재 약 13k 정도입니다. 좀 더 복잡한 봇을 작업한 결과로, 기능들을 테스트하기 위해 여러 거래를 실행했죠. 😅

설정에 문제가 있거나 도움이 필요하면 언제든지 연락해주세요! 언제든 도움이나 조언을 드릴 수 있어요! 🤲

LinkedIn: https://www.linkedin.com/in/kbor/

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

아래는 Markdown 형식으로 변경된 내용입니다.

Discord: @ konstantinmb

Twitter/X: [https://twitter.com/konstantinmbv](https://twitter.com/konstantinmbv)

# Experiment

![Experiment](/TIL/assets/img/2024-07-14-RunningATradingBotWith15000Step-By-StepGuideOnHowToDoIt_4.png)

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

💪 소프트웨어 엔지니어링 및 다른 분야에서 가장 중요한 모토 중 하나일 거예요. 실제로 실습하는 것을 이길 게 없죠. 스스로 시도해보는 것만큼 가르쳐줄 수 있는 자습서는 없어요!

🆓 바이낸스 테스트넷을 통해 가상 화폐를 사용하고 실제 플랫폼에서 원하는 대로 실험할 수 있어요! 이 프로그래밍 거래 기술을 개발할 수 있는 놀라운 기회라구요!

저도 현재 그렇게 하고 있어요! 함께 해요! 🍻

즐거웠길 바랄게요! 저는 거래 봇을 만드는 것을 좋아하며 여기와 Substack에서 나의 경험을 더 많이 공유할 거에요!

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

여기서 다른 블로그도 확인해보세요 👇

이와 함께 👏을 클릭하고 제 프로필 @ Konstantin Borimechkov을 구독해 주세요. 이와 같은 콘텐츠를 더 보고 싶다면!

글쓰기와 지식 공유를 사랑하고 있어요. 여러분의 지원은 저에게 큰 힘이 됩니다! 💚