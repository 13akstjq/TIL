---
title: "Seaborn을 활용한 데이터 시각화 방법"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-DataVisualizationwithSeaborn_0.png"
date: 2024-07-14 20:29
ogImage: 
  url: /TIL/assets/img/2024-07-14-DataVisualizationwithSeaborn_0.png
tag: Tech
originalTitle: "Data Visualization with Seaborn"
link: "https://medium.com/@mdkarim_87449/data-visualization-with-seaborn-c0de379047d3"
---



![그림](/TIL/assets/img/2024-07-14-DataVisualizationwithSeaborn_0.png)

데이터 시각화는 데이터 과학에서 중요한 기술로, 분석가들이 데이터에서 숨겨진 패턴과 통찰을 발견할 수 있게 합니다. Seaborn은 Matplotlib 위에 구축된 Python 라이브러리로, 매력적이고 유익한 통계 그래픽을 만들기 위한 고수준 인터페이스를 제공합니다. Seaborn은 아름다운 시각화를 만드는 과정을 간단하게 만들어주며, 다양한 플롯 스타일과 색상 팔레트를 제공하면서 Matplotlib이 사용자에게 맡기는 복잡성 중 많은 부분을 처리합니다. 특히 탐색적 데이터 분석에 적합하며 Pandas 데이터 구조와 원활하게 통합됩니다.

환경 설정하기

시각화에 들어가기 전에, Seaborn이 설치되어 있는지 확인해 주세요:


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
!pip install seaborn
```

Seaborn을 설치했으면, 다른 필요한 라이브러리와 함께 불러올 수 있어요:

```js
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
```

동물원 데이터셋 불러오기


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

저희 동물원 프로젝트를 위해 동물의 다양한 속성을 포함하는 데이터셋이 필요합니다. 시연 목적을 위해 가상의 데이터셋을 만들어 보겠습니다:

```js
# 샘플 동물원 데이터셋
data = {
    '동물': ['사자', '호랑이', '곰', '얼룩말', '기린'],
    '물_수요': [350, 410, 290, 220, 300],
    '체길이': [1.8, 2.0, 1.5, 2.5, 3.0],
    '줄무늬': [0, 0, 0, 1, 0]
}
zoo_df = pd.DataFrame(data)
```

# 기본 시각화 생성

## 막대 그래프

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

막대 플롯은 각 사각형의 높이로 수치 변수의 중심 경향 추정을 나타내고 오차 막대를 사용하여 해당 추정 주변의 불확실성을 표시합니다.


sns.barplot(x='Animal', y='Water_Need', data=zoo_df)
plt.title('Water Need by Animal')
plt.show()


출력

![Image](/TIL/assets/img/2024-07-14-DataVisualizationwithSeaborn_1.png)

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

## 히스토그램

히스토그램은 수치 변수의 분포를 나타내는데, x-축을 구간으로 나누고 각 구간에 속하는 관측치의 수를 세는 방식입니다.

```js
sns.histplot(zoo_df['Body_Length'], bins=5)
plt.title('Distribution of Body Lengths')
plt.show()
```

결과

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


![image](/TIL/assets/img/2024-07-14-DataVisualizationwithSeaborn_2.png)

## 산점도

산점도는 변수 간의 관계를 관찰하는 데 사용됩니다. Seaborn의 scatterplot 기능은 이러한 플롯을 만드는 데 편리한 인터페이스를 제공합니다.

```js
sns.scatterplot(x='Body_Length', y='Water_Need', data=zoo_df)
plt.title('체길 길이 대 물 필요량')
plt.show()
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

출력

<img src="/TIL/assets/img/2024-07-14-DataVisualizationwithSeaborn_3.png" />

## Jointplot

Seaborn을 사용하여 jointplot은 두 변수 간의 관계를 표시하며 각 변수의 주변 분포를 함께 보여줍니다.

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
sns.jointplot(x='Body_Length', y='Water_Need', data=zoo_df, kind='reg')
plt.suptitle('Jointplot of Body Length and Water Need')
plt.show()
```

결과

<img src="/TIL/assets/img/2024-07-14-DataVisualizationwithSeaborn_4.png" />

## Pairplot


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

`pairplot` 함수는 데이터 세트의 각 변수가 한 행을 통해 y 축을 공유하고 한 열을 통해 x 축을 공유하도록 하는 Axes 그리드를 생성합니다.

```python
sns.pairplot(zoo_df)
plt.suptitle('Zoo 데이터 세트의 Pairplot')
plt.show()
```

출력

<img src="/TIL/assets/img/2024-07-14-DataVisualizationwithSeaborn_5.png" />

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

# 차트 서식의 기본

Seaborn은 차트의 미학을 사용자 정의하여 더 매력적이고 해석하기 쉽게 만들 수 있는 여러 가지 방법을 제공합니다.

테마와 스타일

Seaborn은 모든 차트에 전역적으로 적용할 수 있는 사전 설정된 테마를 제공합니다. 예를 들어, darkgrid 테마를 설정하는 것은 매우 간단합니다:

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


sns.set_theme(style="darkgrid")


이 명령은 차트에 어두운 배경과 그리드 라인을 즉시 적용하여 플롯을 통해 값들을 추적하는 데 도움이 됩니다.

set_style로 사용자 정의하기

set_style을 사용하여 플롯의 모양을 더욱 사용자 정의할 수 있습니다. 이 함수를 사용하면 배경, 그리드 라인 및 spines를 제어할 수 있습니다.


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
sns.set_style("whitegrid", {"grid.linestyle": ":"})
```

이 코드는 점선 그리드 라인이 있는 흰색 그리드 배경을 설정합니다.

다양한 맥락에 맞게 그래프 크기 조정하기

Seaborn의 set_context 함수를 사용하면 그래프 요소의 크기를 조정하여 프레젠테이션, 포스터 또는 보고서에 적합한 차트를 만들 수 있습니다.

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
sns.set_context("talk")
```

이 명령은 레이블 및 선과 같은 요소를 확대하여 토크나 프레젠테이션 설정에서 더 읽기 쉽게 만듭니다.

색상 팔레트 사용자 정의

Seaborn은 프로젝트 테마에 맞게 색상 팔레트를 사용자 정의하거나 색맹 뷰어들을 위해 가독성을 향상시킬 수 있도록 합니다.

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
custom_palette = sns.color_palette("Paired", 9)
sns.set_palette(custom_palette)
```

이 코드 조각은 쌍으로 된 색상으로 사용자 정의 색상 팔레트를 생성하고 설정합니다. 이것은 범주 간의 구분을 좋게하는 데 좋습니다.

플롯 요소 수정

축, 범례 및 제목과 같은 플롯의 특정 요소를 수정하여 명확성과 미학을 향상시킬 수 있습니다.

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
# 커스터마이즈된 요소로 플로팅하기
ax = sns.barplot(x="동물", y="수분_요구량", data=zoo_data)
ax.set_title("동물과 그들의 수분 요구량")
ax.set_xlabel("동물")
ax.set_ylabel("수분 요구량(리터)")
sns.despine()  # 상단과 우측의 가장자리를 제거합니다
```

이 예제는 제목과 레이블이 달린 축을 가진 막대 플롯을 생성하며, 깔끔한 모습을 위해 불필요한 가장자리를 제거합니다.

# FacetGrid 이해하기

FacetGrid는 조건부 관계를 플로팅하기 위한 다중 플롯 그리드입니다. 데이터셋의 하위 그룹 내에서 여러 변수의 분포를 비교하거나 변수 간의 관계를 비교하고 싶을 때 특히 유용합니다. 생성된 플롯은 종종 "lattice", "trellis", 또는 "small-multiple" 그래픽이라고 합니다.

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

동물원 데이터 세트 로딩

우리의 동물원 프로젝트를 위해, 우리는 동물들의 종류, 수분 요구량 및 몸길이와 같은 다양한 속성을 포함하는 zoo.csv 데이터 세트를 가정해 봅시다. 아래는 데이터를 로드하는 방법입니다:

```js
import pandas as pd
zoo_data = pd.read_csv('zoo.csv')
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

FacetGrid을 사용한 기본적인 워크플로우는 세 단계로 구성됩니다:

- 데이터셋 및 그리드 구조를 정의하는 변수와 함께 FacetGrid 객체를 초기화합니다.
- 각 하위 집합에 하나 이상의 플로팅 함수를 적용하려면 FacetGrid.map() 또는 FacetGrid.map_dataframe()을 호출합니다.
- 축 레이블을 변경하거나 범례를 추가하는 등의 작업을 수행하기 위해 다른 메서드를 사용하여 그림을 수정합니다.

그리드 초기화하기

우리의 동물원 데이터셋에서 각 동물 유형에 대해 별도의 플롯을 생성하고 싶다고 가정해 봅시다:

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
g = sns.FacetGrid(zoo_data, col="species", col_wrap=4, height=3)
```

이 코드는 각 열이 다른 동물 유형을 나타내며 네 개의 열에서 줄바꿈되고 각 플롯의 높이가 세 인치인 그리드를 초기화합니다.

플롯 매핑

이제 각 패싯에 플로팅 함수를 매핑할 수 있습니다. 예를 들어 각 동물의 수분 필요량을 시각화하려고 한다고 가정해 보겠습니다:

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
g.map(sns.barplot, "동물", "물 필요량")
```

각 그리드 면에 막대 그래프를 만들어동물별 물 필요량을 보여줍니다.

그리드 사용자 정의

마지막으로, 우리는 그리드를 사용자 정의하여 정보를 더 잘 전달하고 시각적으로 매력적으로 만들 수 있습니다.


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
# 각 패싯에 제목 설정
g.set_titles("{col_name}")
# 축 레이블 설정
g.set_axis_labels("동물", "수요량 (리터)")
# 범례 추가
g.add_legend()
# 플롯 보이기
plt.show()
```

다중 변수 시각화

동물의 몸 길이와 수요량을 시각화하는 FacetGrid를 만들어보겠습니다. 이는 동물의 종류와 줄무늬 여부에 따라 나뉩니다.

```js
# 그리드 초기화
g = sns.FacetGrid(zoo_data, row="줄무늬 여부", col="동물 종류", margin_titles=True)
# 각 패싯에 산점도 매핑
g.map(sns.scatterplot, "몸 길이", "물 수요량")
# 그리드 사용자 지정
g.set_titles(col_template="{col_name} 동물", row_template="줄무늬: {row_name}")
g.set_axis_labels("몸 길이 (센티미터)", "물 수요량 (리터)")
g.add_legend()
# 플롯 보이기
plt.show()
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

이 예시에서 그리드의 각 행은 줄무늬가 있는 동물과 없는 동물을 나타내고, 각 열은 다른 종류의 동물을 나타냅니다. 산점도는 이 하위 집합들 간의 몸 길이와 수분 필요량 사이의 관골을 보여줍니다.

# Seaborn과 Matplotlib의 시너지

Seaborn은 Matplotlib을 기반으로 구축되었으며, Pandas 데이터 구조와 긴밀하게 통합되어 있어 통계적 데이터 시각화에 이상적인 선택지입니다.

동물원 프로젝트에서 데이터를 시각화하여 실제 예시에 대해 자세히 살펴보겠습니다. 서로 다른 종, 서식지 및 다양한 특성 간의 관골을 탐색해 보겠습니다.

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

Zoo.cvs

<img src="/TIL/assets/img/2024-07-14-DataVisualizationwithSeaborn_6.png" />

```js
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
# 동물원 데이터셋 불러오기
zoo_data = pd.read_csv('zoo.csv')
# 데이터셋 개요 출력
print(zoo_data.head())
# 변수간 관계 시각화를 위한 페어플롯 생성
sns.pairplot(zoo_data, hue='species')
plt.show()
# 각 종의 water_need 분포 시각화
plt.figure(figsize=(12, 6))
sns.boxplot(x='species', y='water_need', data=zoo_data)
plt.title('종에 따른 수분 필요량')
plt.show()
# 서식지 면적을 바이올린 플롯으로 분석
plt.figure(figsize=(12, 6))
sns.violinplot(x='habitat_area', y='species', data=zoo_data, scale='width')
plt.title('종에 따른 서식지 면적 분포')
plt.show()
```

결과

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

위의 코드에서는 필요한 라이브러리를 가져와서 동물원 데이터셋을 로드하는 방식으로 시작합니다. 그런 다음 sns.pairplot()을 사용하여 변수 간의 쌍별 관계를 이해할 수 있게 해주는 산점도 그리드를 만들고, 종에 따라 색칠합니다. 그다음 sns.boxplot()을 사용하여 서로 다른 종 간의 수분 요구량 분포를 시각화하여 요구 사항에 대한 통찰을 얻습니다. 마지막으로 sns.violinplot()을 사용하여 각 종의 서식지 면적 분포에 대한 더 깊은 이해를 제공하며, scale='width' 매개변수가 각 바이올린이 동일한 폭을 갖도록 하며 데이터의 밀도를 나타냅니다.

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

Seaborn과 Matplotlib의 통합은 정교한 데이터 시각화를 만들기 위한 견고한 프레임워크를 제공합니다. 함께 사용하면 우리의 동물원 프로젝트 데이터를 통찰력 있고 매력적인 시각적 이야기로 변화시킬 수 있는 강력한 조합이 됩니다.

데이터 시각화 관련 기사:

Matplotlib를 이용한 데이터 시각화

Plotly를 이용한 데이터 시각화

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

![Image](/TIL/assets/img/2024-07-14-DataVisualizationwithSeaborn_10.png)