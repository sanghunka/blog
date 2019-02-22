---
title: "[python]cat을 이용해서 stdin 인풋 주기"
metaAlignment: center
date: 2017-12-02
categories:
- dev
tags:
- python
- unix
---

`cat iris.txt | python test.py`

<!--more-->

import sys
import re

pattern= r'^([(\d\.)]+) - - \[(.*?)\] "(.*?) (.*?) (.*?)" (\d+) (\d+)'
for line in sys.stdin:
  data = line.strip()
  print("1" + data)