---
title: "[Jekyll] 블로그 포스팅하는 방법"
excerpt: "마크다운 파일을 작성하여 Github 원격 저장소에 업로드 해보자. 에디터는 Visual Studio code 사용! 로컬 서버에서 확인도 해보자."

categories:
  - Blog
tags:
  - [Blog, jekyll, Github, Git]

toc: true
toc_sticky: true

date: 2021-12-02
last_modified_at: 2021-12-02
---

## 1. 마크다운을 지원하는 에디터를 실행

나는 Visual Studio Code 에디터를 선택했다. 에디터를 열고 Jekyll 테마 내용물이 들어 있는 `[github_id].github.io` 이름의 로컬 폴더를 열어준다.

## 2. \_post 폴더를 생성

`[github_id].github.io` 이름의 로컬 폴더 위치에 \_posts 폴더를 생성해 준다. 모든 포스트 파일은 이 \_posts 내에 위치하여야 한다.

## 3. yyyy-mm-dd-title.md 형식의 파일 생성

포스트 파일의 확장자는 `.md`여야 한다. `yyyy-mm-dd`형식의 날짜와 함께 포스트제목(title)을 붙여 `yyyy-mm-dd-title` 형식의 제목을 만들어 준다. 포스트 제목을 영어로 쓴다.

## 4. 머릿말(Front-Matter) 작성

이제 포스트를 작성해 보자. md 파일의 상단에 포스트의 정보를 머릿말로 적어 준다.

```
---
title:  "[Jekyll] 블로그 포스팅하는 방법"
excerpt: "마크다운 파일을 작성하여 Github 원격 저장소에 업로드 해보자. 에디터는 Visual Studio code 사용! 로컬 서버에서 확인도 해보자."

categories:
  - Blog
tags:
  - [Blog, jekyll, Github, Git]

toc: true
toc_sticky: true

date: 2020-05-25
last_modified_at: 2020-05-25
---
```

Front-Matter의 위, 아래에 `---`을 써서 머릿말을 쓰는 영역을 구분해주어야 한다.

title : 포스트의 제목을 큰 따옴표로 적어 준다. 이 title을 적어주지 않으면 `.md`파일 이름으로 적어 주었던 title 부분이 제목으로 업로드 된다.
