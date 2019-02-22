---
title: "[airflow] 2. 튜토리얼"
metaAlignment: center
date: 2017-11-21 17:00:00+09:00
timezone: Asia/Seoul
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

pipeline을 따라 만들어보며 Airflow의 concept, object, usage를 습득하기.

<!--more-->

<!--toc-->


# Example Pipeline definition

```py
"""
Code that goes along with the Airflow tutorial located at:
https://github.com/airbnb/airflow/blob/master/airflow/example_dags/tutorial.py
"""
from airflow import DAG
from airflow.operators.bash_operator import BashOperator
from datetime import datetime, timedelta


default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'start_date': datetime(2015, 6, 1),
    'email': ['airflow@airflow.com'],
    'email_on_failure': False,
    'email_on_retry': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
    # 'queue': 'bash_queue',
    # 'pool': 'backfill',
    # 'priority_weight': 10,
    # 'end_date': datetime(2016, 1, 1),
}

dag = DAG('tutorial', default_args=default_args)

# t1, t2 and t3 are examples of tasks created by instantiating operators
t1 = BashOperator(
    task_id='print_date',
    bash_command='date',
    dag=dag)

t2 = BashOperator(
    task_id='sleep',
    bash_command='sleep 5',
    retries=3,
    dag=dag)

templated_command = """
    {% for i in range(5) %}
        echo "{{ ds }}"
        echo "{{ macros.ds_add(ds, 7)}}"
        echo "{{ params.my_param }}"
    {% endfor %}
"""

t3 = BashOperator(
    task_id='templated',
    bash_command=templated_command,
    params={'my_param': 'Parameter I passed in'},
    dag=dag)

t2.set_upstream(t1)
t3.set_upstream(t1)
```


# It's a DAG definition file

- Airflow에서 DAG의 sturcture는 python 스크립트로 표현된다.
- DAG내에 정의된 각 task들은 다른 context상에서 실행된다.
- 각기 다른 시간에, 다른 worker에 의해, 다른 task들이 실행된다. 이말은 즉, 작성된 스크립트는 task간의 cross communication을 지원하지 않는다는 뜻이다.
- cross communication을 위한 `XCom`이라는 feature가 따로 존재한다.
- 사람들은 DAG가 정의된 python 스크립트에서 실제 data processing이 일어난다고 착각한다. 스크립트의 목적은 DAG object를 정의하는것이다.
- 변동사항을 반영하기 위해 스케쥴러는 DAG를 주기적으로 실행한다. 그렇기에 DAG는 (분단위가 아니라)초단위로 빠르게 evaluate될 수 있어야 한다.


# Importing Modules

- DAG를 정의하는 파이썬 스크립트 그 자체가 Airflow pipeline이다.

```py
# The DAG object; we'll need this to instantiate a DAG
from airflow import DAG

# Operators; we need this to operate!
from airflow.operators.bash_operator import BashOperator
```


# Default Arguments

- 각 task별로 명시적으로(explicitly!) arguments를 넘겨주거나 OR default arguments의 dictionary를 만들어서 사용하면 된다.

```py
from datetime import datetime, timedelta

default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'start_date': datetime(2015, 6, 1),
    'email': ['airflow@airflow.com'],
    'email_on_failure': False,
    'email_on_retry': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
    # 'queue': 'bash_queue',
    # 'pool': 'backfill',
    # 'priority_weight': 10,
    # 'end_date': datetime(2016, 1, 1),
}
```


# Instantiate a DAG

```py
dag = DAG(
    'tutorial', default_args=default_args, schedule_interval=timedelta(1))
```

- 첫번째 인자는 `dag_id`. DAG의 unique identifier이다.
- 두번째로 위에서 정의한 default arguments의 dictionary를 넘겨준다.
- 세번째로 schedule interval을 넘겨준다.

## Schedule interval
- `schedule_interval=timedelta(1)` 파이썬 datetime library로 표현 가능. 인터벌이 하루라는 뜻.
- `schedule_interval=None`, `schedule_interval="@once"` 이런식으로 작성 가능.
- `schedule_interval='*/1 * * * *'` cron 형태로 작성 가능.


# Tasks

```sh
t1 = BashOperator(
    task_id='print_date',
    bash_command='date',
    dag=dag)

t2 = BashOperator(
    task_id='sleep',
    bash_command='sleep 5',
    retries=3, ## default argument에 retries=1로 되어있더라도 retries=3으로 override됨.
    dag=dag)
```


- Operator object를 instantiating하면 task가 생성된다.
- operator에는 `bashOperator`와 `pythonOperator`가 있다.
- 첫번째 인자는 `task_id`이고 이는 task의 unique identifier이다.
- 각 Task는 DAG정의할때 사용했던 default argument를 상속받는다.
- default argument가 있더라도 Task정의할때 argument값을 지정하면 override한다.
- argument의 우선순위는 아래와 같다.
    1. 명시적으로 인자로 지정된 arguments
    2. default_args에 존재하는 arguments
    3. (만약 존재한다면) operator의 default value
- task는 `task_id`와 `owner`를 무조건 포함하여야 한다.(명시적으로 지정되거나 상속받거나) 그러지 않을 경우, airflow가 exception을 raise한다.


# Templating with Jinja

- airflow에는 built-in parameters와 macros가 있으며 이들은 Jinja template을 사용한다.


# Setting up Dependencies

```py
t2.set_upstream(t1)

# This means that t2 will depend on t1
# running successfully to run
# It is equivalent to
# t1.set_downstream(t2)

t3.set_upstream(t1)

# all of this is equivalent to
# dag.set_dependency('print_date', 'sleep')
# dag.set_dependency('print_date', 'templated')
```

- `task.set_upstream()`, `task.set_downstream()`
- `dag.set_dependency()`


# Testing

## Running the Script

`python ~/airflow/dags/tutorial.py`

- 스크립트를 실행시켰을때, exception이 발생하지 않는다면 잘못된 부분이 없다는 뜻.

## command line Metadata Validation

```sh
# print the list of active DAGs
airflow list_dags

# prints the list of tasks the "tutorial" dag_id
airflow list_tasks tutorial

# prints the hierarchy of tasks in the tutorial DAG
airflow list_tasks tutorial --tree
```

## Testing

```sh
# command layout: command subcommand dag_id task_id date

# testing print_date
airflow test tutorial print_date 2015-06-01

# testing sleep
airflow test tutorial sleep 2015-06-

# testing templated
airflow test tutorial templated 2015-06-01
```

- `airflow test` 는 task를 로컬에서 실행시키는 명령어이다.
- 그래서 아웃풋도 스크린에 stdout된다.
- 한번에 하나의 task만 테스트 가능하다.
- dependency를 무시한다.
- communication state(running, success, failed,...)가 DB에 기록되지 않는다.


## Backfill

```sh
# optional, start a web server in debug mode in the background
# airflow webserver --debug &

# start your backfill on a date range
airflow backfill tutorial -s 2015-06-01 -e 2015-06-07
```

- dependency를 고려한다.
- log파일을 생성한다.
- communication state를 DB에 기록한다.


> http://airflow.incubator.apache.org/tutorial.html