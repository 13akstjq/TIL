---
title: "데이터 사이언스 마스터를 위한 11가지 필수 Python Matplotlib 시각화"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-11EssentialPythonMatplotlibVisualizationsforDataScienceMastery_0.png"
date: 2024-07-14 19:59
ogImage: 
  url: /TIL/assets/img/2024-07-14-11EssentialPythonMatplotlibVisualizationsforDataScienceMastery_0.png
tag: Tech
originalTitle: "11 Essential Python Matplotlib Visualizations for Data Science Mastery"
link: "https://medium.com/top-python-libraries/11-essential-matplotlib-visualizations-for-data-science-mastery-4a4cb1995c47"
---


## 파이썬 맷플롯립 시각화

맷플롯립을 사용하면 파이썬으로 멋진 그림을 그릴 수 있어요. 데이터를 다양한 방식으로 표시할 수 있습니다. 많은 양의 코드가 필요하지 않아요. 선 그래프, 막대 차트 및 여러 종류의 차트를 만들 수 있어요.

# 간단한 선 그래프

- figures 와 axes: 맷플롯립에서 figure 는 상자 같아요. 플롯의 모든 부분을 보유해요. axes 는 figure 내부의 더 작은 상자처럼 생겼어요. 주변에 선들과 플롯의 의미를 알려주는 단어들이 있어요.
- 변수 명명: Figure 객체는 일반적으로 `fig`로 이름을 짓고, axes 객체는 `ax`로 이름을 지어요.
- 데이터 플로팅: `ax.plot` 메서드를 사용하여 axes에 데이터를 플로팅하세요.
- Pylab 인터페이스: MATLAB과 유사한 인터페이스를 제공하여 자동으로 figure와 axes를 만들어줘요.
- 여러 선: 동일한 figure에 여러 선을 그리려면 `plot` 함수를 여러 번 호출하세요.

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
import numpy as np
import matplotlib.pyplot as plt
plt.style.use('seaborn-v0_8-whitegrid')
fig = plt.figure()
ax = plt.axes()
x = np.linspace(0, 10, 1000)
plt.plot(x, np.sin(x))
plt.plot(x, np.cos(x))
plt.show()
```

![image](/TIL/assets/img/2024-07-14-11EssentialPythonMatplotlibVisualizationsforDataScienceMastery_0.png)

# 간단한 산점도

- 무엇인가: 산점도는 순서가 없는 점들을 가지고 있습니다. 선 그래프는 점들이 연결되어 있습니다.
- 만드는 방법: 산점도를 그리려면 `plt.plot` 또는 `ax.plot`을 사용할 수 있습니다.
- 점 스타일: 특별한 코드로 점의 모양을 변경할 수 있습니다. 문서에 어떤 코드를 사용해야 하는지 설명되어 있습니다.
- `plt.scatter`의 재미있는 점: 각 점을 특별하게 만들 수 있습니다. 크기와 색상이 다를 수 있습니다. 이를 위해 데이터 세트를 사용합니다.
- 투명한 점: 점을 서로 겹치게 만들 때 `alpha` 설정으로 점을 투명하게 만들 수 있습니다.

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
import numpy as np
import matplotlib.pyplot as plt
plt.style.use('seaborn-v0_8-whitegrid')
rng = np.random.RandomState(0)
x = rng.randn(100)
y = rng.randn(100)
colors = rng.rand(100)
sizes = 1000 * rng.rand(100)
plt.scatter(x, y, c=colors, s=sizes, alpha=0.3,
            cmap='viridis')
plt.colorbar();
plt.show()
```

<img src="/TIL/assets/img/2024-07-14-11EssentialPythonMatplotlibVisualizationsforDataScienceMastery_1.png" />

# 오류 시각화

- 왜 중요한가: 과학에서는 오류를 올바르게 이해하는 것이 측정 자체보다 중요하다.
- 예를 들어: 허블 상수를 추측할 때 오류에 대해 고려해야 합니다. 이를 통해 서로 다른 측정 결과가 일치하는지 알 수 있습니다.
- 불확실함 표현: 측정 값을 보고할 때 얼마나 확신이 없는지도 함께 말해야 합니다 (예: 71±2.5 (km/s)/Mpc). 이렇게 하면 데이터를 더 잘 비교할 수 있습니다.
- 오류 표시: 차트에 오류를 넣으면 더 다양하고 정확한 정보를 얻을 수 있습니다.
- `fmt` 설정: 차트에서 선과 점이 어떻게 보이는지를 제어합니다. `plt.plot` 코드와 비슷한 역할을 합니다.
- `errorbar` 함수: 오차 막대 기호의 모양을 변경할 수 있게 해줍니다. 예를 들어, 가로로 만들어 진해지게 만들 수 있습니다.
- 자신만의 스타일: 측면 오차 막대, 단측 오차 막대 등에 대한 더 많은 `errorbar` 설정을 활용하여 차트를 원하는 대로 꾸밉니다.

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
import numpy as np
import matplotlib.pyplot as plt
plt.style.use('seaborn-v0_8-whitegrid')
x = np.linspace(0, 10, 50)
dy = 0.8
y = np.sin(x) + dy * np.random.randn(50)
plt.errorbar(x, y, yerr=dy, fmt='o', color='black',
             ecolor='lightgray', elinewidth=3, capsize=0)
plt.show()
```

<img src="/TIL/assets/img/2024-07-14-11EssentialPythonMatplotlibVisualizationsforDataScienceMastery_2.png" />

# 밀도 및 등고도 플롯

- 3D 데이터 시각화: 3D 데이터를 2D 차트로 표현하는 등고선 플롯이나 히트맵과 같은 차트 사용.
- 함수 사용: 등고선 플로팅에는 `plt.contour`, 채워진 등고선 플로팅에는 `plt.contourf`, 이미지 형태로 플로팅에는 `plt.imshow` 사용.
- 데이터 전처리: 1차원 배열로부터 2차원 그리드 생성하는 데 `np.meshgrid` 활용.
- 등고선 플로팅하기: `plt.contour`는 그리드와 높이 값을 나타내는 `x`, `y`, `z`가 필요.
- 선 스타일: 흑백 등고선 플롯에서 점선은 음수를, 실선은 양수를 의미.
- 컬러 맵: `cmap`을 활용하여 등고선 색상 지정. 예를 들어, `RdGy`는 군집 데이터에 적합.
- 컬러 바: 색상표를 만들기 위해 `plt.colorbar` 사용. 컬러 값의 범위를 보여줌.
- 채워진 등고선 플로팅: `plt.contourf`를 사용하여 등고선 플로팅을 채움. 간격이 적고 보기 좋음.


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
import numpy as np
import matplotlib.pyplot as plt
plt.style.use('seaborn-v0_8-whitegrid')
def f(x, y):
    return np.sin(x) ** 10 + np.cos(10 + y * x) * np.cos(x)
x = np.linspace(0, 5, 50)
y = np.linspace(0, 5, 40)
X, Y = np.meshgrid(x, y)
Z = f(X, Y)
plt.contourf(X, Y, Z, 20, cmap='RdGy')
plt.colorbar();
plt.show()
```

![Image](/TIL/assets/img/2024-07-14-11EssentialPythonMatplotlibVisualizationsforDataScienceMastery_3.png)

# 히스토그램, Binning 및 Density

- 기본 히스토그램: Matplotlib의 `hist()` 함수를 사용하여 빠르게 기본 히스토그램을 생성합니다.
- 매개변수 조정: `hist()`는 히스토그램 계산 및 표시를 조정하기 위한 매개변수를 제공합니다.
- 설정 변경: 더 이상 `normed`를 사용하지 마세요. 대신 정규화하기 위해 `density`를 사용하세요.
- 자신만의 스타일: `histtype='stepfilled'` 및 `alpha`를 사용하여 투명한 막대를 만들어서 서로 다른 데이터를 일치시키는 데 유용합니다.
- 자세히 알아보기: `plt.hist` 문서를 확인하여 변경할 수 있는 방법에 대해 더 배우세요.
- 숫자만 필요한 경우: 플롯 없이 히스토그램 숫자만 얻으려면 `np.histogram()`을 사용하세요.


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
import numpy as np
import matplotlib.pyplot as plt
plt.style.use('seaborn-v0_8-whitegrid')
x1 = np.random.normal(0, 0.8, 1000)
x2 = np.random.normal(-2, 1, 1000)
x3 = np.random.normal(3, 2, 1000)
kwargs = dict(histtype='stepfilled', alpha=0.3, density=True, bins=40)
plt.hist(x1, **kwargs)
plt.hist(x2, **kwargs)
plt.hist(x3, **kwargs);
plt.show()
```

<img src="/TIL/assets/img/2024-07-14-11EssentialPythonMatplotlibVisualizationsforDataScienceMastery_4.png" />

# 사용자 정의 범례

- 중요성: 범례는 시각화에 의미를 부여하여 다양한 차트 요소를 명확하게 해줍니다.
- 키 만들기: 이름으로 차트 부분에 키를 만들려면 `plt.legend()`를 사용합니다.
- 다양한 설정: 키가 위치하게 할 곳을 선택하세요. 상자를 제거하세요. 여러 행을 사용하세요. 둥근 모서리 또는 그림자를 사용하세요. 투명도와 단어 간 간격을 조절하세요.
- 표시할 항목 선택: 키에 들어갈 부분과 이름을 지정하세요. 이를 수행하려면 'plot'이 제공하는 것을 설정하세요.
- 다수의 선: `plt.plot`은 여러 개의 선을 제공할 수 있습니다. 원하는대로 키를 만들려면 `plt.legend()`에 제공할 선을 선택하세요.
- 부분에 이름 지정: 차트 부분에 직접 이름을 지정하세요. 그런 다음 키에 그 이름을 넣어서 쉽게 식별할 수 있도록 하세요.

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
import numpy as np
import matplotlib.pyplot as plt
plt.style.use('seaborn-v0_8-whitegrid')
x = np.linspace(0, 10, 1000)
y = np.sin(x[:, np.newaxis] + np.pi * np.arange(0, 2, 0.5))

plt.plot(x, y[:, 0], label='first')
plt.plot(x, y[:, 1], label='second')
plt.plot(x, y[:, 2:])
plt.legend(framealpha=1, frameon=True)
plt.show()
```

![Custom Colorbars](/TIL/assets/img/2024-07-14-11EssentialPythonMatplotlibVisualizationsforDataScienceMastery_5.png)

# 사용자 정의 컬러바

범례 vs. 컬러바: 범례는 이산적인 레이블을 나타내는 반면, 컬러바는 연속값-색상 관계를 나타냅니다.


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

색상 막대 만들기: `plt.colorbar()`을 사용하여 색상 막대를 만듭니다.

칼라맵: `cmap` 매개변수를 사용하여 칼라맵을 지정합니다.

칼라맵 자원: 모든 사용 가능한 칼라맵은 `plt.cm` 모듈에서 찾을 수 있습니다. 칼라맵을 나열하려면 IPython의 탭 자동완성을 사용하세요.

칼라맵 유형:

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

- a. 연속 색상지도: `binary` 또는 `viridis`와 같은 단일 연속 색상 시리즈입니다.
- b. 발산 색상지도: `RdBu` 또는 `PuOr`와 같은 두 가지 대조되는 색상 시리즈입니다.
- c. 질적 색상지도: `rainbow` 또는 `jet`과 같이 연속적이지 않은 색상 조합입니다.

```js
import numpy as np
import matplotlib.pyplot as plt
plt.style.use('classic')
x = np.linspace(0, 10, 1000)
I = np.sin(x) * np.cos(x[:, np.newaxis])
plt.imshow(I, aspect='auto', cmap='viridis')
plt.colorbar()
plt.show()
```

![Visualization](/TIL/assets/img/2024-07-14-11EssentialPythonMatplotlibVisualizationsforDataScienceMastery_6.png)

# 다중 서브플롯

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

- 컨셉: Matplotlib은 데이터를 옆으로 나란히 표시하기 위해 단일 그림에서 여러 서브플롯을 허용합니다.
- 서브플롯 생성: `plt.axes()`를 사용하여 서브플롯을 생성하고, 위치와 크기를 `[왼쪽, 아래, 너비, 높이]`의 리스트 매개변수로 지정합니다.
- 기본 동작: `plt.axes()`는 보통 전체 그림 영역을 채우는 축 객체를 만듭니다.
- 정렬: `plt.subplot()`을 사용하여 그리드 안에 서브플롯을 생성하고, 행, 열 및 서브플롯 인덱스에 대한 매개변수를 사용합니다.
- 다중 행/열: `plt.GridSpec()`를 사용하여 여러 행 또는 열에 걸쳐있는 서브플롯을 만들고, 그리드 레이아웃을 정의합니다.
- GridSpec 사용법: `plt.GridSpec()`은 그리드 레이아웃을 정의하고, 서브플롯 생성 시 `plt.subplot()`에 전달할 수 있습니다.
- GridSpec 특징: 직접 차트를 만들지는 않지만 서브플롯 그리드 레이아웃을 정의합니다.

```js
import numpy as np
import matplotlib.pyplot as plt
plt.style.use('seaborn-v0_8-whitegrid')
mean = [0, 0]
cov = [[1, 1], [1, 2]]
x, y = np.random.multivariate_normal(mean, cov, 3000).T
# GridSpec를 사용하여 그리드를 생성하고 서브플롯을 추가합니다
fig = plt.figure(figsize=(6, 6))
grid = plt.GridSpec(4, 4, hspace=0.2, wspace=0.2)
main_ax = fig.add_subplot(grid[:-1, 1:])
y_hist = fig.add_subplot(grid[:-1, 0], xticklabels=[], sharey=main_ax)
x_hist = fig.add_subplot(grid[-1, 1:], yticklabels=[], sharex=main_ax)
# 주요 서브플롯에 산점도를 플롯합니다
main_ax.plot(x, y, 'ok', markersize=3, alpha=0.2)
# x와 y 방향으로 히스토그램을 플롯합니다
x_hist.hist(x, 40, histtype='stepfilled',
            orientation='vertical', color='gray')
x_hist.invert_yaxis() # x축 히스토그램의 y축 방향을 반전합니다 (우측 하단에 위치)
y_hist.hist(y, 40, histtype='stepfilled',
            orientation='horizontal', color='gray')
y_hist.invert_xaxis() # y축 히스토그램의 x축 방향을 반전합니다 (상단 좌측에 위치)
plt.show()
```

<img src="/TIL/assets/img/2024-07-14-11EssentialPythonMatplotlibVisualizationsforDataScienceMastery_7.png" />

# 텍스트 및 주석

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

- 목적: 시각화는 데이터 뒤에 숨겨진 이야기를 독자에게 전달하기 위해 노력합니다.
- 표현: 일부 차트는 그 자체로 이야기를 전달하지만, 다른 차트는 텍스트 지원이 필요합니다.
- 주석: 차트에 텍스트 주석을 추가하여 독자의 주의를 끌고 이해를 돕습니다.
- 텍스트 배치: 차트의 특정 위치(`x`, `y`)에 텍스트를 추가하려면 `plt.text` 또는 `ax.text`를 사용하세요.
- 가이드: 정보 전달을 향상시키기 위해 텍스트 주석을 통해 독자를 주요 차트 기능으로 안내하세요.

```js
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import matplotlib.dates as mdates
plt.style.use('seaborn-v0_8-whitegrid')
# 샘플 데이터셋 생성
dates = pd.date_range(start='1969-01-01', end='1988-12-31', freq='D')
data = np.random.randint(3000, 5000, size=len(dates))
births = pd.DataFrame(data, columns=['births'], index=dates)
fig, ax = plt.subplots(figsize=(12, 4))
births_by_date = births.pivot_table('births', [births.index.month, births.index.day])
births_by_date.index = [pd.Timestamp(year=2012, month=month, day=day) for month, day in births_by_date.index]
births_by_date.plot(ax=ax)
# 선 플롯의 특정 위치에 주석 추가
style = dict(size=10, color='gray')
ax.text(pd.Timestamp('2012-1-1'), 3950, "New Year's Day", **style)
ax.text(pd.Timestamp('2012-7-4'), 4250, "Independence Day", ha='center', **style)
ax.text(pd.Timestamp('2012-9-4'), 4850, "Labor Day", ha='center', **style)
ax.text(pd.Timestamp('2012-10-31'), 4600, "Halloween", ha='right', **style)
ax.text(pd.Timestamp('2012-11-25'), 4450, "Thanksgiving", ha='center', **style)
ax.text(pd.Timestamp('2012-12-25'), 3850, "Christmas", ha='right', **style)
# 제목과 y축 레이블 설정
ax.set(title='USA births by day of year (1969-1988)',
       ylabel='average daily births')
# x축에 월 레이블을 중앙에 정렬
ax.xaxis.set_major_locator(mdates.MonthLocator())
ax.xaxis.set_minor_locator(mdates.MonthLocator(bymonthday=15))
ax.xaxis.set_major_formatter(plt.NullFormatter())
ax.xaxis.set_minor_formatter(mdates.DateFormatter('%b'))
plt.show()
```

# 커스텀 눈금

- 눈금 변경: Matplotlib의 보통 눈금은 대부분의 차트에 적합합니다. 그러나 일부를 위해 변경해야 할 수도 있습니다.
- 물건 속의 물건: Matplotlib의 모든 차트 부분은 물건입니다. 도표에는 축이 있습니다. 축에는 차트 요소가 있습니다.
- 축 요소: 축에는 `xaxis`와 `yaxis`가 있습니다. 이들은 선, 눈금, 이름 등을 가지고 있습니다.
- 새 버전: Matplotlib 2.0 이후로 긴 축에 작은 눈금이 숨겨지게 되었습니다. 이것은 `xlim`과 `ylim`으로 변경할 수 있습니다.

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
import numpy as np
import matplotlib.pyplot as plt

plt.style.use('seaborn-v0_8-whitegrid')
fig, ax = plt.subplots()
x = np.linspace(0, 3 * np.pi, 1000)
ax.plot(x, np.sin(x), lw=3, label='Sine')
ax.plot(x, np.cos(x), lw=3, label='Cosine')
ax.grid(True)
ax.legend(frameon=False)
ax.axis('equal')
ax.set_xlim(0, 3 * np.pi)
plt.show()
```

<img src="/TIL/assets/img/2024-07-14-11EssentialPythonMatplotlibVisualizationsforDataScienceMastery_8.png" />

# Matplotlib에서 3D 플롯 만들기

- 2D에서 3D로: Matplotlib은 처음에는 2D 플롯만 지원했지만, 3D 플롯 도구는 버전 1.0에서 소개되었습니다.
- 3D 도구 모음: 3D 플로팅은 Matplotlib과 함께 제공되는 `mplot3d` 도구 모음을 로드하여 수행됩니다.
- 3D 플롯 유형: 3D 축을 활성화한 후 3D 라인 및 산점도와 같은 다양한 3D 플롯을 그릴 수 있습니다.
- 3D 플로팅 함수: 3D 라인 플롯 및 산점도 플롯에는 각각 `ax.plot3D` 및 `ax.scatter3D`를 사용합니다.
- 투명도와 깊이: 깊이를 나타내기 위해 산점도 플롯에는 다양한 투명도가 적용됩니다.
- 대화식 뷰: 대화식 뷰를 통해 더 나은 3D 시각 경험을 제공합니다.
- 3D 버프 맵: `mplot3d` 도구 모음에는 3D 버프 맵을 만드는 도구가 있습니다. 이것들은 2D `ax.contour`와 유사합니다.
- 3D 표면 차트: 표면 차트는 색의 변화로 3D 표면을 보여줍니다. 와이어 차트보다 더 나은 시각적 효과를 제공합니다.
- 그리드 데이터 이미지: 그리드 데이터에서 생성된 3D 이미지에는 와이어 차트와 표면 차트가 있습니다. 데이터를 3D 모양에 표시하여 더 명확하고 설득력있게 만듭니다.
- 틱 스타일 변경: 일반 틱 스타일은 대부분의 작업에 적합하지만, 사인 및 코사인 차트와 같은 특정 차트에 대해 변경해야 할 수 있습니다.
- 사용자 정의 예제: 틱이 위치하고 어떻게 보이는지 변경하면 차트가 무엇을 의미하는지 더 잘 보여줄 수 있습니다. 데이터에 딱 맞게 맞출 수 있습니다.


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


import numpy as np
import matplotlib.pyplot as plt
fig = plt.figure()
ax = plt.axes(projection='3d')
def f(x, y):
    return np.sin(np.sqrt(x ** 2 + y ** 2))
x = np.linspace(-6, 6, 30)
y = np.linspace(-6, 6, 30)
X, Y = np.meshgrid(x, y)
Z = f(X, Y)
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_zlabel('z')
# Flatten the X, Y, Z arrays for plot_trisurf
ax.plot_trisurf(X.flatten(), Y.flatten(), Z.flatten(),
                cmap='viridis', edgecolor='none')
plt.show()


![Image](/TIL/assets/img/2024-07-14-11EssentialPythonMatplotlibVisualizationsforDataScienceMastery_9.png)

저는 "Medium에서 팔로워를 빠르게 얻는 법"에 대한 전자책을 쓰고 있어요. 왜냐하면 제가 최고의 증거이기 때문이죠 – 딱 한 달 만에 5,000명 이상의 팔로워를 얻었어요. 기대해 주세요!

Substack에서 "GPT 소개" 시리즈를 쓰고 있어요. 관심 있으시면 팔로우 해주세요!


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

총 10개의 글 중 일곱 번째가 이미 완료되었어요!

독서📖, 형광펜 강조🖍️, 박수👏, 댓글💬, 공유🗣️해 주셔서 감사합니다. "Medium 친구"로서, 매일 작가 친구들에게 호의를 돌려드릴 수 있도록 최선을 다하고 있어요.

또한, 뉴스레터📰를 구독하여 이와 같은 멋진 콘텐츠가 올라올 때마다 알림을 받을 수 있어요. 고맙구나, 친애하는 챔프!🤓

최신 파이썬 이야기를 계속해서 받아보려면 Substack에서 저희와 연결해주세요. 함께 파이썬의 미래를 함께 만들어요!

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

# 즐겨보세요