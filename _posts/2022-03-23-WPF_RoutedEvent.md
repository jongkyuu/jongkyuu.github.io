---
title: "[WPF] Routed Event (DotnetSkool 강의)"
excerpt:

categories:
  - WPF
tags:
  - [C#, WPF, Client]

toc: true
toc_sticky: true

date: 2022-03-23
last_modified_at: 2022-03-23
---

## Routed Events in WPF

A routed event is a type of event that can invoke handlers on multiple listeners in an element tree rather than just the object that raised the event.

라우트된 이벤트는 이벤트를 발생시킨 개체가 아니라 요소 트리의 여러 수신기에서 처리기를 호출할 수 있는 이벤트 유형입니다.

Routed Events have three main routing strategies.

라우팅된 이벤트에는 세 가지 주요 라우팅 전략이 있습니다.

- Direct Event
- Bubbling Event
- Tunneling Event

Direct Event is normal CLR event.

Bubbling Event travels up in Visual Tree hierarchy. Tunneling Event travels down in Visual Tree hierarchy.

버블링 이벤트는 Visual Tree 계층에서 위로 이동합니다. 터널링 이벤트는 Visual Tree 계층에서 아래로 이동합니다.
