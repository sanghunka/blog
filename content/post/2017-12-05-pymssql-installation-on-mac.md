---
title: "[Mac] pymssql 설치 에러날경우"
metaAlignment: center
date: 2017-12-05
categories:
- dev
tags:
- python
- pymssql
- mssql
- mac
---

<!--more-->

- Mac에서 pymssql 설치시 에러가 나는경우 아래와 같은 방법을 시도해보자.

```sh
# 이미 freedts가 설치되어있다면 삭제해준다.
# brew uninstall freedts

brew install freetds091
brew link --force freetds@0.91
pip install pymssql
```