---
title: "macOS Sierra에서 virtualenv에 Opencv3 설치"
metaAlignment: center
date: 2016-12-06
categories:
- dev
tags:
- python, 
- opencv, 
- virtualenv
- osx
thumbnailImage: //prateekvjoshi.files.wordpress.com/2015/10/1-main.png
thumbnailImagePosition: left
---


<!--more-->

#### python3 설치
- 간단하게 brew로 설치하자

```bash
brew install python3
```

### opencv3 설치
- 3은 아직 베타라고 한다. 안전한 버전을 원하면 opencv2를 설치하자.
- 나는 그냥 3설치 했다.

```bash
brew tap homebrew/science
brew install opencv3 --with-python3 --with-ffmpeg --with-tbb --with-contrib
```


#### 2016.12.04 임시 설치법
- 현재 mac OS Sierra에서 Opencv 설치에 문제가 있다.
- --HEAD를 추가해 아래 방법대로 하면 된다. (16.12.4 기준)
- https://github.com/Homebrew/homebrew-science/issues/4104#issuecomment-249362870

```bash
brew install opencv3 --HEAD --with-python3 --with-ffmpeg --with-tbb --with-contrib
```

### lookup 만들어주기
- `Ln -s {opencv의 site-packages} {사용하는 python환경의 site-packages}` 형태로 lookup을 만들어 준다.
- virtualenv python 환경도 이 방식으로 opencv사용이 가능해진다

```bash
Ln -s /usr/local/Cellar/opencv3/HEAD-c48d7f8_4/lib/python3.5/site-packages/cv2.cpython-35m-darwin.so
 /Users/elon/.pyenv/versions/py3/lib/python3.5/site-packages
```


- opencv 설치 후 아래와 같은 메세지가 떴는데 일단 작동은 잘 된다.

```
This formula is keg-only, which means it was not symlinked into /usr/local.

opencv3 and opencv install many of the same files.

Generally there are no consequences of this for you. If you build your
own software and it requires this formula, you'll need to add to your
build variables:

    LDFLAGS:  -L/usr/local/opt/opencv3/lib
    CPPFLAGS: -I/usr/local/opt/opencv3/include
    PKG_CONFIG_PATH: /usr/local/opt/opencv3/lib/pkgconfig
```

### 참조

- [https://www.linkedin.com/pulse/install-opencv-virtualenv-mac-lin-dong](https://www.linkedin.com/pulse/install-opencv-virtualenv-mac-lin-dong)
- [http://qiita.com/masaori/items/0c78fcd58a6c6bf4f655](http://qiita.com/masaori/items/0c78fcd58a6c6bf4f655)
- [https://github.com/Homebrew/homebrew-science/issues/4104#issuecomment-249362870](https://github.com/Homebrew/homebrew-science/issues/4104#issuecomment-249362870)
