---
title: "Blog 카테고리"
excerpt:

categories:
  - Blog
tags:
  - [HTML, jekyll, Blog]

toc: true
toc_sticky: true

date: 2021-12-26
last_modified_at: 2021-12-26
---

# Minimal-Mistakes 테마 구조

- [jekyll 한글문서](https://jekyllrb-ko.github.io/)
- [Minimal-Mistakes 공식 문서](https://mmistakes.github.io/minimal-mistakes/)

## 전반적인 구조

기본적으로 Jekyll 디렉터리 구조를 뼈대로 하고 있다.

```
minimal-mistakes
├── 📁_data                      # data files for customizing the theme
├── 📁_includes
├── 📁_layouts
├── 📁_sass                      # SCSS partials
├── 📁assets
├── 📝_config.yml                # site configuration
├── 📝Gemfile                    # gem file dependencies
├── 📝index.html                 # paginated home page showing recent posts
└── 📝package.json               # NPM build scripts
```

## \_data 폴더Permalink

테마를 커스터마이징하기 위한 데이터 파일들이 모여있는 폴더이다. 사이트에 사용할 데이터를 적절한 포맷으로 정리하여 보관한다. 이 디렉터리에 .yml .yaml, json, csv, tsv 같은 파일들을 둔다면 이 파일들을 자동으로 읽어들여 site.data로 사용할 수 있다. 예를 들어 \_data 디렉터리에 members.yml라는 파일이 있다면 site.data.members로 입력하여 그 파일을 사용할 수 있다.

구조

```
├── 📁_data                      # data files for customizing the theme
|  ├── 📘navigation.yml          # main navigation links
|  └── 📘ui-text.yml             # text used throughout the theme's UI
```

### navigation.yml

```
# main links
main:
  - title: "Quick-Start Guide"
    url: https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/
  - title: "About"
    url: https://mmistakes.github.io/minimal-mistakes/about/
  - title: "Sample Posts"
    url: /year-archive/
  - title: "Sample Collections"
    url: /collection-archive/
  - title: "Sitemap"
    url: /sitemap/
```

상단 메뉴바의 링크이다. 예를들어 Quick-Start Guide를 누르면 Minimal Mistakes 문서 페이지로 이동한다.

### ui-text.ymlPermalink

각국 언어별로 어떤 텍스트로 표시되는지를 나열한 문서이다.
