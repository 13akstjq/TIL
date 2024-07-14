---
title: "네트워크 과학으로 예측한 다음에 죽을 왕좌의 게임 캐릭터는 누구"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-NetworkSciencePredictsWhoDiesNextinGameofThrones_0.png"
date: 2024-07-14 20:11
ogImage: 
  url: /TIL/assets/img/2024-07-14-NetworkSciencePredictsWhoDiesNextinGameofThrones_0.png
tag: Tech
originalTitle: "Network Science Predicts Who Dies Next in Game of Thrones"
link: "https://medium.com/@janosovm/network-science-predicts-who-dies-next-in-game-of-thrones-50aa492b9f94"
---



![Network Science Predicts Who Dies Next in Game of Thrones](/TIL/assets/img/2024-07-14-NetworkSciencePredictsWhoDiesNextinGameofThrones_0.png)

# 소셜 네트워크 분석과 머신 러닝은 최근 몇 년간 수많은 응용 분야에서 사용되고 있습니다. 예를 들어, 2017년에 이루어진 이 짧은 프로젝트는 몇 가지 미디어 관심을 끌었는데, 주된 목표는 네트워크 과학과 예측 모델링을 결합하여 인기 있는 TV 및 도서 시리즈인 'Game of Thrones'의 주요 캐릭터들 중 어떤 이들이 그들의 최후를 맞이할 가능성이 높은지 예측하는 것이었습니다.

# 1. 소개

이 글은 2017년에 발표된 두 개의 블로그 포스트 [1, 2]를 요약한 것으로, 이들은 네트워크 과학, 예측 모델링, TV 프로그램 (및 책 시리즈적 적응) 'Game of Thrones'을 결합하여 시리즈의 그 당시 에피소드에서 주요 인물들이 그들의 최후를 맞이할 가능성이 높은 캐릭터를 식별하기 위해 노력했습니다. 다양한 미디어에서 받은 높은 관심으로 인해 [4–10], 저자는 이 프로젝트를 요약하였습니다.


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

이 프로젝트에서는 TV 시리즈의 라벨이 붙은 자막을 기반으로 작업했어요. 여기에는 발화된 텍스트 뿐만 아니라 발화자의 이름도 포함되어 있었죠. 게다가 사용된 데이터셋 [11]은 서로 다른 장면 사이의 구분 요소에 대한 정보를 가지고 있었어요. 이 두 가지 정보를 결합하여 모든 장면에 나타나는 캐릭터 목록을 추출했어요. 총 600명 정도였죠. 그런 다음 이러한 장면을 쇼의 사회 시스템의 기본 구성 요소로 가정하고 웨스테로스의 사회 지도를 작성하는 데 사용했어요. 이 네트워크에서 각 주요 캐릭터는 네트워크 노드로 표시되며, 같은 장면에서 함께 출연한 경우에는 두 플레이어 사이에 연결이 있어요. 게다가, 그들이 함께 출연하는 빈도가 더 높을수록 연결이 더 강해져요. 그런 다음 이러한 캐릭터(네트워크 노드)의 다양한 네트워크 중심성 측정 값을 계산하여 예측 기능으로 사용했고, 이들 캐릭터가 6시즌 동안 살았는지 여부를 라벨링했어요. 마지막으로, 아직 살아있는 캐릭터 중 어떤 캐릭터가 죽을 가능성이 높은지 예측하기 위해 널리 사용되는 선형 모델인 서포트 벡터 머신을 적용했어요.

두 번째 섹션에서는 처음 6시즌의 자막을 기반으로 한 원래 예측에 대한 자세한 내용이 소개되었습니다. 세 번째 섹션에서는 7시즌 이후의 예측이 업데이트되었어요. 나중에 추가된 요약은 최종 결과에 대한 간단한 평가를 제공해요.

# 2. 왕좌의 게임 예측

왕좌의 게임의 새 시즌이 곧 다가오며 팬들은 무엇을 기대할지 흥분하고 있어요. 제가 즐겼던 캐릭터 중 어떤 캐릭터가 죽을 것인지, 누가 다음 시즌에 살아남을지 궁금해서 혼자만은 아니에요. 그래서 캐릭터들을 순위 매기기로 결정했어요. 왕좌의 게임은 사회적 위치와 진정한 친구들이 꽤 중요한 복잡한 세계이기 때문에 각 캐릭터의 사회 상호 작용 패턴을 네트워크 과학 도구를 사용하여 양적으로 표현했어요. 그런 다음 기계 학습 방법을 사용하여 그들의 운명을 예측했죠.

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

2.1. 웨스테로스의 네트워크 만들기

데이터 원본으로는 팬 웹사이트 [11]에서 대화 형식으로 수집된 쇼 자막을 사용했습니다. 불행하게도 2, 3 시즌 대부분의 에피소드가 누락되어 있지만, 나머지 네 시즌에는 거의 600개의 장면이 포함된 일관된 형식으로 제공됩니다.

먼저 왕국 사회 체계의 집계된 네트워크를 구축했습니다. 이 네트워크에서 각 노드는 이야기의 캐릭터를 나타내고 각 쌍의 캐릭터 사이의 연결 가중치는 그들의 사회적 상호 작용의 강도를 상징합니다. 사회 상호 작용의 기본 단위로 장면을 고려했습니다 (평균 에피소드에는 약 스무 개의 장면이 포함됩니다). 이는 한 장면에서 한 번 (두 번) 나온 모든 사람들이 강도가 하나 (둘)인 관계를 가지며, 장면 내에서는 모든 사람이 서로 연결되어 있다는 것을 의미합니다. 다시 말하면, 장면은 모든 사람들 간의 관계를 한 단계씩 높여 모두가 서로 강하게 연결된 완전 그래프 또는 클리크입니다. 이러한 장면 수준의 완전 네트워크를 계산한 다음 이를 집계하여 우리는 웨스테로스의 글로벌 사회 네트워크 (도식 1)에 도달하게 되며, 거의 400개의 노드와 3000개 이상의 엣지를 갖습니다.

네트워크 시각화에서 (도식 1) 위대한 가문의 모든 구성원은 서로 다른 색상으로 표시됩니다 (예: 파랑 - 스타크, 빨강 - 란니스터, 노랑 - 마텔). 다른 사람들은 회색으로 표시됩니다. 노드 크기는 각 사람이 가진 연락처 수에 비례하며 가장 인기 있는 캐릭터 이름이 레이블로 추가됩니다. 연결 중심성이 매우 낮은 흥미로운 없는 노드는 걸러집니다. 월 주변 야인들 주변에 분리된 공동체가 보이며, 요한 스노우 주변의 사람들은 왕국의 나머지와의 연락이 거의 없다는 것을 보여줍니다. 티리온은 별도의 역할을 맡고 있습니다: 그는 다이네리스 타거리엔를 네트워크의 중앙에 있는 왕의 땅을 포함하여 연결하고 있습니다. 이곳에서 두 개의 큰 공동체를 볼 수 있습니다. 이들은 스타크와 란니스터 그리고 스타크와 탈리 가문 간의 결속 및 란니스터와 마텔 간의 갈등과 같은 영향영역 및 상호작용을 형성하여 이야기의 핵심부에서 밀집된 웹을 형성하고 있습니다.

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

수학으로 넘어가 봅시다. 우리는 노드의 중요성을 측정할 수 있는 다양한 지표를 계산할 수 있어요. 우리는 이러한 측정값을 캐릭터에 연결하여 이 사회 생태계에서의 중요성을 설명할 거예요. 이러한 측정값 중 일부는 i) 노드 차수 — 사람이 가지는 연락 수; ii) 가중치된 차수 — 특정 노드의 엣지 가중치 총합; iii) 클러스터링 — 노드의 연락처 쌍이 자주 연락하는 정도; 그리고 iv) 중심성, 그 노드가 얼마나 정보 흐름에서 다리 역할을 하는지를 나타내는 중요도, 다른 노드 쌍 사이의 최단 경로 상에 얼마나 자주 위치하는지를 측정합니다 (Table 2). 중요한 사람과 중요하지 않은 사람을 더 잘 파악할 뿐만 아니라, 어떤 캐릭터가 최초 6시즌에서 사망했는지 데이터로부터 알아낼 수도 있어요. 그래서 우리의 목표는 네트워크 위치와 생존을 연결하는 것입니다: 한 가지를 다른 가지와 연결시키는 것은 가능할까요? 다시 말해, 캐릭터가 사망했는지 여부를 예측하는 데 어떤 네트워크 측정값이 도움이 될지 알아내기 위해 알고리즘을 훈련하고 싶어요.

**2.2. 예측**

우리에게는 관심 있는 94명의 캐릭터 집합이 있어요. 모든 캐릭터는 사회적인 중요성의 다양한 측면을 나타내는 일곱 가지 다른 네트워크 기반 특성으로 설명됩니다. 또한 우리는 그 중 61명이 이미 사망한 캐릭터를 알고 있어요. 이 지식을 바탕으로 우리는 누가 곧 죽을지 굉장히 예측할 수 있어요: 아직 살아 있는 사람 중에서 이미 사망한 사람과 비슷한 특성을 가진 사람은 누구일까요? 이 문제는 데이터 과학에서 널리 연구된 이탈 문제와 비슷합니다. 다양한 분류 기반 알고리즘으로 이 문제를 해결할 수 있어요. 이 분석에서는 가장 정확한 Support Vector Machine (SVM)을 사용했어요. 이 알고리즘은 Python에서 쉽게 사용할 수 있어요. 이 기능을 집에서 시도해보고 싶다면 사용해보세요.

기계 학습 알고리즘은 모든 특성을 고려하고 대상 변수의 가능한 값을 예측합니다. 이를 위해 샘플 데이터를 무작위로 테스트 세트와 훈련 세트로 분할하고 여러 번 반복해 예측을 수행한 후 최종 결과를 평가합니다. 이 교차 검증 전략을 통해 SVM 분류기는 경우의 72.3%에서 올바른 클래스(사망 또는 생존)를 예측했어요. 데이터의 규모와 성격을 고려하면 이는 만족스러운 결과입니다. 정확성을 설명하기 위해 모델은 8명의 캐릭터가 사망해서는 안 되지만 이야기 속에서는 그런 캐릭터들이 사망했음을 말해요 — 여왕의 죽음은 왕보다 덜 가능성이 있다는 것인데, 그리고 강력한 친구들이 그를 구할 수 없었던 진노스 슬린트도 그렇습니다.

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

![Network Science Predicts Who Dies Next in Game of Thrones](/TIL/assets/img/2024-07-14-NetworkSciencePredictsWhoDiesNextinGameofThrones_1.png)

다른 유형의 특징(예: 성별, 귀족 가문의 일원 여부, 연설의 감정 분석)을 포함하거나 보다 완전한 데이터셋을 사용하거나 TV 프로그램을 책과 비교하는 등의 작업은 예측의 정확성을 높일 수 있습니다. 또한, 이 모델은 Jon Snow가 죽었다가 부활하거나 Benjen Stark가 중간 어딘가에 있는 등의 불일치를 무시합니다.

2.3. 결과 — 스포일러 주의

SVM 모델을 사용하여 각 살아있는 잘 알려진 캐릭터가 죽을 확률을 얻을 수 있습니다. 네트워크 측정값들은 종종 매우 상관관계가 있기 때문에 단독으로 높은 예측력을 가진 하나 또는 둘을 선택할 수 없지만, 보이는 바와 같이 betweenness가 높고 clustering이 낮으며 차수가 높은 캐릭터들은 죽을 가능성이 더 낮습니다. 어쨌든 기계 학습 접근 방식의 강점은 많은 특징들 중에서 숨겨진 관계를 발견하는 것입니다. 예측 중에 5-fold 교차 검증을 사용하였고, 이를 100번 반복하여 각 확률의 통계적 값 및 오차에 대한 추정을 얻었습니다. 마지막으로, 최종 예측 모델에 따라 생존 순으로 순위 매겨진 캐릭터 목록이 여기에 있습니다 (테이블 2).

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

![Network Science Predicts Who Dies Next in Game of Thrones](/TIL/assets/img/2024-07-14-NetworkSciencePredictsWhoDiesNextinGameofThrones_2.png)

이 목록은 많은 흥미로운 것을 알려줍니다. 첫째, 다네리스는 상당히 죽을 가능성이 높아 보이며, 많은 추측과 겹칩니다. 반면 티리온과 존 스노우는 비교적 안전해 보입니다. 둘째, 항상 인기 있는 아리아 스타크와 덜 친절한 하운드는 이미 죽을 위기를 여러 차례 겪었는데도 위험한 위치에 있습니다. 놀랍게도 현재 철 왕좌에 앉아있는 세르세이와 그곳에 오르려는 베일리시는 훨씬 좋은 위치에 있는 것으로 보입니다. 조라 몰몬트는 화파병에 대한 치료법을 찾을 것으로 보이며, 그가 겪어온 모든 것에도 불구하고 테온 그레이조이는 아마도 살아남을 것입니다. 안타깝게도, 에린 가족에 대해서는 그렇지 못할 것으로 보입니다.

# 3. 게임 오브 스론 예측 2.0

뜻밖의 소식을 가져오고 끊임없는 논쟁을 일으킨 게임 오브 스론 7시즌이 끝났고 중요한 캐릭터들을 자연스럽게 죽였습니다. 그래서 이제 이전 예측을 검증할 수 있는 기회가 왔습니다.

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

3.1. 지금까지 무슨 일이 있었나요?

이전 블로그 게시물에서 TV 프로그램의 에피소드를 사용하여 웨스테로스 왕국의 소셜 네트워크를 만들고 죽은 캐릭터와 살아있는 캐릭터의 다양한 중심성 매개변수를 결정하고, 각 살아있는 캐릭터가 죽을 가능성을 예측하기 위해 서포트 벡터 머신 모델을 사용했습니다. 결과는 살아있는 캐릭터들이 시리즈 종료 전에 죽을 가능성에 따라 순위가 매겨진 캐릭터 목록 형태로 나왔습니다. 제 예측은 시간을 고려하지 않습니다: 캐릭터가 첫 번째 에피소드에서 즉시 죽을지 또는 막바지에야 죽을지는 예측하지 않습니다. 그래서 최종 시즌은 아직 남아 있지만, 모델의 예비 결과에 대해 이야기하고 싶습니다. 이전 예측은 34명의 잘 알려진 캐릭터의 운명을 다루었으며, 그 중 대부분은 최신 시즌에 나타났습니다. 그러나 그중에는 지금까지 4명이 죽었는데, 만약 죽음이 무작위로 발생했다면 모든 캐릭터가 죽을 확률은 대략 11%를 의미합니다. 모델의 한계와 단순함에도 불구하고, 상당히 잘 수행했습니다(Table 3): 목록의 처음에 있는 타인, 그리고 올레나 타이렐과 페티르 베일리쉬의 사망을 예측했지만, 타인의 자매 사망은 예측하지 못했습니다. 반면, 조라는 (죽을 것으로 생각되는) 치명적인 질병에 대한 치료법을 발견하고 생존할 것으로 예측한 것이 맞았습니다. 마찬가지로, 테온은 모든 고통을 겪었음에도 계속해서 살아갈 것이라는 것에 대한 정확한 예측이었습니다.

<img src="/TIL/assets/img/2024-07-14-NetworkSciencePredictsWhoDiesNextinGameofThrones_3.png" />

3.2. 예측 2.0

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

이전에 소개된 방법론을 사용하여 캐릭터들의 사망 확률에 기반한 현재 순위도 계산해보았어요. 이 정보는 약간의 스포일러가 포함되어 있으니 이 게시물의 끝에서 확인할 수 있어요.

가장 중요한 이벤트는 의심할 여지 없이 다네리스와 존, 그리고 세르세이와의 만남입니다. 이것이 바로 이 네트워크의 (도식 2) 이전에 연결되지 않았던 도메인들이 마침내 연결되었다는 의미에요 (도식 3). 이를 통해 많은 새로운 연결이 생성되고 원래의 소셜 네트워크가 변형되며, 이는 또한 캐릭터들의 예측된 미래를 바꾸게 된다고 해요. 제1-6 시즌을 기반으로 한 중심 캐릭터들의 필터링된 네트워크는 도식 2에서 확인할 수 있고, 1-7 시즌을 기반으로 한 네트워크는 도식 3에 나와 있어요. 노드는 연결 강도의 총량에 비례하는 크기로 캐릭터들을 나타내고, 색상은 7 시즌에 등장하는 대가문을 나타내는 색으로 설정되어 있어요 (파란색 - 스타크, 빨간색 - 란니스터, 마젠타색 - 타르가르엔, 초록색 - 타이렐, 노란색 - 그레이조이).

![Figure 2](/TIL/assets/img/2024-07-14-NetworkSciencePredictsWhoDiesNextinGameofThrones_4.png)

![Figure 3](/TIL/assets/img/2024-07-14-NetworkSciencePredictsWhoDiesNextinGameofThrones_5)

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

리스트와 네트워크를 통해 몇 가지 사실을 추론할 수 있습니다:

- 거의 모든 네트워크 측정을 기반으로 중요한 캐릭터인 Jon;
- Bran은 결국 행동에 가까워졌는데, 그는 불길한 미래를 암시하는 것으로 보입니다;
- Daenerys는 영향력 있는 친구들을 충분히 얻었는데, 그러나 그녀 주변의 사람들이 그녀의 임무를 위해 희생될 수도 있습니다;
- Cersei 주변의 사람들이 사라지는 것을 보면 (그림 3에서 란니스터 가문과 유사한 작은 빨간 지역을 보십시오), 그녀와 가까이 있는 사람들에게 영향을 미치는 것 같습니다;
- Theon과 Jorah는 생존할 가능성이 높아 보이며 (아마 이전 경험에 기반할지도 모릅니다);
- 많은 사람들에게 친애받는 와일링 Tormund는 폐쇄되는 성벽에서 탈출뿐만 아니라 전쟁 전체에서도 생존할 것으로 보입니다.

<img src="/TIL/assets/img/2024-07-14-NetworkSciencePredictsWhoDiesNextinGameofThrones_6.png" />

# 4. 결론

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

Figure 4에서 예측된 확률이 87% 미만에서 명확한 신호와 일치하지 않았지만 무작위 기준선(동전 던지기로 예측을 구축했다면 얻을 확률) 주변에 매우 겹치고 변동했습니다. 그러나 예측된 적중이 실제 적중과 매우 겹치는 87% 이상의 임계값을 넘어서 빠르고 명확한 증가를 보았습니다. 이 결론은 네트워크 특징이 '왕좌의 게임'에서 가장 명백한 죽음을 포착할 수 있었다는 것을 시사합니다. 그러나 불확실한 상황에서는 잘 작동하지 않았을 수 있습니다.

![Image](/TIL/assets/img/2024-07-14-NetworkSciencePredictsWhoDiesNextinGameofThrones_7.png)

부정확성과 한계는 여러 가지 차원이 있습니다. 먼저, 데이터의 청결함과 양을 개선할 수 있었을 것입니다. 둘째, 데이터 형식과 특징을 확장할 수 있었고, 예를 들어, 인물에 대한 메타 정보(예: 나이, 성별)를 이용하여 네트워크 특징을 보강하고 TV 시리즈 및 원작 책에서 정보를 결합할 수 있습니다. 마지막으로 가장 흥미로운 사항은 예측된 기간인 시즌 7과 8이 책을 기반으로 하지 않았다는 점입니다. 한편 다른 훈련 데이터는 그랬습니다. 이 차원은 성이 구축된 방식에 대한 논리가 어떻게 구성되었는지에 대한 관점에서 매우 중요하지만, 양적 방법으로 연구하기가 매우 어렵고 분명한 미래 연구 대상이 될 것입니다.

# 5. 데이터 접근성

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

이 연구와 관련된 지원 파일은 https://github.com/milanjanosov/GameOfThron에서 찾을 수 있습니다.

## 6. 면책 조항

블로그 게시물은 원본 형태 그대로 섹션 두와 세에 게시되었지만, 일부 오타 및 문법 문제가 수정되었습니다. 이는 텍스트 의미에 영향을 미치지 않았습니다.

## 7. 감사의 글

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

저자는 중앙 유럽 대학교 학술 글쓰기 센터의 Ágnes Diós-Tóth, Thomas Rooney 및 Robin Bellers에게 이 논문과 원본 블로그 포스트의 여러 부분을 검토해 준 것에 대해 감사의 말씀을 전합니다.