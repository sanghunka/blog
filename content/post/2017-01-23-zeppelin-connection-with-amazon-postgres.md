---
title: "apache zeppelin에 postgres DB 연결하기"
metaAlignment: center
date: 2017-01-23
categories:
- dev
tags:
- dev
- python
- zeppelin
- postgres
- database
thumbnailImage: //zeppelin-project.org/assets/themes/nflabs-sb/img/zeppelin-logo.svg
thumbnailImagePosition: left
autoThumbnailImage: yes
---

<!--more-->


<center><img src="https://lh3.googleusercontent.com/Q8LW9mTJUIkb23HIJ1jdNK5NtBL3YHRXukmJhp-AooObWhmj09OoDazYNDR8awR39Kj6-At3VV7m90EsIA5V1-QxtKpbJKl8QexL4BKwpM5ozjGA5qHe1IXbl9iQ38IdF_DjQVrZ9_5yRaco_MTJAn_veRNkemZmggFnngcqAq4KYIx86l0HZOK86QkRbEQIqxdAi59MELgHt_d_PDTtN3iu4HabFVZxrfv-9xNHIF5h0phoLTblzopFQumX6O_ZZ06Lv-r31TkXJhsoA5921udDsmBkrGxAvA4zuNeCBEm-2AkN_wWFo6CpXdv4nGKYTQbIUc5d4AKvGbuK_s7Y-KsKaqL7ySOKlAnzseuA7pnVvsU46QpyJE_b8a3mjStG7RRoYQ1htjloGKNtmdxKXAfWDAbB17ZBF1cjXw21XVDm2-CNQZEsYnH_A-ejmxVctcsrU4fDm9hUoU_3lvh0msEO-7fKOm4bfO1pJt2K1ktridLHfvSMRlLMbuuONV-rnbdgjMwWmY41LqEFkKKLXesFuQurBksCDPKYyxXdFsTHpAITPN1ZQfFnJsJ_BaI0RFaydxpDr0g8lJvyCaVf2y3pPbCjhkNZYKcpZ1g9mV3VbS2iGNO9mDTyDgGt38afKTJ5EqYHRCfpQQb746ZyzDIMAHW6antf931V=w576-h384-no" width="600"></center>

- 제플린 빌드 후 오른쪽 상단에서 Interpreter 클릭

<center><img src="https://lh3.googleusercontent.com/yMvKCxzH2CdpfY2B3F7dt6cHEcNGi-6j1QI6Pl04fLs7fxDAGobchoNowirRoG4YvvCS-V2FBAbs_tq2ouKL2JGY79TxWQ-JLbWW-jcdYZOb9a7PYB5BO5J1fwoD3IyH-P_5RbG1XdhufBCt0QD20gkr7QmHybV1jnnZW5uY_Q3e5tryvx3EJ2xh2BHrX_Z4gpQQCTkFZEfMhXb1wBfM_SvquLqW_0ZJaqXMKl3aFkOQ7ox9RVvbZHecPTLyjIyPA693znr2cF2E-GVx_5kwIny7x-5f-IS2OHa4Tg1WaIdy7QXLDY1u0AcIZE9rSAu-F8wrsUV3xOIVPpDH31EDWoe2CKdzIXw9YamIBAG9MufGWw_fV5yxRvk8q5j0PP6uVv7pMaDjTknigwLaMTTa6n7-nGCz6qEHN_6iG2zt5d1DrWH-9JRHIrbYBc5v1UkKDkkShspazXSor4IS0brfiGV5IJBDBhqBwlBqGsrWwHhH0tuOOwWA63_Oa7E0XDBGvi8rBOPlpx0VleAfnxf0NEB6dHdOghNlfz8IJUn7G0xylMInBpNNuqEuER-gmIVpDJX-UkfPvYyxFnJ4jY9dfHNHYREqdD2wY6cvpxqIXZwy06CwTE8GB5cglFcK9VwiQ1VPNJcQRYFw1JxO16V99wFLpsT0nTKRuNn7=w922-h884-no" width="600"></center>

- `jdbc`에서 DB 정보 입력

- `common.max_count`: 한번에 몇개의 row를 조회할 것인지 설정
- `default.driver`: org.postgresql.Driver
- `default.password`: DB 패스워드
- `default.url`: DB 주소. jdbc:postgresql://DNS_ADDRESS:PORT/DBNAME 형태로 입력.
- `default.user`: DB user name 입력

<center><img src="https://lh3.googleusercontent.com/wggft4gcGbk7tvytygmITvH6spqk-2JNeQTZgzrSy7Xld4TelQzhiYouVl_cT_wnD2Aou_LUgIV2Y8aJRaalJOETCzgsoKm6cUHLGevSYvZJnkTpsZJRz_uLxxQnNSKLnYAeATTYL9NJtidgZn27OeEGE_raY0R0pTI7FLuLvSfixhr_TsFs33acA_CasxmRCkR0GhRjhIXwtM-nHwjRmV1Ledp_maKBExw6mWyFqZh1-KnpXrXqLYxNSW1zeVRFZwxEKaE0ytZwn8OU6MR6g3ArgPGDnMHKAwBIs0gBJKmQv2sS4I8bEdjigw5_g4Jgqv9YgLaMGyfU9OC5MmDiMJBvFOCdARv9CFtsHGq-2JvTqps-uNkkVHb4hQW2wjR73M9cPHrDin19nNk_DMUCqQbjf9Ea6g-8nuOFSl4OAZ_0-Yyl--ZDv9yP1tuMHmcGkEBSDh3v875sjxmPRhCETCt6hV8ZrtmtiXxDe-NbLFUvoaf83BviAtAaJTlnXl2F-DgPApUPyWYcL26yPzswJ4fU9a7oCBhizRJeSPhhesjkrCLkwRlDFtmgYHm_YwwRp5aq2ACa7sF0Gqm0csvmSMVCJR7noMysz0TX9M7QbLgk-BHYlHcGy1I6qtIUnR7ltMfB9k42QxPav8XNzz9XLOWUeTa6RnPRmO-_=w1440-h650-no" width="600"></center>

- notebook에서 첫줄에 `%jdbc`입력 후 테스트 쿼리를 날려보면 잘 되는걸 확인 할 수 있다.
