---
title: "[airflow] 4. CeleryExecutor 사용하기"
metaAlignment: center
date: 2017-12-05 17:00:00+09:00
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

Airflow CeleryExecutor 사용하기

<!--more-->

<!--toc-->

Airflow는 기본값으로 sqlite를 사용한다. sqlite에서는 SequentialExecutor만 설정가능하기에 DAG내에서 task의 병렬실행이 불가능하다. 병렬실행을 가능하게 하려면 LocalExecutor나 CeleryExecutor를 사용해야하는데 그러기위해선 Database를 Sqlite가 아닌 다른 Database를 사용해야 한다. 

Database설치&설정은 [이전 글](sanghkaang.github.io/2017/12/airflow-3.-localexecutor-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0/)을 참조하자.

Celery는 message broker로 RabbitMQ, Redis, Amazon SQS등을 사용할 수 있는데 이 글에선 RabbitMQ의 경우로 예시를 들겠다.

# Rabbitmq, Celery 설치

```sh
brew install rabbitmq
pip install celery
# rabbitmq web ui 사용가능하게 하기
# rabbitmq-plugins enable rabbitmq_management
```

- 설정값을 변경하지 않았다면 기본적으로 rabbitmq는 **5672** port를 사용하고 webUI는 **15672** port를 사용한다.(http://localhost:15672/)
- 설치가 완료되면 자동으로 guest계정이 생성되어 있다.(username: guest, password: guest)

# Rabbitmq 설정

```sh
# rabbitmq server 실행
rabbitmq-server 
```

```sh
# 설정
rabbitmqctl add_user sanghun sanghun
rabbitmqctl add_vhost airflow_vhost
rabbitmqctl set_user_tags sanghun airflow
rabbitmqctl set_permissions -p airflow_vhost sanghun ".*" ".*" ".*"

# Format
# rabbitmqctl add_user {username} {password}
# rabbitmqctl add_vhost {rabbitmq_virtual_host_name}
# rabbitmqctl set_user_tags {username} {tag_name}
# rabbitmqctl set_permissions -p {rabbitmq_virtual_host_name} {username} ".*" ".*" ".*"
```

# airflow.cfg 수정

airflow home폴더 아래의 `airflow.cfg`를 아래와 같이 수정해준다.

```sh
executor = CeleryExecutor
broker_url = amqp://sanghun:sanghun@localhost:5672/airflow_vhost

# Format: transport://userid:password@hostname:port/virtual_host
# port는 rabbitmq의 port

celery_result_backend = amqp://sanghun:sanghun@localhost:5672/airflow_vhost
# celery_result_backend는 
# 1. broker_url과 동일하게 하거나 
# 2. meta db url(sql_alchemy_conn)을 적어주면 된다.

donot_pickle=True
# 이 옵션을 수정해줘야 CeleryExecutor가 실행된다. 이유는 모르겠다. 
```

## localhost??

localhost가 아닐 경우도 있다. rabbitmq-server를 실행하면 아래처럼 메세지가 나온다.

```sh
ubuntu@ip-172-19-32-248:~/airflow/dags$ sudo rabbitmq-server

              RabbitMQ 3.5.7. Copyright (C) 2007-2015 Pivotal Software, Inc.
  ##  ##      Licensed under the MPL.  See http://www.rabbitmq.com/
  ##  ##
  ##########  Logs: /var/log/rabbitmq/rabbit@ip-172-19-32-248.log
  ######  ##        /var/log/rabbitmq/rabbit@ip-172-19-32-248-sasl.log
  ##########
              Starting broker... completed with 6 plugins.
```

여기서는 localhost가 아닌 **ip-172-19-32-248**를 broker_url, celery_result_backend에 적어줘야 한다.

# meta db 설정

```sh
# Modify /usr/local/var/postgres/pg_hba.conf to add Client Authentication Record
# IPv4 local connections:
host    all         all         0.0.0.0/0          md5 # 0.0.0.0/0 stands for all ips; use CIDR address to restrict access; md5 for pwd authentication

# Change the Listen Address in /usr/local/var/postgres/postgresql.conf
listen_addresses = '*'

# Create a user and grant privileges (run the commands below under superuser of postgres)
CREATE USER your_postgres_user_name WITH ENCRYPTED PASSWORD 'your_postgres_pwd';
GRANT ALL PRIVILEGES ON DATABASE your_database_name TO your_postgres_user_name;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO your_postgres_user_name;

# Restart the PostgreSQL server and test it out
brew services restart postgresql
psql -U [postgres_user_name] -h [postgres_host_name] -d [postgres_database_name]

# IMPORTANT: update your sql_alchemy_conn string in airflow.cfg
```

> http://suite.opengeo.org/docs/latest/dataadmin/pgGettingStarted/firstconnect.html

# 실행

- celeryExecutor의 경우, `airflow worker`를 실행해줘야 한다.

```
rabbitmq-server
airflow worker
airflow webserver
airflow scheduler
```


# 참조

> https://stlong0521.github.io/20161023%20-%20Airflow.html
> http://blog.csdn.net/qazplm12_3/article/details/53065654
> https://stackoverflow.com/questions/39221442/no-module-named-unusual-prefix
> http://blog.genesino.com/2016/05/airflow/#配置celeryexecutor-rabbitmq支持
> https://cwiki.apache.org/confluence/display/AIRFLOW/Airflow+Home