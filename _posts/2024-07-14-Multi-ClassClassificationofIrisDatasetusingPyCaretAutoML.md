---
title: "PyCaret Auto ML로 아이리스 데이터셋에서 다중 클래스 분류하는 방법"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_0.png"
date: 2024-07-14 20:31
ogImage: 
  url: /TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_0.png
tag: Tech
originalTitle: "Multi-Class Classification of Iris Dataset using PyCaret Auto ML"
link: "https://medium.com/tech-tensorflow/multi-class-classification-of-iris-dataset-using-pycaret-auto-ml-9c688017a7f4"
---


![이미지](/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_0.png)

Scikit Learn은 수년간 가장 인기 있는 머신러닝 라이브러리였습니다. 그러나 이제는 우리의 작업을 더 쉽게 만들어 줄 새로운 라이브러리를 사용할 때가 왔습니다.

이 글에서는 Pycaret라는 Auto ML 라이브러리를 소개하려고 합니다. 저는 PyCaret 라이브러리를 사용하여 붓꽃 데이터셋에서 분류를 수행하는 방법을 설명하겠습니다.

이 글에서 사용된 머신러닝 소스 코드는 다음에서 확인하실 수 있습니다: github.com

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

아래 링크를 통해 PyCaret에 대해 더 자세히 알아보세요:

## PyCaret이 뭐죠?

PyCaret은 머신러닝 모델 개발 전체 과정을 자동화하는 저코드, 오픈소스 파이썬 머신러닝 라이브러리입니다. PyCaret의 목표는 대부분의 작업을 자동화하여 머신러닝 모델의 개발, 배포 및 관리의 효율성을 증가시키는 것이며, 사용자가 매우 적은 코드를 사용하는 것입니다. PyCaret은 코드 작성 없이 복잡한 작업을 완료할 수 있기 때문에 초심자부터 전문가까지에게 좋은 옵션입니다.

공식 문서: PyCaret

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

# 아이리스 데이터셋

분석을 시작하기 전에, 데이터셋을 살펴봅시다.

아이리스는 통계 및 머신 러닝에서 잘 알려진 데이터셋입니다.

![img](/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_1.png)

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

1936년, 영국 수학자이자 생물학자인 로날드 피셔는 그의 논문 "The Use of Multiple Measurements in Taxonomic Problems"에서 판별 분석을 통한 이를 설명하기 위해 제시했습니다.

이 데이터셋은 150개의 붓꽃 샘플과 각각의 네 가지 특징인 꽃받침의 길이, 꽃받침의 너비, 꽃잎의 길이, 그리고 꽃잎의 너비로 구성되어 있습니다. 타깃 변수는 붓꽃의 종입니다:

- 부채붓꽃 (Irissetosa)
- 세토사붓꽃 (Irisversicolor)
- 버지니카붓꽃 (Irisvirginica).

# Auto ML 절차 — 단계별 설명

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

이 기사에서는 다음 단계를 따라갑니다:

시작해 봅시다!

## 단계 1: 라이브러리 가져오기 및 데이터셋 불러오기

먼저, 필요한 라이브러리를 가져와서 PyCaret을 사용하여 Iris 데이터셋을 로드해야 합니다.

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
# pycaret 설치
pip install pycaret
```

```python
# 필요한 라이브러리 가져오기
from pycaret.datasets import get_data
from pycaret.classification import *
import pandas as pd

# 데이터 불러오기
data = get_data('iris')
```

## 단계 2: 탐색적 데이터 분석 (EDA)

EDA는 데이터를 이해하는 데 도움이 됩니다. 먼저 대상 변수의 분포, 요약 통계 및 데이터 세트의 처음 몇 행을 살펴보겠습니다.

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
#데이터셋의 처음 몇 행 표시
print(data.head())
```

표는 데이터셋의 처음 몇 행을 보여줍니다. 각 붓꽃의 측정값과 종을 확인할 수 있습니다.

```js
#데이터셋의 요약 통계
print(data.describe())
```

요약 통계는 각 기능의 분포를 보여줍니다. 평균, 표준편차, 백분위 등이 포함됩니다.

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
# 타겟 변수 분포
print(data['species'].value_counts())
```

데이터셋은 각 종별로 50개의 샘플로 완벽히 균형을 이룹니다.

## 시각화:

특성 간의 관계를 이해하기 위해 페어 플롯(pair plots) 및 박스 플롯(box plots)과 같은 시각화를 사용할 수 있습니다.

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

표 태그를 Markdown 형식으로 변경하겠습니다.

## Pair plot

![Pair plot](/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_2.png)

Pair plot은 각 특성 쌍 간의 관계를 종에 따라 색칠하여 보여줍니다. 꽃잎 길이와 꽃잎 너비와 같은 몇 가지 특성 쌍은 종 간에 명확한 분리를 보여줍니다.

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

## 상자 그림

![Box Plot](/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_3.png)

상자 그림은 각 특징이 서로 다른 종에 대해 분포를 보여줍니다. 이는 서로 다른 종에 대해 몇 가지 특징이 구별되는 분포를 가지고 있음을 나타내며, 이는 분류에 유용할 수 있습니다.

## 단계 3: 환경 설정

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

다음으로, PyCaret 환경을 설정해보겠습니다. 이 단계에는 데이터셋과 대상 변수를 지정하는 과정이 포함됩니다.

```js
clf = setup(data, target='species', session_id=123, log_experiment=True, experiment_name='iris_classification')
```

- setup(): PyCaret 환경을 초기화하며, 범주형 변수의 인코딩, 누락된 값의 보간, 데이터를 학습 및 테스트 세트로 분할하는 전처리 작업을 처리합니다.
- data: 사용할 데이터셋입니다.
- target: 예측하고자 하는 대상 변수입니다.
- session_id: 재현성을 위한 랜덤 시드입니다.
- log_experiment: 실험 로깅을 활성화합니다.
- experiment_name: 실험에 이름을 지정합니다.

## 단계 4: 모델 학습 및 선택

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

PyCaret은 여러 모델을 쉽게 학습하고 비교할 수 있게 해줍니다. compare_models() 함수를 사용하여 최상의 성능을 낼 모델을 찾을 수 있습니다.

이전에 models() 명령어로 사용 가능한 모든 모델을 확인할 수 있습니다.

![image](/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_4.png)

목록을 확인한 후에는 데이터 및 해당 특징에 기반하여 비교할 필요가 있는 모델을 선택합니다.

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

여기서는 Random Forest (rf), Decision Tree (dt), Light Gradient Boosting Machine (lightgbm), Extra Trees Classifier (et) 및 Extreme Gradient Boosting (xgboost)와 같은 트리 기반 모델을 비교하기 위해 선택했습니다.

![image](/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_5.png)

compare_models() 함수는 기본 하이퍼파라미터를 사용하여 여러 모델을 학습하고 정확도, 정밀도, 재현율 및 F1 점수를 포함한 다양한 메트릭을 사용하여 성능을 평가합니다. 결과에 따라 가장 우수한 성능을 보이는 모델을 반환하고 최상위 모델을 강조합니다.

## 단계 5: 모델 평가

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

최적 모델을 선택한 후, 다양한 메트릭과 시각화를 사용하여 모델의 성능을 평가합니다.

![이미지](/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_6.png)

`evaluate_model()`은 혼동 행렬, ROC 곡선, 분류 보고서와 같은 모델 성능을 평가하기 위한 여러 메트릭과 시각화 도구를 제공합니다.

이것은 `evaluate_model()`에 의해 생성된 의사결정 경계 플롯입니다.

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

<img src="/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_7.png" />

## 단계 6: 모델 튜닝

tune_model() 함수를 사용하여 모델의 하이퍼파라미터를 조정하여 성능을 향상시킬 수 있습니다.

<img src="/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_8.png" />

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

선택한 모델의 하이퍼파라미터를 최적화하여 성능을 향상시킵니다. 최적의 하이퍼파라미터 조합을 찾기 위해 그리드 탐색 또는 랜덤 탐색 방법을 사용합니다.

최종 모델 파이프라인은 다음과 같습니다

![이미지](/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_9.png)

## 단계 7: 플로팅과 시각화

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

모델 성능과 특성 중요도를 시각화하기 위해 PyCaret은 다양한 플로팅 함수를 제공합니다.

![Multi-Class Classification of Iris Dataset using PyCaret AutoML](/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_10.png)

생성된 혼동 행렬은 모델의 성능을 실제 양성, 실제 음성, 거짓 양성 및 거짓 음성 측면에서 시각화합니다.

![Multi-Class Classification of Iris Dataset using PyCaret AutoML](/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_11.png)

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

이 그림은 예측을 하는 데 각 기능의 중요성을 나타냅니다. 이것은 어떤 기능이 가장 영향력이 있는지 이해하는 데 도움이 됩니다.

조정된 모델의 최적 매개변수는 다음과 같습니다:

![그림](/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_12.png)

plot_model() 함수는 모델 성능과 해석 가능성을 평가하는 시각화를 생성합니다.

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

## 단계 8: 모델 해석

모델을 해석하는 것은 모델이 예측을 어떻게 생성하는지 이해하는 데 중요합니다. 모델의 출력은 SHAP(Shapley Additive exPlanations) 값을 사용하여 설명됩니다. 이를 통해 각 피쳐가 모델에 의해 만들어진 예측에 어떤 영향을 미치는지에 대한 소중한 관점을 제공합니다.

![image](/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_13.png)

`interpret_model()`은 SHAP 값을 사용하여 피쳐 중요도와 모델 예측에 대한 통찰을 제공합니다.

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

## 단계 10: 모델 예측

튜닝된 모델의 예측 전체적인 정확도를 확인할 수 있습니다.

![이미지](/TIL/assets/img/2024-07-14-Multi-ClassClassificationofIrisDatasetusingPyCaretAutoML_14.png)

보시다시피, 97.7%의 정확도를 얻었습니다. 좋은 결과입니다.

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

## 단계 10: 모델 배포

마지막으로, 모델을 저장하여 나중에 사용하고 다시 불러오는 방법을 시연할 수 있습니다.

```js
# 모델 저장하기
save_model(tuned_model, 'best_ensembled_model')
# 모델 불러오기
loaded_model = load_model('best_ensembled_model')
```

여기서, save_model()은 훈련된 모델을 디스크에 저장하고, load_model()은 디스크에서 저장된 모델을 불러오는 데 사용됩니다.

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

# 결론

이 프로젝트에서는 PyCaret을 사용하여 아이리스 데이터셋을 분류했습니다. PyCaret은 로우코드 방법론과 다양한 기능 세트로 효율적으로 머신러닝 모델을 생성하고 구현하는데 훌륭한 도구입니다.

제 블로그를 읽어 주셔서 감사합니다!!! 이 글이 여러분께 새로운 것을 배울 수 있는데 도움이 되었으면 좋겠어요!

가기 전에, 이미 하지 않았다면:

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

- ⭐️ 만약 이에 쏟은 시간과 노력에 감사하시다면, 제 팔로우와 박수👏 부탁드립니다. 감사합니다🙌
- ⭐️ 피드백을 댓글로 남겨주세요! 🙂
- ⭐️ 제 YouTube 채널 구독해주세요. 새 동영상이 곧 업로드됩니다!
- ⭐️ Github, Github, LinkedIn을 팔로우해주세요. 어떤 것이든 상관없습니다.
다음에 또 봐요!