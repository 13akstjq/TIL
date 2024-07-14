---
title: "오늘 바로 해킹의 첫 걸음 내딛기"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-Takeyourfirststepinhackingtoday_0.png"
date: 2024-07-14 20:35
ogImage: 
  url: /TIL/assets/img/2024-07-14-Takeyourfirststepinhackingtoday_0.png
tag: Tech
originalTitle: "Take your first step in hacking today "
link: "https://medium.com/@imran-niaz/take-your-first-step-in-hacking-today-b880f017edca"
---


만약 좋고 전문적인 방법으로 해킹을 배우고 싶다면, 해킹의 첫 걸음을 따라가는데 도움 될 수 있는 몇 가지 항목을 배워야 할 수도 있습니다. 이미 사이버 보안 자격증이나 학사 학위를 취득했다면 더 좋습니다.

# 버전 0.01 :

모든 문서는 기본 버전이며 매 번 업데이트되므로 방문할 때마다 최신 정보를 얻을 수 있습니다.

# 작은 걸음

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

- 리눅스, 윈도우, macOS를 위한 명령어
- 디렉토리 이동
- 웹 또는 데스크톱 애플리케이션 지식
- 데이터베이스 관리 시스템 작동 방식
- 페이로드와 그 효과
- 요청 유형
- 진입하는 세 가지 방법

# 리눅스, 윈도우, macOS를 위한 명령어,

해킹을 배우려는 사람이라면 명령어 이해가 매우 중요합니다. 때로는 여러 가지가 여러분에게 열려 있지만 들어가려고 노력해야 할 때가 있습니다.

# Curl 요청에 대한 매우 간단한 예제

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

이 명령어는 매우 간단해요. curl을 사용해서 명령어를 실행하면 되요.

![사진](/TIL/assets/img/2024-07-14-Takeyourfirststepinhackingtoday_0.png)

Gabriel Heinzer님의 Unsplash에서 제공된 사진입니다.

```js
curl -X POST -H "Content-Type: application/json" -d '{"username":"exampleuser","password":"examplepass"}' http://example.com/api/login
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

cURL 명령어의 구성 요소를 살펴보겠습니다.

Curl은 -X POST를 사용하여 Curl과 함께 사용하는 요청 방법을 말합니다. `Post`를 사용하여 입력 정보를 서버로 제출합니다.

# 웹 또는 데스크톱 애플리케이션 지식.

웹 테스팅을 배우고 싶다면 웹사이트 해킹을 하려는 것이 중요합니다. 모든 것을 잘 알아야 하는 것이 중요한 것은 아닙니다. 작은 단계로 배워야 합니다. 예를 들어,

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

온라인 오픈 웹 서버를 탐험하고 사이버 보안 직업으로 이끌어주는 중이시군요.

웹 애플리케이션에서 버그나 취약점을 찾는 것은 누구에게나 쉬울 수 있어요. 몇 가지 간단한 방법을 사용하면 됩니다.

Google Dorks를 이용해 공개 서버의 데이터를 탐색할 수 있어요. 구글 해킹 데이터베이스

```js
intitle:index.of intext:crm.zip
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

이 간단한 구글 스트링은 귀하의 첫 번째 단계를 이끌어주는 데 도움이 될 수 있습니다. 이렇게 쉬울 것이라고 생각하실 수도 있습니다. 네, 맞습니다. 그러나 이러한 종류의 스트링을 사용하여 많은 중요한 정보를 발견했습니다.

[Takeyourfirststepinhackingtoday_1.png 이미지](/TIL/assets/img/2024-07-14-Takeyourfirststepinhackingtoday_1.png)

Bernd 📷 Dittrich의 Unsplash에서 제공하는 사진입니다.

웹 애플리케이션이 데이터베이스와 함께 작동하는 방식에 관심이 있다면 이 문서를 확인해보세요. 웹에 대한 기본 지식만 있다면 더 잘 이해할 수 있습니다.

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

# 데이터베이스에서 외래 키 관계를 정의할 때 마주칠 수 있는 문제

# 여러분은 아마도 외래 키가 일반적으로 데이터베이스의 두 테이블 사이에 연결을 설정하는 데 사용된다는 것을 알고 있을 겁니다...

medium.com

# 공격적인 경로 vs 방어적인 경로:

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

사이버 보안을 향한 당신의 길은 이미 분명해야 합니다. 제가 강의에서 언급한 것처럼, 프로그래밍이 잘되어야 하는 경우도 있습니다. 복잡한 문제를 해결할 수 없습니다. 방어 분야에서 활동하고 싶다면 다음과 같은 것들에 대한 좋은 이해가 있어야 합니다.

- 사이버 보안 소개
- 네트워크 보안
- 안전한 코딩과 응용 프로그램 보안
- 암호학
- 사이버 위협 인텔리전스
- 사고 대응과 관리
- 위험 관리와 준수
- 윤리적 해킹 및 침투 테스트
- 디지털 포렌식
- 보안 운영 및 모니터링
- 안전한 시스템 및 인프라
- 개인 정보 보호

만약 공격적 보안에 관심이 있다면 이해해야 할 내용들은 다음과 같습니다.

- 윤리적 해킹
- 침투 테스팅
- 취약점 평가
- 익스플로잇 개발
- 레드팀
- 사회 공학
- 무선 네트워크 해킹
- 웹 애플리케이션 해킹
- 네트워크 스캐닝 및 열거
- 암호 해독
- 역공학
- 악성 코드 분석
- OS 및 네트워크 공격
- 사후 공격 기술
- 위협 인텔리전스 수집
- 적대적 전술, 기술 및 절차 (TTP)
- 물리적 보안 평가
- 무선 네트워크 감사
- 클라우드 보안 평가
- IoT 보안 테스트

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

나의 마스터 클래스에 참여하신 모든 분들께, 마스터 또는 박사 과정 학생 또는 초심자들 모두 모든 것을 얻을 수 있어요. 이것은 여러분이 완전한 사람으로 성장하는 데 도움이 될 거예요.

더 많은 정보를 원하시면 계속해서 함께해주세요...