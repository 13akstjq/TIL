---
title: "Python 덕분에 250만 원 절약한 방법 그리고 큰 골칫거리를 피한 비결"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-PythonSavedMeOver2500andaMajorHeadache_0.png"
date: 2024-07-14 20:20
ogImage: 
  url: /TIL/assets/img/2024-07-14-PythonSavedMeOver2500andaMajorHeadache_0.png
tag: Tech
originalTitle: "Python Saved Me Over 2500 and a Major Headache"
link: "https://medium.com/@willkeefe/python-saved-me-over-2500-and-a-major-headache-52a0d1095b92"
---


<img src="/TIL/assets/img/2024-07-14-PythonSavedMeOver2500andaMajorHeadache_0.png" />

약 1년 반 전에 나는 남동 펜실베니아 소도시에서 나시빌, 테네시로 일하기 위해 이사했습니다. 그 지역에서는 어릴 때 한 지역에서 자랐고 다른 지역에서 엔지니어링 역할로 일했던 경험이 많았습니다. 당시 치과의사(이웃 포함)를 좋아했고, 함께 쌓아온 관계와 상생을 대신할만한 사람을 찾는 데 어려움을 겪었습니다. 다른 미국 사람들처럼 나도 치과에 대한 작은 두려움을 가지고 있고, 치과용 픽스와 드릴에 대해 생각하면 맥박이 조금 빨라집니다. 나시빌로 이사한 후에 새로운 임대 계약을 체결하고 주소를 변경하고 새 은행과 일반 의료 제공자를 구하며 그러한 성인들이 필요로 하는 새로운 자원을 찾아다니는 과정에서 나는 권장되는 6개월 간격의 치과 클리닝을 약간 소홀히 하게 되었습니다. 결국 나는 내 치과 보험 웹사이트를 살펴보아 우편번호에서 20분 이내 거리에 있는 새로운 제공자를 찾았고 결과에 압도당했습니다.

그건 엄청난 양이며, 게다가 MetLife 웹사이트는 실제로 나에게 필요한 데이터량을 제공하지 않아 결과를 필터링하기에 내게 큰 어려움을 줍니다. 대부분의 환자들에게는 필터링을 위한 제공된 기준이 중요하지 않을 것이기 때문입니다. 아래에 예시가 있습니다.

<img src="/TIL/assets/img/2024-07-14-PythonSavedMeOver2500andaMajorHeadache_1.png" />

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

일반적인 식별 정보인 이름 및 연락 정보뿐만 아니라 신규 환자, 언어, 교육 등에 대한 세부 정보도 포함되어 있습니다. 대부분의 경우 추가 필터는 경계 조건을 강조하며, 필터가 비영어권 사용자나 특정 전문 분야를 선호하는 사람들에게 적합한 경우가 있지만, 주로 "신규 환자 접수"가 모두가 활성화하기를 원할 유일한 필터입니다. 또 다른 불평은 특정 실천법이나 회사를 제거하는 것과 같이 집계용 필터가 없다는 것입니다. 저는 지역과 강한 평판을 가진 "맘앤팝" 치과 의사들을 국가적인 치과 의사 대기업보다 선호하며, 주관적으로 더 많은 쿠키 커터 및 비인간적인 상황을 경험했습니다. 책임을 물을 리뷰도 표시되지 않습니다!

몇 달 전 가장 가까운 치과의사에게 예약을 했는데, 그는 현재 신규 환자를 받고 있었고, 그곳은 대규모 체인의 사무실이었습니다. 불행히도, 최고의 경험을 하지 못했습니다. 치과 위생사가 치은을 닦는 동안 조금 거칠었고 나에게 따뜻함을 느끼지 못했습니다. 그럼 의사가 나에게 내 엑스레이 사진을 가져왔습니다. 서로 인접한 둘의 모알이가 뿌리 치료 및 치아 왕관으로 교체되어야 한다고 생각했습니다. 정말 깜짝 놀랐습니다! 저는 통증이나 민감도, 그 곳에 대한 부정적인 느낌이 없었습니다. 엑스레이는 뿌리에서 검은 선이 보였는데, 그에게 "뿌리 채과나 안녕과 상관없이 추가적인 부패일 수 있지만 뿌리 치과로 대체되어야 한다"고 말했습니다. 어차피 어떤 치료 전 단계로서 뿌리 치료가 거의 마지막 단계라고 하자, 증상이 치료를 정당화할 때까지 어떤 치료도 보류하겠다고 말했습니다.

지난 달, 제 몇 개의 어금니가 아프다는 느낌을 느끼기 시작했습니다. "문제 치아"뿐만 아니라 전체적으로 입안이 괴로웠습니다. 저는 항상 수면 중에 이감요증이라는 습관이 있었고 아마 이것이 휴일 스트레스와 너무 많은 여행을 관통했을 수도 있습니다. 이번에도 두 번째 방문한 같은 치과 의사는 다른 변화를 의심하지 않았지만 이제 빨리 뿌리치료를 강조하며 다음 주에 시간이 있는지 물어보았습니다. 저는 연말에 시간이 날 것이며 견적을 요청했습니다.

보험 없이는 수술, 노동 및 재료에 대해 1만 달러 이상의 요금을 부담해야 했을 것이며, 이는 당신의 고용주가 제공하면 보험 가입이 중요함을 미국의 모든 독자들에게 알리는 교훈이되어야 합니다. 제 보험으로는 그 금액이 2,500 달러로 줄었습니다. 여전히 엄청난 금액이지만 이전 숫자보다 훨씬 부담이 적었습니다. 또한 내 보험에 직접 청구된 몇 가지 의심스러운 요금을 발견했습니다. 그것을 "사기"라고 단정하진 않지만, 절대적인 긴급 상황이 아닌 한 돌아가지 않기로 결정했습니다. 그 후 다른 치과를 찾기로 결정했습니다.

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

새 의료 공급업체를 찾는 가장 좋은 방법이 무엇인가요? 이 문제에 대한 충고를 열 명의 다른 사람에게 물어보면 열 가지 다른 답변을 받을 것 같아요. 저는 먼저 지역 사람들이 신뢰하는 사람과 내 보험을 받는 사람에 대해 알고 싶었어요. 추천 사항을 공유하는 지역 주민을 위한 페이스북 그룹에 가입했어요. 삼 개의 서류를 전화로 확인해 보았는데 모두 내 보험을 받지 않았어요. 그 후 나시빌에 몇 년간 살았던 동료에게 선호하는 의료 공급업체에 대해 물어봤어요 — 그래도 즉시 의견을 얻을 수 없었어요. 많은 사무실들이 단순히 연휴로 인해 휴무 중이었고 1월 중후 다시 오픈할 예정이었어요. 이웃들에게 선호하는 곳을 물어봤더니 마침내 제가 찾던 답을 얻었어요. 그 사이트에는 내 보험을 받는다고 명시되어 있었지만 전화한 결과 새해에는 더 이상 보험을 받지 않는다고 밝혀졌어요. 어이쿠.

전화와 웹사이트를 번갈아가며 다섯-여섯 일 동안의 시간을 보낸 후, 나에게 가장 적합한 치과의사를 찾는 나만의 해결책을 찾을 수 있는지 보기 위해 다시 보험사의 웹사이트를 방문하기로 결정했어요.

## 웹사이트 읽기

제공자를 식별하기 위한 도구를 구축하는 첫 번째 단계는 보험사의 웹사이트가 어떻게 작동하는지 이해하는 것이에요. Chrome 화면 (또는 비슷한 명령을 갖춘 다른 선택한 브라우저) 내에서 마우스 오른쪽 버튼을 클릭하고 시트를 "검사"하는 것으로 할 수 있어요. 구체적으로는 "Fetch/XHR" 선택에서 JSON 또는 유사한 데이터 기반의 쿼리와 응답을 찾으며 이것들을 발견할 수 있어요.

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

<img src="/TIL/assets/img/2024-07-14-PythonSavedMeOver2500andaMajorHeadache_2.png" />

다양한 응답을 살펴보면 실제 창에서 보았던 결과에 대한 의미 있는 데이터를 찾아냈어요. "API" 호출의 내용이 환자들에게 표시된 결과를 채우는 것을 발견했지요. 구체적으로, 각 치과 의사에 대한 데이터를 볼 수 있었고, 실제 고객에게 표시된 것보다 더 많은 데이터가 백엔드에 있었어요. 엄청난 일이지요! 리뷰까지도 존재했어요! 쿼리에서 인식하기 어려운 나쁜 요소들이나 제 참여를 원하지 않는 실천단체들을 제거하는 것이 얼마나 유용한지 알 수 있었겠죠. 여기에는 응답에서 선택 가능한 기준의 완전한 목록이 있어요:

```js
Index(['links', 'item.number', 'item.self', 'item.name.firstName',
       'item.name.middleName', 'item.name.lastName', 'item.name.suffix',
       'item.businessName', 'item.acceptsNewPatients', 'item.genderCode',
       'item.typeCode', 'item.specialties', 'item.degrees', 'item.networks',
       'item.languages', 'item.distance.value', 'item.distance.unitCode',
       'item.distance.unitDescription', 'item.phoneNumbers', 'item.addresses',
       'item.locationCoordinate', 'item.emails', 'item.websites',
       'item.extension.facilityNumber', 'item.extension.preferredName',
       'item.extension.visitHours',
       'item.extension.isSpecialNeedsCareAvailable',
       'item.extension.hasHospitalAdmitPrivileges',
       'item.extension.isProviderAvailableByReferral',
       'item.extension.admitPrivilegedHospitals',
       'item.extension.nationalProviderIdentifier',
       'item.extension.isBoardCertified', 'item.extension.licenseNumber',
       'item.extension.hasHandicapAccess', 'item.extension.vendors',
       'item.extension.memberOfNetwork',
       'item.extension.isYelpCacheRefreshEnabled', 'item.genderDescription',
       'item.typeDescription', 'metadata.lastModifiedDate',
       'item.extension.yelpReviews.providerCode',
       'item.extension.yelpReviews.businessIdentifier',
       'item.extension.yelpReviews.businessName',
       'item.extension.yelpReviews.self',
       'item.extension.yelpReviews.reviewCount',
       'item.extension.yelpReviews.rating',
       'item.extension.businessIdentifier', 'item.extension.localmedURL'],
      dtype='object')
```

다음 과제는 Python으로 이 과정을 시뮬레이션하여 1,500개 이상의 결과 중 25개씩이 아니라 전부를 추출하고 필터링 및 조사할 수 있는 포괄적인 데이터 세트를 작성하는 것이었어요.

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

비교적 간단하게 해결할 수 있습니다. 필요한 몇 가지 라이브러리만 사용하면 됩니다. 아래에서 해당 라이브러리를 구현해보겠습니다.

```js
import requests
import json
import pandas as pd
import time
```

다음으로는 웹 사이트에 발행할 로드를 미리 입력해야 합니다. 이를 위해 쿠키, 헤더 및 JSON 데이터를 구성해야 합니다. 이 작업은 수동으로 수행하는 것이 까다로울 수 있습니다. Chrome은 간단한 "파이썬으로 페이로드 복사" 기능을 제공하지 않지만, 검사 화면에서 API 응답을 선택하고 마우스 오른쪽 단추로 클릭하여 "curl로 복사"할 수 있습니다. 그런 다음 온라인 웹 서비스인 curl converter를 사용하여 이를 파이썬으로 변환할 수 있습니다.

<img src="/TIL/assets/img/2024-07-14-PythonSavedMeOver2500andaMajorHeadache_3.png" />


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

위의 표를 Markdown 형식으로 변경해 주세요.

웹사이트마다 모든 쿠키와 헤더가 필요한 것은 아니지만, 저희는 단순히 우리 연결로부터 웹사이트가 일반적으로 받는 것을 모사하고 있기 때문에, 다른 웹사이트에 대한 쿼리를 구성하는 데 조정이 필요할 수 있다는 점을 참고해 주세요. 또한 웹 스크레이핑은 책임있게 수행되어야 합니다 — 즉, 윤리적이고 적절한 허가를 받은 경우에 진행되어야 하며, 응답 서버를 악의적으로 과도하게 무리하게 하는 일이 없어야 합니다. 그러나 몇 분에 걸쳐 각각의 데이터를 선택하면, 이 웹사이트에서 한 번 선택하는 것만으로는 일반적인 응답을 클릭하는 것보다 큰 영향을 미치지 않았습니다.

다음 스크립트를 사용하여 Python으로 데이터를 검색할 수 있습니다.

```js
쿠키 = {
  #쿠키 내용 입력
}

헤더 = {
  #헤더 내용 입력
}
데이터_리스트 = []
for i in range(0, 27):
    페이지 = 50 * i
    json_데이터 = {
        'operation': 'dispatch',
        'data': {
            'origin': '/findDentist?searchType=findDentistMetLife&networkID=1&zip=37209&qsType=DPPO',
            'screenId': 'us-metlocator-screen',
            'viewId': 'view-us-metlocator-container',
            'eventName': 'clickPagination',
            'eventData': '/v1/tenants/{tenantId}/providers/search?product.typeCode=200&product.nameCode=202&networks.typeCode=N001&locationCoordinate&radius.value=25&radius.unitCode=301&limit=50&'+f'offset={페이지}&orderBy=distance.value,name.lastName,name.firstName',
        },
    }

    응답 = requests.post('https://providers.online.metlife.com/findDentist/api', cookies=쿠키, headers=헤더, json=json_데이터)
    print(len(응답.text))
    데이터 = 응답.json()
    제공자 = 데이터['presentation']['views']['update'][1]['model']['application']['responseFromPagination']['items']
    df = pd.json_normalize(제공자)
    데이터_리스트.append(df)
```

위의 코드에서 출력을 offset을 채워가며 페이지를 반복하고 있는 것을 알 수 있습니다. 처음 50개 결과 이후에는 offset을 50으로 설정하여 그 다음 50개, 100개, 150개 등을 보려 합니다. 모든 결과를 반복하려면 필요한 총 페이지 수를 통해 범위를 설정할 수 있습니다. 각 응답마다 JSON 응답을 pandas 데이터프레임으로 정규화하고 모든 응답의 목록을 작성하고 있습니다. pandas 데이터프레임은 우리 목적에 따라 필터링하고 조작할 수 있는 테이블과 동의어입니다. 우리가 필요한 데이터만 추출하는 프로세스는 각 웹사이트의 응답에 따라 맞춤화되어야 합니다. 이 작업은 노트북에서 시행착오가 필요할 수 있습니다!

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
마스터 = pd.concat(list_of_dfs) 
마스터
```

그런 다음에 우리는 모든 응답을 하나의 큰 데이터프레임으로 연결할 수 있어요. 보여지는 결과는 적절한 데이터프레임이 어떻게 보일지에 대한 예시를 보여줘요.

<img src="/TIL/assets/img/2024-07-14-PythonSavedMeOver2500andaMajorHeadache_4.png" />

이전에 지정한 응답에서 모든 열을 선택했다는 점을 주목해 주세요! 우리는 이 값들을 다루어 우리가 적절하다고 생각하는 데이터를 필터링하거나 데이터셋을 확장하기 위해 이 데이터셋을 결합할 수 있어요. 제 경우에는 Yelp 리뷰와 카운트를 문자열 응답에서 숫자로 변환하고 5점 평점을 가진 치과 의사만 보기 위해 필터를 추가했어요.

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
master['item.extension.yelpReviews.rating'] = pd.to_numeric(master['item.extension.yelpReviews.rating'])
master['item.extension.yelpReviews.reviewCount'] = pd.to_numeric(master['item.extension.yelpReviews.reviewCount'])
master[master['item.extension.yelpReviews.rating']==5]
```

제가 수천 명의 치과의사 목록을 42명까지 줄였어요. 그후에 해당되지 않는 전문분야를 수동으로 제외하고 새 환자를 받는 병원을 선택했고, 계속 진행하려 했던 병원을 찾았어요. 그리고 구글 리뷰 시스템을 참고한 결과, 해당 병원은 5성급 평가를 받은 리뷰가 300개가 넘었고 전화를 걸었어요. 새 환자를 받고 있었을 뿐만 아니라 다음 주에 예약을 잡을 수 있었어요!

치과 진료 날이 드디어 다가왔어요. 병원으로 들어가자마자 매우 친절한 스태프와 안심할 수 있는 치위생사에 의해 맞이받았어요. 추가적인 엑스레이를 찍었고, 진단에 대한 두 번째 의견을 듣기 위해 치과의사를 만났어요. 의사는 의심스러운 치아 어느쪽에 대해서도 뿌리 채움을 권장하지 않았을 뿐만 아니라, 채우는 것도 하고 싶지 않았어요! 안도가 되는 순간이었어요! 엑스레이를 확인한 후, 그는 채우기물의 색상이 약간 다른 것은 단지 혼성재료의 유형 때문에 뿐이라고 믿었고, 그 이후 많은 경험으로 이에 대해 처리해 온 상황들이라했어요. 심지어 그는 치아조각도 만지고 싶지 않았어요! 그러나 그는 뚜렷한 씹고 이물질이 생겼다는 증거를 발견했고, 이것이 일반적인 예민성을 높이고 있다는 것을 알아채고, $2,500에 비해 $50으로 만들어진 입냄새보호대를 처방하고 싶어했어요. 난 동의했고, 빠른 장착을 위해 3D 스캔을 했고 서류에 서명했어요. 그 후로 따뜻한 수건과 선물 가방을 받고 길을 나가기 전에 이야기를 나누었어요. 아무래도 이 보수적인 방법이 나에게 편안해요, 가다가 한날 채움물을 더 교체하는 엄청난 절차를 필요로 할 때일지라도 말이에요. 나중에 6개월간의 재방문을 똑같은 제공 업체로 잡았어요.

모든 날이 파이썬을 사용해 문제를 해결할 수 있는 것은 아니지만, 일상적인 문제를 해결하도록 데이터 과학을 활용할 수 있게 되어 있다면 항상 신나는 일이에요. 여기서 사용한 방법과 문제 해결 프로세스를 사용하여 더 나은 의료 전문가를 찾고, 불편을 피하며, 당신에게 관련된 정보에 대한 품질과 접근성을 향상시킬 수 있기를 바랍니다. 또한 보험회사에 당신의 상황에 적절한 정보를 제공하도록 요청하여 우리가 건강에 대해 판단할 수 있도록 하고, 그에 따라 우리의 삶도 더 나아질 수 있기를 바라요!

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

위의 것과 다르게 하고 싶은 점이 있으면 자유롭게 아래에 알려주세요. 그리고 LinkedIn에서 연결하고 네트워킹을 하고 싶어요!