---
title: "Python이 너무 느려요 우리 응용 프로그램에 사용할 수 없는 이유 5가지"
description: ""
coverImage: "/TIL/assets/no-image.jpg"
date: 2024-07-07 02:24
ogImage: 
  url: /TIL/assets/no-image.jpg
tag: Tech
originalTitle: "Python is Too Slow… We Can’t Use It for Our Application"
link: "https://medium.com/@danielbuilescu/python-is-too-slow-we-cant-use-it-for-our-application-a5c8bdec41fd"
---


안녕하세요 여러분, 다니엘 부일레스쿠입니다. 프리랜서 웹 개발자이자 에이전시 소유자로서, 다양한 팀 역학을 경험해왔어요. 그중에서도 인상적이었던 이야기가 있어요. 새로운 기술을 주도하는 팀 리더를 설득하여 웹 앱에 파이썬을 사용하도록 한 적이 있어요. 그 분은 항상 최신이며 눈부신 도구를 찾아내려는 리더였는데, 때때로 그것이 일에 가장 적합한 것은 아니더라도 말이에요. 이 프로젝트도 예외는 아니었어요. 속도에 관해서만 얘기하던 그 분은 파이썬 아이디어에 처음에는 반대했죠. 어떻게 그의 마음을 바꿨는지 (그리고 아마 여러분의 것도) 이야기할게요!

# 팀 리더의 관점: 열정적인 혹평가자

우리 팀 리더는 모든 기술 뉴스레터를 구독하고 있었으며, 유명인의 인스타그램만큼 화려한 GitHub 프로필을 가지고 있었어요. 그 분은 최신 JavaScript 프레임워크와 실험적인 언어를 좋아했죠. 그래서 우리가 새로운 웹 애플리케이션을 개발하게 된다면, 이미 가장 첨단 기술 스택을 상상하고 있었어요.

하지만 문제가 있었어요: 저는 이 작업에 파이썬이 적절한 도구라고 알고 있었어요. 그것은 웹 개발에 완벽한, 견고한 언어였거든요. 그러나 제 팀 리더는 파이썬이 너무 느리다고 생각했어요. 그 분은 몇몇 자신이 관심을 가진 최신 언어들과 비교했을 때 특히 파이썬은 느리다고 주장했어요. 고객이 많은 트래픽을 처리하고 빠른 속도를 제공할 수 있는 언어가 필요하다고 하면서 계속해서 벤치마크와 사례 연구들을 거론했죠...