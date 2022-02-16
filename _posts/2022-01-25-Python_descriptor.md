---
title: "파이썬 디스크립터(Descriptor)"
excerpt:

categories:
  - Python
tags:
  - [Python, Effective Python, Theory]

toc: true
toc_sticky: true

date: 2022-01-25
last_modified_at: 2022-01-25
---

디스크립터란 파이썬 핵심 기능 중에 하나로 이전 강의에 몇번 언급 되었던 classmethod, staticmethod, property 등을 구현할 때 사용 되는 기능이다.

디스크립터(descriptor)를 간단하게 한 줄로 요약하면, "`__get__`, `__set__` 또는 `__delete__` 스페셜 메소드 중 한개 이상 구현 되어 있는 객체"를 말한다. 이렇게 구현된 디스크립터는 다른 객체의 속성으로 정의 될 수 있으며, 그 속성에 대한 읽기, 쓰기, 삭제 연산을 할 때 동작에 따라 각각 구현된 스페셜 메소드가 호출 된다.

이전에 @property 데코레이터에 대해 알아보았는데 데코레이터의 단점은 @property가 데코레이션 하는 메서드를 같은 클래스에 속한 여러 애트리뷰트에 사용할 수 없고, 서로 무관한 클래스 사이에서 @property 데코레이터를 적용한 메서드를 재사용할 수 없다.
