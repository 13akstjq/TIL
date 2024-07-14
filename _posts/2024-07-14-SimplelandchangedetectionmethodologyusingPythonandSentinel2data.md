---
title: "파이썬과 Sentinel 2 데이터를 활용한 간단한 토지 변화 감지 방법"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-SimplelandchangedetectionmethodologyusingPythonandSentinel2data_0.png"
date: 2024-07-14 20:26
ogImage: 
  url: /TIL/assets/img/2024-07-14-SimplelandchangedetectionmethodologyusingPythonandSentinel2data_0.png
tag: Tech
originalTitle: "Simple land change detection methodology using Python and Sentinel 2 data"
link: "https://medium.com/python-in-plain-english/simple-land-change-detection-methodology-using-python-and-sentinel-2-data-6b6cc4b86154"
---


# 소개

YouTube를 둘러보다가 남부 이집트의 Toshka 프로젝트에 대한 매료되는 영상을 발견했습니다. 그 지역 농업에 미친 영향에 흥미가 생겨 이 주제를 더 깊이 알아보았습니다. 프로젝트가 수자원을 활용하고 메마른 풍경을 비옥한 농지로 변화시키는 데 초점을 맞춘 것이 혁명적으로 다가왔습니다. 이 기사에서는 Toshka 프로젝트가 농업에 미치는 영향에 대해 탐구하고, 물과 농업 변화 감지의 간단하고 효과적인 방법을 통해 어떻게 영향을 미쳤는지 살펴보려 합니다. 이러한 접근 방식은 실용적인 것뿐만 아니라 종합적인 사례 연구에서 필수적이며, 실행 가능한 통찰을 제공하지 못할 수 있는 지나치게 복잡한 방법론을 피하기 위한 것입니다.

## 연구 지역

<img src="/TIL/assets/img/2024-07-14-SimplelandchangedetectionmethodologyusingPythonandSentinel2data_0.png" />

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

## 준비 사항

이 여정에서는 다음을 설치해야합니다:

- Python 3.10+
- 시각화를 위한 QGIS 3.X.X +

이 외에도 필요한 것들이 있습니다:

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

- Copernicus 사이트에서 다운로드할 데이터셋은 2024년 4월 말과 2017년의 두 Sentinel 2 2A 이미지를 사용할 예정입니다 (이미지 참조: T36QUL_20170223 및 T36QUL_20240227).
- 이 데이터는 Copernicus 플랫폼에서 다운로드할 수 있습니다. 데이터 다운로드에 대한 자세한 내용은 문서를 확인해보세요.

# 방법론

수면 및 식물의 변화를 모니터링하기 위해 여러 지수를 사용할 수 있습니다. NDVI는 식물 변화를 감지하는 데 적합하며, 정규화된 수면 차이 지수 (NDWI)는 수면의 변화를 탐지하는 데 효과적입니다.

아래는 채택할 수 있는 기본 방법론의 그림입니다:

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


![Image before significant advancement](/TIL/assets/img/2024-07-14-SimplelandchangedetectionmethodologyusingPythonandSentinel2data_1.png)

Let’s take a look at our images. Here is the image before the significant advancement in the project:

![Image before significant advancement](/TIL/assets/img/2024-07-14-SimplelandchangedetectionmethodologyusingPythonandSentinel2data_2.png)

And a recent image from last February of this year:


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

![이미지](/TIL/assets/img/2024-07-14-SimplelandchangedetectionmethodologyusingPythonandSentinel2data_3.png)

보시다시피 중요한 변화가 있습니다. 그래서 여기서의 질문은 이러한 변화를 어떻게 감지하고, 또 다른 수준에서 어떻게 양적으로 표현할 것인가 하는 것입니다. 이 프리젠테이션에서는 첫 번째 질문에 초점을 맞추어 농작물과 물에 대한 변화에 특히 집중할 것입니다.

# 실용적인 조치

## 1. 리포지토리(Clone) 복제

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

먼저 변경 감지 스크립트를 가져와 봅시다:

```js
git clone https://github.com/kokatic/rs-sentinel2-rf.git
```

가져와야 할 파일은 아래와 같습니다:

![이미지](/TIL/assets/img/2024-07-14-SimplelandchangedetectionmethodologyusingPythonandSentinel2data_4.png)

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

s2_ndvi_ndwi_changes.py를 사용할 것입니다. 스크립트를 간단히 살펴봅시다:

```js
import numpy as np
import rasterio
import matplotlib.pyplot as plt
from matplotlib.colors import ListedColormap

def read_band(file_path):
    """GeoTIFF 이미지에서 단일 밴드를 읽어 numpy 배열로 반환하고 메타데이터와 함께 반환합니다."""
    with rasterio.open(file_path) as src:
        band = src.read(1).astype('float32') # 첫 번째(유일한) 밴드 읽기
        meta = src.profile
    return band, meta

...

# 여러 함수와 주요 작업들이 정의되어 있습니다.

...

# 과제에 따라 ndwi_threshold 및 ndvi_threshold 값을 조정하는 등 특정 요구에 맞게 코드를 사용자 정의할 수 있습니다.

# 2. 가상 환경을 만들고 요구 사항을 설치하는 방법:
```

여러 함수와 주요 작업이 정의되어 있는 기본적인 스크립트이며, 특정 요구에 맞게 코드를 수정할 수 있습니다. 특히 ndwi_threshold 및 ndvi_threshold 값을 필요에 맞게 조정할 수 있습니다.

## 2. 가상 환경 만들기 및 요구 사항 설치:

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

현재 작업 폴더 디렉토리 내에서 명령줄 인터페이스를 열어 주세요:

```js
python -m venv venv
```

그런 다음 가상 환경을 활성화하세요:

Windows의 경우 (CMD):

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
venv\Scripts\activate
```

Linux에서는 다음을 실행하세요:

```js
source venv/bin/activate
```

그런 다음 필요한 패키지 설치하세요:

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
pip install -r requirements.txt
```

## 3. 스크립트 실행

스크립트를 실행해 보세요:

```js
python .\s2_ndvi_ndwi_changes.py
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

조금 시간이 걸리겠지만, 주요 변경 사항의 이미지를 matplotlib을 사용하여 내보낼 것입니다:

![image](/TIL/assets/img/2024-07-14-SimplelandchangedetectionmethodologyusingPythonandSentinel2data_5.png)

## 4. 결과 해석

각 지수를 독립적으로 확인해 봅시다:

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

NDWI 변화:

물이 이미 운하를 통해 토슈카 호수로 펌프되었다는 사실을 아래에서 확인할 수 있습니다:

![image](/TIL/assets/img/2024-07-14-SimplelandchangedetectionmethodologyusingPythonandSentinel2data_6.png)

NDVI 변화:

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

대박, 물은 생명을 부르죠! 이미지 속 2024년 새로운 피벗들이 2017년 이미지 위에 초록색으로 나타나 보이는 것처럼, 물이 성장을 촉진하는 방법을 보여줍니다.

![이미지](/TIL/assets/img/2024-07-14-SimplelandchangedetectionmethodologyusingPythonandSentinel2data_7.png)

두 가지 변경 사항: NDVI 및 NDWI

![이미지](/TIL/assets/img/2024-07-14-SimplelandchangedetectionmethodologyusingPythonandSentinel2data_8.png)

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

![이미지](/TIL/assets/img/2024-07-14-SimplelandchangedetectionmethodologyusingPythonandSentinel2data_9.png)

# 결론

NDVI 및 NDWI 지수를 사용한 분석은 식물 및 물체의 중요한 변화를 시간 경과에 따라 강조했습니다. 서로 다른 연도의 이미지를 비교하면 2024년에 신규 녹색 피벗이 2017년과 비교했을 때 농업 확장에 물 가용성이 미치는 영향을 명확히 보여줍니다. 이는 수자원이 농업 성장과 생태계 건강 유지에 중요한 역할을 한다는 것을 보여줍니다. 이러한 방법의 추가 세부 조정 및 지속적인 모니터링은 미래에 국가적 의사결정과 지속 가능한 자원 관리에 필수적일 것입니다.

# 팔로우 해주세요

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

- 내 랜딩 페이지
- 내 Youtube 채널
- 내 SaaS 서비스
- 내 GitHub

# 간단한 영어로 🚀

In Plain English 커뮤니티의 일원이 되어 주셔서 감사합니다! 떠나기 전에:

- 작가를 박수와 팔로우해주세요 ️👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠 확인하기