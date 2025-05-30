---
title: "더 적은 것으로 더 많은 AI를 구현하는 방법"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-MoreAIwithlesswhat_0.png"
date: 2024-07-14 20:33
ogImage: 
  url: /TIL/assets/img/2024-07-14-MoreAIwithlesswhat_0.png
tag: Tech
originalTitle: "More AI with lesswhat"
link: "https://medium.com/stackademic/more-ai-with-less-what-8367ecad23be"
---


<img src="/TIL/assets/img/2024-07-14-MoreAIwithlesswhat_0.png" />

작은 언어 모델들은 정말 좋았어요.

저는 예전 얘기를 하는 것이 아니에요. 이게 18개월 전까지의 표준이었어요. 어떤 비논리적인 이유 때문에 GPT-3가 등장하자 모든 기존 모델들이 버려졌어요.

그리고 이것은 큰 문제일 수도 있어요! ChatGPT가 전체 생성적 AI 세계의 교사이자 목적지이자 심판이 될 수 있을까요?

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

그래도 다른 선택지가 없을까요?

함께 확인해 보겠습니다.

# 인코더-디코더 모델의 시대

인공지능 발전의 물결 속에서, 수십억 개의 매개변수를 가진 거인 모델들이 주목을 받는 가운데, 조용한 챔피언들을 잊기 쉽습니다. 구글의 민첩한 언어 모델인 Flan-T5는 그 중 한 예로, 크기가 모든 것은 아니라는 것을 증명하고 있습니다. 2024년에도 여전히 새로운 거대 모델이 등장하고 있지만, Flan은 속도, 정확성, 그리고 접근성의 영속적인 힘을 상징하는 모델로 남아 있습니다.

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

일상 및 비즈니스 용도에 정말 큰 모델이 필요할까요? 인공 지능을 구현하고 사용하기 위해 서버 팜이 필요할까요?

많은 중소기업(SMB)들에게는 인공 지능(AI)이 미래적인 과학 소설 영화에서 온 것처럼 보일 수 있습니다. 그러나 현실은 AI가 더 이상 기술 거물들만을 위한 것이 아니라는 것입니다. 심지어 게임에서 제일 작은 참가자들에게도 접근하기 쉽고 극도로 유용해지고 있습니다. 그래서, SMB에서 AI의 주요 사용 사례는 무엇이며, Flan-T5와 같은 작은 대형 언어 모델(LLMs)이 어떻게 적합한지 알아보겠습니다.

[이미지 참조](/TIL/assets/img/2024-07-14-MoreAIwithlesswhat_1.png)

## 효율성 향상 및 시간 절약을 위해서

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

- 작업 자동화: 데이터 입력, 일정 관리, 고객 서비스 및 콘텐츠 생성과 같은 작업들은 AI 기술을 활용하여 자동화할 수 있습니다. 이를 통해 사업주와 직원들은 더 전략적인 작업에 집중할 수 있는 소중한 시간을 확보할 수 있습니다. 소규모 LLM은 대용량 텍스트 말뭉치에서 정보를 쉽게 추출하여 데이터 입력 과정에 사용할 수 있습니다.

- 요약: AI는 큰 양의 텍스트를 간결한 요약으로 효율적으로 축소할 수 있어 연구, 보고서 작성 및 정보 수집 시간과 노력을 절약할 수 있습니다. 예를 들어 Flan-T5는 이메일, 기사, 고객 피드백 등을 능숙하게 요약하여 필수적인 통찰을 빠르게 제공합니다.

**통찰을 찾고 데이터 기반 의사결정을 내리기 위해**

- 검색 증강 생성: 소규모이지만 잘 세밀하게 조정된 언어 모델은 지식 검색에 기반한 작은 진실(검색 증강 생성으로도 불림)을 활용하여 외부 정보 소스에 근거하여 더 포괄적이고 정확한 응답 생성할 수 있습니다. 이를 통해 생성된 콘텐츠가 사실적이고 관련성이 있으며 믿을 수 있어 더 나은 의사결정을 지원합니다.

**경쟁력 유지하고 새 고객 확보하기위해**

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

- 개인화된 고객 상호작용: AI 기술을 활용한 챗봇은 대화를 개인화하여 제품 추천과 고객 서비스를 개별 필요에 맞게 제공함으로써 고객 만족도와 충성도를 향상시킬 수 있습니다.
  
- 고객 피드백 요약: 고객 리뷰와 설문 조사에서 주요 요점을 빠르게 이해하는 것은 제품 개발과 고객 서비스 전략을 개선하여 최종적으로 고객 만족도를 높일 수 있습니다.

한정된 자원으로도 SMB들은 AI 기술의 힘을 활용하여 운영을 간소화하고 가치 있는 통찰력을 얻고, 개인화된 고객 경험을 제공하며 매력적인 콘텐츠를 생성할 수 있습니다.

Flan-T5와 같은 작은 LLM들은 접근성 있는 가격대에서 강력한 기능을 제공하며(당신만의 하드웨어를 사용하면 무료로도 이용 가능합니다...), AI 도입은 모든 규모의 기업에게 실행 가능하고 전략적인 선택지가 됩니다.

![이미지](/TIL/assets/img/2024-07-14-MoreAIwithlesswhat_2.png)

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

Flan-T5은 Google에서 개발한 강력하고 다재다능한 언어 모델로, 다양한 언어 작업에서 뛰어납니다. 이 모델의 주요 기능은 다음과 같습니다:

- 오픈 소스 및 상업적 이용 가능: Flan-T5는 누구나 자유롭게 사용, 수정하고 상업 애플리케이션에 통합할 수 있습니다. 이러한 접근성은 혁신을 촉진시키고 AI 능력을 더 많은 사람들에게 제공합니다.
- 시퀀스-투-시퀀스 아키텍처: 이 모델은 인간의 의사소통을 모방하도록 훈련되어 텍스트를 처리하고 생성합니다. 입력으로 단어 시퀀스(질문 또는 프롬프트와 같은)를 받아 해당하는 출력 단어 시퀀스(답변 또는 응답과 같은)를 생성합니다.
- 대형 언어 모델 (LLM): Flan-T5에는 수백만 개에서 수십억 개의 매개변수를 가진 대규모 신경망이 탑재되어 있어 광범위한 언어 데이터를 저장하고 처리할 수 있습니다. 이를 통해 인간 수준의 텍스트를 이해하고 생성할 수 있습니다.
- 다중 작업에 대한 튜닝: 이 모델은 단일 목적 도구가 아닙니다. 다양한 언어 작업에 대해 훈련을 받아 실제 시나리오에 적용할 수 있습니다.
- 텍스트-투-텍스트 프레임워크: Flan-T5이 속한 T5 모델 패밀리는 다양한 언어 작업을 텍스트-투-텍스트 형식으로 접근합니다. 이는 번역, 문장 유사성, 문서 요약 등 다양한 도전 과제를 텍스트 기반 입력 및 출력으로 처리하여 문제 해결 접근 방식을 간소화합니다.
- 인상적인 기능: 최신 대형 언어 모델보다 작은 크기임에도 불구하고 Flan-T5는 놀라운 정확도, 속도 및 다재다능성을 보여줍니다. 텍스트 요약, 정보 추출, 다양한 창의적인 텍스트 형식 생성 및 질의 응답 챗봇 제공도 가능합니다.
- 액세스 가능한 하드웨어 요구 사항: 일부 자원 집약적인 대형 언어 모델과 달리, Flan-T5는 전문적인 GPU가 필요하지 않는 일반 소비자 하드웨어에서 작동할 수 있습니다. 이는 개인 및 기업 모두에게 저렴하고 실용적입니다. 

![이미지](/TIL/assets/img/2024-07-14-MoreAIwithlesswhat_3.png)

한 해 전에 작성한 기사가 있는데, 그 기사에서 여전히 최고인 시리즈 모델에 대해 소개했어요: 초소형이지만 매우 빠르고 전용 GPU가 없어도 모든 컴퓨터에서 실행할 수 있습니다. 자세한 내용은 [여기](링크)에서 확인할 수 있어요!

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

<img src="/TIL/assets/img/2024-07-14-MoreAIwithlesswhat_4.png" />

# 컨텍스트 창 문제

안녕하세요. 인코더-디코더 모델에 관해 소개해 드렸습니다. 그렇게 좋다면 왜 아무도 사용하지 않을까요?

거기서 중요한 것은 컨텍스트 창입니다!

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

이 개념에 따르면 인퍼런스 과정 중에 Large Language Model이 처리할 수 있는 토큰의 양을 설명합니다. 그래서 Flan-T5-XL(다른 변형들에 대해서도 동일합니다)을 살펴보죠: 프롬프트와 생성 사이의 토큰 최대 양은 512개입니다.

이것은 Gemini가 컨텍스트 창에서 최대 100만 토큰까지 사용하면서 완전한 기능과 정밀도를 보고할 때 실제로 적은 양이라고 할 수 있습니다.

이제 이 윈도우의 크기와 인텍스트 검색의 성능이 모델마다 다르다는 것은 사실입니다. 즉, 모든 컨텍스트 윈도우가 동일하게 작동하는 것은 아닙니다. 컨텍스트 윈도우의 길이와 모델 성능의 변동성은 기존의 어플리케이션 설계 시 고려해야 하는 중요한 디자인 요소 범주를 소개합니다.

![이미지](/TIL/assets/img/2024-07-14-MoreAIwithlesswhat_5.png)

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

왜 맥락이 중요할까요? 대형 언어 모델(Large Language Models, LLMs)은 단어 처리 과정에서 실제 의미를 이해하기 위해 맥락 창을 강하게 의존합니다. 대화를 상상해보세요 — 적절하게 대답하려면 이전에 언급된 내용을 고려해야 합니다. 마찬가지로 LLMs는 시퀀스의 이전 단어(토큰)를 분석하기 위해 맥락 창을 사용합니다. 이를 통해 LLMs는 다음과 같은 기능을 수행할 수 있습니다:

- 주제에 집중: 과거 정보를 참조함으로써 LLMs는 관련 없는 주제로 산만하게 이야기하지 않을 수 있습니다. 대화나 글쓰기에 명확히 집중할 수 있습니다.
- 자연스럽게 흘러가게: 적절한 크기의 맥락 창을 통해 LLMs는 대화의 전체 흐름에 매끄럽게 연결되는 응답을 생성할 수 있습니다. 갑작스럽게 변경되거나 터무니없는 답변은 과거의 일이 됩니다.
- 더 똑똑한 예측하기: 더 넓은 맥락은 LLMs에게 다음 단계를 예측할 수 있는 소중한 단서를 제공합니다. 이는 더 잘 조언된 예측을 이끌어내고 궁극적으로 높은 품질의 텍스트 생성을 이끌어냅니다.

실제로 LLMs는 모든 것을 알지는 않습니다. 즉, 이들은 거대한 데이터셋에서 훈련을 받았지만 훈련 시점의 시간 제한과 크기 제한이 있습니다.

- 시간 제한은 훈련 시작의 고정된 날짜로, 그 이후 일어난 모든 사항은 모델에 알려지지 않은 것입니다.
- 크기 제한은 훈련 비용과 훈련 데이터셋의 선택으로 제한됩니다.

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

특정 지식 베이스에 대한 대형 언어 모델의 경우 답을 찾지 못해서 환각하는 경향이 있습니다. 이러한 경우에 RAG가 최선의 선택입니다. RAG(검색 증강 생성)은 정보 검색과 생성 모델을 결합한 기술입니다.

RAG를 사용할 때 응답을 생성하는 과정 중에 프롬프트에 정보 조각을 추가합니다. 이 정보는 보통 데이터베이스를 특별한 검색 기술을 사용하여 검색해서 얻은 문단이나 텍스트 조각입니다. LLM이 응답을 생성해야 할 때, 이 검색된 텍스트가 추가 입력으로 제공됩니다.

RAG를 통해 프롬프트 또는 입력이 관련성이 있고 맥락을 갖는 지원 정보로 보강됩니다. 이를 통해 LLM은 사용자의 입력과 일치하는 정보를 제공하고 맥락적으로 정확한 응답을 생성할 수 있게 됩니다.

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

# 어쩌면 우리가 필요한 것은 무지한 LLM일지도 몰라요

LLM이 갖고 있는 지식은 그 안에 내재되어 있어요. 그러나 이 지식은 노출된 학습 데이터에 한정되어 있어요. 이 데이터는 특정 시간대에만 해당되며, 가장 최신 정보나 현재의 사건을 포함하지 않을 수 있어요.

LLM은 대화형 대화를 관리하고 자연어 생성(NLG) 기술을 사용하여 간결한 응답을 생성하는 데 중요한 역할을 합니다. 효과적인 소통을 위해 필요한 기반을 제공하는 중추 역할을 하죠.

하지만 LLM에 내장된 필요 없는 지식을 모두 제거한다면 어떨까요? 정말로 필요한 지식일까요? 끝으로 RAG 전략을 사용하여 필요한 것을 언제든지 주입할 수 있어요. 이 상황에서 LLM은 다른 모든 맥락에 대해 무지하게 될 수 있고, 완전히 무관심해질 수도 있어요.

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

LLM이 데이터나 복잡한 애플리케이션을 처리하지 않고 주요 작업에 집중할 때가 가장 좋은 상황입니다. RAG 구현을 통해 대규모 맥락 창이 필요한 사용 사례를 LLM과 별도로 처리할 수 있습니다. 이렇게 하면 LLM은 핵심 작업에 집중할 수 있으면서도 포괄적인 맥락 정보가 필요한 사용 사례의 요구를 충족시킬 수 있습니다.

'Textbooks Are All You Need'라는 유명한 연구는 완전히 새로운 패러다임을 소개합니다. 새로운 모델은 인간 학생처럼 교과서를 사용하여 훈련되었습니다. 이 아이디어의 놀라운 점은 이러한 교과서가 합성 생성되었으며, 가벼운 모델(Phi 1.5에는 13억 개의 매개변수만 있음)이 사람 평가 점수에서 뛰어난 성과를 내고 있다는 것입니다. 이것은 다양한 학문 분야에서 기본적인 추론만 가르치고, 다른 불필요한 지식 없이 자연어 능력을 부여하는 것과 같습니다.

LLM 커뮤니티에서 현재 진행 중인 한 가지 방향은 복잡한 프롬프트를 이해할 수 있는 일반적인 추론 모델에 초점을 맞추고 지식 기반을 RAG 전략이나 프롬프트에서 제공된 예제에 맡기는 것입니다. Phi-1.5와 LaMini는 매우 우수한 데이터로 가벼운 모델을 훈련시키는 몇 가지 시도 중 하나입니다.

예시로 몇 가지 연구를 소개합니다:

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

- 대형 언어 모델 기반 자율 에이전트에 대한 조사
- 에이전트들: 자율 언어 에이전트를 위한 오픈소스 프레임워크
- AgentVerse: 에이전트 간 협력 촉진과 에이전트들의 신생 행동 탐색

LLM이 데이터 처리에서 제외되면, 남은 것은 검색 애플리케이션을 최적화하는 것입니다.

![이미지](/TIL/assets/img/2024-07-14-MoreAIwithlesswhat_6.png)

# 아마도 우리는 AI를 축소할 수 있을지도...

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

대규모 Large Language Models (LLMs)이 똑똑함을 잃지 않고 줄어들 수 있을까 궁금했던 적이 있나요? 그렇다면 오늘의 블로그 포스트로 뛰어들어보세요! 레이어의 중복에 관한 흥미진진한 이야기와 AI 분야의 효율성을 새롭게 정의하는 방법에 대해 알아볼 거에요.

우선, 확실한 사실부터 말씀드리자면 모델의 거의 절반 이상은 그저 공간만 차지하고 있다고 해요. 맞아요! 최근 연구들에서 성능을 희생하지 않고 레이어를 줄일 수 있다는 사실이 밝혀졌답니다. 정말 멋지죠?

하지만, 먼저 상황을 파악해보죠. LLMs은 인기를 끌며 우리 일상에서 필수적인 도구로 자리잡았어요. 단순한 연구 프로젝트에서 시장의 거대한 존재로 성장하며 수십억 개의 매개변수를 자랑하고 있어요. 이러한 성장은 달러뿐만 아니라 에너지 소비와 환경 영향에서도 큰 비용이 발생해요.

## 레이어 퇴사 (AI를 위한!)

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

연구자들은 LLMs의 많은 층이 중복되어 있다는 것을 발견했어요. 즉, 이러한 층들이 모델의 전체 성능에 크게 기여하지 않는다는 뜻이에요. 이 발견은 이 모델들을 완전히 재설계하고 더 지속 가능하게 만드는 데 많은 가능성을 열어줍니다.

그래서, 어떻게 이 층 가지치기를 할까요? 한 번 살펴봅시다:

- 양자화: 우리는 부피가 큰 float32 가중치를 가벼운 정수로 변환하고 있어요. 이 기술은 계산 부하와 메모리 공간을 줄여 모델을 더 가볍고 효율적으로 만듭니다. 이들은 제 사랑하는 GGUF들이죠 😍!
- 가지치기: 요령껏 "수술대" 게임을 시작해 보는 건 어떨까요? 가지치기를 통해 필요 없는 가중치를 제거할 수 있어요. 이를 통해 모델의 성능을 훼밀하지 않으면서도 불필요한 부하를 제거할 수 있어요. 요컨대 AI에 스마트한 다이어트를 시켜주는 것이죠!
- 지식 증류: "적게가 더 많다"라는 속담을 들어보셨나요? 지식 증류는 큰 모델의 본질을 가져와 더 작고 특화된 버전으로 요약하는 것을 의미해요. 우리는 똑똑하고 다루기 쉬운 작은 부분을 가져온답니다.

<img src="/TIL/assets/img/2024-07-14-MoreAIwithlesswhat_7.png" />

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

# 하지만, 더 많은 것이 있어요!

혁신적인 연구자들은 모델 구조를 최적화하는 방법을 탐구하고 있어요. 그들은 리소스를 많이 소비하는 구성 요소인 self-attention을 더 효율적인 대안으로 대체하는 간결한 모델을 설계하고 있어요. 그리고 필요할 때만 관련 부분이 활성화되는 동적 네트워크를 잊지 말아야 해요. 친환경적인 것에 대해 이야기하고 있는 셈이죠!

한 연구에서는 LLaMA-270B와 같은 모델의 레이어 중 최대 50%가 제거되어도 성능에 영향을 주지 않을 수 있다는 혁신적인 내용이 제시되었어요. 네 맞아요, 여러분 — 우리는 레이어의 잠재적인 50% 감소에 대해 이야기하고 있어요! 이렇게 광범위한 가지치기 이후에 원활한 진행을 보장하기 위해서는 불일치를 해소하기 위해 QLoRA를 사용한 조정이 필요해요.

여기 주목할 점은, 이것이 일회성 현상이 아니라는 것이에요. Qwen에서 Mistral까지 널리 퍼져 있는 이 같은 겹침 현상을 모델이 공유하고 있어요. 우리는 AI 우주에서의 보편적인 진리를 발견하고 있는 걸까요?

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

# 더 많이 훈련하고, 작게 유지하기

"친칠라 법칙"이라는 용어를 들어본 적이 있을지 모르겠지만, 이것은 인공 지능 분야의 공식 법칙이 아니라 2022년 DeepMind에서 발표한 연구 논문에서 영감을 받은 개념이나 원칙입니다. "트레이닝 컴퓨트-최적 큰 언어 모델"이라는 제목의 이 논문은 친칠라 모델을 소개했는데, 이는 모델 크기와 데이터셋 크기 사이의 관계에 대한 AI 커뮤니티의 선입견에 도전했습니다.

과거 AI 연구에서는 다양한 작업에서 성능을 향상시키기 위해 매우 많은 파라미터를 가진 점차 커지는 모델을 만드는 추세가 있었습니다. 그러나 친칠라 모델은 이러한 대형 모델이 효과적으로 용량을 활용하지 못할 수도 있다고 제안했습니다. 이는 그들이 충분한 데이터로 훈련되지 않아 크기에 비해 데이터가 부족하다는 의미입니다.

친칠라 모델에서 주요 인사이트는 훈련 데이터셋의 크기를 증가시킴으로써 모델 성능 향상을 달성하는 데 있어서 모델 크기를 단순히 늘리는 것보다 더 큰 성과를 이끌어낼 수 있다는 것입니다.

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

![STLM Research Proposal](/TIL/assets/img/2024-07-14-MoreAIwithlesswhat_8.png)

안녕하세요! A*STAR 연구팀이 제안한 슈퍼 타이니 언어 모델(STLM)에 관한 야심찬 연구 제안이 있습니다. 이 모델은 10M, 50M 및 100M의 파라미터만으로 경쟁력 있는 성능을 GSM8K, MMLU 및 LMSYS 챗봇 아레나의 3B-7B 파라미터 범위 모델과 비교하려 합니다. 이들은 10층 llama2 기반 모델을 시작점으로 삼아 두 가지 방향에서 더 탐색할 계획입니다:

- 모델 수준: 가중치 결속, 바이트 수준 토큰화 및 풀링 메커니즘, 깊이 혼합, 레이어 스킵 및 텍스트 예측.
- 데이터 수준: 고품질 데이터 선택 및 지식 증류.

더 큰 데이터셋에 더 작은 모델을 훈련시키는 것이 더 효과적인 것으로 보입니다. 이 아이디어는 Chinchilla Law로 요약될 수 있으며, 즉, 상대적으로 큰 모델이 이미 존재한다는 전제 하에 훈련 데이터의 양을 늘리는 것이 모델 크기를 늘리는 것보다 더 나은 기계 학습 모델 성능을 향상시킨다는 것입니다.

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

# 결론... 현재까지

이 연구의 함의는 깊이 있습니다. 계층을 신중히 제거하면 단일 상업용 GPU 또는 CPU에서 실행할 수 있는 더 조밀한 모델이 만들어집니다. 이는 인공 지능이 보다 넓은 대중에게 접근 가능해지고 장벽을 허물고 혁신을 촉진한다는 것을 의미합니다.

그리고 더 큰 모델은 제거에 대해 더 강한 것으로 판명되어 여분의 중복성을 활용할 가능성을 보여줍니다. 이는 이기적인 상황입니다: 메모리 풋프린트가 감소하고 추론 시간이 빨라지며 더욱 친환경적인 행성이 됩니다.

그렇다면, 이 모든 것이 인공 지능의 미래에 대해 무엇을 의미하는 걸까요?

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

저희가 레이어 중복성에 대한 이해를 계속 발전시키는 동안, LLM 디자인에서 패러다임 변화를 목격할 것으로 예상됩니다. 더 작고 효율적인 모델들이 널리 채택되고 AI 기술의 민주화를 열 것입니다.

다음 글에서는 5억 개 이하의 매개변수를 가진 모든 현대 LLM에 대해 이야기하겠습니다. 기대해 주세요!

만약 이 이야기가 가치가 있었고 조금이라도 지원을 보내고 싶다면:

- 더 나은 RAG를 만들기 위한 제안을 댓글로 작성해 주세요.
- Medium 멤버십 가입 - (한 달에 $5 지불하여 무제한 Medium 이야기 읽기)
- Medium에서 팔로우 하기
- 제 최신 글을 읽어보세요 https://medium.com/@fabio.matricardi

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

한편으로 이곳을 검토해보세요:

참고 자료:

- Stackademic 🎓

끝까지 읽어주셔서 감사합니다. 다 읽어주셔서 감사합니다 전에:

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

- 작성자를 박수 치시고 팔로우해주세요! 👏
- 팔로우하기 X | LinkedIn | YouTube | Discord
- 다른 플랫폼 방문하기: In Plain English | CoFeed | Differ
- Stackademic.com에서 더 많은 콘텐츠 확인하기