---
title: "2024년 프로덕션 레디 LLM 애플리케이션을 위한 기술 스택"
description: ""
coverImage: "/TIL/assets/img/2024-07-15-TechStackForProduction-ReadyLLMApplicationsIn2024_0.png"
date: 2024-07-15 00:06
ogImage: 
  url: /TIL/assets/img/2024-07-15-TechStackForProduction-ReadyLLMApplicationsIn2024_0.png
tag: Tech
originalTitle: "Tech Stack For Production-Ready LLM Applications In 2024"
link: "https://medium.com/python-in-plain-english/tech-stack-for-production-ready-llm-applications-in-2024-5eb14105d1b4"
---


ML 작업을 6년 이상 한 뒤, AI/LLM 스타트업 3개를 론칭하고 수십 개의 프로젝트와 파이프라인을 구축했습니다. RAG와 Agent 프레임워크부터 적절한 테스트, CI/CD 및 관측성이 있는 LLM 기반 서비스까지 많은 LLM 도구를 실험했습니다.

본문은 다양한 이유로 가장 좋아하는 도구 목록입니다. RAG 데이터베이스와 에이전트에서부터 API 엔드포인트, HTTPS 및 자동 배포 파이프라인, 적절한 관측성까지 전체 LLM 애플리케이션 개발 수명주기를 따라갈 것입니다. 그 과정에서 각 단계의 프레임워크와 도구를 살펴볼 것입니다.

그리고 한 가지 더 — 나의 기술 스택에 동의하지 않을 수 있습니다. 그러나 주로 사용의 간결함을 기반으로 도구를 선택했습니다. 예를 들어, VPS에 배포하는 대신 Azure와 같은 대형 클라우드 제공업체를 사용할 수 있습니다. 그렇게 하면 확장성이 높아지고, out-of-box 서비스가 더 많아지며, 등등의 혜택을 받을 것입니다. 그러나 모든 것을 마스터하기까지 훨씬 더 많은 시간이 소요되며, 벤더 락인될 가능성이 높아집니다. Coolify와 함께 VPS를 사용하면 (나중에 언급할 것입니다), 시작하는 데 약 10분이 걸리고, 데이터베이스, 프론트엔드/백엔드 배포, 자동화된 CI/CD 및 백업이 필요한 모든 것이 직관적이고 사용하기 쉬운 UI로 제공됩니다.

![TechStackForProduction-ReadyLLMApplicationsIn2024](/TIL/assets/img/2024-07-15-TechStackForProduction-ReadyLLMApplicationsIn2024_0.png)

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

# LLM API 및 Self Hosted 옵션

대형 언어 모델 API에는 두 가지 주요 옵션이 있습니다:

- 자체 호스팅 버전을 선택하는 경우 Ollama가 그 방법입니다. "왜 자체 호스팅 LLM이 필요할까요? 더 비싸고 설정하기 어렵며 응답 품질도 떨어집니다"라고 묻는다면, 답은 간단합니다 — 개인 정보 보호입니다. 이미 개인 정보 보호와 LLM에 대해 썼지만 간단히 말하면, API를 사용하면 데이터를 다른 회사와 공유하며, 그 회사들은 데이터를 더 많은 제3자와 공유합니다. 어떤 프로젝트에서는 이를 용납할 수 없습니다.
- 프로젝트가 높은 개인 정보 보호와 보안을 요구하지 않는다면, 항상 Openrouter를 사용합니다. 간단하게 다양한 공급 업체 위에 구축된 Openrouter가 항상 다른 어떤 LLM 공급자보다 훨씬 낫습니다. 하나의 LLM에 대한 파이프라인을 작성했다면 요청 문자열을 몇 개 변경함으로써 LLM을 쉽게 변경할 수 있습니다. 추가 라이브러리나 공급 업체 전환 없이 매우 간단합니다. 자세한 내용을 알고 싶다면 Openrouter 안내서를 확인하세요.

together.ai, Mistral 등의 다른 LLM 공급 업체도 많지만, Openrouter를 더 선호하는 이유는 간단한 인터페이스와 제공하는 추가 유용한 기능(자동 로깅, 모델 비교, 자동 모델 선택, 무료 평가판 등) 때문입니다.

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


![이미지](/TIL/assets/img/2024-07-15-TechStackForProduction-ReadyLLMApplicationsIn2024_1.png)

# RAG 데이터베이스

대부분의 LLM은 다음과 같은 문제를 가지고 있습니다:

- 최신 정보가 아님;
- 도메인 지식이 없음;
- 환각;
- 출처를 인용하지 않음;
- 편향된 응답.


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

모든 이 문제들은 적절한 RAG (검색 보완 생성)을 사용하여 해결할 수 있어요. 모든 RAG 알고리즘은 어떠한 형식의 벡터 데이터베이스를 요구해요. 제가 가장 선호하는 선택지는 다음과 같아요:
  
- ChromaDB — 프로토타입 및 PoC (개념 증명) 작업에 완벽해요. SQLite를 기반으로 구축되어 있어 설치가 매우 쉽고 (PC의 파일로 구성되어 있어요), 사용법도 매우 직관적이고 문서화가 잘 돼 있어요.
- 더 심각한 프로젝트를 위해, 저는 Supabase (PostgreSQL 기반)와 PGVector 플러그인을 사용해요. Supabase의 장점은 직관적이고 사용하기 쉬운 UI, 백업, 역할 관리, API 및 Python 패키지와 같은 많은 기능이 있어요. Supabase를 사용하는 방법에 대한 가이드도 많이 있고, 개인적으로 좋아하는 점은 오픈 소스이며 자체 서버에 호스팅할 수 있다는 점이에요.

보통은 사용자 지정 솔루션을 사용하지만, 때로는 LangChain이 RAG 파이프라인 작성에 크게 도움이 될 수 있어요.

![이미지](/TIL/assets/img/2024-07-15-TechStackForProduction-ReadyLLMApplicationsIn2024_2.png)

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

더 정교한 LLM 파이프라인을 생성하거나 LLM을 사용하여 어려운 문제를 해결하려면 에이전틱 접근 방식을 사용해야 합니다. LLM 에이전트는 순차적(단계별) 추론을 사용하는 AI 시스템입니다. 기본적으로 LLM에 대한 지침 세트를 정의하는 대신 문제를 해결할 수 있는 자유와 도구 세트를 제공합니다.

저는 에이전트를 생성하기 위해 사용하는 Python 라이브러리들은 다음과 같습니다:

- LangChain — 현재 가장 인기 있는 에이전트용 프레임워크이며, 훌륭한 문서, 거대한 커뮤니티, 그리고 가이드가 많이 포함된 많은 자습서가 있습니다. 제가 권장하는 한 가지는 create_react_agent나 create_json_agent와 같은 새로운 방법 대신 initialize_agent 메서드를 사용하는 것입니다. 이 방법을 선택한 이유는 새로운 방법에는 아직 많은 가이드나 지침이 없어서 제 개인적으로는 조금 혼란스러웠기 때문입니다. initialize_agent 메서드는 사용하기 쉽고 다양한 접근 방법을 가진 가이드가 수십 개 있습니다.
- LlamaIndex — LangChain의 주요 경쟁 업체로, 인덱싱 및 데이터 검색에서의 우수한 최적화로 알려져 있습니다. 간단히 말해, RAG 파이프라인을 구축할 때, llamaindex가 더 빠르고 나은 옵션일 수 있지만, 더 복잡한 프로젝트와 파이프라인의 경우 LangChain을 사용하는 것이 좋습니다.

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

일반적으로, LangChain을 사용하여 LLMs를 학습하는 것이 더 기능이 풍부하고 사용하기 쉽습니다.

# 관찰 가능성

이제 LLM 애플리케이션을 배포했다면, 답변의 품질, 정확성 및 속도를 측정해야합니다. 하지만 LLMs로 어떻게 할 수 있을까요? 저는 추적, 평가, 시각화 및 모니터링을 제공하는 두 개의 오픈 소스 LLM 관찰 가능성 플랫폼을 사용합니다:

- Arize Phoenix
- LangSmith

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

저는 LangSmith를 사용하기가 조금 더 쉬웠어요. 그들은 입력-출력을 추적하는 아주 좋은 방법을 가지고 있고, 평가를 개발하는 것은 꽤 빠릅니다. 그러나 Phoenix는 데이터를 더 많이 추적할 수 있어서 더 나은 무료 티어를 가지고 있습니다. LLM observability로 시작하려면 LangSmith를 꼭 추천드릴 수 있어요 — PoC와 프로토타입에 완벽합니다.

![Tech Stack](/TIL/assets/img/2024-07-15-TechStackForProduction-ReadyLLMApplicationsIn2024_3.png)

# 백엔드

백엔드는 항상 Python, FastAPI, Pydantic를 사용해요. 여기에 사용하는 이유들을 짧게 나열해볼게요:

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

- FastAPI은 상당히 빠르며 매우 좋은 문서가 제공되며 데이터베이스 ORM, 인증, CORS 및 멀티 스레드 배포와 같이 LLM 애플리케이션 개발에 필요한 모든 기능을 포함하고 있어요.
- Pydantic은 함수와 엔드포인트에 엄격한 유형을 추가하는 데 도움이 되어 항상 원하는 유형의 모든 변수를 확신할 수 있게 해줘요. 애플리케이션을 에러로부터 보호하는 데 도움이 됩니다.

일반적으로 Gunicorn + Uvicorn 워커와 함께 결합하여 동시에 여러 요청을 처리할 수 있어요.

그럼에도 불구하고 Flask, Django와 같은 모든 백엔드 프레임워크를 사용할 수 있습니다. 이들은 모두 공통 기능을 가지고 있으며 모두 제품 준비가 되어 있습니다.

# 배포

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

배포 작업에 대해서 말씀드리자면, 저는 항상 Docker를 사용하여 애플리케이션을 컨테이너화합니다. 주요 이유는 다음과 같습니다:

- 재현성 — 앱과 함께 Docker 이미지를 빌드하면 어떤 시스템이나 CPU 아키텍처에서도 실행할 수 있습니다;
- 확장성 — 앱이 도커화되어 있으면 여러 인스턴스를 쉽게 생성하거나 클러스터에 배포할 수 있습니다;
- 배포 — Docker 이미지를 배포하는 것이 매우 간단합니다. 특히 Docker Compose를 사용하는 경우 더욱 쉬워집니다. 데이터베이스, 리버스 프록시 및 백엔드를 docker-compose.yml 파일에 추가하여 모든 것을 별도로 걱정하지 않고 한꺼번에 배포할 수 있습니다.

Docker 이미지를 배포할 때 Coolify를 사용하고 있습니다 — 이는 배포 프로세스를 크게 단순화해 주는 오픈 소스, 자체 호스팅 플랫폼입니다. 다음과 같은 기능을 제공합니다:

- SSL 인증서 자동 관리 — 백엔드를 https 호환으로 만들 필요가 없습니다;
- 벤더 잠금 없음 — 모든 제공업체로부터 자유롭게 이점을 취할 수 있는 추상화 계층을 추가하여 Coolify를 어디서든 VPS 또는 클라우드 제공자에 배포할 수 있습니다;
- 간편한 리버스 프록시 — Coolify에서는 보안과 도메인에 대해 걱정할 필요가 없습니다. 사용하려는 도메인만 Coolify에 작성하면 됩니다;
- 자동화된 DB 백업 — PostgreSQL, MySQL, Redis 등과 같은 일반 이미지를 사용하는 경우 몇 번의 클릭으로 백업 설정을 할 수 있습니다. 사용자 정의 솔루션을 작성하거나 돈을 지불할 필요가 없습니다.
- CI/CD 파이프라인 — 깃허브와 연결하여 자동 배포 파이프라인을 무료로 이용할 수 있습니다. 앱을 업데이트하면 자동으로 배포됩니다;
- 협업 기능 — 팀원들과 Coolify를 공유하고, 서로 다른 측면의 앱에 대해 함께 작업할 수 있습니다. 각 사용자에 대한 권한도 제어할 수 있습니다.
- 모니터링 및 알림 — Telegram/Discord와 같은 다양한 응용 프로그램 중 하나를 설치해 배포 상황에 변화가 있을 때 알림을 받을 수 있습니다.

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

![Tech Stack](/TIL/assets/img/2024-07-15-TechStackForProduction-ReadyLLMApplicationsIn2024_4.png)

# 요약

이 기사에서는 여러분의 LLM 애플리케이션을 작성하기 위한 멋진 기술 스택을 다루었습니다. 이 도구들을 사용함으로써 다음을 얻을 수 있습니다:

- 신뢰할 수 있고 쉽게 사용할 수 있는 벡터 DB;
- 다양한 크기와 적절한 개인 정보 보호를 갖춘 모든 문제에 대한 LLM;
- 공급업체 락인 없음;
- 파이프라인을 위한 적절한 로깅과 모니터링;
- 애플리케이션을 쉽게 배포할 수 있는 확장 가능하고 유지 보수가능한 방법.

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

기술 스택을 댓글로 공유해 주세요. 함께 토론하며 최적의 도구 세트를 찾아보겠습니다! 이 글이 마음에 드셨다면 박수를 보내주시고 답글을 남겨주세요. 여러분의 피드백은 저에게 귀중합니다.

마지막으로, 제 스타트업인 비즈니스 아이디어를 검증하는 https://validator.yazero.io 를 방문해 지원해 주시면 감사하겠습니다.

# 참고 소스

- https://www.trychroma.com
- https://supabase.com
- https://python.langchain.com/v0.1/docs/modules/agents/
- https://www.llamaindex.ai
- https://phoenix.arize.com
- https://stackshare.io/langsmith
- https://fastapi.tiangolo.com
- https://docs.pydantic.dev/latest/
- https://www.docker.com
- https://coolify.io

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

# 친절한 한국어 번역 🚀

In Plain English 커뮤니티에 참여해 주셔서 감사합니다! 이제 가시기 전에:

- 반드시 박수를 보내고 작가를 팔로우해주세요 👏️️
- 저희를 팔로우해주세요: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼도 방문해주세요: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠를 만나보세요