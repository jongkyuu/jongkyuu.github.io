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

### ui-text.yml

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

### analytics-providers

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

### comments-providers

어떤 댓글 플랫폼을 사용할 것인지. ex) disqus, facebook, …
마찬가지로 custom.html은 \_includes/comments-providers에 없는 댓글 플랫폼을 사용하려 할 때 여기에 embeded code를 추가해주자. 플랫폼별로 사용법

### footer, head

폴더에 들어있는 cumtom.html에 footer와 head의 커스터마이징 내용을 적어주면 될 것 같다. favicon 파비콘 삽입 태그를 이 \_includes/head/custom.html 에 삽입해주었다.

### search

어떤 검색 엔진을 사용할 것인지. 우선 블로그 내 검색 기능을 사용하려면 \_config.yml에 search: true값으로 변경해주어야 한다. 검색 엔진 별 자세한 설명

```
├── 📁analytics-providers
| ├── 📝algolia-search-scripts.html # algoria 검색 엔진
| ├── 📝google-search-scripts.html # Google 검색 엔진
| ├── 📝lunar-search-scripts.html # Lunar 검색 엔진
| └── 📝search_form.html #
```

디폴트 검색 엔진은 Lunar 이며 Google Custom Search Engine에서 내 입맛대로 검색 엔진을 만들 수 있다. 검색 기능을 사용하려면 \_config.yml에 search_provider값을 추가하면 된다.

### Helpers

[설명 링크](https://mmistakes.github.io/minimal-mistakes/docs/helpers/)

### feature_row

마치 갤러리처럼 여러개의 사진들을 한 줄로 나열된 형태. Gallery와의 차이점이 있다면 사진마다 제목과 설명 텍스트가 달려 있음. 머릿말에 아래와 같은 정보가 담긴 feature_row 변수를 추가하고 포스트 본문에서 Liquid 태그를 { { % include feature_row % } } 이렇게 적어주면 그 자리에 feature_row가 출력될 것이다.

```
feature_row: # 3개의 이미지와 각가의 텍스트가 담긴 feature_row
- image_path: /assets/images/unsplash-gallery-image-1-th.jpg
  alt: "placeholder image 1"
  title: "Placeholder 1"
  excerpt: "This is some sample content that goes here with **Markdown** formatting."
- image_path: /assets/images/unsplash-gallery-image-2-th.jpg
  alt: "placeholder image 2"
  title: "Placeholder 2"
  excerpt: "This is some sample content that goes here with **Markdown** formatting."
  url: "#test-link"
  btn_label: "Read More"
  btn_class: "btn--inverse"
- image_path: /assets/images/unsplash-gallery-image-3-th.jpg
  title: "Placeholder 3"
  excerpt: "This is some sample content that goes here with **Markdown** formatting."
```

### gallery

feature_row와 다르게 텍스트 없이 한 줄에 사진 여러개만 있다. feature_row와 똑같은 방법으로 쓰면 된다. 머릿말에 각 이미지들의 url, path, alt, title 정보가 담긴 gallery 변수 지정해주고 본문에서 Liquid 태그로 출력.

### group-by-array

[사용방법 링크](https://github.com/mushishi78/jekyll-group-by-array)

### nav_list

메뉴 상단바 리스트. 아래와 같이 \_data 폴더에 있는 navigation.yml 에 예를 들어 foo라는 이름의 네비게이션을 작성한다고 가정하자. Parent Link 1, 2라는 이름의 페이지가 상단 메뉴바에 생길 것이고 각각의 자식 페이지는 child-1,2-page, child-1,2,3-page가 될 것이다. 이러고 포스트 머리말에 side bar : nav : "foo" 혹은 포스트 본문에 { % include nav_list nav="foo" % }을 써주면 foo라는 이름으로 지정한 네비게이션이 삽입될 것이다.

```
# \_data/navigation.yml
foo:

- title: "Parent Link 1"
  url: /parent-1-page-url/
  children:

  - title: "Child Link 1"
    url: /child-1-page-url/
  - title: "Child Link 2"
    url: /child-2-page-url/

- title: "Parent Link 2"
  url: /parent-2-page-url/
  children: - title: "Child Link 1"
  url: /child-1-page-url/ - title: "Child Link 2"
  url: /child-2-page-url/ - title: "Child Link 3"
  url: /child-3-page-url/
```

### toc

toc 목차를 사용하고 싶다면 머릿말에 toc: true를 지정

### video

Youtube, Vimeo 같은 비디오를 embeding 하는 helper. 유튜브의 경우 영상의 긴 url 말고 짧은 url을 따서 url의 뒷부분을 id로 넣어준다.

```
예를 들어 https://youtu.be/XsxDH4HcOWA url을 가진 유튜브 영상이라면

{% include video id="XsxDH4HcOWA" provider="youtube" %}

_include 파일에 있는 video helper 코드를 재사용하여 삽입

```

\_include 파일에 있는 video helper 코드를 재사용하여 삽입한다. helper내의 id, provider 값은 각각 짧은 url과 youtube로 설정해준다. `?start=110`을 붙여주면 유튜브 영상이 110초부터 재생되게끔 할 수 있다.  
Vimeo와 Google Drive에 있는 영상도 비슷한 방법으로 embeding 하면 된다.

### figure

한 개의 이미지 요소를 생성한다.

```
{% include figure image_path="/assets/images/unsplash-image-10.jpg" alt="this is a placeholder image" caption="This is a figure caption." %}
```

image_path는 필수이며 alt와 caption은 옵션이다. Liquid 태그로 include figure 이미지를 불러오는 역할을 하는 HTML 코드가 담겨있는 figure가 불러와진다.

### analytics.html

```
analytics:
provider: "google-gtag"
google:
tracking_id: "UA-1234567-8"
anonymize_ip: false # default
```

이렇게 yml 형식으로 써서 analytics.html에 애널리틱스의 provider 정보와 tracking_id, anonymize_ip 정보를 넘겨준다.
