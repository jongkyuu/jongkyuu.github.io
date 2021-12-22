---
title: "Blog 웹 폰트 설정"
excerpt:

categories:
  - Blog
tags:
  - [HTML, jekyll, Liquid, Blog]

toc: true
toc_sticky: true

date: 2021-12-19
last_modified_at: 2021-12-19
---

## 폰트 고르기

우선 무료 웹 폰트 사이트에서 사용할 폰트를 고른다.

- [Google Font](https://fonts.google.com/)
- [눈누](https://noonnu.cc/index)

### 눈누에서 폰트 가져오기

눈누에서 원하는 폰트를 클릭하면 웹 폰트로 사용이라는 부분이 있는데 여기 내용을 임포트해야 한다.

```
@font-face {
    font-family: 'GowunDodum-Regular';
    src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_2108@1.1/GowunDodum-Regular.woff') format('woff');
    font-weight: normal;
    font-style: normal;
}
```
