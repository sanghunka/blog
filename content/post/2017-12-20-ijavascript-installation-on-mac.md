---
title: "[Mac] ijavascript 설치"
metaAlignment: center
date: 2017-12-20 12:00:00+09:00
categories:
- dev
tags:
- jupyter
- javascript
- ijavascript
thumbnailImage: https://raw.githubusercontent.com/n-riesco/ijavascript/HEAD/images/logo-128x128.png
thumbnailImagePosition: left
---

<!--more-->

<!--toc-->


```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install pkg-config node zeromq
sudo easy_install pip
sudo pip install --upgrade pyzmq jupyter
sudo npm install -g ijavascript
```

- 설치하고자 하는 가상환경에서 `ijsinstall`
- `ijsnotebook`으로 실행