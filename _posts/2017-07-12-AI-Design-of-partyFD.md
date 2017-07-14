---
layout: post
title: AI Design Using Behaviour Tree (PartyFD)
categories: project
tags: [game, unity3d, ai, Behaviour tree]
comments: true
description: Using free Behaviour Tree of unity asset-store, we can make ai :)
---

Ranged Mage Character (원거리 마법사)의 AI memo

![My helpful screenshot]({{ site.url }}/assets/media/KakaoTalk_20170712_060158755.jpg)

캐릭터는 한 타임에 하나의 액션만을 수행한다. 이후 들어오는 액션은 현재 액션을 interrupt&start하거나 pending, drop next등이 발생한다.

액션의 우선순위는 아래에 정리한다
* 진형이동 혹은 강제이동
* 회피
* 탱커 팔로우
* 공격 및 타켓 접근

위의 액션과 우선순위는 캐릭터 특성을 따르므로 날코딩으로 할 경우 일이 줄지 않는다.

그래서 unity asset중 무료 behaviour tree솔루션인 Behaviour Bricks를 이용하여 구현한다. 자유도 높고 간단한 편이다
이 [QuickStartGuide]({{ site.url }}/assets/media/QuickStartGuide.pdf)를 참조하여 간단한 사용법을 익힐 수 있다.

프로그래머를 위한 문서는 [ProgrammersQuickStartGuide]({{ site.url }}/assets/media/ProgrammersQuickStartGuide.pdf) 확인할 수 있다.

주의할 노드는 decorator기능을 수행하는 컨퍼넌트노드인 priority selector이다. 이 노드는 연결된 하위 노드들의 condition을 매번 재평가하고 변경을 감지하여 abort한다. 예를 들어 마우스클릭(조건)을 감지하여 액션을 수행중이더라도 마우스클릭을 재평가해서 액션을 중지실 수 있다. (끔찍?) 이 경우 조건노드에 재평가를 미루는 함수2개를 제공하니 override하면 된다.

다시 원거리 마법사로 넘어와서 BB를 이용하여 구현하였다.

![My helpful screenshot]({{ site.url }}/assets/media/ranged-mage-ai.png)

유저에 의해 이동이 발생하면 모든 캐릭터는 강제 이동을 한다. 