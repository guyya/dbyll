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
