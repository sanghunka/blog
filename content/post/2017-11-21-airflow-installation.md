---
title: "[airflow] 1. 설치"
metaAlignment: center
date: 2017-11-21 16:00:00+09:00
categories:
- dev
tags:
- data engineering
- data
- airflow
- open source
thumbnailImage: //airflow.apache.org/_images/pin_large.png
thumbnailImagePosition: left
---

Airflow 설치하기

<!--more-->



<!--toc-->

# Getting Airflow

`pip install airflow`

extra packages를 설치하고자 하는 경우 아래처럼 패키지명을 명시해준다.
`pip install "airflow[s3, postgres]"`


# Extra Packages

celery, jdbc, hive, mysql, s3 등등.
전체 목록은 아래 링크 참조.

> http://airflow.incubator.apache.org/installation.html