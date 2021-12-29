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

## \_data 폴더

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

### \_includes 폴더

많이 재사용 되는 html 파일들을 모아 둔 폴더이다. 댓글, 카테고리, 태그, 비디오, head, footer, toc 등등 블로그에서 자주 쓰이거나 항상 보이는 공통된 컴포넌트들을 담은 코드를 모아두었다.  
필요에 따라 Liquid 언어의 태그로 포스트나 레이아웃에 \_includes 폴더 내의 코드를 쉽게 삽입하여 재사용할 수 있다. 예를들어 { % include file.ext % } 를 쓰면 이 부분에 \_includes 폴더에 있는 file.ext 파일의 코드가 삽입되는 식이다.

구조

```
├── 📁_includes
|  ├── 📁analytics-providers     # snippets for analytics (Google and custom)
|  ├── 📁comments-providers      # snippets for comments
|  ├── 📁footer                  # custom snippets to add to site footer
|  ├── 📁head                    # custom snippets to add to site head
|  ├── 📘feature_row             # feature row helper
|  ├── 📘gallery                 # image gallery helper
|  ├── 📘group-by-array          # group by array helper for archives
|  ├── 📘nav_list                # navigation list helper
|  ├── 📘toc                     # table of contents helper
|  ├── 📘video                   # embeding vedeo like youtube helper
|  ├── 📘figure                  #
|  ├── 📝analytics.html          #
|  ├── 📝archive-single.html     #
|  ├── 📝author-profile-custom-links.html #
|  ├── 📝author-profiles.html    #
|  ├── 📝breadcrumbs.html        #
|  ├── 📝browser-upgrade.html    #
|  ├── 📝category-list.html      #
|  ├── 📝commtent.html           #
|  ├── 📝commtents.html           #
|  ├── 📝documents-collection.html #
|  ├── 📝footer.html             #
|  ├── 📝head.html               #
|  ├── 📝masthead.html           #
|  ├── 📝page__hero_video.html   #
|  ├── 📝page__hero.html         #
|  ├── 📝page__taxonomy.html     #
|  ├── 📝paginator.html          #
|  ├── 📝post_pagination.html    #
|  ├── 📝posts-category.html     #
|  ├── 📝posts-tag.html           #
|  ├── 📝read-time.html          #
|  ├── 📝scripts.html            #
|  ├── 📝seo.html                #
|  ├── 📝sidebar.html            #
|  ├── 📝skip-links.html         #
|  ├── 📝social-share.html       #
|  ├── 📝tag-list.html           #
|  └── 📝toc.html                #
```

### analytics-providersPermalink

어떤 analytics 플랫폼을 사용할 것인지.

```
├── 📁analytics-providers
|  ├── 📝google.html          # Google Standard Analytics를 사용할 때
|  ├── 📝google-gtag.html     # Google Analytics Global Site Tag를 사용할 때
|  ├── 📝google-universal.html # Google Universal Analytics를 사용할 때
|  └── 📝custom.html           # 그 밖에 다른 analytics를 사용할 때
```

사용 방법

```
analytics:
  provider: "google-gtag"
  google:
    tracking_id: "UA-1234567-8"
    anonymize_ip: false # default
```

provider에 사용할 analytics에 맞는 html 파일 이름을 문자열로 적어준다. 구글 analytics 말고 다른 analytics를 사용하려면 provider: custom을 한 후 custom.html에 그 analytics의 embed code를 추가해 준다.
