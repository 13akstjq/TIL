---
title: "지리 공간 빅 데이터를 활용한 짧은 영상 제작을 위한 긴 Python 스크립트"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-ALongPythonScripttoMakeaShortVideoUsingGeospatialBigData_0.png"
date: 2024-07-14 19:43
ogImage: 
  url: /TIL/assets/img/2024-07-14-ALongPythonScripttoMakeaShortVideoUsingGeospatialBigData_0.png
tag: Tech
originalTitle: "A Long Python Script to Make a Short Video Using Geospatial Big Data"
link: "https://medium.com/@mahyar.aboutalebi/a-long-python-script-to-make-a-short-video-using-geospatial-big-data-9b0de4a7e47f"
---



![이미지](/TIL/assets/img/2024-07-14-ALongPythonScripttoMakeaShortVideoUsingGeospatialBigData_0.png)

# 목차

- 🌟 소개
- 🔍 설정: 설치 및 라이브러리 임포트
- ⏳ "NetCDF" 파일의 쿼리 및 처리
- 🗺️ 캘리포니아 지역 및 NDVI 지도 플로팅을 위한 이미지 클리핑
- 🎥 NDVI 지도를 비디오로 내보내기
- 📝 결론
- 📚 참고 자료

## 🌟 소개


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

원하는 곳에서 매일 또는 매주 촬영된 위성 이미지를 기반으로 동영상과 애니메이션을 만드는 것을 즐기며 대규모 지리 공간 데이터를 시각화하는 일에 깊은 열정을 갖고 있습니다. 그러나 이 많은 양의 이미지를 처리하는 것은 다운로드, 클리핑, 위성 이미지 표시 등의 단계를 거치기 때문에 시간이 많이 걸립니다. 이 과정에는 API, 코딩, 지리 데이터에 대한 심도 있는 이해, 데이터 시각화에 대한 뛰어난 기술이 필요합니다. 이 이야기는 '위에 언급된 모든' 기술의 조합에 관한 것입니다.

다른 게시물에서 Sentinel-2 및 Sentinel-3 이미지의 시각화에 대해 작성하던 중, 이 중 하나의 위성이 특정 위치 위에서 촬영한 모든 이미지를 모아 애니메이션으로 제시하는 아이디어에 주목했습니다. 오늘은 이 비전을 실현하고 2023년 캘리포니아 위에서 캡처된 Sentinel-3의 NDVI 맵을 탐색해보는 날입니다!

이 이야기에서는 이러한 NDVI 맵을 처리, 시각화하고 애니메이션을 만들기 위해 Google Colab에서 강력한 Python 스크립트를 작성할 것입니다. 여기서는 이미지 다운로드 부분을 다루지 않습니다. 이미 별도의 이야기에서 자세히 다루었기 때문에 해당 링크에서 확인할 수 있습니다:

## 🔍 설정: 설치 및 라이브러리 가져오기

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

첫 번째 단계는 다음 폴더를 생성하는 것입니다: NetCDF, Geotiff, Geotiff_reproj, Geotiff_reproj_clipped, Shapefile, JPEG 및 Animation. 다음과 같이 수행할 수 있습니다:

```js
import os

# 폴더 이름 정의
folders = ['NetCDF', 'Geotiff', 'Geotiff_reproj', 'Geotiff_reproj_clipped', 'Shapefile', 'JPEG', 'Animation']

# 폴더 생성
for folder in folders:
    if not os.path.exists(folder):
        os.makedirs(folder)

# 확인 메시지 출력
print('폴더가 성공적으로 생성되었습니다!')
```

<img src="/TIL/assets/img/2024-07-14-ALongPythonScripttoMakeaShortVideoUsingGeospatialBigData_1.png" />

NetCDF 폴더는 Sentinel-3 NDVI의 raw 데이터를 저장합니다. 추가로, Geotiff, Geotiff_reproj, Geotiff_reproj_clipped 폴더는 래스터 파일, 재투영된 래스터 파일 및 래스터 파일 클립을 GeoTIFF 형식으로 저장하기 위해 생성됩니다 (모두 NetCDF 파일에서 변환된 형식). Shapefile 폴더에는 캘리포니아 형태 파일(우리의 관심 영역(AOI)로 Sentinel-3 이미지 클리핑을 위해)이 포함되어 있습니다. JPEG 폴더는 NDVI 맵을 ".jpg" 형식으로 저장하기 위해 생성되며, Animation 폴더는 최종 비디오를 저장하기 위해 특별히 생성되었습니다.

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

또한, Google Colab에 몇 가지 라이브러리를 설치하고 가져와야 합니다.

```js
pip install pandas rasterio netCDF4 rioxarray
```

```js
import os
import re
import sys
import random
from pathlib import Path

import requests
import json
import xml.etree.ElementTree as ET
import certifi

import pandas as pd
import numpy as np

import rasterio
import matplotlib.pyplot as plt
import matplotlib.image
from rasterio.windows import Window

import netCDF4 as nc

import rioxarray

from rasterio.control import GroundControlPoint
```

## ⏳ "NetCDF" 파일의 쿼리 및 처리

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

다음 단계는 쿼리를 제출하고 시작 날짜, 종료 날짜 및 AOI를 지정하는 것입니다. NDVI 지도는 토양 표면 온도 수준의 부수 데이터이므로, 여전히 레벨 변수로 "LST"를 사용할 수 있습니다. 미국의 다른 주에 대한 애니메이션을 만들고 싶다면, AOI 지점이나 폴리곤을 해당하도록 수정하세요. 또한, 시작 날짜와 종료 날짜로 2023년 5월 1일 및 8월 1일을 고려해보세요. 다음과 같은 줄로 이 작업을 수행할 수 있습니다:

```js
url_dataspace = "https://catalogue.dataspace.copernicus.eu/odata/v1"

# 필터링
satellite = "SENTINEL-3"
level= "LST"

aoi_point ="POINT(-121.669668 38.372428)"
#aoi_polygon = "POLYGON ((-121.0616 37.6391, -120.966 37.6391, -120.966 37.6987, -121.0616 37.6987, -121.0616 37.6391))"

start_date = "2023-05-01"
end_date = "2023-08-01"
start_date_full =start_date+"T00:00:00.000Z"
end_date_full = end_date +"T00:00:00.000Z"
```

이 정보를 통해 쿼리를 제출할 준비가 되었습니다. 데이터베이스에서 사용 가능한 이미지 목록을 내보내야 하므로, 쿼리 끝에 "&$top=1000&$expand=Attributes"를 포함해야 합니다. 이는 데이터베이스에서 이미지 내보내기의 기본 수가 20이며, 2023년에 Sentinel-3가 촬영한 이미지 수가 20을 초과하기 때문에 필요합니다:

```js
query = f"{url_dataspace}/Products?$filter=Collection/Name eq '{satellite}' and Attributes/OData.CSC.StringAttribute/any(att:att/Name eq 'productType' and att/OData.CSC.StringAttribute/Value eq '{level}') and OData.CSC.Intersects(area=geography'SRID=4326;{aoi_point}') and ContentDate/Start gt {start_date_full} and ContentDate/Start lt {end_date_full}&$top=1000&$expand=Attributes"
response = requests.get(query).json()
result = pd.DataFrame.from_dict(response["value"])

# 'Online' 열이 True인 레코드 필터링
result = result[result['Online'] == True]

# 처음 10개 결과 출력
result.head(10)

result.to_csv('result_LST.csv', index=False)
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

다음 단계에서는 온라인에서 사용 가능한 이미지 목록을 필터링하여 정오쯤 촬영된 이미지에 특히 주목할 것입니다. 파일의 감지 시간은 UTM을 기준으로 하기 때문에 18:00:00은 태평양 표준시 (캘리포니아 시간) 기준으로 대략 12:00:00 오후에 해당합니다:

```js
filtered_df = result[result["Name"].str.split("_").str[8].str.contains("T18") & result["Name"].str.split("_").str[14].str.contains("PS2") & result["Name"].str.split("_").str[16].str.contains("NT")]

filtered_df = filtered_df.reset_index(drop=True)

print(filtered_df['Name'])
```

```js
filtered_df.to_csv('result_LST_Filtered.csv', index=False)
```

필터링된 데이터프레임을 기반으로, 2023년 5월 1일부터 8월 1일까지 캘리포니아 상공에서 촬영된 Sentinel-3의 53장의 이미지가 있습니다.

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

<img src="/TIL/assets/img/2024-07-14-ALongPythonScripttoMakeaShortVideoUsingGeospatialBigData_2.png" />

이미지가 흐릿할 수 있다는 점을 기억하세요. 센티넬-3 이미지를 구름 덮개로 걸러내는 것을 제어할 수 없기 때문입니다.

다음 단계는 필터링된 목록의 제품 ID를 사용하여 NetCDF 파일을 다운로드하는 것을 포함합니다. 이 부분은 이미 다음 포스트의 섹션들(📥 "NetCDF" 파일 다운로드 및 🛠️ Geotiff로 변환)에서 설명된 자료의 중복을 피하기 위해 건너뛰었습니다:

## 🗺️ 캘리포니아를 위한 이미지 클리핑과 NDVI 지도 플로팅

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

이 단계를 자세히 설명하기 전에 다음 패키지를 설치하고 로드해주세요:

```js
pip install matplotlib-scalebar
```

```js
import os
import matplotlib.pyplot as plt
from matplotlib import colors
from mpl_toolkits.axes_grid1 import make_axes_locatable
from mpl_toolkits.axes_grid1.inset_locator import inset_axes
from matplotlib_scalebar.scalebar import ScaleBar
from matplotlib.offsetbox import OffsetImage, AnnotationBbox
from datetime import datetime
import rasterio.plot
```

이 단계에서는 Geotiff_reproj 폴더에 저장된 53개의 래스터 파일 각각을 읽어올 것입니다. 캘리포니아 쉐이프파일 투영과 래스터 파일 간의 일관성을 확인할 것입니다. 필요한 경우 쉐이프파일을 재투영할 것입니다. 그런 다음, 래스터 파일을 캘리포니아의 경계에 맞게 자릅니다. 해당 플롯을 플로팅하고 JPEG 폴더에 저장할 것입니다. 캘리포니아 쉐이프파일을 다운로드하려면 다음 URL을 방문하여 "California State Boundry"를 다운로드하고 Shapefile 폴더에 업로드해주세요:

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

[California Geographic Boundaries](https://data.ca.gov/dataset/ca-geographic-boundaries)

![Image](/TIL/assets/img/2024-07-14-ALongPythonScripttoMakeaShortVideoUsingGeospatialBigData_3.png)

The following code also requires an image for the north arrow symbol. You can either download one from the internet or, like me, draw it in PowerPoint, save it, and upload it to your main folder.

![Image](/TIL/assets/img/2024-07-14-ALongPythonScripttoMakeaShortVideoUsingGeospatialBigData_4.png)

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

맵에 이 아이콘이 표시되는 것은 완전히 선택 사항입니다. 맵에 북쪽 화살표를 표시하고 싶지 않다면 해당 섹션을 주석 처리할 수 있습니다:

```js
# 디렉토리 경로 설정
directory = '/content/Geotiff_reproj_clipped/'


# 디렉토리 내 모든 파일 목록 가져오기
files = os.listdir(directory)

# NDVI TIF 파일만 포함하는 목록 필터링
ndvi_files = [f for f in files if f.endswith('.tif')]


# 색상 한계 설정
vmin = 0
vmax = 1
ndvi_norm = colors.Normalize(vmin, vmax)
ndvi_cmap = plt.cm.RdYlGn
ndvi_cmap.set_bad(color='white')

# 각각의 NDVI 파일을 루프하여 플롯하기
for ndvi_file in ndvi_files:
    print(ndvi_file)
    date_str = ndvi_file.split('_')[0]
    date_obj = datetime.strptime(date_str, "%Y%m%d")
    formatted_date_str = date_obj.strftime("%Y/%-m/%-d")

    # NDVI 래스터 열기
    with rasterio.open(f"/content/Geotiff_reproj_clipped/{ndvi_file}") as ndvi:
      # shapefile 읽기
      california_shapefile = gpd.read_file('/content/Shapefile/CA_State_TIGER2016.shp')

      ndvi_data = ndvi.read(1).astype(float)
      ndvi_data[ndvi_data<=-1]=np.nan
      ndvi_crs = ndvi.crs
      extent = rasterio.plot.plotting_extent(ndvi)

      # shapefile을 래스터의 투영과 일치하도록 재투영
      california_shapefile = california_shapefile.to_crs(ndvi_crs)


    # 플롯 생성
    fig, ax = plt.subplots(figsize=(10, 6))

    
    image= rasterio.plot.show(ndvi_data, ax=ax, extent=extent, alpha=1, cmap=ndvi_cmap,norm=ndvi_norm,)
    im = image.get_images()[0]  # 컬러바에 대한 트릭 (rasterio.plot.show와 호환되지 않음)
    fig.colorbar(im, ax=ax)
   

    ax.set_title(formatted_date_str, fontsize=16,fontweight='bold')
    california_shapefile.boundary.plot(ax=ax, color='k', linewidth=1)

    ax.set_xticklabels([])
    ax.set_yticklabels([])

    # 북쪽 화살표 추가
    north_arrow_image = plt.imread('North_sign.png')
    imagebox = OffsetImage(north_arrow_image, zoom=0.25)
    ab = AnnotationBbox(imagebox, (0.8, 0.99), xycoords='axes fraction', box_alignment=(0, 1), frameon=False)
    ax.add_artist(ab)

    # 스케일 바 추가
    scalebar = ScaleBar(1000, 'm', length_fraction=0.2, location='lower right', font_properties={'size':12})
    ax.add_artist(scalebar)

    # 맵 주변에 네모 상자 끄기
    ax.axis('off')

    # 플롯을 JPEG 파일로 저장
    plt.savefig(os.path.join('/content/JPEG', ndvi_file.split('_')[0] + '.jpg'), dpi=300, bbox_inches='tight')

    # 메모리 해제를 위해 플롯 닫기
    plt.close(fig)
```

이 단계를 완료하면 "JPEG" 폴더에 센싱 날짜를 기준으로 저장된 53개의 NDVI 지도가 있게 될 것입니다:

<img src="/TIL/assets/img/2024-07-14-ALongPythonScripttoMakeaShortVideoUsingGeospatialBigData_5.png" />

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

## 🎥 NDVI 지도를 비디오로 내보내기

3개월 동안 센티넬-3에 의해 촬영된 53개의 NDVI 지도(대략 매 2일마다 1장씩)를 사용하여, 2023년 캘리포니아 지역 전체의 NDVI 패턴과 변화를 보여주는 애니메이션을 만들 수 있습니다. 이 비디오를 생성하려면, 다음 스크립트를 실행하세요:

```js
pip install pillow
```

```js
from PIL import Image
import os
import glob

# 'path/to/folder'를 여러분의 jpg 이미지가 저장된 폴더 경로로 교체하세요
folder_path = '/content/JPEG'
output_gif = os.path.join('/content/Animation', 'NDVI_animation.gif')

# 폴더 내 모든 jpg 파일의 정렬된 목록을 가져옵니다
file_list = sorted(glob.glob(os.path.join(folder_path, '*.jpg')))

# 이미지의 원하는 크기를 설정합니다
width, height = 1000, 1000

# 이미지를 읽어서 리스트에 저장합니다
frames = []
for file in file_list:
    frame = Image.open(file)

    # 이미지 크기 조정
    frame = frame.resize((width, height), Image.ANTIALIAS)

    # 이미지에 흰색 배경 추가
    background = Image.new('RGB', frame.size, (255, 255, 255))
    background.paste(frame)
    frame = background.convert('RGB')

    # 이미지를 P 모드로 변환하고 전역 색상 테이블을 사용합니다
    frame = frame.convert('P', palette=Image.ADAPTIVE, colors=256)
    frames.append(frame)

# 애니메이션 GIF로 프레임을 저장합니다
if frames:
    frames[0].save(
        output_gif,
        save_all=True,
        append_images=frames[1:],
        duration=500,  # 프레임 간 지속 시간(밀리초)을 설정합니다
        loop=0,  # 루프 횟수를 설정합니다 (0은 무한을 의미합니다)
        optimize=True,
    )
else:
    print("jpg 파일을 찾을 수 없습니다.")
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

작업이 완료되면 화면에서 만들어진 지퍼 영상을 다운로드하여 시청할 수 있어요!

![이미지](https://miro.medium.com/v2/resize:fit:1000/1*B8CQ1b8hgqEPrGj33oxrzQ.gif)

구름이 가려져 있기 때문에 지도 내 몇 군데는 흰색 영역으로 표시됩니다. 또한 특정 날짜에 캘리포니아의 모든 영역을 커버하지 않아 일부 이미지가 잘려 나와 있을 수 있습니다. 프레임이 너무 빨리 변경되는 것 같다면, 각 프레임 간의 지속 시간을 조절할 수 있어요. 긴 스크립트를 작성한 후에는 이 아름다운 짧은 애니메이션을 즐기시기 바랍니다.

## 📝 결론

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

요약하자면, 상상력을 데이터 시각화와 코딩을 통해 현실화하는 것은 때로는 꽤 많은 시간을 소요할 수 있습니다. 이 이야기에서는 50초짜리 비디오를 만들기 위해 300줄 이상의 코드를 작성했습니다. 상당한 시간 투자에도 불구하고, 그 노력은 가치가 있다고 믿습니다. 지도의 순차적인 표현은 숨겨진 패턴과 동적 변화를 드러내며 더 많은 탐구를 유도할 수 있습니다. 예를 들어, 이 이야기에서는 6월 이전의 NDVI 변동성 관측과 6월 중순부터 7월까지 안정화된 모습이 우리에게 추가적인 질문을 던지며 더 많은 조사와 답을 발견하도록 동기부여합니다.

## 📚 참고 자료

https://documentation.dataspace.copernicus.eu/APIs/OData.html

- 📱 더 많은 상호작용이 있는 콘텐츠를 위해 다른 플랫폼에서 저와 연결하세요! LinkedIn, ResearchGate, Github, 그리고 Twitter.

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

해당 링크를 통해 관련 게시물을 확인할 수 있습니다: