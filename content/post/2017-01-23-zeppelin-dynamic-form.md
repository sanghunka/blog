---
title: "apache zeppelin의 dynamic form 정리"
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

dynamic form을 이용하면 충분히 쓸만한 custom dashboard를 만들 수 있다.

<!--more-->
<!--toc-->
> 참조. [http://zeppelin.apache.org/docs/0.6.2/manual/dynamicform.html](http://zeppelin.apache.org/docs/0.6.2/manual/dynamicform.html)



# 1. Text input form

<center><img src="https://lh3.googleusercontent.com/k9Cm69foCtTOX-YuzmtBoPoFF9dXe70bDvlAd48ChjbCVAOXEsN6inpR_rVUG2OHmboJ4A6xcRmFaTTP5qYoxLiMlnaeI_28rWDJqXksMzB2k_zgH7fIPVZbs383PT4osFoLyaml3nxweA9DBm3xmLDv_4IVA5owJ0C3zwV3ObarUvr_KROZkuEb_v0tvBvd13JQ_FlxvMxXFuB9rU9fGUk7mzCN6Q_Z2ofwblV_C6_NBI0u8BDPJF0nftT0RM4mEidbMHruxFzPvUt8HnQnqAJ4E1zrmc_5esu3huGoSoyxED9tWNi9-xmFnM1OSS8-GfhDHmS2_zxZF2n6_4kFoYgkqTmbCPCCHWctUmP3SGGur7cnA5cMshyHfbRQ2p7lvtZl06_3z64lUUZugYhAlGXxudnnzALB--SDr3OejsqK5yy22Sl2QYMefK4DTBD7zrKTNN-B8wSdhx5mm8rf2sslubJW4cHzeUTXsLYNzWMdeEgHzfP2TbnplDnBzrIdeN22tPNx6FblE_t1NpbTVj_gU2ClhZDXq0bUBICn3Y4gXZ-To7_zJMMGDy0Yw-R6aPy8I8yBpga5EnUC4Fk-3qAk0yfhvFNCgoWWNNt07wE-9AZ09gNbQ5f-SaTOKg0nBbnBQ87ytRTARa3w9hTFk6vvtJviLKdIGkCv=w1392-h882-no" width="600"></center>

&nbsp;

- 기본 형태: `${formName}`
- default value 지정 형태: `${formName=defaultValue}`
  - 기본값을 지정하는 경우 첫 실행시 default value로 input이 들어가서 실행된다.

# 2. Select form

<center><img src="https://lh3.googleusercontent.com/BlLSr5xqqh5pR01PbeU-HM1gZRfAEaQD_qJ8dk23BYCKxfzHCCbAFdfkRM8vOMnUPdAtcqcpnixtKnU-gtwodQudrQAqKFXxvGnWOfwywA8kQYAyWs8LrppXU8L1FWfREBAj6xO5heqAhJKYqSV_g62k8g3pweX9Wn-Gqxm56VnR8BBNQ0DKlyCkfYm-8OpT9zF34rA82zA5LCuQBnUQHGWtxZjchuEo2IbN3NoV70yJQ8vwj-l2m5gjo9i0CyhksjxIzqnAMWM26BF_GOnC5K1y3FbrIbUx9eGkwQWps-wgdh67_q3w8NO0r-plPb4PCwKPiw1saAdu2rBgHG60BcCrdpCkrlFd9v4SezsbKuNaxEdGVxnF4yriNvebGMCW_AF3UNpXu8UgWnBUhrXy8HR77kyZ72CVNSRJO0zkJeT2uODBibjUbiC5lTAqumnciD8-Y4rLNiefmPB5aSCUvMmIiztLcBQRrzivIwaAJV6cNll6xLn7Fb0AoCgbrkAhHTxE23l7ZCkD_PimEhpAa0HzSe1UPUaEWiUlV6HWE_z8hJagHti0_YeczB7xcCO2jVNeHBx7VS079teKUrxPG3m4kC0OSpz5iVwgxXLH_EDQfkmbHPJLT7K-oU8U-wIxxtcaLGVeqnMS2_CwLrorxOEwmnZe5ZMjDX1x=w1390-h1186-no" width="600"></center>

&nbsp;

- 기본 형태: `${formName=,option1|option2...}`
- default value 지정 형태: `${formName=defaultValue,option1|option2...}`
- 선택값과 출력값을 다르게 지정한 형태: `${formName=defaultValue,option1(DisplayName)|option2(DisplayName)...}`

# 3. Checkbox form

- 쿼리 사용할 때 유용하다.
- in 뒤의 () 안에 string 값들이 들어갈 땐 `'`기호가 필요한데 이럴땐 delimiter를 `','` 형태로 주고 전체 dynamic form을 `'`로 감싸주면 된다.

<center><img src="https://lh3.googleusercontent.com/eowu2TOGXNMwBZt51h48KGdJLiXverw6IpuQ70GLg2RjtdZKP-WIy6FaEsg5zrr0xs8GCIeXtYhVjpdUS6NF9hakYKPGk2nXjFKyYkjzmX19LoVFHYgFqOdjry1lPwMa1oumuU4MyaMovNwrq-k6QdiogShHyOehJaC15x584N7MJbSeV54x0I_XqSJa1cOWinMWKLYRGGQSFSCfvR4SSOat6MMC9pDs6HZqenlkkGqw9UInC3rhWsASf2lmzWlMVdkeXXUs5JMyRLJcYe4FlG4ZgOCazUbBHqE4TO5gdy0EYf1Todcw120nuesg9PoVyJOuBMFzIMe4At5BrtpGWdXfKPkdLH9BAkS2sqy4366qaadmhjN7KjRQU0OjcjHsJtRJhirLxpXzzerBqXVBRnjqqOwMjyrFFBQurCfSuaufFbBW2rasJKOB4WmE2o9rH5CVRM3a_2OGn4QgW944WCIlqa7nlzFf26gYsCX9gGU5TaHy60eMGcVHP9-egOJm-CutuISYhXYLWAokurONogh2QOYLh47GIfeX8QM-HiT8dR9BZaEiVBGMphN01IsAPFsi9PUXU7lg7VOAtAHKxnbV4CePurY7TxlG5g8pAyCjFKgY_J1DnEyJr0hm1GQVmOLe4b1jCRaKmwGQL2dNytKO8yE2kWKtSPb4=w1374-h754-no" width="600"></center>
&nbsp;
<center><img src="https://lh3.googleusercontent.com/vlOOdboPDi3bwxq6CfhLuxjmdsNcutgRsUVh3cvBHClwaOuzhmBlAetM8fTVN68RbxwAdqecczR9SPa0-_er-LgnXdxzkDprrucZMmGoVzqf1pJyLa7rYg_-GMocl1qHJj6h4n5FdG3pvTy3_rBznRYQUbWf0WWWH-vn4lWEjCUrXvKIP104WnEm4z9ApTd5UAJxSnEuHKlQuc6lt-xqwUY9g0fWBA0x-rSbD4jUKKFH075MjbOkUVas9OLN5LBX66XT1-NBJlc55zlg32moSAU0ARVUOZ924qfH-MRNuyybu9f2lCQRfW2FmL7uHeYpkKyqZ3aKVj9RRKsnEPtQ5weEkV21jZr1N5jdYVt8VFqCX0v4Usr2dE6rlRqlk9RRsMuNyu5gaEvIP3biVybhaqAcU2MQE4F95tT9-OhY7vSxmz8WQtFBYcWUtvjBeeFWfWuMpJ2f8JFC6BDpGgMSaqcPxwOuqWFcKtGiGENZ0j6kW8OV1HiJtXpTSsNALy0Fx5reuxpdQII1yk2nO2sz4wdnZAkiR6QiKiIJXRFgKjM786nSjCHsXJvV09R90uCwh7BRjjzaDHzQBtQbM5abVg2xfneewR7tCbuCkVvxBgdS-fWJaC9NzfpqAKIUHmym0WLZ3vUAr7AcMEH8X7lplsguHk8e7g-8kQ_R=w1406-h782-no" width="600"></center>
&nbsp;

<center><img src="https://lh3.googleusercontent.com/AQiV2fgpu01dblb8VIjiyZKId-UgX20AIGTdRF2jg29-XFHx4oql25X-lS_lJkt8iBS7_9LAc1aVGMY5ccpOxXTLVuRR7oMP05hM-TOG6ycTQKPiR57L7a5BLZP4tPNz0RjKPzNc5cchNv4sUgrZGIKZfYeJxoIWM5NhIOQysIJ6tu64QAKcAmQXEK_Lcf6WQaScs7ezf99DDWTjRRcncBjyhco-zBGH27aPj21QAsxcyKaYGK-dscS-6Hq_zwF1IDczqmI1if2ZruqNzkRqxEqf9jv8OQTg-OtaK5OgNgZzjQ2wyniAUzwJTw8WooumBDm8yia4Tia5WrpOEhT83DgXDhyhsJaFNoBEj94KJkMQrCawkaQF0fS5YF5GAJa3bOPPrtd7Z5vdCCdiswm6uAIfOiOddzZVN4BpMt22aw8YCWwK2UjCAGfX_LYQBE-nMtb1cnkfYeXus--oQZqhjKa2P24XfqAM9bvkz0XdDrcw86y_kL5tueHQ4jEQQrMJOFSilbbKB7tShhFbjXbOa5YQpwgFqgL5MqlXxKXMJu78sHFAeNJNSHGFMcRu-OeLhTX6t9whAFXld9aEtqJg3LZWW33at03u6z-nPIQDpoWcFmvkFNN9j90QV9Jn_z7ln549_H8oGE6noykm2psEF_VlvpR8Qw1dcULp=w1376-h1238-no" width="600"></center>

&nbsp;

- 기본 형태: `${checkbox:formName=,option1|option2...}`
- default value 지정 형태: `${checkbox:formName=defaultValue1|defaultValue2...,option1|option2...}`
- delimiter 지정 형태: `${checkbox(delimiter):formName=,option1|option2...}`
