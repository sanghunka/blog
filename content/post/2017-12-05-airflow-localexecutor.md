---
title: "[airflow] 3. LocalExecutor 사용하기"
metaAlignment: center
date: 2017-12-05 16:00:00+09:00
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

Airflow LocalExecutor 사용하기

<!--more-->

<!--toc-->

Airflow는 기본값으로 sqlite를 사용한다. sqlite에서는 SequentialExecutor만 설정가능하기에 DAG내에서 task의 병렬실행이 불가능하다. 병렬실행을 가능하게 하려면 LocalExecutor나 CeleryExecutor를 사용해야하는데 그러기위해선 Database를 Sqlite가 아닌 다른 Database를 사용해야 한다.

이 글에선 postgres를 예로 들어 설명하겠다.

# Postgres 설치

`brew install postgres`

# Database 설정

- `psql`명령어를 통해 postgres terminal 접속.
- airflow용 계정과 database를 만든 뒤, 계정에 db접근권한을 부여한다.
- 테이블 권한을 부여하기 위해선, 해당 테이블과 연결된 상태여야 한다. (\c 명령어를 이용)
- 권한 부여하기 귀찮으면 superuser를 이용하면 된다.

```sql
postgres=# CREATE DATABASE airflow;
postgres=# CREATE USER sanghun with ENCRYPTED PASSWORD 'sanghun';
postgres=# GRANT all privileges on DATABASE airflow to sanghun;
postgres=# \c airflow
postgres=# grant all privileges on all tables in schema public to sanghun;

-- Format
-- CREATE DATABASE {database_name};
-- CREATE USER {user_name} with ENCRYPTED PASSWORD '{password}';
-- GRANT all privileges on DATABASE {database_name} to {user_name};
-- \c {database_name}
-- grant all privileges on all tables in schema public to {user_name};
```

# airflow.cfg 수정

airflow home폴더 아래의 `airflow.cfg`를 아래와 같이 수정해준다.

```sh
executor = LocalExecutor
sql_alchemy_conn = postgresql+psycopg2://sanghun:sanghun@localhost/airflow

# Format： dialect+driver://username:password@host:port/database
```

# meta db 설정

```sh
# Modify /usr/local/var/postgres/pg_hba.conf to add Client Authentication Record
# IPv4 local connections:
host    all         all         0.0.0.0/0          md5 # 0.0.0.0/0 stands for all ips; use CIDR address to restrict access; md5 for pwd authentication
```

# airflow initdb

`airflow initdb`명령어를 통해 DB초기화 해주면 이제 sqlite가 아닌 postgres를 사용할 수 있다.

# 참조

> https://gist.github.com/rosiehoyem/9e111067fe4373eb701daf9e7abcc423
> https://stlong0521.github.io/20161023%20-%20Airflow.html   Mysql을 사용한 예시.
> http://blog.csdn.net/qazplm12_3/article/details/53065654
> https://stackoverflow.com/questions/39221442/no-module-named-unusual-prefix
> http://blog.genesino.com/2016/05/airflow/#配置celeryexecutor-rabbitmq支持
> https://cwiki.apache.org/confluence/display/AIRFLOW/Airflow+Home
