---
title: "스트라티스 qt월렛 리눅스에서 빌드하기"
date: 2017-05-09
metaAlignment: center
thumbnailImagePosition: left
thumbnailImage: //bittrexblobstorage.blob.core.windows.net/public/74083684-8817-4e0e-9107-7fcc129f4e73.png
coverImage: //themerkle.com/wp-content/uploads/2016/06/Stratis.jpg
coverMeta: out
categories:
- blockchain
- cryptocurency
tags:
- stratis
- blockchain
- cryptocurrency
- wallet
- linux
---


코인을 거래소에 두는건 위험합니다. 해커들이 호시탐탐 노리고있으며 거래소가 먹튀할 확률도 있습니다.
소중한 스트라티스를 개인지갑에 소중히 보관하고자 하는 취지에서 이번글을 작성해 봅니다.
<!--more-->

<!-- toc -->
스트라티스 월렛은 공식 [github][stratisx]에서 다운로드 받을 수 있습니다.
윈도우와 맥의 경우 바로 설치해서 사용할 수 있도록 제공되지만 linux의 경우엔 직접 빌드해야합니다. GUI지갑인 stratis-qt가 제공되긴 하지만 직접 빌드하려면 까다로운게 사실입니다.

이 글에선 리눅스 터미널에서 따라만해도 stratis-qt가 쉽게 빌드가 가능하도록 작성해보겠습니다.

> 이 글은 스트라티스 커뮤니티 olcko의 글 중 일부를 허락 받고 참조하였습니다.



# stratisd 컴파일

## 필요 소프트웨어 설치
-   스트라티스 월렛을 빌드하기 전 필요한 소프트웨어들을 설치해 줍니다.
-   약 20분 정도 소요됩니다.

```bash
sudo apt-get install -f build-essential git g++ libtool make unzip wget libboost-all-dev libssl-dev libdb++-dev libdb5.3++-dev libdb5.3-dev libminiupnpc-dev libqrencode-dev -y
```

## 소스코드 다운로드
-   소스코드 다운로드 후 다운받은 폴더로 이동해주세요.
```bash
git clone https://github.com/stratisproject/stratisX.git
cd stratisX
```

##   빌드
-   최신버전인지 확인 `git checkout master`
-   `cd src && make -f makefile.unix`
-   터미널에 무수한 명령어들이 뜨며 빌드가 시작됩니다. 30-40분 정도 소요됩니다.


![](https://lh3.googleusercontent.com/6ZAtwnhZEu6co2yF6GuMwQioSRKTTFpmL1oHp1s_hbzDJUihxLECofgBt9RCI8vswgvNCrkvMBzmX9nG4xDm5RUX8zjWihAuVsWcPOc2YqAVdLkDrlTLDSKpc3Rnk9CcfldI0zuM7x5wttKX6iyMKAQQfrqme3hy0RmKofvGoXhl__ZfTONUF9oh7LWiFvzoauOecUpBe5qm4epveXk5x9PY33M-ITMZmhnWLTgAPmZaYMBua99P39uKqGLMS_a2kHxmeICCvSrnmzoaeA3-C5sZmL5VDu_DWELyLS0-tNP5fZ0BNidNIdlJ1xHJk8mL2GRVJnCO-xLZqhT_OnMx6RBqPiO7IgO9pi-7jTG8V2mBagDzImmdH6JRX72hVH8O9kRHsAx6KlrhRN0PKKfA8eEcwcyerp3EBxNVuLdqJ8vLQyCmWZY1-Ac28pPjTay39nrFo8ObHlQgPYb7Oj3kZsKT42Dw2ep4wpRV3Hds5N0zocV7E7oUF5T0YYj5zpGTVR6dVOq8idFRlyI3J1V0aJsagxOL47_3thtfrLaX9uAUtr5U1GIWeRbcxynzrIWfZAMu-94buSib69R7s7DKyW45amj_OS_HaKianvTSMvmJN9rZtZXV_TJZqUub5P4NFpTiNRv4C33cLAocYHYYQ-a5ytK12jN15SY2=w1306-h692-no)

## 필요없는 데이터 잘라내기
-   `strip stratisd`
-   빌드중에 생성된 디버깅정보들을 삭제해주는 과정입니다.

## 전역에서 실행 가능케하기
-   `sudo mv stratisd /usr/bin`
-   어떤 경로에서라도 stratisd 명령어가 실행되도록 해줍니다


# Stratis-qt 컴파일

## 필요 소프트웨어 설치
```bash
sudo apt-get install -f build-essential autoconf automake git g++ libtool make unzip wget qt5-default qt5-qmake qtbase5-dev qtbase5-dev-tools libqt5webkit5 libqt5webkit5-dev libqt5qml5 libqt5quickwidgets5 qml-module-qt-labs-settings qtdeclarative5-dev-tools qttools5-dev-tools libboost-all-dev libssl-dev libdb++-dev libdb5.3++-dev libdb5.3-dev libminiupnpc-dev libqrencode-dev libprotobuf-dev
```

## 빌드&필요없는 데이터 잘라내기
-   `cd stratisX;qmake;make;strip stratis-qt`

## 전역에서 실행 가능케 하기
-   `sudo cp stratis-qt /usr/local/bin`



# 끝
-   터미널에서 `stratis-qt`를 입력하시면 잘 실행되는걸 확인할 수 있습니다.
-   안전한 코인투자 되세요

![](https://lh3.googleusercontent.com/jQlabHSaTrvnl0TyY7IgssePldZfWvLQBIiVxe82fbaF01d8FHPC_ZQ874i_zS58A_R7vJxhWsZIR2-aU9bJsddT5XuaKzZ37tOdfzaBiwmJY8UkkKMBEmTQ5mk5z7qivDTSPs6qdYpSKVcqrVgD-nTU4JFNRJCwYq2MmkXcaPuhYxK__tkUdEUY44_e5dJqVUaZ5dCj2c1sbjMS4oDz5xFFLFAKcLKgLuQjUYOJiXArOeBIqpYi0UI0yILNxEHTBxpnRL8wl2YOrJFImo3m5NIMZzfJKhKfjqKZVC9saP3SOK9WhfOUMsoUIgGf_zCxAuMYk9xV-ZSD_WW_kwrQESC__lXqi8WY9UyYqLYlO5_5nx65wHamLsjGncTImzLBx0wHCW8s4alRlUpKt2KSMohZDonXFM5IwsSAJ6_8lGzAMVLyvQYQmcjlxlqChPNbsrV4xpaKCxtBkD4m616vrzVKh1-mfHEvv3zNukm-YZTS0er_RYw__awmmkTdAb0Wxmm53LSsOV8I1BTicF4-xQll0sxyMkNLiLSpYpogKjtbx6UR74EYBNrO-u8zPgEDVLtzUur-x3wcAmNoCygi8xTxy5Lz5Mn5Nv3Avf-P1l57vGk9Z7JAMxB9Vd6vgRy8MGV1_1fy8TAjGOvdEZQofKdMReurkQXk4oOa=w1440-h898-no)



[stratisx]: https://github.com/stratisproject/stratisX/releases
