---
title: "상호작용 Python Plotly GIS 지도 제작을 위한 더 나은 GPT-4 프롬프트 작성 방법"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-BetterGPT-4PromptingForInteractivePythonPlotlyGISMaps_0.png"
date: 2024-07-14 19:41
ogImage: 
  url: /TIL/assets/img/2024-07-14-BetterGPT-4PromptingForInteractivePythonPlotlyGISMaps_0.png
tag: Tech
originalTitle: "Better GPT-4 Prompting For Interactive Python Plotly GIS Maps"
link: "https://medium.com/towards-artificial-intelligence/better-gpt-4-prompting-for-interactive-python-plotly-gis-maps-c0d7e3150582"
---



![TIL image 0](/TIL/assets/img/2024-07-14-BetterGPT-4PromptingForInteractivePythonPlotlyGISMaps_0.png)

인터넷에는 역사적인 난파선 데이터를 포함한 데이터 세트에 대한 멋진 자료가 많이 있습니다.

사용 가능한 데이터 세트에 대한 나의 지식 확장의 일환으로 매주 업데이트를 받는 소스 중 하나로 Data is Plural이 있습니다:

![TIL image 1](/TIL/assets/img/2024-07-14-BetterGPT-4PromptingForInteractivePythonPlotlyGISMaps_1.png)


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

이 사이트는 흥미로운 데이터 세트에 관한 주간 뉴스레터를 제공합니다.

가장 최근 버전 (2024년 7월 10일)에는 하버드 대학교에서 가져온 고대 바다 난파선에 관한 데이터 세트가 있습니다. 이 데이터 세트에는 기원전 1500년부터 기원후 1500년까지의 기록이 포함되어 있으며 지중해와 북유럽을 중심으로 하고 있습니다. 정말 멋져요!

저는 이 데이터 세트를 GPT-4에 업로드하여 각 포인트 데이터를 지도에 배치하는 Python 코드를 생성할 수 있는지 궁금했습니다.

답변은? 네, 가능합니다. 다만 주의해서 프롬프트를 할 필요가 있습니다. 여기서 저가 거친 단계를 안내해 드리겠습니다.

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

# 제네릭 GPT-4 프롬프트로 시작하기

우선, GPT-4가 데이터 세트를 의미있는 방식으로 해석할 수 있는지 확인해보고 싶었습니다. GIS 지도 코드를 생성할 수 있는지 보기 위한 일반적인 프롬프트입니다.

GPT-4에 대한 프롬프트: 첨부된 데이터 세트를 사용하여, 각 난파선을 지도상의 점으로 표시하는 전역 지도의 파이썬 코드를 제공해주세요.

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

![image](/TIL/assets/img/2024-07-14-BetterGPT-4PromptingForInteractivePythonPlotlyGISMaps_2.png)

이거 그래프네요! 신기하게도 각 포인트들이 정확해요. 아래에 지도 레이어를 넣으면 정확하게 매칭돼요. 그런데, 이건 우리가 원하는 게 아니에요.

첫 번째 제네릭 프롬프가 반 성공했어요. 여기서 더 구체적으로 알아보아야 해요.

GPT-4에게 프롬프트를 보내면: "이건 지도가 아니에요. 각 포인트를 실제 지도에 표시하는 파이썬 코드를 생성해 주세요."

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

이번에는 GPT-4가 matplotlib 및 shapely 라이브러리를 사용하여 Python 코드를 생성했습니다. 결과는 다음과 같습니다:

![이미지](/TIL/assets/img/2024-07-14-BetterGPT-4PromptingForInteractivePythonPlotlyGISMaps_3.png)

이제 지도를 가지고 있지만, 이 지도는 완전히 정적입니다. 확대 및 축소가 불가능하며 상호 작용할 수 없습니다.

또한 각 개별 점을 해석하기가 매우 어렵습니다. 점들이 주로 유럽에 있다는 것을 알 수 있습니다.

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

# 대화식 매핑을 위한 안내

더 세부적으로 알아보고 GPT-4에게 상호작용 옵션을 요청해 봅시다.

GPT-4에게 제시할 프롬프트: 저는 상호작용이 필요해요. 지도를 확대해서 볼 수 있고 각 데이터 포인트 위에 마우스를 올려 선박 이름과 화물을 표시할 수 있기를 원합니다.

GPT-4는 툴팁을 사용하여 folium 지도를 만드는 파이썬 코드를 생성합니다:

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

![이미지](/TIL/assets/img/2024-07-14-BetterGPT-4PromptingForInteractivePythonPlotlyGISMaps_4.png)

와, 여기에는 더 많은 세부 정보가 있네요. 개별 난파선을 식별하는 데이터 포인트를 더 명확하게 볼 수 있습니다. 아쉽게도, 이 지도는 HTML 파일로 생성되어 있어 이상적이지는 않습니다. 보기에는 매우 멋지지만 약간 어색한 방법입니다. GPT-4가 필요한 것은 더 엄격한 안내입니다.

GPT-4에게 알려줄 것: 이 방법은 좋아보이지 않아요. 이를 수행하는 데 왜 Plotly를 사용하지 않을까요?

GPT-4의 응답: 정말 좋은 아이디어네요! 더 나은 상호 작용을 위해 Plotly를 사용하여 코드를 다시 작성합시다:

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
import pandas as pd
import plotly.express as px

# 데이터셋 불러오기
file_path = 'Geodatabase of Shipwrecks Hardard.csv'
shipwreck_data = pd.read_csv(file_path)
# 컬럼 헤더 수정
shipwreck_data.columns = shipwreck_data.iloc[0]
shipwreck_data = shipwreck_data.drop(0)
# 유효한 좌표가 없는 행 제외
shipwreck_data = shipwreck_data.dropna(subset=['Latitude', 'Longitude'])
# 위도와 경도를 숫자로 변환
shipwreck_data['Latitude'] = pd.to_numeric(shipwreck_data['Latitude'], errors='coerce')
shipwreck_data['Longitude'] = pd.to_numeric(shipwreck_data['Longitude'], errors='coerce')
# 변환 후 유효하지 않은 좌표가 있는 행 제외
shipwreck_data = shipwreck_data.dropna(subset=['Latitude', 'Longitude'])
# Plotly와 Mapbox를 사용하여 인터랙티브 맵 생성
fig = px.scatter_mapbox(shipwreck_data,
                        lat='Latitude',
                        lon='Longitude',
                        hover_name='NAME',
                        hover_data={'Longitude': False, 'Latitude': False, 'Cargo_1': True},
                        title='Global Map of Shipwrecks',
                        mapbox_style='carto-positron')  # Using mapbox
# 더 나은 모양을 위해 레이아웃 업데이트
fig.update_traces(marker=dict(color='red', size=10))  # 색상 및 크기 지정
fig.update_layout(
    mapbox=dict(
        accesstoken='your_mapbox_access_token', 
        center={'lat': 44, 'lon': 18},  # 유럽 지도 중심으로 설정
        zoom=4
    ),
    margin=dict(l=0, r=0, t=0, b=0),
    title_font_size=24,
    font=dict(size=14)
)
# 인터랙티브 맵 표시
fig.show()
```

그리고 이 코드는 정말 잘 작동합니다.

결과:

<img src="/TIL/assets/img/2024-07-14-BetterGPT-4PromptingForInteractivePythonPlotlyGISMaps_5.png" />


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

멋지네요, 이제 조금씩 진행 중이네요. 그리고 각 지점 데이터 위에 마우스를 올려서 난파선 이름과 수송품 정보(알려진 경우)를 찾을 수 있어요:

![image](/TIL/assets/img/2024-07-14-BetterGPT-4PromptingForInteractivePythonPlotlyGISMaps_6.png)

GPT-4는 실제로 지도에 GIS 지점 데이터를 표시하는 Python 코드를 효과적으로 생성하는 데 활용될 수 있어요.

이 예제에서 원하는 결과를 생성하도록 GPT-4에 제공해야 하는 정보는 다음과 같아요:

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

- 데이터 세트를로드하고 열 헤더를 수정하십시오.
- 유효한 좌표가없는 행을 필터링하십시오.
- 위도와 경도를 숫자 값으로 변환하십시오.
- Plotly의 scatter_mapbox 함수를 사용하여 대화 형 지도를 만듭니다.
- 각 포인트의 색상을 빨간색으로 설정하고 크기를 5픽셀로 설정하십시오.
- 지중해로 확대 된지도를 표시하십시오.
- Mapbox 타일에 'carto-positron' 스타일을 사용하십시오.
- 위도 34 및 경도 18을 중심으로하고 줌 레벨이 4 인 맵을 센터에 배치하십시오.
- 코드의 적절한 위치에 Mapbox 액세스 토큰을 추가하십시오.

모두입니다.

# 요약

GPT-4는 Python 코드를 간단한 CSV 파일에서 GIS 포인트 데이터를지도에 표시하는데 쉽게 생성할 수 있습니다.

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

이거 조심해서 잘 만들어야 해. 이 LLM을 최적화하려면 좋은 결과를 얻을 때까지 여러 번 시도해야 할 수도 있어.

GPT-4를 광범위하게 활용하면서, 최적의 결과를 찾기 전에 여러 가지 방법을 고려할 수 있음을 알았어. GPT-4에게 가능한 접근법을 묻는 것이 좋을 뿐 아니라 (이런 예시처럼) 더 나은 방법을 알고 있다면, GPT-4에게 사용하도록 명확하게 요청해.

그리고 LLM 프롬프팅 작업을 할 때 중요한 점은, GPT-4는 결과물에 변화를 주는 방식으로 설계되었기 때문에, 당신이 프롬프트로 받는 코드가 여기서 보여주는 것과 정확히 일치하지 않을 수 있다는 거야.

코멘트를 남겨서 프롬프팅이 어떻게 진행되었는지 알려줘!

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

만약 이 유형의 이야기가 당신의 취향이고 저를 작가로 지원하고 싶다면, 제 Substack에 가입해주세요.

Substack에서는 매주 두 번 뉴스레터와 다른 플랫폼에서는 찾을 수 없는 기사들을 발행하고 있어요.