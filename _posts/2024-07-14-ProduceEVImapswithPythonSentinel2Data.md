---
title: "Sentinel 2 데이터를 이용한 Python으로 EVI 지도 생성하는 방법"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-ProduceEVImapswithPythonSentinel2Data_0.png"
date: 2024-07-14 23:46
ogImage: 
  url: /TIL/assets/img/2024-07-14-ProduceEVImapswithPythonSentinel2Data_0.png
tag: Tech
originalTitle: "Produce EVI maps with Python , Sentinel 2 Data"
link: "https://medium.com/@koka-tic/produce-evi-maps-with-python-sentinel-2-data-e5d584f9b0f9"
---


# TL;TR

증강 식물 지수 (EVI)는 원격 감지에서 널리 사용되는 분광 지수로, 위성 영상을 통해 식물 건강 및 영역을 평가하는 데 사용됩니다. 일반화된 차이 식물 지수 (NDVI)를 개선하여 대기 영향을 최소화하고 수관 구조 변화 및 밀집 식물 지역에 대한 민감도를 향상시켰습니다.

## EVI의 주요 특징:

- 식물에 대한 민감도: EVI는 대기 영향을 최소화하고 수관 특성에 대한 민감도를 증가시킴으로써 특히 밀집 식물 지역에서 효과적으로 작동하도록 설계되었습니다.
- 공식: EVI 공식은 빨강, 근적외선 (NIR), 파랑 분광대를 포함하여 식물에 대한 민감도를 개선합니다.

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

<img src="/TIL/assets/img/2024-07-14-ProduceEVImapswithPythonSentinel2Data_0.png" />

여기서:

- NIR은 근적외선 대역의 반사율입니다.
- Red는 빨간 대역의 반사율입니다.
- Blue는 파란 대역의 반사율입니다.
- G는 이득 요소입니다 (기본값: 2.5).
- C1 및 C2는 대기의 영향을 최소화하기 위한 계수입니다 (기본값: 각각 6 및 7.5).
- L은 수관 배경 조정 요소입니다 (기본값: 1).

값의 범위: EVI 값은 일반적으로 -1에서 1 사이의 범위를 갖습니다.

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

- 높은 값은 밀도가 높은 식물을 나타냅니다.
- 0 또는 음수에 가까운 값은 비식물 또는 희소식물이 자리 잡은 지역을 나타냅니다.

## 응용:

- 식물 모니터링: EVI는 식물 동태를 모니터링하는 데 널리 사용되며 성장 패턴, 건강 평가, 그리고 환경 요소 또는 인간 활동으로 인한 변화 감지에 도움이 됩니다.
- 토지 이용 및 토지 피복 매핑: 특히 식물로 지배되는 토지 이용 유형을 매핑하고 분류하는 데 도움이 됩니다.
- 생태학 연구: 식물 분포를 분석하는 데 유용하며, 생물 다양성 평가 및 서식지 매핑에 활용됩니다.

## 장점:

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

- 민감도 향상: NDVI와 비교하여 밀집 식물 지역의 변화에 대한 감도를 향상시켰습니다.
- 대기 보정: 대기 간섭을 줄이기 위한 계수를 포함하여 대기 보정을 통해 전 세계 및 지역 규모의 응용 프로그램에 더 견고하게 만들었습니다.

## 고려 사항:

- 데이터 요구 사항: 일반적으로 중고해상도 위성 플랫폼에서 널리 사용되는 빨강, NIR 및 파랑 밴드를 갖춘 다중 스펙트럼 위성 영상이 필요합니다.
- 매개변수 민감도: 계수 G, C1, C2 및 L의 선택에 민감하며 특정 환경 조건 및 연구 목표에 따라 조정이 필요할 수 있습니다.

## 선행 조건:

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

이 여정에서는 다음을 설치해야 합니다:

- Python 3.10+
- 시각화를 위한 QGIS 3.X.X +

또한 다음이 필요합니다:

- Copernicus 사이트에서 다운로드할 데이터 세트. 2024년 2월 27일 센티넬 2 2A 이미지로 작업할 예정입니다 (이미지 참조: T36QUL_20240227T082911).
- Copernicus 플랫폼에서 다운로드할 수 있습니다; 데이터 다운로드에 대한 문서를 확인할 수 있습니다.

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

# Sentinel-2 데이터를 활용한 실제 적용:

## 1. 저장소 복제

분류 스크립트 및 학습 샘플을 가져오세요:

```js
git clone https://github.com/kokatic/remote_sensing
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

이렇게 작성하시면 됩니다 :


<img src="/TIL/assets/img/2024-07-14-ProduceEVImapswithPythonSentinel2Data_1.png" />

내 레포지토리(/data/)에 데이터 폴더를 추가했습니다. 이 폴더에는 2024년 4월 30일 에르 우에드 지역의 Sentinel-2 데이터가 포함되어 있습니다. 기준 이미지는 T32SKC_20240430T102021이며, 밴드 4와 8을 포함하고 있습니다.

## 2. 가상 환경 생성 및 필수 모듈 설치


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

현재 작업 폴더 디렉토리에서 명령 줄 인터페이스를 엽니다:

```js
python -m venv venv
```

그런 다음 가상 환경을 활성화합니다:

Windows용 (CMD)에서:

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


venv\Scripts\activate # Linux의 경우: source venv/bin/activate


필요한 패키지 설치 후:


pip install -r requirements.txt


## 3. 스크립트 실행


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

동일한 폴더에서 스크립트를 실행하려면 터미널 또는 명령 프롬프트를 열고 스크립트가 있는 폴더로 이동한 다음 다음 명령을 실행하세요:

```js
import rasterio
import numpy as np

def calculate_evi(red_band_path, nir_band_path, blue_band_path, output_path):
    try:
        # rasterio 데이터셋으로 빨간색, NIR, 파란색 밴드 열기
        with rasterio.open(red_band_path) as red_dataset, \
             rasterio.open(nir_band_path) as nir_dataset, \
             rasterio.open(blue_band_path) as blue_dataset:

            # rasterio 데이터셋에서 데이터 읽기
            red = red_dataset.read(1, masked=True).astype('float32')
            nir = nir_dataset.read(1, masked=True).astype('float32')
            blue = blue_dataset.read(1, masked=True).astype('float32')

            # EVI 계산
            G = 2.5
            C1 = 6
            C2 = 7.5
            L = 1
            epsilon = 1e-6  # 0으로 나누기를 피하기 위한 작은 엡실론 값
            denominator = nir + C1 * red - C2 * blue + L
            evi = np.where(denominator != 0, G * ((nir - red) / denominator), np.nan)

            # EVI 값을 [-1, 1] 범위로 클리핑
            evi = np.clip(evi, -1, 1)

            # EVI 출력을 위한 프로필 업데이트
            profile = red_dataset.profile
            profile.update(dtype=rasterio.float32, count=1, compress='lzw')

            # 새 GeoTIFF 파일에 EVI 결과 작성
            with rasterio.open(output_path, 'w', **profile) as dst:
                dst.write(evi.astype(np.float32), 1)

    except Exception as e:
        print(f"오류가 발생했습니다: {e}")

# 예시 파일 경로
red_band_path = 'data/Toshka/S2A_MSIL2A_20240227T082911/T36QUL_20240227T082911_B04_10m.jp2'  # 빨간색 밴드
nir_band_path = 'data/Toshka/S2A_MSIL2A_20240227T082911/T36QUL_20240227T082911_B08_10m.jp2'  # 근적외선 밴드
blue_band_path = 'data/Toshka/S2A_MSIL2A_20240227T082911/T36QUL_20240227T082911_B02_10m.jp2'  # 파란색 밴드
output_path = 'output/evi.tif'  # EVI 출력 파일

calculate_evi(red_band_path, nir_band_path, blue_band_path, output_path)
```

스크립트를 실행한 후 output/ 폴더에 EVI 출력물을 찾을 수 있습니다:

![EVI 출력물](/TIL/assets/img/2024-07-14-ProduceEVImapswithPythonSentinel2Data_2.png)

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

3- 결과 확인: 시각화와 분석

- EVI 출력 파일 (evi.tif)을 QGIS에 로드합니다.
- 더 밀집한 식물 상층을 나타내는 더 높은 EVI 값의 시각화를 위해 적합한 컬러 팔레트를 적용합니다.
- EVI 값에 기반하여 관심 영역을 강조하기 위해 시각화 설정을 조정하여 식물 건강 평가 및 토지 피복 맵핑을 지원합니다.

![이미지1](/TIL/assets/img/2024-07-14-ProduceEVImapswithPythonSentinel2Data_3.png)

![이미지2](/TIL/assets/img/2024-07-14-ProduceEVImapswithPythonSentinel2Data_4.png)

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

요약하자면, 향상된 식물지수(EVI)는 원격 감지 응용 프로그램에 강력한 도구로, 식물 역학 및 건강 평가에 대한 감도를 향상시켜줍니다. Sentinel-2 위성 이미지와 Python 및 QGIS와 같은 컴퓨터 도구를 활용하여 연구원 및 실무자는 효과적으로 식물 변화를 모니터링하고 토지 피복 유형을 분류하며 생태학 연구 및 환경 관리에 기여할 수 있습니다. 더 자세한 내용이나 문의 사항이 있으시면 언제든지 댓글을 남기거나 직접 연락해주세요.

# 팔로우

- 내 랜딩 페이지
- 내 Youtube 채널
- 내 SaaS 서비스
- 내 GitHub