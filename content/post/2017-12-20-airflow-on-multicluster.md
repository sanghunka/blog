---
title: "[airflow] 6. Multi cluster에서 airflow 실행하기"
metaAlignment: center
date: 2017-12-20 14:00:00+09:00
categories:
- dev
tags:
- data engineering
- data
- airflow
- open source
- rabbitmq
- celery
thumbnailImage: //airflow.apache.org/_images/pin_large.png
thumbnailImagePosition: left
---


<!--more-->

<!--toc-->

# 요약
- 다루는 내용
    - 분산 인스턴스에서 각각 airflow worker를 실행하고 task를 분산해서 실행하는법
    - task가 실행될 worker를 명시적으로 지정하는법
- 테스트 환경
    - 두 개의 Amazon EC2 Instance 사용
    - 1번 Instance에 아래와 같이 셋팅
        - metadata database(postsgres)
        - rabbitmq
        - airflow webserver
        - airflow worker 
    - 2번 Instance에 아래와 같이 셋팅
        - airflow worker

# airflow configuration
- 1번과 2번 instance에 airflow를 설치한다.
- dag폴더에 동일한 파일을 넣어준다.
- dag폴더를 Git repository로 세팅하고 Chef, Puppet, Ansible등으로 동기화 해주는 방법도 있다.
- 1번과 2번 instance에 airflow를 설치하고 동일한 설정값을 준다.
    - CeleryExecutor를 사용할것.
    - 동일한 metadata database, broker_url, celery_result_backend를 설정해준다.

```sh
executor=CeleryExecutor

# 1번 Instance의 ip를 이용해 metadata database 접속
sql_alchemy_conn = postgresql+psycopg2://xxxxxx

broker_url = amqp://xxxxxx

celery_result_backend = amqp://xxxxxx
```

# 두개의 인스턴스에서 각각 worker 실행하기

![](https://i.imgur.com/uMU5KjD.jpg)

- 왼쪽은 1번 인스턴스, 오른쪽은 2번 인스턴스이다.
- metadata database는 1번 인스턴스에 위치해있다.
- rabbitmq, airflow webserver는 1번 인스턴스에서 실행시킨다.
- airflow worker는 1번과 2번 인스턴스에서 실행시킨다.

![](https://i.imgur.com/0lAXQ51.jpg)

- [tutorial DAG](https://airflow.apache.org/tutorial.html)를 실행해보면 1번 worker와 2번 worker에서 task가 분산되어 실행되는걸 확인할 수 있다.


# task가 실행될 worker를 명시적으로 지정하기

1. worker를 실행할때 queue를 지정해준다.
```sh
# 1번 인스턴스에서
airflow worker -q q1

# 2번 인스턴스에서
airflow worker -q q2
```

2. task를 정의할때 queue parameter에 task가 실행될 queue를 지정해준다.

```py
dag = DAG(
    'test-queue', default_args=default_args, schedule_interval=timedelta(1))

t1 = BashOperator(
    task_id='t1',
    bash_command="sleep 1",
    queue="q1",
    dag=dag)

t2 = BashOperator(
    task_id='t2',
    bash_command="sleep 1",
    queue="q2",
    dag=dag)

t3 = BashOperator(
    task_id='t3',
    bash_command="sleep 1",
    queue="q1",
    dag=dag)

t4 = BashOperator(
    task_id='t4',
    bash_command="sleep 1",
    queue="q2",
    dag=dag)


t2.set_upstream(t1)
t3.set_upstream(t2)
t4.set_upstream(t3)
```

![](https://i.imgur.com/OSOzC6J.jpg)

- -q옵션을 주면 이전과는 다른 메세지를 볼 수 있다.


![](https://i.imgur.com/xouSeVf.jpg)

- 지정해준대로 t1, t3는 q1에서 실행되었고 t3, t4는 q2에서 실행되었다.
- task를 정의할때 queue를 지정해주고 worker를 실행할때 queue를 지정해주지 않는다면 task가 존재해도 worker가 이를 처리하지 않는다.
- 서로 다른 인스턴스에 각각 worker가 있기에, 따로 설정을 해주지 않는다면 log역시 각각의 인스턴스에 기록된다.
- test-queue.py의 전체 코드는 아래와 같다.

```py
"""
Code that goes along with the Airflow located at:
http://airflow.readthedocs.org/en/latest/tutorial.html
"""
from airflow import DAG
from airflow.operators.bash_operator import BashOperator
from airflow.operators.python_operator import PythonOperator
from datetime import datetime, timedelta
import os


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

dag = DAG(
    'test-queue', default_args=default_args, schedule_interval=timedelta(1))

t1 = BashOperator(
    task_id='t1',
    bash_command="sleep 1",
    queue="q1",
    dag=dag)

t2 = BashOperator(
    task_id='t2',
    bash_command="sleep 1",
    queue="q2",
    dag=dag)

t3 = BashOperator(
    task_id='t3',
    bash_command="sleep 1",
    queue="q1",
    dag=dag)

t4 = BashOperator(
    task_id='t4',
    bash_command="sleep 1",
    queue="q2",
    dag=dag)


t2.set_upstream(t1)
t3.set_upstream(t2)
t4.set_upstream(t3)
```


# 참조
- https://stackoverflow.com/questions/43186335/assigning-tasks-to-specific-machines-with-airflow
- https://airflow.apache.org/cli.html