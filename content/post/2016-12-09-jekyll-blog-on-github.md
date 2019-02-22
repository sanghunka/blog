---
title: "github.io 블로그를 만들어 보자"
metaAlignment: center
date: 2017-05-09
categories:
- dev
tags:
- jekyll, 
- blog, 
- github, 
- codecademy
thumbnailImage: //s3.amazonaws.com/clarityfm-production/attachments/546/default/logo-jekyll-udemy.png?1393285554
thumbnailImagePosition: left
---

<!--more-->

## github page는 무엇인가?

[https://pages.github.com/](https://pages.github.com/) 는 깃허브에서 다이렉트로 호스팅해주는 서비스이다. 무료이다. 단 깃허브 계정 하나에 한 페이지만 만들 수 있다. 개발자들은 주로 포트폴리오나 cv, 또는 개인 블로그로 사용하고있다. 내 블로그인 sanghun.xyz도 깃허브 리포지터리를 이용한 페이지이다. 도메인은 aws에서 구매해 연결했다.

## 참조

나는 codecademy의 [deploy-a-website](https://www.codecademy.com/learn/deploy-a-website)를 따라해서 만들었다. 쉽고 친절한 수업이라 따라만 해도 쉽게 웹사이트를 배포할 수 있다. 물론 배포까지만! 그 다음은 개인의 웹개발 역량에 달려있다. 나는 괜찮아보이는 공개 theme를 가져다 썼다. 아래의 방법은 codecademy의 수업을 요약한 것이다.

## How to

### repository 생성

- repository를 생성할땐 naming convention을 따라야 한다.
- ```username.github.io``` 형태로 만들어야 한다. 아니면 github에서 안해줌. 아마 한 계정당 하나의 페이지만 호스팅해주기 위해서 이런 정책을 만든게 아닌가 싶다.

### 지킬 설치

Jekyll is a simple, blog-aware, static site generator for personal, project, or organization sites. static한 사이트를 만들기에 좋은 도구인것 같다. 사실 지킬 이전에도 이것저것 사용해봤으나 지킬이 제일 마음에 든다. 네이버 블로그나 티스토리는 뭔가 부족하고 워드프레스는 기능은 많은것 같은데 너무 무거운것 같았다. 무엇보다 github에서 jekyll을 권장하고 있다. 왜 그럴까 했더니 jekyll의 개발자가 github의 코파운더였던것!

#### mac OS 기준

```zsh
#brew 설치
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

#ruby 설치
brew install ruby

#jekyll 설치
gem install jekyll
gem install jekyll bundler

#jekyll 설치 확인
jekyll -v
```

### 웹사이트 생성

```zsh
# 새로운 웹사이트 생성
jekyll new personal-website

# 생성한 폴더로 이동
cd personal-website

# 실행
jekyll serve

# localhost:4000에서 실행되는걸 확인할 수 있다.
```

### jekyll폴더와 repository 연결

```zsh
# jekyll 폴더에서
git init
git remote add origin https://github.com/username/username.github.io.git
```

## 후기

참 좋은것 같다. 무엇보다도 무료이며 개발할 일이 없는 내 입장에선 블로그를 하면서나마 git을 사용하게된다는게 큰 장점이다. jekyll덕분에 웹개발도 그리고 루비도 공부하고 싶어진다. 일단은 jekyll의 구조를 학습해서 내 입맛대로 사이트를 꾸며보고 싶다.

It's important to understand Jekyll's default directory structure and contents of your site:

```
_config.yml - This is a configuration file that contains many values that need to be edited only once. These values are used across your site, for example, your site's title, your e-mail, and more. Note that this is a .yml file, which you can learn more about here.
_includes/ - This directory contains all the partials (code templates that keep you from repeating your code over and over) that your site uses to load common components, like the header and the footer.
_posts/ - This directory is where blog posts are stored. New blog posts can be added and will be rendered with the site's styling, as long as the file name follows Jekyll's standard naming convention.
_layouts/ - This directory contains templates that are used to style certain types of posts within the site. For example, new blog posts will use the HTML layout defined in post.html.
You can learn more about the Jekyll directory structure here.
```
