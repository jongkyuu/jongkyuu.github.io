---
title: "[WPF] Data Template (DotnetSkool 강의)"
excerpt:

categories:
  - WPF
tags:
  - [C#, WPF, Client]

toc: true
toc_sticky: true

date: 2022-03-16
last_modified_at: 2022-03-16
---

## Data Template In WPF

```xml
<Label Content="Simple Label displaying string text" Foreground="Blue"/>
```

- Since this content here is a simple string so WPF engine understands this string text. That is why this string text is being able to be displayed without any data template.
- 이 내용은 간단한 문자열이므로 WPF 엔진은 이 문자열 텍스트를 이해합니다. 이것이 이 문자열 텍스트가 데이터 템플릿 없이 표시될 수 있는 이유입니다.

WPF는 object를 화면에 어떻게 표시해할지 모른다. WPF는 string content를 화면에 표시하는 방법만 이해한다. 그래서 이전의 예시처럼 string content는 화면에 표시되었지만 지금 예시에서 object를 바인딩한 경우 원하는 화면을 얻지 못한다.ㅍ
