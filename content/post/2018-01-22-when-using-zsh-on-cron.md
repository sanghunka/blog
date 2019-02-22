---
title: "zsh에 설정해둔 alias를 crontab에 사용하고 싶을때"
date: 2018-01-22
metaAlignment: center
categories:
- dev
tags:
- zsh
- linux
---



<!--more-->

<!--toc-->


Put your functions in .zshenv.

.zshenv is sourced on all invocations of the shell, unless the -f option is set. It should contain commands to set the command search path, plus other important environment variables. .zshenv should not contain commands that produce output or assume the shell is attached to a tty.

.zshrc is sourced in interactive shells. It should contain commands to set up aliases, functions, options, key bindings, etc.


/usr/bin/zsh test.sh  이렇게 작업해 그럼 잘 됨