---
title: "파이썬 데이터 분석을 위한 반드시 알아야 할 10가지 판다스 기술"
description: ""
coverImage: "/TIL/assets/no-image.jpg"
date: 2024-07-14 20:38
ogImage: 
  url: /TIL/assets/no-image.jpg
tag: Tech
originalTitle: "10 Must-Know Pandas Techniques for Python Data Analysis"
link: "https://medium.com/top-python-libraries/10-must-know-pandas-techniques-for-python-data-analysis-16b4c562cdd7"
---


## 판다스(Pandas)로 파이썬 데이터 분석 능력을 향상시켜요

안녕하세요! 오늘은 Pandas를 사용하여 데이터 분석 능력을 향상시키는 방법에 대해 알려 드릴게요!

# 1. 빠른 데이터 가져오기 및 미리보기

```python
import pandas as pd
# "data.csv"라는 파일이 있다고 가정합니다
df = pd.read_csv('data.csv')
# 첫 5개 행을 미리보기, 마치 미지를 드러내는 것처럼
print(df.head())
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

이 단계는 새 서점에 들어가서 목차를 훑는 것과 비슷합니다.

DataFrame은 판다스의 핵심입니다. `read_csv`를 사용하면 CSV 파일을 테이블로 변환할 수 있고, `head()`를 사용하면 미리보기를 볼 수 있습니다.

# 2. 데이터 정리: 누락된 값 처리

```js
# 누락된 값 확인
print(df.isnull().sum())
# 누락된 값들을 해당 열의 평균값으로 채우기
df.fillna(df.mean(), inplace=True)
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

데이터 정리는 먼지가 쌓인 보석을 닦는 것과 같아요. `isnull().sum()`은 먼지가 있는 곳을 보여주는데, `fillna`은 보석을 깨끗이 닦는 도구에요. 평균을 활용해서 닦습니다.

# 3. 선택과 필터링: 정확한 대상 선정

```js
# 'Age' 열 선택
age_column = df['Age']
# 나이가 25보다 큰 행 필터링
adults = df[df['Age'] > 25]
```

슈퍼마켓 통로를 걷고 있는 것처럼 상상해봐요. `df[‘Age’]`은 라벨을 확인하는 것처럼, `df[df[‘Age’] > 25]`는 기준을 충족하는 항목을 고르는 것과 같아요.

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

# 4. 탐색적 데이터 분석: 기술 통계

```js
# 개요를 위한 기술 통계
print(df.describe())
```

이것은 건강 진단과 같아요. 평균값, 최댓값 등을 보여줘서 데이터 상태를 알려줘요.

# 5. 데이터 병합: 퍼즐 게임

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
# 가정: df2는 또 다른 DataFrame이라고 가정합니다.
merged_df = pd.concat([df, df2], axis=0)  # 수직으로 쌓기
merged_df = pd.merge(df, df2, on='공통_컬럼')  # 공통 열을 기준으로 조인

```

데이터 병합은 퍼즐 조각을 맞추는 것과 비슷합니다. `concat`은 쌓고, `merge`는 키 열을 기반으로 결합합니다.

# 6. 그룹화: 패턴 찾기

```js
# 성별로 그룹화하여 평균 나이 계산
average_age_by_gender = df.groupby('성별')['나이'].mean()
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

그룹화는 데이터 마이닝의 시작입니다. 비슷한 항목을 그룹화하고 그들의 평균 무게를 측정하는 것과 같아요. `groupby`는 분석에 깊이를 더해줍니다.

## 7. 데이터 형태 변형: 큐브 회전

```js
# 다른 관점을 위해 데이터 전치
df_transposed = df.T
# 유연한 분석을 위한 피벗 테이블
pivot_table = df.pivot_table(index='Gender', values='Age', aggfunc='mean')
```

데이터 형태 변형은 루빅 큐브를 돌리는 것과 같아요. `T`는 측면을 보여주고, `pivot_table`은 다차원 분석을 위한 사용자 정의 관점을 제공해줘요.

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

# 8. 조건부 필터링: 정밀 타겟팅

```js
# Filter rows where age is between 25 and 30 and city is 'New York'
filtered_df = df[(df['Age'] > 25) & (df['Age'] < 30) & (df['City'] == 'New York')]
```

조건부 필터링은 마치 목표를 정확하게 조준하는 것과 같아요. 논리 연산자 `&` (그리고)와 `|` (또는)를 사용하여 필요한 데이터를 정밀하게 찾을 수 있어요.

# 9. 데이터 정렬: 누가 선두일까요?

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
# 나이를 오름차순으로 정렬합니다
sorted_df = df.sort_values(by='나이')
# 혹은 내림차순으로 정렬할 수도 있습니다
sorted_df_desc = df.sort_values(by='나이', ascending=False)
```

정렬은 마치 경쟁에서 순위 매기는 것과 같아요. `sort_values`는 지정된 열을 기준으로 데이터를 정렬해주는데, `ascending=False`는 가장 빠른 선수들을 보여줘요.

# 10. 데이터 시각화: 즉각적인 통찰

판다스로 직접 제공되지는 않지만, 맷플롯립이나 시본과 결합하면 데이터가 생동각을 불어넣어줄 수 있어요!


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


import matplotlib.pyplot as plt
# 나이에 대한 히스토그램 그리기
df['Age'].hist()
plt.title('나이 분포')
plt.show()


시각화는 데이터 분석의 하이라이트에요. 히스토그램, 산점도... `hist()` 함수를 사용하면 한눈에 나이 분포 패턴을 확인할 수 있어요. 제목을 추가하고 차트를 표시하면, 여러분의 이야기가 살아날 거예요!

이 10가지 판다스 팁은 데이터 분석을 위한 10가지 마법 주문 같아요. 이를 마스터하면 데이터 바다를 탐험하며 더 많은 비밀을 발견할 수 있어요. 이제 파이썬을 실행하고 데이터를 춤추게 해보세요!

최신 인공지능 이야기에 대한 소식을 받으려면 Substack에서 저희와 연락하세요.함께 인공지능의 미래를 만들어가요!


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

저희와 함께 파이썬 이야기를 업데이트 받으려면 Substack에서 연락을 유지하세요. 함께 파이썬을 배워봐요!