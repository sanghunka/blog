---
title: "appsflyer pull api를 이용해 daily report 적재하기"
metaAlignment: center
date: 2017-02-09
categories:
- dev
tags:
- appsflyer
- postgresql
- data loading
thumbnailImage: https://images.g2crowd.com/uploads/product/image/large_detail/large_detail_1531913368/appsflyer.jpg
thumbnailImagePosition: left
---


<!--more-->



- 참조: [appsflyer pull api 가이드](https://support.appsflyer.com/hc/en-us/articles/207034346-Pull-APIs-Pulling-AppsFlyer-Reports-by-APIs)


- push api가 아닌 pull api를 사용하는 이유.
  - 앱스플라이어 이벤트를 많이 정의할수록 push api가 빈번하게 호출된다.
  - 이는 서비스 품질 저하로 이어질 수 있음. (완벽한 분석용 DB가 따로 구축되어 있다면 상관 없다.)
  - **그러므로 실시간 데이터 적재보다는 하루에 한번, 전일자 데이터를 적재하기로 결정했고 이런 용도에는 pull api가 적합하다.**

- product와 archive에 동시에 적재하는 이유.
  - 앱스플라이어에서 데이터를 무한정 제공하진 않는다.
  - 가입한 플랜에 따라 최근 x일자 데이터만 조회가 가능하다.
  - 그렇기에 미리미리 자체적으로 데이터를 보관해야 한다.
  - 이를 위해 앱스플라이어에서 직접 AWS s3로 데이터를 알아서 쌓아주는 [data locker](https://support.appsflyer.com/hc/en-us/articles/218125823-AppsFlyer-Data-Locker)라는 서비스를 제공하지만 매우 비싸다.
  - 내부 db와의 join을 위해 product에 적재해야 하지만 계속 쌓아두기엔 용량의 압박이 있다.
  - **그러므로 동일한 데이터를 product와 archive에 동시에 쌓고 일정 시간이 지난 데이터는 product에서 삭제해서 용량 확보.**

- 주의점
  - api url에 timezone을 꼭 설정해주자.
    - 만약 한국시간대 기준으로 데이터를 적재하고 싶다면 `&timezone=Asia/Seoul`을 추가해 준다. https://hq.appsflyer.com/export/{APP_ID}}/{REPORT_TYPE}/v5?api_token={API_TOKEN}&from={FROM_DATE}&to={TO_DATE}&timezone=Asia/Seoul
  - [가이드](https://support.appsflyer.com/hc/en-us/articles/207034346-Pull-APIs-Pulling-AppsFlyer-Reports-by-APIs)대로라면 UTC+9를 의미하는 `&timezone=%2B09:00`도 동일한 결과를 보여야 하지만 동일한 결과를 보여주지 않는다. appsflyer에 문의 결과 문제가 있는것 같으니 `&timezone=Asia/Seoul`를 사용하라고 안내를 받았다.
  - 글로벌 서비스라면 currency도 정할 수 있다. 역시 [가이드](https://support.appsflyer.com/hc/en-us/articles/207034346-Pull-APIs-Pulling-AppsFlyer-Reports-by-APIs)참조.



## 1. API 정리

- Raw data reports의 installs report와 in-app-events를 한 테이블에 적재
- performance report - Partners by Date의 경우 이벤트 실행 횟수가 기록되어있는데 이벤트가 아직 확정되지 않아 보류
- 안드로이드
  - 인스톨: https://hq.appsflyer.com/export/{}/installs_report/v5?api_token={}&from={from_date}&to={to_date}&timezone=Asia/Seoul
  - 인앱이벤트: https://hq.appsflyer.com/export/{}/in_app_events_report/v5?api_token={}&from={from_date}&to={to_date}&timezone=Asia/Seoul
  - 리타게팅 인앱이벤트: api 엑세스 지원 X (추후 지원 예정)
- ios
  - 인스톨: https://hq.appsflyer.com/export/{}/installs_report/v5?api_token={}&from={from_date}&to={to_date}&timezone=Asia/Seoul
  - 인앱이벤트: https://hq.appsflyer.com/export/{}/in_app_events_report/v5?api_token={}&from={from_date}&to={to_date}&timezone=Asia/Seoul
  - 리타게팅 인앱이벤트: api 엑세스 지원 X (추후 지원 예정)

  - performance report - Daily를 사용.
  - performance report - Partners by Date의 경우 이벤트 실행 횟수가 기록되어있는데 이벤트가 아직 확정되지 않아 보류
  - 안드로이드: https://hq.appsflyer.com/export/{}/daily_report/v4?api_token={}&from={from_date}&to={to_date}&timezone=Asia/Seoul
  - ios: https://hq.appsflyer.com/export/{}/daily_report/v4?api_token={}&from={from_date}&to={to_date}&timezone=Asia/Seoul


## 2. 테이블 설계

|api 제공 column|필요?|디비 column 명|datatype|
|-|-|-|-|
|Date|O|date|date|
|Agency/PMD (af_prt)|X|agency|text|
|Media Source (pid)|O|media_source|text|
|Campaign (c)|O|campaign|text|
|Impressions|O|impressions|integer|
|Clicks|O|clicks|integer|
|CTR|O|ctr|numeric|
|Installs|O|installs|integer|
|Conversion Rate|O|conversion_rate|numeric|
|Sessions|O|sessions|integer|
|Loyal Users|O|loyal_users|integer|
|Loyal Users/Installs|O|loyal_users_over_installs|numeric|
|Total Cost|O|total_cost|numeric|
|Average eCPI|O|average_ecpi|numeric|
|(api에는 없지만 어떤 플랫폼에서 온 데이터인지 기록하기 위해 넣은 column)|O|platform|text|

## 3. 테이블 생성

```sql
create table appsflyer_daily (
    date date,
    agency text,
    media_source text,
    campaign text,
    impressions integer,
    clicks integer,
    ctr numeric,
    installs integer,
    conversion_rate numeric,
    sessions integer,
    loyal_users integer,
    loyal_users_over_installs numeric,
    total_cost numeric,
    average_ecpi numeric,
    platform text  -- android or ios
)
```

## 4. 안드로이드&ios dataframe 합치기

- 둘 다 동일한 format이라 문제 없음
- 다만 어떤 플랫폼의 데이터인지 구분해주기 위해 platform column 추가

## 5. temp테이블로 to_sql

- temp_appsflyer_raw_data

## 6. temp테이블 -> 적재테이블로 insert

## 전체 스크립트

```python
#-*-coding:utf-8
import datetime
import locale
import datetime
import os
import pandas as pd
import sqlalchemy
import psycopg2
import numpy as np
# from slacker import Slacker

# 작업이 완료되면 슬랙으로 noti를 주기위해 넣음. 다른 방식으로 작업 결과를 체크한다면 지워도 무방함.
# SLACK_CHANNEL = 'YOUR_SLACK_CHANNEL'
# SLACK_TOKEN = "YOUR_SLACK_BOT_TOKEN"

# DB연결 function
def connect(user, password, db, host, port=DEFAULT_PORT):
    '''Returns a connection and a metadata object'''
    url = 'postgresql://{}:{}@{}:{}/{}'
    url = url.format(user, password, host, port, db)

    # The return value of create_engine() is our connection object
    connection = sqlalchemy.create_engine(url, client_encoding='utf8')

    return connection

# product DB 연결 정보
product_user = ''
product_password = ''
product_host=''
product_port =
product_db = ''

# archive DB 연결 정보
archive_user = ''
archive_password = ''
archive_host=''
archive_port =
archive_db = ''

# 어제자 데이터를 가져와 적재하기 위해 어제 날자를 구함
today = datetime.datetime.now().date()
yesterday = today - datetime.timedelta(1)

# 어제 데이터만 조회하기 위함
from_date=to_date=yesterday


product_connection_string = "dbname={dbname} user={user} host={host} password={password} port={port}"\
                            .format(dbname=product_db,
                                    user=product_user,
                                    host=product_host,
                                    password=product_password,
                                    port=product_port)

archive_connection_string = "dbname={dbname} user={user} host={host} password={password} port={port}"\
                            .format(dbname=archive_db,
                                    user=archive_user,
                                    host=archive_host,
                                    password=archive_password,
                                    port=archive_port)

# 다운로드 경로 & api url 지정
download_path_daily_report = "WRITE_YOUR_PATH/{date}_{platform}_daily_report.csv"


download_path_android_daily_report = download_path_daily_report.format(date=yesterday, platform='android')
download_path_ios_daily_report = download_path_daily_report.format(date=yesterday, platform='ios')


api_url_android_daily_report = "https://PULL_API_URL_AND_API_KEY&from={from_date}&to={to_date}&timezone=Asia/Seoul"
api_url_ios_daily_report = "https://PULL_API_URL_AND_API_KEY&from={from_date}&to={to_date}&timezone=Asia/Seoul"


api_url_android_daily_report = api_url_android_daily_report.format(from_date=from_date
                                                                 , to_date=to_date)
api_url_ios_daily_report = api_url_ios_daily_report.format(from_date=from_date
                                                                 , to_date=to_date)

api_call="wget -O {download_path} '{api_url}'"
api_call_android_daily_report = api_call.format(download_path=download_path_android_daily_report
                                               , api_url = api_url_android_daily_report)
api_call_ios_daily_report = api_call.format(download_path=download_path_ios_daily_report
                                               , api_url = api_url_ios_daily_report)

# 위에서 만든 명령어로 레포트를 다운로드
os.system(api_call_android_daily_report)
os.system(api_call_ios_daily_report)

# 각 os별 레프트를 불러와 플랫폼 구분자를 넣어주고 하나의 dataframe으로 합치기
android_report = pd.read_csv(download_path_android_daily_report)
ios_report = pd.read_csv(download_path_ios_daily_report)

android_report['platform'] = 'android'
ios_report['platform'] = 'ios'

total_report = pd.concat([android_report, ios_report], axis = 0)

# column명을 정제. 대문자를 소문자로, 공백문자를 underscore로
total_report.columns = ['date', 'agency', 'media_source', 'campaign',
       'impressions', 'clicks', 'ctr', 'installs', 'conversion_rate',
       'sessions', 'loyas_users', 'loyal_users_over_installs', 'total_cost',
       'average_ecpi', 'platform']

# agency가 없을 경우 'None'이라는 스트링값으로 기록이 되어 이를 np.nan으로 replace
total_report['agency'] = total_report['agency'].replace('None', np.nan)

#connect production DB with sqlalchemy
product = connect(product_user, product_password, product_db, product_host, product_port)

# 기존에 적재하는 테이블에 어제자 데이터를 insert하는 형식으로 하기보단 아래 순서대로 작업을 진행.
# 1. 다운받은 csv를 temp 테이블로 옮긴 뒤,
# 2. temp테이블의 모든 레코드를 기존 적재 테이블로 옮기고
# 3. temp테이블 drop
# 4. 다운받은 csv파일 삭제

# 1. 다운받은 csv를 temp 테이블로 옮긴 뒤,
total_report.to_sql('temp_appsflyer_daily', product, index=False,
                     dtype={'date':sqlalchemy.types.Date(),
                            'agency':sqlalchemy.types.Text(),
                            'media_source':sqlalchemy.types.Text(),
                            'campaign':sqlalchemy.types.Text(),
                            'impressions':sqlalchemy.types.Integer(),
                            'clicks':sqlalchemy.types.Integer(),
                            'ctr':sqlalchemy.types.Numeric(),
                            'installs':sqlalchemy.types.Integer(),
                            'conversion_rate':sqlalchemy.types.Numeric(),
                            'sessions':sqlalchemy.types.Integer(),
                            'loyas_users':sqlalchemy.types.Integer(),
                            'loyal_users_over_installs':sqlalchemy.types.Numeric(),
                            'total_cost':sqlalchemy.types.Numeric(),
                            'average_ecpi':sqlalchemy.types.Numeric(),
                            'platform':sqlalchemy.types.Text()
                           }
                   )


# connect production DB wiht psycopg2
product_psycopg = psycopg2.connect(product_connection_string)
pc = product_psycopg.cursor()

# 2. temp테이블의 모든 레코드를 기존 적재 테이블로 옮기고
# 3. temp테이블 drop
sql_insert = """
insert into appsflyer_daily
select *
from temp_appsflyer_daily
"""
sql_drop = """drop table temp_appsflyer_daily"""

pc.execute(sql_insert)
pc.execute('commit')
pc.execute(sql_drop)
pc.execute('commit')
pc.close()

# 동일한 작업을 archive DB에서도 반복
# connect archive DB with sqlalchemy
archive = connect(archive_user, archive_password, archive_db, archive_host, archive_port)

total_report.to_sql('temp_appsflyer_daily', archive, index=False,
                     dtype={'date':sqlalchemy.types.Date(),
                            'agency':sqlalchemy.types.Text(),
                            'media_source':sqlalchemy.types.Text(),
                            'campaign':sqlalchemy.types.Text(),
                            'impressions':sqlalchemy.types.Integer(),
                            'clicks':sqlalchemy.types.Integer(),
                            'ctr':sqlalchemy.types.Numeric(),
                            'installs':sqlalchemy.types.Integer(),
                            'conversion_rate':sqlalchemy.types.Numeric(),
                            'sessions':sqlalchemy.types.Integer(),
                            'loyas_users':sqlalchemy.types.Integer(),
                            'loyal_users_over_installs':sqlalchemy.types.Numeric(),
                            'total_cost':sqlalchemy.types.Numeric(),
                            'average_ecpi':sqlalchemy.types.Numeric(),
                            'platform':sqlalchemy.types.Text()
                           }
                   )

# connect archive DB wiht psycopg2
archive_psycopg = psycopg2.connect(archive_connection_string)
ac = archive_psycopg.cursor()

ac.execute(sql_insert)
ac.execute('commit')
ac.execute(sql_drop)
ac.execute('commit')
ac.close()

# 4. 다운받은 csv파일 삭제
os.remove(download_path_android_daily_report)
os.remove(download_path_ios_daily_report)

# 작업이 완료되면 슬랙으로 noti
# slack = Slacker(SLACK_TOKEN)
# slack.chat.post_message(SLACK_CHANNEL, "appsflyer_daily.py done", as_user=True)
```
