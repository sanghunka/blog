---
title: "jekyll github blog gemfile 버전 에러"
date: 2016-11-21
thumbnailImagePosition: left
thumbnailImage: //avatars1.githubusercontent.com/u/3083652?s=400&v=4
metaAlignment: center
categories:
- dev
tags:
- ruby
- jekyll
- gemfile
- blog
---

이런 에러가 났다. config파일을 바꿔보고 루비와 jekyll을 지웠다 다시 깔아보고 했지만 모두 실패함.
<!--more-->
```bash
➜  sanghkaang.github.io git:(master) jekyll serve
WARN: Unresolved specs during Gem::Specification.reset:
      jekyll-watch (~> 1.1)
WARN: Clearing out unresolved specs.
Please report a bug if this causes problems.
/Library/Ruby/Gems/2.0.0/gems/bundler-1.13.6/lib/bundler/runtime.rb:40:in `block in setup': You have already activated addressable 2.5.0, but your Gemfile requires addressable 2.4.0. Prepending `bundle exec` to your command may solve this. (Gem::LoadError)
	from /Library/Ruby/Gems/2.0.0/gems/bundler-1.13.6/lib/bundler/runtime.rb:25:in `map'
	from /Library/Ruby/Gems/2.0.0/gems/bundler-1.13.6/lib/bundler/runtime.rb:25:in `setup'
	from /Library/Ruby/Gems/2.0.0/gems/bundler-1.13.6/lib/bundler.rb:99:in `setup'
	from /Library/Ruby/Gems/2.0.0/gems/jekyll-3.3.1/lib/jekyll/plugin_manager.rb:36:in `require_from_bundler'
	from /Library/Ruby/Gems/2.0.0/gems/jekyll-3.3.1/exe/jekyll:9:in `<top (required)>'
	from /usr/local/bin/jekyll:23:in `load'
	from /usr/local/bin/jekyll:23:in `<main>'
```


방법은 간단했다.

1. Gemfile.lock 삭제
2. ```bundle install```

오오...역시 스택오버플로우 짱. 루비도 한번 제대로 공부해보고 싶다. 항상 오류가 날때마다 검색후 오류를 해결하긴 하지만 왜 오류가 났는지, 왜 오류가 해결되었는지에 대한 배움이 전혀 없다.

- 참고: http://stackoverflow.com/questions/7060749/deactivate-a-gem-you-have-already-activated-rake-0-9-3-beta-1-but-my-gemfile
