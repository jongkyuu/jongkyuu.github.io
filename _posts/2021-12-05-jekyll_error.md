---
title: "[Jekyll] 로컬 서버 실행할때 webrick 오류"
excerpt: "webrick 오류 발생할때 해결하는 방법을 알아보자"

categories:
  - Blog
tags:
  - [Blog, jekyll, Github, Git, Error]

toc: true
toc_sticky: true

date: 2021-12-05
last_modified_at: 2021-12-05
---

### webrick LoadError가 발생할때 에러 메세지

`cannot load such file -- webrick (LoadError)`라는 에러 메세지가 발생한다. 아래는 에러 메세지 전문이다.

```
C:\GoogleDrive\Study\blog\jongkyuu.github.io>bundle exec jekyll serve
Configuration file: C:/GoogleDrive/Study/blog/jongkyuu.github.io/_config.yml
            Source: C:/GoogleDrive/Study/blog/jongkyuu.github.io
       Destination: C:/GoogleDrive/Study/blog/jongkyuu.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
             Error: could not read file C:/GoogleDrive/Study/blog/jongkyuu.github.io/_posts/2021-12-05-00-23-54.png: invalid byte sequence in UTF-8
       Jekyll Feed: Generating feed for posts
                    done in 0.634 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'C:/GoogleDrive/Study/blog/jongkyuu.github.io'
                    ------------------------------------------------
      Jekyll 4.2.1   Please append `--trace` to the `serve` command
                     for any additional information or backtrace.
                    ------------------------------------------------
C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.1/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.1/lib/jekyll/commands/serve/servlet.rb:3:in `<top (required)>'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.1/lib/jekyll/commands/serve.rb:179:in `require_relative'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.1/lib/jekyll/commands/serve.rb:179:in `setup'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.1/lib/jekyll/commands/serve.rb:100:in `process'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.1/lib/jekyll/command.rb:91:in `block in process_with_graceful_fail'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.1/lib/jekyll/command.rb:91:in `each'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.1/lib/jekyll/command.rb:91:in `process_with_graceful_fail'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.1/lib/jekyll/commands/serve.rb:86:in `block (2 levels) in init_with_program'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `block in execute'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `each'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `execute'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/mercenary-0.4.0/lib/mercenary/program.rb:44:in `go'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/mercenary-0.4.0/lib/mercenary.rb:21:in `program'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.1/exe/jekyll:15:in `<top (required)>'
        from C:/Ruby30-x64/bin/jekyll:25:in `load'
        from C:/Ruby30-x64/bin/jekyll:25:in `<main>'
```

위 메세지를 보면 `--trace`를 `serve` 명령어 뒤에 붙이라고 하지만 그렇게 명령어를 실행해도 정상적으로 동작하지 않는다. 해결 방법은 간단한데 webrick을 추가해주고 다시 실행하면 된다.

```
$ bundle add webrick

Fetching gem metadata from https://rubygems.org/.........
Resolving dependencies...
Fetching gem metadata from https://rubygems.org/.........
Resolving dependencies...
Using rake 13.0.6
Using public_suffix 4.0.6
Using addressable 2.8.0
Using bundler 2.2.32
...
Using jekyll-gist 1.5.0
Using jekyll-include-cache 0.2.1
Using jekyll-paginate 1.1.0
Using jekyll-sitemap 1.4.0
Using minimal-mistakes-jekyll 4.24.0 from source at `.`
Fetching webrick 1.7.0
Installing webrick 1.7.0

// jekyll 로컬서버 실행
$ bundle exec jekyll serve
Configuration file: C:/GoogleDrive/Study/blog/jongkyuu.github.io/_config.yml
            Source: C:/GoogleDrive/Study/blog/jongkyuu.github.io
       Destination: C:/GoogleDrive/Study/blog/jongkyuu.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
             Error: could not read file C:/GoogleDrive/Study/blog/jongkyuu.github.io/_posts/2021-12-05-00-23-54.png: invalid byte sequence in UTF-8
       Jekyll Feed: Generating feed for posts
                    done in 0.645 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'C:/GoogleDrive/Study/blog/jongkyuu.github.io'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
```

- **참고** : ruby 3.0.0부터 webrick이 기본 gem에서 빠졌기 때문에 위와 같은 현상이 발생한다고 한다.
