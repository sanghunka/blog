---
title: "[airflow] 0. Quickstart"
metaAlignment: center
date: 2017-11-21 15:00:00+09:00
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

<!--more-->


```sh
# airflow needs a home, ~/airflow is the default,
# but you can lay foundation somewhere else if you prefer
# (optional)
export AIRFLOW_HOME=~/airflow

# install from pypi using pip
pip install airflow

# initialize the database
airflow initdb

# start the web server, default port is 8080
airflow webserver -p 8080
```

- `export AIRFLOW_HOME=~/airflow` 명령어로 설치 경로를 지정할 수 있다.
- AIRFLOW_HOME을 지정하지 않을 경우 default 경로는 `~/airflow`
- 설치는 pip로 간단하게 할 수 있다. `pip install airflow`
- 설치 후 db를 초기화 해 준다. `airflow initdb`
- 웹서버를 띄우기 위한 명령어 `airflow webserver -p 8080`
    - -p는 옵션이다. default값은 8080
- 설정은 AIRFLOW_HOME에 있는 `airflow.cfg`에서 관리. 웹UI의 `Admin -> Configuration`에서도 확인 가능하다.
- 웹서버 PID는 AIRFLOW_HOME에 있는 `airflow-webserver.pid`에 저장된다.
- systemd에 의해 실행될 경우 `/run/airflow/webserver.pid`에 저장된다. 
- 기본값으로 sqlite를 사용하는데 이는 `SequentialExcutor`를 사용하기에 parallelization이 불가능하다.


# 실행 명령어 예시 

```sh
# run your first task instance
airflow run example_bash_operator runme_0 2015-01-01
# run a backfill over 2 days
airflow backfill example_bash_operator -s 2015-01-01 -e 2015-01-02
```

> http://airflow.incubator.apache.org/start.html