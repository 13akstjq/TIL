---
title: "삼체 문제의 트리플 별 직렬 현상 물리학 실제로 사람이 행성에서 끌려갈 수 있을까"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_0.png"
date: 2024-07-14 19:52
ogImage: 
  url: /TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_0.png
tag: Tech
originalTitle: "Physics of the Tri-Solar Syzygy from The 3 Body Problem Could People Really Be Pulled Off a Planet"
link: "https://medium.com/@rjallain/physics-of-the-tri-solar-syzygy-from-the-3-body-problem-3820c53ba032"
---



![Tri-Solar Syzygy](/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_0.png)

영화 예고편에 있는 내용이라면 스포일러가 아닙니다. 제가 말하고 싶은 것은 Trisolaris 행성 주민들이 표면에서 끌려 내려가는 장면을 언급한 것입니다.

여기 중요한 사항들만 알려 드릴게요. Trisolaris라는 행성이 세 개의 태양이 있는 천체계에 있다는 점입니다 (그래서 3-체 문제라고 불립니다). 어느 순간, 사람들 (실제로는 사람이 아니죠)이 행성 표면에서 끌려올라가는 장면이 있는데요. 책에서 이렇게 이야기하고 있어요.

이 부분이 플롯의 현실적인 면에서 옳은지 여부는 사실 중요하지 않아요 — 이겈 이야기이고, 때로는 이야기가 물리 법칙을 따르지 않기도 해요. 그래서 더 재미있는 거죠. 하지만 여전히 이 Tri-Solar Syzygy의 물리학을 살펴볼 거에요 (실은 '시지지'가 무엇인지는 잘 모르겠어요. 이제 물어보기도 부끄러워집니다).


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

# 3체 문제

만약에 중력 상호작용이 일어나는 두 물체가 있다면 (이렇게 해서 이진 별계에 있는 두 별이 될 수 있어요), 해석적으로 운동 방정식을 풀어낼 수 있어요. 초기 조건을 알고 있다면 두 물체의 미래 위치와 속도를 찾을 수 있는 식을 얻을 수 있어요. 놀랍게도, 이를 2체 문제라고 부를 수 있어요. 만일 더 자세한 유도를 원한다면, 제가 도와드릴 수 있어요.

하지만 만약 서로 상호작용하는 세 물체를 갖고 있다면 어떨까요? 예를 들어, 세 개의 별이 있다고 해 봅시다: 별 1, 별 2, 별 3. 이 경우에는 별 1과 2 사이의 상호작용, 2와 3 사이의 상호작용, 그리고 1과 3 사이의 상호작용이 존재할 거에요. 이는 2체 문제의 좀 더 복잡한 버전일 것 같지만, 이 시스템을 위한 해석적인 해결책은 없어요. (적어도 우리가 아는 바로는) 이를 방정식 형태로 풀 수 있는 방법은 불가능해요.

3체 문제 같은 문제를 해결할 수 있는 한 가지 방법이 있어요 - 수치적인 해법입니다. 아이디어는 운동을 짧은 시간 간격으로 나누는 것이에요. 이 간격 각각에 우리는 모든 중력 힘을 계산하고, 그 짧은 시간 동안에는 이들이 일정하다고 가정할 수 있어요. 일정한 힘을 가질 때, 세 개의 별의 운동량과 위치가 어떻게 변하는지 알 수 있어요. 그 후에는 그냥 다음 시간 간격에 대해 이 프로세스를 반복하기만 하면 돼요.

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

각 시간 간격 동안의 계산은 비교적 간단합니다 — 하지만 그만큼 많은 계산을 해야 합니다. 누구나 그 모든 계산을 하고 싶어하진 않으니까, 컴퓨터에게 맡기는 게 편합니다 (그나저나 컴퓨터는 크게 불평하지 않아요 — 적어도 지금까진요).

여기 저의 매우 상세한 3체 문제의 수치해법입니다.

# 안정적인 3체 해법

일반적으로, 세 개체의 움직임을 결정하기 위해서는 수치 계산이 필요합니다. 그러나 세 별의 해법을 얻을 수 있는 특별한 상황도 있습니다. 몇 가지 중요한 물리 개념이 있으니, 그 부분부터 알아봅시다.

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

두 질량 간의 중력 상호작용을 다음 방정식으로 모델링할 수 있습니다:

![equation1](/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_1.png)

여기서 M1과 m2는 벡터 r로 구분된 두 질량입니다. G는 만유인력상수이고 r-hat은 힘이 벡터가 되도록 하는 단위 벡터입니다. 그렇다면 두 개 이상의 질량이 있다면 어떻게 될까요? 그 경우, 총 힘은 각 질량에서 오는 힘의 벡터 합일 뿐입니다.

![equation2](/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_2.png)

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

이제 우리는 원운동에 대해 생각해야 합니다. 삼성계의 가장 간단한 움직임은 원을 그리는 것이겠죠. 물체가 원 안에서 움직이고 있다면, 원의 중심을 향하는 가속도가 발생하며 그 크기는 다음과 같습니다:

![원운동 가속도](/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_3.png)

원운동의 반지름은 r이고 속도는 v입니다. 각속도(ω)의 운동가속도를 계산하는 것도 가능합니다.

자, 이제 우리는 안정된 움직임을 준비했습니다. 세 개의 별이 같은 질량을 가지고 있으며 한 별이 고정되어 있고 다른 두 별이 그 고정된 별 주위를 공전하는 것으로 가정합니다.

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


![image1](/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_4.png)

중간에 있는 별 1에는 두 개의 중력이 작용함을 알 수 있습니다. 그러나 대칭이라면, 별 2와 별 3이 반대 방향으로 같은 힘으로 작용하여 별 1이 정지 상태에 있을 수 있도록 하지요.

별 2를 보면, 왼쪽으로 두 개의 중력이 작용합니다 - 별 1과 별 3에서. 이 순방향 힘은 별 2가 왼쪽으로 가속화되도록 합니다. 이 순방향 힘이 별 2가 원운동 할 수 있도록 만드는 속도(v)를 계산할 수 있게 됩니다. 별 1은 별 2에서 거리 r 떨어져 있지만, 별 3는 2r의 거리에 있습니다.

![image2](/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_5.png)


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

태양의 크기 (r)를 알고 있다고 가정해 봅시다. 그러면 별 2의 속도를 구할 수 있습니다 (이는 별 3의 속도가 될 것입니다).

![image](/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_6.png)

이게 실제로 작동할까요? 한 번 시도해 볼 수 있습니다. 세 개의 같은 질량을 가진 태양으로 수치 계산을 해보겠습니다. 중간에 있는 별은 정지 상태에서 시작하고, 나머지 두 개의 별은 계산된 속도로 시작합니다. 그런 다음, 별의 움직임을 모델링하기 위해 수치 계산 방법을 사용할 수 있습니다. 전체 Web VPython 코드는 여기에서 확인할 수 있습니다. 애니메이션은 다음과 같이 보입니다:

![animation](https://miro.medium.com/v2/resize:fit:960/1*RA24sRZkOic5eOCj7FBVzg.gif)

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

물론, 중요한 점은 삼합성 시스템이 안정한 궤도에 있지 않다는 것입니다 — 이것이 바로 삼합성의 거주자들이 마주한 문제 중 하나입니다. 그러나 이 책에서는 별들이 어떻게 이러한 배열에 도달하는지 알 수 있습니다.

# 삼합성 행성의 궤도

이제 행성을 추가해보죠. 이로 인해 "네 체 문제"가 되지는 않을 것입니다. 행성의 질량은 별들에 비해 매우 작다고 가정할 수 있기 때문입니다. 이는 행성이 별 세 개에게 발생시키는 중력 힘이 그들의 움직임에 별다른 영향을 미치지 않을 것을 뜻합니다. 물론, 4개의 서로 작용하는 물체의 움직임을 쉽게 모델링할 수 있겠지만, 이 가정은 설정을 더 쉽게 만들 것입니다.

행성의 관점에서 볼 때, 세 별은 일직선으로 유지되어야 합니다. 이것은 삼합성이 두 별과 같은 각속도를 가지고 있을 때(가운데 별이 고정일 때) 발생합니다. 다음은 다이어그램입니다.

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


![image](/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_7.png)

당연히 이것은 실제 크기로 표현된 것이 아닙니다 (작은 행성은 볼 수 없겠죠). 이 행성에는 세 개의 중력 작용이 있습니다 (세 개의 별로 인한 것입니다). 그에 따라 행성의 가속은 각속도(ω)와 궤도 거리(r_p)에 따라 달라집니다.

이 위치에서 모든 중력은 가속과 같은 방향에 있으므로, 뉴턴의 제2 법칙에 대한 다음 방정식을 작성할 수 있습니다 (참고: 각 별의 질량은 M이고 행성의 질량은 m입니다).

![image](/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_8.png)


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

행성의 각 운동 속도는 외부 별 중 하나의 각 운동 속도에서 구할 수 있습니다.

![image](/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_9.png)

이제 행성 트리솔라리스의 공전 거리 (r_p)를 구하는 것은 여전히 어려운 방정식으로 보입니다. 괜찮아요. 실제로 이 방정식을 풀 필요는 없어요. 숫자적 해법을 찾을 수 있으며, r_p의 다른 값에 대해 방정식의 좌변과 우변의 값을 구하여 해의 값을 찾을 수 있어요. 이 두 변을 그래프로 그리면 교차하는 지점이 해를 주는 r_p의 숫자 값이 될 거에요. 한 번 확인해보세요.

![image](/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_10.png)

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

스케일 조정을 해 보면서, 행성 공전 거리를 0.21065 AU로 결정했어요. 그럼, 그걸로 테스트해 볼게요. 여기 4번째 객체를 넣은 3체 문제 계산 결과입니다.

![image](https://miro.medium.com/v2/resize:fit:1000/1*EHiES60H5-45wmU7IZpYDQ.gif)

별의 회전 운동과 대부분 일치하면서 행성이 이동하는 모습을 보실 수 있어요. 애니메이션 끝 쪽에 조금씩 벗어나는데요 — 근데 괜찮아요. 이 삼중태양 허물기는 어차피 영원히 지속되지 않거든요.

# 삼중태양 허물기 모델링

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

사람들이 행성 표면에서 당겨지는 이유가 무엇일까요? 첫 번째 아이디어는 세 개의 별로부터의 중력 힘의 합이 사람들을 당겨내는 중력 힘보다 크다는 것입니다. 이미지를 보세요!

![image](/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_11.png)

좋아요. 우리는 이러한 힘들을 계산할 수 있어요 (일부 가정을 가지고). 지구와 똑같은 행성이라고 가정해 봅시다 (왜냐하면요). 이것은 즉, 사람을 행성으로 당기는 중력 힘이 1킬로그램당 9.8 뉴턴이 될 것이라는 것을 의미해요. 세 개의 별로 인한 순박한 힘으로 인한 순박한 힘의 경우, 1.5 N/kg 정도일 것입니다 - 9.8 N/kg 보다 작아요. 네, 이것이 삼합운 돌아가는 내 특정 값에 대한 값이에요.

그런데 기다려 주세요! 중력 힘의 이 차이로 실제로는 당신이 트리솔라리스의 표면에서 더 가벼워진다는 느낌을 받지 못할 거예요. 왜냐하면요? 그것은 행성이 정지해 있지 않기 때문에 그렇습니다 - 사실 행성은 이미 원형 운동으로 움직이고 있기 때문에 별들 쪽으로 가속하고 있어요.

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

걱정하지 마세요, 선이 3개인 라인의 누적 효과를 계산할 수 있어요. 하지만 약간의 잡수작을 필요로 할 거예요. 3개의 별과 중력 상호작용은 행성에 힘을 가해 가속시키는데, 이는 행성의 중심에서의 가속임을 명심해야 해요. 표면에 있는 비인간은 행성 중심에서의 가속도 값을 가질 것인데, 별들에 더 가까운 지점에서의 중력 힘을 받게 될 거예요. 네, 조금 복잡한 일이죠.

트리솔라리스 표면의 겉으로 보이는 중력 힘은 행성의 중심에서의 중력장과 표면에서의 차이로 나타날 거예요. 행성의 반지름이 6.3 x 10⁶ 미터인 것으로 가정해봅시다 (지구와 같다 가정). 그 경우, 별들에서 오는 유효한 힘은 1kg 당 0.00074 뉴턴을 향해 당겨주게 될 거예요.

그러니까, 질량이 70kg인 생명체는 보통 686 뉴턴의 무게를 느낄 수 있을 텐데요 — 그러나 세 개의 별이 일렬로 위치할 때 그들은 685.99 뉴턴만 느낄 거예요. 만약 숫자에 좀 미숙하다면, 그것은 무게 차이가 크지 않다는 거죠.

이제 맞추세요! 만약 한 사람 (실제 사람이 아닌)이 행성의 반대편에 있다면, 그들도 역시 겉으로 보이는 무게가 더 낮게 느껴질 거예요. 이 경우에 별들로부터의 중력 힘은 조금 더 낮아지겠지만, 행성의 중심의 가속도는 그들에게 지속적으로 멀어지는 것처럼 보일 거예요.

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

이야기 한 번 더 들려줄게요. 정확히 여러분이 지구에 두 개의 조수가 있는 이유입니다. 그 이유는 달로부터 오는 중력에 의한 가속이 지구의 중심에서 발생하기 때문입니다. 지구의 중심에서의 가속은 지구 표면에서의 달로부터 오는 중력과는 다릅니다. 이러한 차이로 인해 물이 두 군데에서 풍겨져 두 개의 물 축적이 발생하는 것이죠. 여기 지구의 표면 가속과 실제 중력장 사이의 차이를 시각화한 그림이 있어요.

![Earth Visualization](/TIL/assets/img/2024-07-14-PhysicsoftheTri-SolarSyzygyfromThe3BodyProblemCouldPeopleReallyBePulledOffaPlanet_12.png)

만약 이 이미지를 생성하는 과정의 자세한 내용을 보고 싶다면, 전부 제공해 드리겠습니다.

그러면, 여기 주의! "3체 문제"는 넷플릭스 쇼로 제작된 책일 뿐이에요. 실제로는 존재하지 않습니다. 물리학이 완벽하진 않아도 괜찮아요. 물론, 행성의 크기나 궤도를 변경하면 효과적인 힘을 더 크게 얻을 수도 있겠지만, 제가 설정한 값들이 합리적으로 보인다고 생각해요.