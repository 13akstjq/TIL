---
title: "코드 없는 GenAI 에이전트 워크플로우 오케스트레이션 로컬 Mistral AI 모델과 함께하는 AutoGen Studio 사용법"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-NoCodeGenAIAgentsWorkflowOrchestrationAutoGenStudiowithLocalMistralAImodel_0.png"
date: 2024-07-14 20:04
ogImage: 
  url: /TIL/assets/img/2024-07-14-NoCodeGenAIAgentsWorkflowOrchestrationAutoGenStudiowithLocalMistralAImodel_0.png
tag: Tech
originalTitle: "No Code GenAI Agents Workflow Orchestration AutoGen Studio with Local Mistral AI model"
link: "https://medium.com/towards-data-science/no-code-genai-agents-workflow-orchestration-autogen-studio-with-local-mistral-ai-model-7566546a16d9"
---


자유 링크 — 이 LinkedIn 게시물을 좋아해 주세요

AutoGen은 Microsoft에서 개발한 프레임워크로, 특히 LLM 에이전트를 조정하는 다중 에이전트 애플리케이션 개발을 간소화하기 위해 설계되었습니다.

다중 에이전트 애플리케이션은 여러 LLM 또는 다중 모달 에이전트 또는 엔티티가 전체 워크플로에서 상호 작용하여 특정 목표나 작업을 달성하는 시스템을 의미합니다. 이러한 에이전트는 LLM 에이전트, 검색에이전트 또는 독립적인 결정을 내리거나 기능 호출 또는 조치를 취할 수 있는 다른 에이전트일 수 있습니다.

AutoGen에 대해 더 알고 싶다면 이전 기사를 참조해주세요: AutoGen In-depth yet Simple.

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

미스트랄 AI는 2023년 4월에 메타와 구글 연구원 출신인 프랑스의 AI 회사입니다. 이 회사는 오픈 대형 언어 모델 (LLM)을 제작하는 데 중점을 두며 오픈 소스 AI 모델의 중요성을 강조합니다.

이 기사에서는 AutoGen Studio의 직관적인 노코드 플랫폼과 지역 통합 미스트랄 AI 모델의 혁명적인 융합에 초점을 맞출 것입니다. 이 조합은 AI를 응용프로그램에 쉽게 통합하는 데만 그치는 것이 아니라, 다양한 생성적 AI 에이전트를 어떻게 상호 작용, 배포하며 현실적인 업계 업무 흐름에서 혜택을 누르는 데에 도움을 주는 데 있습니다.

# AutoGen Studio의 인터페이스 탐색:

## AutoGen Studio 설치

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

AutoGen Studio는 오픈 소스 코드로 여기에서 사용할 수 있으며 pip를 통해 설치할 수 있습니다.

```js
pip install autogenstudio
```

## Autogen Studio 실행

설치가 완료되면 터미널에 다음을 입력하여 웹 UI를 실행할 수 있습니다:

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


autogenstudio ui --port 8082


<img src="/TIL/assets/img/2024-07-14-NoCodeGenAIAgentsWorkflowOrchestrationAutoGenStudiowithLocalMistralAImodel_0.png" />

AutoGen Studio는 다중 에이전트 AI 응용 프로그램의 생성 및 관리를 용이하게 하는 간소화되고 사용자 친화적인 인터페이스를 제공합니다. 이 인터페이스는 Skills, Models, Agents 및 Workflows와 같은 몇 가지 섹션으로 분할되어 있으며, 각각이 응용 프로그램 개발에 필수적인 역할을 합니다.

## Skills


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

기술 섹션에서는 에이전트가 작업을 해결하는 데 사용할 Python 함수를 개발하고 저장할 수 있습니다. 여기는 응용 프로그램의 기능이 정의되고 개발되는 프로그래밍 환경입니다.

## 모델

모델 섹션은 GPT-4 및 기타 로컬 또는 사용자 정의 모델을 구성하고 관리할 수 있는 사용자 모델입니다.

사용자는 Mistral AI와 같은 모델을 설정할 수 있으며, 스튜디오가 로컬 AI 모델 통합 및 관리를 지원한다는 것을 나타냅니다. 이러한 유연성을 통해 OpenAI의 GPT-4 같은 강력한 모델 및 특정 작업에 맞게 개발된 자사의 또는 전문화된 모델을 함께 사용할 수 있습니다.

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

# 에이전트

'에이전트' 섹션은 재사용 가능한 에이전트를 구성하는 데 사용됩니다. 사용자는 로컬 어시스턴트, 주 에이전트, 사용자 프록시 등 다양한 에이전트의 역할을 생성하고 정의할 수 있으며, 이들이 앱의 워크플로우 내에서 상호 작용하는 방식을 조율할 수 있습니다.

# 워크플로우

'워크플로우' 섹션은 마법이 일어나는 곳입니다. 사용자는 시스템이 작업을 처리하는 방식을 정의하는 복잡한 워크플로우를 설계할 수 있습니다. 이곳에서 모든 구성 요소가 하나로 결합됩니다: 'Skills'는 논리를 제공하고, 'Models'는 AI의 지능을 제공하며, '에이전트'는 작업을 수행하며, '워크플로우'는 모든 것을 일관된 시스템으로 연결합니다.

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

플랫폼의 인터페이스는 직관적이며 각 섹션이 명확히 표시되어 있고 시각적 레이아웃을 통해 네비게이션을 간단하게 유도합니다. 새로운 에이전트, 모델 또는 기술을 추가하는 경우, 일반적으로 몇 번의 클릭만으로 간단한 프로세스로 진행됩니다. 생생한 색상과 현대적인 디자인 요소의 사용으로 사용자 경험을 쾌적하고 사용하기 쉽게 만듭니다.

## 지역 미스트랄 AI 모델 통합

지역 미스트랄 AI 모델을 AutoGen Studio의 생태계에 통합하는 것은 매우 쉽고 직관적입니다. 사용자는 자신의 지역 모델을 스튜디오에 추가하고 앱 내의 다른 구성 요소와 상호작용하는 방법을 정의할 수 있습니다. GPT-4 비전, GPT 3.5, GPT 4 turbo와 같은 다중 모달 모델의 파워와 미스트랄 AI와 같은 지역 모델을 결합해야 하는 사용자들에게 특히 유용합니다.

요약하자면, AutoGen Studio는 다중 에이전트 AI 기반 응용 프로그램을 구축하기 위한 포괄적이고 접근하기 쉬운 도구로, 다양한 AI 모델 및 사용자 정의 기술을 수용하여 개발자와 비즈니스에게 다재다능한 선택지를 제공합니다.

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

# 로컬 Mistral AI 모델 설정하기:

로컬 Mistral AI 모델 설정은 여러 단계로 이루어져 있으며, 각 단계마다 시스템이 Ollama사의 Mistral 모델과 같은 대규모 언어 모델을 로컬에서 실행할 수 있는 환경으로 준비됩니다. 각 단계와 그 필요성에 대한 설명은 다음과 같습니다:

- Ollama 설치: Ollama는 대규모 언어 모델에 액세스할 수 있는 플랫폼입니다. Ollama를 설치하는 것은 첫 번째 단계이며, Mistral 모델이 실행될 환경입니다. 이를 통해 사용자는 Llama 2, Code Llama, Mistral과 같은 기존 모델을 실행하거나 사용자 정의 및 배포할 수 있습니다. Ollama는 다음 링크에서 다운로드할 수 있습니다: https://ollama.ai/

![이미지](/TIL/assets/img/2024-07-14-NoCodeGenAIAgentsWorkflowOrchestrationAutoGenStudiowithLocalMistralAImodel_1.png)

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

2. Ollama Mistral 실행: Ollama 플랫폼에서 아래 명령을 실행하여 Mistral 모델을 시작합니다. 이 명령은 모델을 설정하고 필요한 데이터를 로드하며 쿼리 처리를 담당하는 서비스를 시작합니다.

```js
ollama run mistral
```

2. LiteLLM 설치: LiteLLM 라이브러리는 http 엔드포인트에서 언어 모델을 실행하는 데 도움을 주는 도구입니다. LiteLLM을 설치하거나 업그레이드하면 라이브러리의 최신 버전을 사용할 수 있어서 호환성과 기능의 관점에서 중요합니다.

```js
pip install litellm --upgrade
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

3. Gunicorn 설치: Gunicorn은 UNIX 시스템용 Python WSGI HTTP 서버로, Python 웹 애플리케이션을 실행하는 데 사용됩니다. LiteLLM 프록시를 실행하는 데 필요하며 이를 통해 귀하의 언어 모델에 로컬 HTTP 요청을 보낼 수 있습니다.

```js
pip install gunicorn
```

4. Ollama/Mistral 모델을 사용하여 LiteLLM 실행: 이 단계는 모델을 초기화하고 실행할 준비를 하는 과정입니다. 이 명령은 LiteLLM이 Ollama가 제공하는 Mistral 모델을 사용하도록 지시합니다. 상호작용을 위해 모델을 준비하여 쿼리를 시작하고 응답을 받을 수 있게 합니다.

이 프로세스가 끝나면 로컬의 Mistral 모델이 0.0.0.0:8000에서 1개의 워커와 함께 시작됩니다.

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

이러한 각 단계는 로컬에서 AI 모델과 상호 작용할 수 있는 완전한 작업 환경을 설정하는 데 필요합니다. 이 로컬 설정을 통해 개발자는 클라우드 서비스와 지속적으로 통신할 필요 없이 AI 모델을 테스트하고 통합할 수 있습니다. 이는 개발 속도, 오프라인 기능, API 호출 비용 절감 등에 도움이 될 수 있습니다.

![image](/TIL/assets/img/2024-07-14-NoCodeGenAIAgentsWorkflowOrchestrationAutoGenStudiowithLocalMistralAImodel_2.png)

# AutoGen 및 Mistral AI와의 Workflow Orchestration:

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

파이썬 스크립트를 작성하여 사인 파형을 그래프로 그리고 'sine_wave.png' 파일로 저장하는 상황에서 AutoGen과 Mistral AI를 함께 사용하여 원활한 워크플로우를 만들 수 있습니다. AutoGen Studio는 멀티 에이전트 애플리케이션을 구축하고 관리하기 위한 인터페이스로 작용하며, Mistral AI는 로컬 모델을 통해 처리 능력을 제공합니다.

다음은 두 플랫폼의 기능을 활용하여 오케스트레이션하는 방법입니다:

## 1. 워크플로우 생성:

- AutoGen Studio에서 사인 파형 이미지를 생성하는 특정 작업을 위한 새로운 워크플로우를 시작합니다.
- 우리는 워크플로우에 "Local mistral Agent Workflow"와 같은 이름을 구성합니다.
- 워크플로우 사양에는 송신자 및 수신자 에이전트가 포함되어 있으며, 'userproxy'가 요청을 시작하는 송신자로, 'primary_assistant'가 요청을 처리하고 출력을 제공하는 수신자로 지정될 수 있습니다.
- 사용자 프록시의 역할은 사용자와 상호 작용하고 primary_assistant가 생성한 코드를 실행하는 것입니다. 코드에 오류가 발생하면 해당 오류를 primary_assistant 에이전트에 보냅니다.
- primary_assistant 에이전트는 로컬 Mistral AI 모델을 활용하여 우리 시나리오에서 코드를 생성합니다. 코드에 오류가 있으면 사용자 프록시 에이전트가 보낸 실행 오류에 따라 코드를 조정합니다.

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

## 2. 모델 구성:

- '미스트랄 로컬 모델'이라는 모델이 AutoGen Studio 내에 설정되어 Mistral AI와 상호 작용하도록 구성됩니다.
- 모델의 API 엔드포인트는 Mistral AI가 실행 중인 로컬 서버 주소로 구성됩니다 (예: http://0.0.0.0:8000).
- 이 설정을 통해 AutoGen Studio가 데이터를 Mistral AI 모델로 보내고 처리된 결과를 받을 수 있습니다.

## 3. 세션 실행:

- AutoGen Studio의 'Playground' 인터페이스에서 사용자는 '로컬 미스트랄 에이전트 워크플로'를 선택하여 새 세션을 시작합니다.
- 사용자는 삼각파 플롯을 생성하기 위한 명령 또는 요청을 입력합니다.
- 요청은 사용자 프록시 에이전트가 주요 어시스턴트 에이전트로 전송되어 Mistral AI 모델을 활용합니다. 이렇게 하여 Python 스크립트가 생성되고 사용자 프록시 에이전트가 코드를 실행합니다. 오류가 발생하면 이를 주요 어시스턴트 에이전트로 되돌려보냅니다. 주요 어시스턴트 에이전트가 코드를 재조정하여 사용자 프록시 에이전트로 다시 전송하고 코드가 작동되어 삼각파 플롯 이미지를 생성합니다.

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

## 4. 결과 및 검토:

- Mistral AI에서 스크립트를 실행하면 'sine_wave.png' 파일이 생성되어 디스크에 저장됩니다.
- AutoGen Studio 인터페이스에서는 세션 진행 상황 및 결과가 표시되며, 'sine_wave.png' 파일과 'sine_wave'를 플로팅하는 파이썬 코드가 생성됩니다.
- 사용자는 결과를 검토하고, 널리 사용할 수 있도록 워크플로를 게시하거나, 처음부터 다시 시작하려면 모델을 삭제하거나, 스크립트를 개선하고 워크플로 매개변수를 더 세밀하게 조정하기 위해 추가적으로 반복할 수 있습니다.

# 결론:

AutoGen Studio와 로컬 Mistral AI 모델은 GenAI 에이전트의 워크플로를 조정하는 데 주요한 진전을 보여줍니다. 이 아키텍처는 AutoGen Studio의 직관적인 디자인과 Mistral AI의 우수한 성능을 결합하여, 다중 에이전트가 기여할 수 있는 환경을 구축합니다.

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

AutoGen Studio와 로컬 Mistral AI 모델의 장점:

- 쉬운 통합: 로컬 Mistral AI 모델이 AutoGen Studio에 쉽게 통합되어 다양한 LLM 모델을 한 배포로 효율적으로 활용하는 과정을 간소화합니다.
- 맞춤 및 유연성: 개발자들은 AI 모델을 특정 요구 사항에 맞게 맞춤화하여 하나의 프로젝트의 다양한 필요에 완벽히 맞는 맞춤형 솔루션을 제공할 수 있습니다.
- 향상된 성능: AutoGen과 함께 로컬에서 AI 모델을 실행하면 지연 시간이 줄어들고 응답 시간이 향상되어 실시간 애플리케이션 및 민감한 작업에 중요합니다.
- 데이터 개인 정보 보호 및 보안: AI 모델의 로컬 실행은 데이터에 대한 완전한 통제를 제공하며 외부 환경에 노출되어서는 안 되는 민감하거나 전문성이 있는 정보에 대해 핵심적인 역할을 합니다.
- 비용 효율성: 클라우드 기반 AI 서비스에 대한 의존을 최소화하여 데이터 전송 및 LLM API 사용과 관련된 비용을 크게 절감할 수 있습니다.
- 오프라인 기능: 로컬 Mistral AI 모델을 사용하면 인터넷 연결이 없어도 시스템이 작동되는 것을 보장하여 AI 기반 솔루션의 오프라인 기능을 허용합니다.
- UI로 복잡한 AI 워크플로우 작성: 사용자는 AutoGen Studio의 노코드 인터페이스를 활용하여 AI 기반 워크플로우를 직관적으로 설계하고 관리할 수 있습니다.

이 기사에서는 AutoGen Studio와 로컬 Mistral AI 모델의 통합이 GenAI 워크플로우의 세계에서 혁명적인 발전을 가져왔습니다. 성장하는 고급 AI 애플리케이션 수요를 충족시키는 사용자 친화적이고 유연하며 비용 효율적인 솔루션을 제공하여 AI 애플리케이션을 만들기가 언제보다 쉬워졌습니다.

# 떠나기 전에! 🦸🏻‍♀️

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

- 만약 이 글이 유익하다고 느끼신다면, 이 LinkedIn 글을 ‘좋아요’해 주세요. 또한 무료 친구 링크를 LinkedIn 글에서 찾을 수 있습니다. 여러분의 참여는 이 글을 더 홍보하는 데 도움이 될 것이며, 여러분의 지지는 저에게 큰 원동력이 됩니다. ✍🏻🦾❤️
- 이 글을 50번 클랩해 주시면, 정말로 저를 도와주시고 다른 사람들에게 이 글을 알릴 수 있습니다. 👏
- 제 최신 글을 보려면 Medium, LinkedIn에서 팔로우하고 구독해주세요. 🫶

# 흥미 있는 주제라면, 더 많은 글을 읽어보실 수 있습니다.