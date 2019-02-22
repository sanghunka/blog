---
title: "python에서 postgres DB 연결해서 쿼리 조회하기"
date: 2016-11-21
metaAlignment: center
categories:
- dev
tags:
- python
- pandas
- postgres
- psycopg2
- sqlalchemy
thumbnailImage: //lh4.googleusercontent.com/-vdp4sgLhjK0/Tu35E8S3aBI/AAAAAAAAAOc/55ms0lMst0o/w288-h288/python_postgresql_hA0NV1.png
thumbnailImagePosition: left
---

2번 방법을 추천한다. 이유는 아래에서.

<!--more-->

<!-- toc -->
## 1. psycopg2

```python
#-*-coding:utf-8
import psycopg2
import pandas as pd

def execute(query):
    pc.execute(query)
    return pc.fetchall()


#아래 정보를 입력
user = ''
password = ''
host_product = ''
dbname = ''
port=''

product_connection_string = "dbname={dbname} user={user} host={host} password={password} port={port}"\
                            .format(dbname=dbname,
                                    user=user,
                                    host=host_product,
                                    password=password,
                                    port=port)    
try:
    product = psycopg2.connect(product_connection_string)
except:
    print("I am unable to connect to the database")

pc = product.cursor()


#쿼리 입력
query = """
select id from users limit 1
"""

#일반적인 쿼리 조회 방법
result = execute(query)

#pandas를 통한 조회 방법
pd.read_sql("select id from users limit 1", product)
```

psycopg2를 이용한 db연결은 기존에 내가 사용하던 방법이다. 실행하고자 하는 쿼리를 스트링형태로 그대로 넘겨주면 되서 편하다. 다만 쿼리 실행 결과가 python list 형태로 반환되는데 이를 다루기가 까다롭다. 그리고 결과만 나오는것도 아쉽다.

예를 들어 select id from users라고 했으면 column name인 'id'에 대한 정보까지 따라왔으면 한다. 이를 pandas를 통해 해결할 수 있다. ```pd.read_sql("select id from users limit 1", product)```를 실행해보면 column name을 포함하면서도 깔끔하게 쿼리결과를 조회할 수 있다.



## 2. sqlalchemy


```python
import sqlalchemy
import pandas as pd

def connect(user, password, db, host='여기에 입력', port=여기에 입력):
    '''Returns a connection and a metadata object'''
    # We connect with the help of the PostgreSQL URL
    # postgresql://federer:grandestslam@localhost:5432/tennis
    url = 'postgresql://{}:{}@{}:{}/{}'
    url = url.format(user, password, host, port, db)

    # The return value of create_engine() is our connection object
    con = sqlalchemy.create_engine(url, client_encoding='utf8')

    # We then bind the connection to MetaData()
    #meta = sqlalchemy.MetaData(bind=con, reflect=True)

    return engine#, meta

#연결
engine = connect('user 입력', 'password 입력', 'db name 입력')

#쿼리 조회
pd.read_sql("select * from users limit 1", engine)
```
- sqlalchemy를 이용해 쿼리를 조회하려면 meta를 이용해 select클래스를 사용해야하던데 복잡하다. 나는 sql구문 그대로 조회하기를 원한다.
- 그래서 pandas의 read_sql을 이용해 조회하는걸 선호한다.
- pandas를 통하므로 추후 데이터 핸들링에도 용이함.
- meta는 사용할 일이 없어 그냥 주석처리했음.


2번 방식을 추천하는 이유는
- pandas의 read_sql의 경우 psycopg2로 연결하나 sqlalchemy로 연결하나 상관없음.
- 다만 read_sql_table의 경우 sqlalchemy연결일때만 동작함.
- 아래 코드에서 db부분을 psycopg2_connection와 sqlalchemy_connection로 바꿔가며 실행해 보자.

```python
psycopg2_connection = psycopg2.connect(product_connection_string)
sqlalchemy_connection = connect('user 입력', 'password 입력', 'db name 입력')

#db = psycopg2_connection
#db = sqlalchemy_connection

pd.read_sql_query("select id from users limit 1", db)
pd.read_sql("select id from users limit 1", db)
pd.read_sql_table("users", db)
```

### 참조

- http://www.rmunn.com/sqlalchemy-tutorial/tutorial.html
- https://suhas.org/sqlalchemy-tutorial
