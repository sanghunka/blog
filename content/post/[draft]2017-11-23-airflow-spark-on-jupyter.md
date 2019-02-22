---
title: "[airflow] 5. Pyspark sample code on Airflow"
metaAlignment: center
date: 2017-12-06
categories:
- dev
tags:
- data engineering
- data
- airflow
- open source
thumbnailImage: //airflow.apache.org/_images/pin_large.png
thumbnailImagePosition: left
draft: true
---

<!--more-->

![](https://i.imgur.com/SScKfEF.png)

- data_download, spark_job, sleep 총 3개의 task가 있다.
- data_download가 완료된 후, 동시에 나머지 두개의 task가 실행되는 DAG이다.
- 병렬로 task가 수행된다는걸 보여주기위해 sleep task를 만들었다.
- gantt를 보면 data_download가 완료된 후, 동시에 나머지 두개의 task가 실행되는걸 확인할 수 있다.

![](https://i.imgur.com/53hblz1.png)


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
from urllib.request import urlretrieve
import pyspark


default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'start_date': datetime(2017, 11, 20),
    'email': ['airflow@airflow.com'],
    'email_on_failure': False,
    'email_on_retry': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=1),
    # 'queue': 'bash_queue',
    # 'pool': 'backfill',
    # 'priority_weight': 10,
    # 'end_date': datetime(2016, 1, 1),
}

dag = DAG(
    'spark-test2', default_args=default_args, schedule_interval="* * * * *")


def data_download():
    # 인터넷에 있는 데이터 다운로드
    # url = ("https://archive.ics.uci.edu/ml/machine-learning-databases"
        # "/adult/adult.data")
    url = ("http://samplecsvs.s3.amazonaws.com/Sacramentorealestatetransactions.csv")    
    local_filename = os.path.basename(url)
    if not os.path.exists(local_filename):
        print("Downloading datasets")
        urlretrieve(url, local_filename)


def spark_job():
    sc = pyspark.SparkContext()
    text_file = sc.textFile('Sacramentorealestatetransactions.csv')
    counts = text_file.flatMap(lambda line: line.split(" ")) \
                .map(lambda word: (word, 1)) \
                .reduceByKey(lambda a, b: a + b)
    now = datetime.now().strftime('%Y%m%dT%H%M%S')
    # counts.saveAsTextFile(now)
    f = open('/Users/hyundai/airflow/results/count_result.txt', 'w')
    f.write(str(counts.collect()))
    f.close()

data_download = PythonOperator(
    task_id='data_download',
    python_callable=data_download,
    dag=dag)


spark_job = PythonOperator(
    task_id='spark_job',
    python_callable=spark_job,
    dag=dag)

sleep = BashOperator(
    task_id='sleep30',
    bash_command="sleep 30",
    retries=3,
    dag=dag)

spark_job.set_upstream(data_download)
sleep.set_upstream(data_download)
```
