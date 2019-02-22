---
title: "appsflyer pull api를 이용해 raw data 적재하기"
metaAlignment: center
date: 2017-02-09
categories:
- dev
tags:
- appsflyer
- postgresql
- data loading
thumbnailImage: //pbs.twimg.com/profile_images/559739646804885504/7KnZfyyo.png
thumbnailImagePosition: left
---

<!--more-->


<!--toc-->

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


- 문제점
  - `is_primary_attribution`의 경우, True/False로 들어올때도 있고 0/1로 들어올때도 있었다. 데이터의 통일성을 위해, 그리고 예외처리를 위해 `binary_to_boolean`함수를 만들어서 처리함.
  - 테스트를 위해 `event_value`에 최대한 많은 데이터를 담아 전송했으나 실제 이만큼 많은 데이터를 적재할 필요는 없다. 다른 column들과 많이 중복되기도 하고. 그래서 필요한 부분만 가져오고 나머지는 None으로 만들어버렸다.


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

## 2. 테이블 설계

|api 제공 column|필요?|디비 column 명|datatype|비고|
|-----------------|-------|---------------|----------|------|
|Attributed Touch Type|O|attributed_touch_type|text||
|Attributed Touch Time|O|attributed_touch_time|timestamp||
|Install Time|O|install_time|timestamp||
|Event Time|O|event_time|timestamp||
|Event Name|O|event_name|text||
|Event Value|O|event_value|text||
|Event Revenue|X|event_revenue|||
|Event Revenue Currency|X|event_revenue_currency|||
|Event Revenue USD|X|event_revenue_usd|||
|Event Source|X|event_source|||
|Is Receipt Validated|X|is_receipt_validated|||
|Partner|O|partner|text||
|Media Source|O|media_source|text||
|Channel|O|channel|text||
|Keywords|O|keywords|text||
|Campaign|O|campaign|text||
|Campaign ID|O|campaign_id|text||
|Adset|O|adset|text||
|Adset ID|O|adset_id|text||
|Ad|O|ad|text||
|Ad ID|O|ad_id|text||
|Ad Type|O|ad_type|text||
|Site ID|O|site_id|text||
|Sub Site ID|X|sub_site_id|text||
|Sub Param 1|X|sub_param_1|text||
|Sub Param 2|X|sub_param_2|text||
|Sub Param 3|X|sub_param_3|text||
|Sub Param 4|X|sub_param_4|text||
|Sub Param 5|X|sub_param_5|text||
|Cost Model|X|cost_model|||
|Cost Value|X|cost_value|||
|Cost Currency|X|cost_currency|||
|Contributor 1 Partner|O|contributor_1_partner|text||
|Contributor 1 Media Source|O|contributor_1_media_source|text||
|Contributor 1 Campaign|O|contributor_1_campaign|text||
|Contributor 1 Touch Type|O|contributor_1_touch_type|text||
|Contributor 1 Touch Time|O|contributor_1_touch_time|timestamp||
|Contributor 2 Partner|O|contributor_2_partner|text||
|Contributor 2 Media Source|O|contributor_2_media_source|text||
|Contributor 2 Campaign|O|contributor_2_campaign|text||
|Contributor 2 Touch Type|O|contributor_2_touch_type|text||
|Contributor 2 Touch Time|O|contributor_2_touch_time|timestamp||
|Contributor 3 Partner|O|contributor_3_partner|text||
|Contributor 3 Media Source|O|contributor_3_media_source|text||
|Contributor 3 Campaign|O|contributor_3_campaign|text||
|Contributor 3 Touch Type|O|contributor_3_touch_type|text||
|Contributor 3 Touch Time|O|contributor_3_touch_time|timestamp||
|Region|X|region|text||
|Country Code|X|country_code|text||
|State|X|state|text||
|City|X|city|text||
|Postal Code|X|postal_code|text||
|DMA|X|dma|text||
|IP|X|ip|text||
|WIFI|O|wifi|boolean||
|Operator|X|operator|text||
|Carrier|X|carrier|text||
|Language|X|language|text||
|AppsFlyer ID|O|appsflyer_id|text||
|Advertising ID|O|advertising_id|text||
|IDFA|X|idfa|text|advertising_id에 같이 적재|
|Android ID|X|android_id|text||
|Customer User ID|O|customer_user_id|integer||
|IMEI|X|imei|text||
|IDFV|X|idfv|text||
|Platform|O|platform|text||
|Device Type|X|device_type|||
|OS Version|X|os_version|||
|App Version|X|app_version|||
|SDK Version|X|sdk_version|||
|App ID|X|app_id|||
|App Name|X|app_name|||
|Bundle ID|X|bundle_id|||
|Is Retargeting|O|is_retargeting|boolean||
|Retargeting Conversion Type|O|retargeting_conversion_type|text||
|Attribution Lookback|O|attribution_lookback|text||
|Reengagement Window|O|reengagement_window|text||
|Is Primary Attribution|O|is_primary_attribution|boolean||
|User Agent|X|user_agent|||
|HTTP Referrer|X|http_referrer|||
|Original URL|X|original_url|||

## 3. 테이블 생성

```sql
create table appsflyer_raw_data
(
    attributed_touch_type   text,
    attributed_touch_time   timestamp,
    install_time    timestamp,
    event_time  timestamp,
    event_name  text,
    event_value text,
    partner text,
    media_source    text,
    channel text,
    keywords    text,
    campaign    text,
    campaign_id text,
    adset   text,
    adset_id    text,
    ad  text,
    ad_id   text,
    ad_type text,
    site_id text,
    contributor_1_partner   text,
    contributor_1_media_source  text,
    contributor_1_campaign  text,
    contributor_1_touch_type    text,
    contributor_1_touch_time    timestamp,
    contributor_2_partner   text,
    contributor_2_media_source  text,
    contributor_2_campaign  text,
    contributor_2_touch_type    text,
    contributor_2_touch_time    timestamp,
    contributor_3_partner   text,
    contributor_3_media_source  text,
    contributor_3_campaign  text,
    contributor_3_touch_type    text,
    contributor_3_touch_time    timestamp,
    wifi    boolean,
    appsflyer_id    text,
    advertising_id  text,
    customer_user_id integer,  
    platform    text,
    is_retargeting  boolean,
    retargeting_conversion_type text,
    attribution_lookback    text,
    reengagement_window text,
    is_primary_attribution  boolean
)
```

## 4. 안드로이드&ios dataframe 합치기

- 둘 다 동일한 format이라 문제 없음

## 5. temp테이블로 to_sql

- temp_appsflyer_raw_data

## 6. temp테이블 -> 적재테이블로 insert

## 전체 스크립트

```python
#-*-coding:utf-8
import datetime
import locale
import datetime
import time
import os
import pandas as pd
import sqlalchemy
import psycopg2
import numpy as np
import json
# from slacker import Slacker

# 작업이 완료되면 슬랙으로 noti를 주기위해 넣음. 다른 방식으로 작업 결과를 체크한다면 지워도 무방함.
# SLACK_CHANNEL = 'YOUR_SLACK_CHANNEL'
# SLACK_TOKEN = "YOUR_SLACK_BOT_TOKEN"

def api_calling(api_call_command, download_path):
    while True:
        os.system(api_call_command)
        # 용량이 0바이트인 csv가 다운로드 되었을 경우
        if os.path.getsize(download_path) == 0:
            #slack.chat.post_message(SLACK_CHANNEL, "[failed]" + download_path, as_user=True)
            os.remove(download_path)
            time.sleep(10*60)   # 10분 기다렸다가 다시 다운로드 시도해
            continue
        break

def binary_to_boolean(input_str):
    try:
        if int(input_str) == 1:
            return True
        elif int(input_str) == 0:
            return False
        else:
            return input_str
    except:
        return input_str

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


# 다운로드 경로& api url 지정
download_path_report = "WRITE_YOUR_PATH/{date}_{platform}_{type}_report.csv"

download_path_android_install_report = download_path_report.format(date=yesterday, platform='android', type='install')
download_path_android_in_app_events_report = download_path_report.format(date=yesterday, platform='android', type='in_app_events')

download_path_ios_install_report = download_path_report.format(date=yesterday, platform='ios', type='install')
download_path_ios_in_app_events_report = download_path_report.format(date=yesterday, platform='ios', type='in_app_events')

api_url_android_install_report = "https://PULL_API_URL_AND_API_KEY&from={from_date}&to={to_date}&timezone=Asia/Seoul"
api_url_android_in_app_events_report = "https://PULL_API_URL_AND_API_KEY&from={from_date}&to={to_date}&timezone=Asia/Seoul"

api_url_ios_install_report = "https://PULL_API_URL_AND_API_KEY&from={from_date}&to={to_date}&timezone=Asia/Seoul"
api_url_ios_in_app_events_report = "https://PULL_API_URL_AND_API_KEY&from={from_date}&to={to_date}&timezone=Asia/Seoul"

api_url_android_install_report = api_url_android_install_report.format(from_date=from_date
                                                                 , to_date=to_date)
api_url_android_in_app_events_report = api_url_android_in_app_events_report.format(from_date=from_date
                                                                                     , to_date=to_date)
api_url_ios_install_report = api_url_ios_install_report.format(from_date=from_date
                                                                 , to_date=to_date)
api_url_ios_in_app_events_report = api_url_ios_in_app_events_report.format(from_date=from_date
                                                                            , to_date=to_date)


api_call="wget -O '{download_path}' '{api_url}'"
api_call_android_install_report = api_call.format(download_path=download_path_android_install_report
                                               , api_url = api_url_android_install_report)
api_call_android_in_app_events_report = api_call.format(download_path=download_path_android_in_app_events_report
                                               , api_url = api_url_android_in_app_events_report)
api_call_ios_install_report = api_call.format(download_path=download_path_ios_install_report
                                               , api_url = api_url_ios_install_report)
api_call_ios_in_app_events_report = api_call.format(download_path=download_path_ios_in_app_events_report
                                               , api_url = api_url_ios_in_app_events_report)


# raw data report download
api_calling(api_call_android_install_report, download_path_android_install_report)
api_calling(api_call_ios_install_report, download_path_ios_install_report)
api_calling(api_call_android_in_app_events_report, download_path_android_in_app_events_report)
api_calling(api_call_ios_in_app_events_report, download_path_ios_in_app_events_report)


android_install_report = pd.read_csv(download_path_android_install_report)
android_in_app_events_report = pd.read_csv(download_path_android_in_app_events_report)
ios_install_report = pd.read_csv(download_path_ios_install_report)
ios_in_app_events_report = pd.read_csv(download_path_ios_in_app_events_report)

total_report = pd.concat([  android_install_report
                          , android_in_app_events_report
                          , ios_install_report
                          , ios_in_app_events_report], axis = 0)

# column명을 정제. 대문자를 소문자로, 공백문자를 underscore로
total_report.columns = [column.lower().replace(' ', '_') for column in total_report.columns]


# ios의 경우 idfa컬럼값을 advertising_id로 복사
total_report.loc[total_report.platform == 'ios', 'advertising_id'] = total_report.loc[total_report.platform == 'ios', 'idfa']

#db에 적재할 column만 가져오기
necessary_columns = [
    'attributed_touch_type', 'attributed_touch_time', 'install_time', 'event_time', 'event_name', 'event_value',
    'partner', 'media_source', 'channel', 'keywords', 'campaign', 'campaign_id', 'adset', 'adset_id', 'ad','ad_id', 'ad_type', 'site_id',
    'contributor_1_partner', 'contributor_1_media_source', 'contributor_1_campaign', 'contributor_1_touch_type', 'contributor_1_touch_time',
    'contributor_2_partner', 'contributor_2_media_source', 'contributor_2_campaign', 'contributor_2_touch_type', 'contributor_2_touch_time',
    'contributor_3_partner', 'contributor_3_media_source', 'contributor_3_campaign', 'contributor_3_touch_type', 'contributor_3_touch_time',
    'wifi', 'appsflyer_id', 'advertising_id', 'customer_user_id', 'platform', 'is_retargeting', 'retargeting_conversion_type', 'attribution_lookback', 'reengagement_window', 'is_primary_attribution']

sliced_report = total_report[necessary_columns]

#구매 이벤트일 경우에만 영수증 아이디를 남기고 나머지 경우에는 모두 삭제
sliced_report['event_value'] = sliced_report['event_value'].apply(lambda x: int(json.loads(x)['af_receipt_id'])
                                                                       if 'af_receipt_id' in str(x)
                                                                       else None)

# is_primary_attribution의 경우 True/False로 들어올때도 있고 0/1로 들어올 떄도 있다.
# Boolean으로 처리해주기 위해 데이터를 전처리해주자
sliced_report['is_primary_attribution'] = sliced_report['is_primary_attribution'].apply(lambda x: binary_to_boolean(x))

# 기존에 적재하는 테이블에 어제자 데이터를 insert하는 형식으로 하기보단 아래 순서대로 작업을 진행.
# 1. 다운받은 csv를 temp 테이블로 옮긴 뒤,
# 2. temp테이블의 모든 레코드를 기존 적재 테이블로 옮기고
# 3. temp테이블 drop
# 4. 다운받은 csv파일 삭제

#connect production DB with sqlalchemy
product = connect(product_user, product_password, product_db, product_host, product_port)


# 1. 다운받은 csv를 temp 테이블로 옮긴 뒤,
sliced_report.to_sql('temp_appsflyer_raw_data', product, index=False,
                    dtype={
                           'attributed_touch_type':sqlalchemy.types.Text(),
                           'attributed_touch_time':sqlalchemy.types.TIMESTAMP(),
                           'install_time':sqlalchemy.types.TIMESTAMP(),
                           'event_time':sqlalchemy.types.TIMESTAMP(),
                           'event_name':sqlalchemy.types.Text(),
                           'event_value':sqlalchemy.types.Text(),
                           'partner':sqlalchemy.types.Text(),
                           'media_source':sqlalchemy.types.Text(),
                           'channel':sqlalchemy.types.Text(),
                           'keywords':sqlalchemy.types.Text(),
                           'campaign':sqlalchemy.types.Text(),
                           'campaign_id':sqlalchemy.types.Text(),
                           'adset':sqlalchemy.types.Text(),
                           'adset_id':sqlalchemy.types.Text(),
                           'ad':sqlalchemy.types.Text(),
                           'ad_id':sqlalchemy.types.Text(),
                           'ad_type':sqlalchemy.types.Text(),
                           'site_id':sqlalchemy.types.Text(),
                           'contributor_1_partner':sqlalchemy.types.Text(),
                           'contributor_1_media_source':sqlalchemy.types.Text(),
                           'contributor_1_campaign':sqlalchemy.types.Text(),
                           'contributor_1_touch_type':sqlalchemy.types.Text(),
                           'contributor_1_touch_time':sqlalchemy.types.TIMESTAMP(),
                           'contributor_2_partner':sqlalchemy.types.Text(),
                           'contributor_2_media_source':sqlalchemy.types.Text(),
                           'contributor_2_campaign':sqlalchemy.types.Text(),
                           'contributor_2_touch_type':sqlalchemy.types.Text(),
                           'contributor_2_touch_time':sqlalchemy.types.TIMESTAMP(),
                           'contributor_3_partner':sqlalchemy.types.Text(),
                           'contributor_3_media_source':sqlalchemy.types.Text(),
                           'contributor_3_campaign':sqlalchemy.types.Text(),
                           'contributor_3_touch_type':sqlalchemy.types.Text(),
                           'contributor_3_touch_time':sqlalchemy.types.TIMESTAMP(),
                           'wifi':sqlalchemy.types.Boolean(),
                           'appsflyer_id':sqlalchemy.types.Text(),
                           'advertising_id':sqlalchemy.types.Text(),
                           'customer_user_id':sqlalchemy.types.Integer(),
                           'platform':sqlalchemy.types.Text(),
                           'is_retargeting':sqlalchemy.types.Boolean(),
                           'retargeting_conversion_type':sqlalchemy.types.Text(),
                           'attribution_lookback':sqlalchemy.types.Text(),
                           'reengagement_window':sqlalchemy.types.Text(),
                           'is_primary_attribution':sqlalchemy.types.Boolean()
                          }
                  )

# connect production DB wiht psycopg2
product_psycopg = psycopg2.connect(product_connection_string)
pc = product_psycopg.cursor()

# 2. temp테이블의 모든 레코드를 기존 적재 테이블로 옮기고
# 3. temp테이블 drop
sql_insert = """
insert into appsflyer_raw_data
select *
from temp_appsflyer_raw_data
"""
sql_drop = """drop table temp_appsflyer_raw_data"""

pc.execute(sql_insert)
pc.execute('commit')
pc.execute(sql_drop)
pc.execute('commit')
pc.close()

# 동일한 작업을 archive DB에서도 반복
#connect archive DB with sqlalchemy
archive = connect(archive_user, archive_password, archive_db, archive_host, archive_port)

#temp 테이블로 넣기
sliced_report.to_sql('temp_appsflyer_raw_data', archive, index=False,
                    dtype={
                           'attributed_touch_type':sqlalchemy.types.Text(),
                           'attributed_touch_time':sqlalchemy.types.TIMESTAMP(),
                           'install_time':sqlalchemy.types.TIMESTAMP(),
                           'event_time':sqlalchemy.types.TIMESTAMP(),
                           'event_name':sqlalchemy.types.Text(),
                           'event_value':sqlalchemy.types.Text(),
                           'partner':sqlalchemy.types.Text(),
                           'media_source':sqlalchemy.types.Text(),
                           'channel':sqlalchemy.types.Text(),
                           'keywords':sqlalchemy.types.Text(),
                           'campaign':sqlalchemy.types.Text(),
                           'campaign_id':sqlalchemy.types.Text(),
                           'adset':sqlalchemy.types.Text(),
                           'adset_id':sqlalchemy.types.Text(),
                           'ad':sqlalchemy.types.Text(),
                           'ad_id':sqlalchemy.types.Text(),
                           'ad_type':sqlalchemy.types.Text(),
                           'site_id':sqlalchemy.types.Text(),
                           'contributor_1_partner':sqlalchemy.types.Text(),
                           'contributor_1_media_source':sqlalchemy.types.Text(),
                           'contributor_1_campaign':sqlalchemy.types.Text(),
                           'contributor_1_touch_type':sqlalchemy.types.Text(),
                           'contributor_1_touch_time':sqlalchemy.types.TIMESTAMP(),
                           'contributor_2_partner':sqlalchemy.types.Text(),
                           'contributor_2_media_source':sqlalchemy.types.Text(),
                           'contributor_2_campaign':sqlalchemy.types.Text(),
                           'contributor_2_touch_type':sqlalchemy.types.Text(),
                           'contributor_2_touch_time':sqlalchemy.types.TIMESTAMP(),
                           'contributor_3_partner':sqlalchemy.types.Text(),
                           'contributor_3_media_source':sqlalchemy.types.Text(),
                           'contributor_3_campaign':sqlalchemy.types.Text(),
                           'contributor_3_touch_type':sqlalchemy.types.Text(),
                           'contributor_3_touch_time':sqlalchemy.types.TIMESTAMP(),
                           'wifi':sqlalchemy.types.Boolean(),
                           'appsflyer_id':sqlalchemy.types.Text(),
                           'advertising_id':sqlalchemy.types.Text(),
                           'customer_user_id':sqlalchemy.types.Integer(),
                           'platform':sqlalchemy.types.Text(),
                           'is_retargeting':sqlalchemy.types.Boolean(),
                           'retargeting_conversion_type':sqlalchemy.types.Text(),
                           'attribution_lookback':sqlalchemy.types.Text(),
                           'reengagement_window':sqlalchemy.types.Text(),
                           'is_primary_attribution':sqlalchemy.types.Boolean()
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
os.remove(download_path_android_install_report)
os.remove(download_path_android_in_app_events_report)
os.remove(download_path_ios_install_report)
os.remove(download_path_ios_in_app_events_report)

# 작업이 완료되면 슬랙으로 noti
# slack = Slacker(SLACK_TOKEN)
# slack.chat.post_message(SLACK_CHANNEL, "appsflyer_raw_data.py done", as_user=True)
```
