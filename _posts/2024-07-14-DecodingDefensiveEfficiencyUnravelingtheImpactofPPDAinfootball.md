---
title: "PPDA가 축구에서 어떤 영향을 미치는지 알아보는 방법 수비 효율성 분석"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-DecodingDefensiveEfficiencyUnravelingtheImpactofPPDAinfootball_0.png"
date: 2024-07-14 20:22
ogImage: 
  url: /TIL/assets/img/2024-07-14-DecodingDefensiveEfficiencyUnravelingtheImpactofPPDAinfootball_0.png
tag: Tech
originalTitle: "Decoding Defensive Efficiency Unraveling the Impact of PPDA in football"
link: "https://medium.com/dev-genius/decoding-defensive-efficiency-unraveling-the-impact-of-ppda-in-football-0a502a17ab46"
---


수비 조치 당 패스(Passes per defensive action)는 최근 몇 년 동안 등장한 가장 흥미로운 축구 통계 중 하나입니다.

# 목차

- 지표 소개
- 스크래핑
- 상관 관계
- 최종 리그 순위
- 결론

# 지표 소개

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

수비 조치 당당당시에 이용하며, 이후로 PPDA 라고 칭할 것입니다. PPDA 는 상대방이 공격 팀의 수비 없이 몇 번의 패스를 허용하면서 공을 볼 수 있는지를 측정하는 축구 통계치입니다. 낮은 PPDA 는 공을 보유하기를 원하는 팀이 높은 공격 스타일을 채택한다는 것을 나타냅니다. 이는 공격팀이 수비팀의 저항을 만나기 전에 더 적은 패스를 허용하는 것을 의미합니다. 그런 성격상으로 높은 PPDA 지표는 수비라인을 형성하고 어떤 경우에는 버스 주차 전술을 하는 팀을 나타냅니다!

저희가 필드에서 낮은 PPDA 와 높은 PPDA 의 숫자가 어떻게 시각적으로 보일지 알아보겠습니다.

저의 데이터셋에서 발견된 가장 극단적인 값인 PPDA 152의 예시를 소개하겠습니다! (처음에는 믿기 어렵죠, 첨부된 링크에서 원본 데이터를 확인할 수 있습니다) 2018/19 시즌 바이에른 뮌헨 대 한노버 경기에서 발생한 이 값에서는 한노버팀이 55분부터 10명으로 적혀 있었으나 그래도 그들의 PPDA 허용치는 그들이 굉장히 깊게 수비하고 총 점유율을 허용했다는 것을 나타낸다. 베이언에게는 712회의 총 패스가 이뤄지는 반면, 한노버에게는 310회의 패스만 이뤄졌다. 이 숫자와 관련하여 데이터 유효성 문제가 있을 수 있다고 의심하지만, 게임 하이라이트를 보면 한노버가 얼마나 깊게 수비하고 있는지, 그들의 포메이션이 얼마나 혼란스러운지 즉시 알 수 있습니다. 아래의 사진에서 한 노버의 선수 10명이 본인의 골대 내부에 있어 공을 처리하고 있는 코만에게 외곽 수비작전이라고 인식되는 모습입니다. 이 스냅샷은 전반에서 나온 것이라, 10명으로 줄어든 후에는 단순히 바이에른팀으로부터 가속력을 받지 않도록 하는 것만이 더욱 확실해졌을 것입니다.

![이미지](/TIL/assets/img/2024-07-14-DecodingDefensiveEfficiencyUnravelingtheImpactofPPDAinfootball_0.png)

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

데이터 소스에서 이 예시가 너무 극단적이기 때문에 현재 시즌 23/24에서 웨스트햄 대 맨체스터 시티 경기에서 다른 예시를 살펴봤어요. Understat에 따르면 웨스트햄은 맨체스터 시티에 대해 42.25의 PPDA를 기록했고 맨체스터 시티는 11.4를 기록했어요. Pep의 맨체스터 시티를 공격하려는 것은 대부분의 팀에 대해 좋은 아이디어가 아니기 때문에 웨스트햄은 반격하는 스타일을 선택하고 전반전에 1-0으로 앞서나갔어요. 골 이전에 우리는 수비수들로 이루어진 깊은 라인업을 볼 수 있었는데, 이것은 세계 최고의 팀에 대해 전혀 흔한 것입니다.

![이미지](/TIL/assets/img/2024-07-14-DecodingDefensiveEfficiencyUnravelingtheImpactofPPDAinfootball_1.png)

골 이후에도 계획은 변경되지 않았어요 (그리고 왜 변경되어야 할까요!) 이 시점까지는 영토와 패스를 헌 내어주는 것이 옳은 선택임이 입증되었어요. 맨체스터 시티는 빨리 공을 되찾았지만 전반전에는 파고들기 어려웠어요. 그 모든 것이 후반전에 바뀌기 시작했지만 이제 저 블록이 경기장에 어떻게 나타나는지 시각적으로 확인할 수 있고 대략적으로 그에 따른 PPDA 값은 30 이상입니다.

![이미지](/TIL/assets/img/2024-07-14-DecodingDefensiveEfficiencyUnravelingtheImpactofPPDAinfootball_2.png)

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

반면에 피피다(PPDA) 값이 낮은 것이 경기장에서 시각적으로 어떻게 나타나는지 살펴보겠습니다. 리버풀은 22/23 시즌 노팅엄 숲과의 홈 경기에서 리버풀의 PPDA 값이 2.53으로 기록되었고, 숲의 PPDA 값은 32.45로 나타났습니다. 이를 그림 형식으로 시각화하는 것은 어렵기 때문에 죄송하지만 리버풀은 내 DNA의 유튜브 비디오를 첨부하겠습니다. 클롭 아래의 리버풀의 엉뚱한 프레싱을 볼 수 있습니다.

그래서 이제 우리는 PPDA에 대해 모두 알았습니다. 이 지표의 높고 낮은 값이 경기장에서 실제로 어떻게 나타나는지를 살펴보겠습니다. 이 연습에서 저는 EPL, 라리가, 리그 1, 분데스리가, 세리에 A의 11시즌치 게임 데이터를 수집했습니다.

# 데이터 수집

이전에 언급한 대로 데이터 원천으로 Understat.com을 선택했습니다. 이 사이트는 선수권 선수궁 리그에 관한 포괄적인 무료 데이터 가이드이며 JSON 형식으로 상대적으로 쉽게 스크래핑할 수 있습니다. 다음 코드는 각 리그와 관련된 각 페이지를 브라우징하고 각 팀의 시즌 매치 데이터를 검색한 후 마지막에 약간의 정리를 통해 PPDA 및 PPDA 허용 메트릭스를 변환합니다. PPDA를 팀의 수비력이라고 생각하고 PPDA 허용을 그들이 프레스에 얼마나 잘 저항했는지로 생각할 수 있습니다.

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
import json
import numpy as np
from selenium.webdriver.chrome.service import Service
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
from time import sleep, time
import pandas as pd
import warnings
from bs4 import BeautifulSoup
import requests
import time
from tqdm import tqdm
import re

# 타이머 시작
start_time = time.time()

warnings.filterwarnings('ignore')

# URL을 저장할 빈 리스트 생성
base_urls = []
urls = []
df_data = []

seasons = [2014, 2015, 2016, 2017, 2018, 2019, 2020, 2021, 2022, 2023]

chrome_path = "C:/usr/local/bin/chromedriver.exe"

# 'La_Liga', 'Bundesliga', 'Ligue_1', 'Serie_A'

competitions = ['EPL', 'La_Liga', 'Bundesliga', 'Ligue_1', 'Serie_A']

for competition in competitions:
    for season in seasons:
        base_url = f'https://understat.com/league/{competition}/{season}'
        base_urls.append(base_url)

        season_col_value = int(base_url[-4:])
        competition_value = competition

        # Chrome 옵션 설정
        chrome_options = Options()
        chrome_options.headless = True

        # Chrome 서비스 생성
        service = Service(chrome_path)

        # 서비스와 옵션을 사용하여 Chrome 웹드라이버 생성
        driver = webdriver.Chrome(service=service, options=chrome_options)

        # URL로 이동
        driver.get(base_url)

        html_content = requests.get(base_url)
        # 'html_content'에 HTML 콘텐츠가 있는 것으로 가정
        soup = BeautifulSoup(html_content.content, 'html.parser')
        
        # 지정된 JSON 문자열을 포함한 스크립트 태그 찾기
        
        def find_league_standings(tag):
            return tag.name == 'script' and 'teamsData ' in tag.text

        # match_info를 포함하는 모든 스크립트 태그를 가져오기 위해 사용자 정의 함수 사용
        league_Standings_html = soup.find_all(find_league_standings)

        league_info = league_Standings_html[0].string
        
        ind_start = league_info.index("teamsData ") + 24
        ind_end = league_info.index(");\n") - 1

        json_data = league_info[ind_start:ind_end]
        json_data = json_data.encode('utf8').decode('unicode_escape')
        json_data = json.loads(json_data)

        for team_id, team_data in json_data.items():
            for match_data in team_data['history']:
                row = {'team_id': team_id, 'team_title': team_data['title'], 'season': season_col_value,
                       'competition': competition}
                row.update(match_data)
                df_data.append(row)

df = pd.DataFrame(df_data)

df['ppda'] = df.apply(lambda x: x['ppda']['att'] / x['ppda']['def'] if x['ppda']['def'] != 0 else np.nan, axis=1)
df['ppda_allowed'] = df.apply(
    lambda x: x['ppda_allowed']['att'] / x['ppda_allowed']['def'] if x['ppda_allowed']['def'] != 0 else np.nan, axis=1)

df['date'] = pd.to_datetime(df.date, format='ISO8601')
df = df.sort_values(['date', 'season', 'team_id'], ascending=True).reset_index(drop=True)

driver.quit()
```

# 상관 관계

팀의 방어 전략은 상대팀의 품질에 따라 다를 수 있습니다. 이전에 West Ham의 경우에 그들의 접근 방식은 집에서 Sheffield United와는 다를 것입니다. 이에 따라 PPDA 값에 대한 결론을 작은 샘플 크기로 만드는 것은 위험합니다. PPDA와 xPts 사이의 상관 행렬을 살펴보았습니다. 이를 통해 PPDA 점수가 높거나 낮을 때 포인트를 획득할 가능성에 어떤 영향을 미치는지 알 수 있습니다.

관심 있는 줄은 xPts의 맨 아래 줄입니다. 피어슨 상관 계수는 -1에서 1까지의 범위를 갖습니다. 같은 변수끼리 비교할 때 항상 1임을 주의해야 합니다.

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

- 1의 값은 완벽한 양의 선형 관계를 나타냅니다.
- -1의 값은 완벽한 음의 선형 관계를 나타냅니다.
- 0의 값은 선형 관계가 없음을 나타냅니다.

![image](/TIL/assets/img/2024-07-14-DecodingDefensiveEfficiencyUnravelingtheImpactofPPDAinfootball_3.png)

xG가 가장 강한 상관관계가 있음을 볼 수 있습니다. 이는 xPts 메트릭이 이 수치로부터 계산되기 때문에 놀라운 것은 아닙니다. PPDA 기록은 -0.27이고 PPDA 허용은 0.29입니다.

이는 PPDA에 대해 xPts와 더 약한 음의 관계가 있다는 것을 의미합니다. -0.29의 상관 계수는 두 변수가 반대 방향으로 움직일 경향이 있다는 것을 나타냅니다. 그러므로 PPDA가 낮아지면 xPts가 증가하는 경향이 있지만 상당히 작은 관계라고 말할 수 있습니다. 세계 최고의 팀이 하이 프레스 스타일로 경기를 펼치고 있기 때문에 이것이 발생하는 것일 수도 있습니다. 그 반대로, PPDA 허용은 xPts와 양의 관계가 있어 한 변수가 증가하면 다른 변수도 증가하는 경향이 있습니다. 다시 말하면, 이 결과에 대한 내 가능한 가설은 게임 상태와 관련이 있는 것입니다. 팀이 이제 이기고 있으면 영토와 점유율을 양보하고 수비적인 접근을 취하게 될 가능성이 높으므로 이 결과는 어느 정도 이치에 맞는 것으로 생각됩니다.

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

0-0로 끝난 모든 게임을 살펴봤는데, 게임 상태 관련 성능을 개선할 수 있는지 확인했습니다. 같은 부정적(PPDA) 및 긍정적(PPDA_ALLOWED) 측면이 여전히 나타납니다. 그들 간의 상관 관계 차이가 약간 좁아졌습니다. PPDA_ALLOWED가 xPts에 더 강한 상관 관계를 보이는 이유에 대한 제 생각은, 격찬된 게임에서 특히 막판에 경기 관리가 매우 중요한 요소이며, 축구가 득점이 적은 스포츠이기 때문에 공을 더 잘 지켜내는 능력이 상대편보다 더 많은 예상 포인트를 의미한다는 것입니다.

![image](/TIL/assets/img/2024-07-14-DecodingDefensiveEfficiencyUnravelingtheImpactofPPDAinfootball_4.png)

위 도표에서 나타나는 것은 PPDA 및 PPDA_ALLOWED가 중요하지만 xG 및 xG 허용과 같은 다른 통계요소들과 마찬가지로 xPts와 실제 경기 결과를 결정하는 데 중요하다는 것입니다. 하지만 이러한 값들이 완전히 중복되는 것은 아닙니다.

# 최종 리그 순위

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

다음 섹션은 평균 PPDA 및 허용된 PPDA를 조사한 내용 및 최종 리그 순위에 미치는 영향에 대해 설명합니다. 각 시즌의 챔피언들이 기록한 것과 각 팀이 어떻게 성적을 올렸는지 살펴보고 싶었어요.

먼저 EPL의 최종 순위 표에서 PPDA 및 PPDA_ALLOWED의 평균 값을 살펴봅시다. 도표 3는 매우 흥미로운데요, xPts의 감소는 현재 시즌이 아직 끝나지 않았기 때문입니다. PPDA 관점에서 모든 리그 챔피언들이 공통적으로 갖는 특징은 낮은 수치입니다. 대부분의 시즌에서 20 이하로, 이는 리그 우승을 차지하는 팀들이 좋은 압박을 가하는 팀이며 개입하기 전에 많은 패스를 허용하지 않습니다. 여기서 초록색 막대기는 맥락을 고려하면 매우 중요합니다. 높은 숫자는 리그 챔피언들이 압박에 대해 보유를 성공적으로 유지했음을 의미합니다. 2016년에는 2015년의 콘테의 첼시 이후 숫자가 감소했습니다. 그 이유는 레스터시티입니다! 이 지표에서 꽤 이상한 값들을 보여주는 것은 기억에 남는데, 우리는 그들의 저 공격 높은 수비적인 플레이를 기억합니다. 그러나 시각적으로 이후에 보는 것과는 상반되는 많은 점이 있습니다. 더 많은 보유를 하는 팀이 지속적으로 리그에서 승리했습니다.

실제로, 이러한 데이터를 모든 대회로 확장할 때 추세는 계속 유지됩니다. 이 지표는 리그별로 집계된 중앙값입니다. 리그 챔피언들이 효과적이고 빠르게 공을 회수하며 압박에 저항하는 능력이 뛰어나다는 점을 나타내는 낮은 ppda 값과 높은 ppda_allowed 값들은 그대로 유지됩니다.

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

친구 같은 톤으로 위의 텍스트를 한국어로 번역해드리겠습니다.

재미있는 점은, 지난 시즌 최하위로 끝난 팀으로 계산을 할 때 PPDA 값이 높고, PPDA_ALLOWED 값은 훨씬 낮습니다. 이는 이러한 팀들이 공을 되찾는 데 효과가 떨어지고, 더 중요한 것은 압박에 저항하는 데 얼마나 별로인지를 보여줍니다.

# 결론

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

PPDA는 경기 분석을 기반으로 한 유용한 측정 항목으로, 팀이 수비적으로 전술적으로 어떻게 동작하는지 통찰력을 제공합니다. 그러나 한 경기에 대한 결과를 토대로 오랜 기간의 결론을 내리는 것은 위험할 수 있습니다. 왜냐하면 팀들은 분명 약한 상대에 대해 다르게 설정할 것이기 때문입니다. 상관 관계 플롯은 PPDA와 xPts 사이에 약한 음의 관계를 나타내며, PPDA 값이 낮아질수록 xPts가 증가하는 경향이 있음을 보여줍니다. 반면 PPDA_ALLOWED는 약한 양의 상관 관계를 유지했습니다. 축구는 공격과 수비의 경기이며 점수 획득 관점에서 볼 때, 이 분석은 ppda 지표 측면에서 방어가 공격보다 중요함을 나타냅니다. 이러한 지표 자체가 여러 시즌 동안 리그 챔피언은 일반적으로 낮은 PPDA 값을 가지고 있고 높은 PPDA_ALLOWED 값을 가지며 이 두 가지가 결합하여 그들의 우위를 표현함을 증명합니다. 강등된 팀들, 즉 실제 점수 측면에서 그 시즌에 그 부문에서 최악의 팀들은 상당히 높은 PPDA 값을 가지고 있고 매우 낮은 PPDA_ALLOWED 값을 보여주며, 그들이 프레싱과 공 보유에서 얼마나 약한지를 나타냅니다.

내견은 ppda 지표가 전혀 관련이 없는 것은 아니지만 장기적으로 팀의 능력을 종합적으로 제공하는 다른 설명적 게임 지표와 결합하여 유용합니다. 공격 코치들은 팀의 ppda 지표에 주의를 기울여야 합니다. 웨스트햄처럼 팀이 아스널에 대해 동일한 방식으로 세팅할 가능성이 높고 그에 따라 계획하도록 해야 합니다. 마찬가지로 맨체스터 시티와 경기하는 팀들은 그들에게 고 프레싱을 피해야 할 것입니다. 그것은 엘리트 선수들이 계속적으로 이용할 수 있는 커다란 간격으로 이어질 가능성이 높기 때문입니다. 그들이 경기를 영향력을 가지고 더 위에서 더 높은 위치에서 수비하는 유사한 도구와 능력을 가지고 있지 않는 한. 나는 아스날을 바라보고 있어요.