---
title: "Raspberrypi stretch with desktop에서 스트라티스 qt월렛 빌드하기"
date: 2017-12-25
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

Guide대로 Jessie에서 하면 편합니다만 Stretch에서 하시길 원하신다면

<!--more-->

<!--toc-->


RaspberryPi Jessie에선 빌드가 잘 되는데 Stretch에서 빌드를 시도한다면 `cd src && make -f makefile.unix`에서 에러가 난다.

```sh
cd src && make -f makefile.unix
g++ -c -O2  -pthread -Wall -Wextra -Wno-ignored-qualifiers -Wformat -Wformat-security -Wno-unused-parameter -g -DBOOST_SPIRIT_THREADSAFE -I/home/pi/stratisX/src -I/home/pi/stratisX/src/obj -DUSE_UPNP=0 -DENABLE_WALLET -I/home/pi/stratisX/src/leveldb/include -I/home/pi/stratisX/src/leveldb/helpers -DHAVE_BUILD_INFO -fno-stack-protector -fstack-protector-all -Wstack-protector -D_FORTIFY_SOURCE=2  -MMD -MF obj/alert.d -o obj/alert.o alert.cpp
In file included from chainparams.h:9:0,
                from alert.cpp:7:
bignum.h:57:24: error: invalid use of incomplete type ‘BIGNUM {aka struct bignum_st}’
class CBigNum : public BIGNUM
                       ^~~~~~
In file included from /usr/include/openssl/crypto.h:31:0,
                from allocators.h:12,
                from serialize.h:22,
                from alert.h:9,
                from alert.cpp:5:
/usr/include/openssl/ossl_typ.h:80:16: note: forward declaration of ‘BIGNUM {aka struct bignum_st}’
typedef struct bignum_st BIGNUM;
               ^~~~~~~~~
In file included from chainparams.h:9:0,
                from alert.cpp:7:
bignum.h: In constructor ‘CBigNum::CBigNum()’:
bignum.h:62:21: error: ‘BN_init’ was not declared in this scope
        BN_init(this);
                    ^
bignum.h: In copy constructor ‘CBigNum::CBigNum(const CBigNum&)’:
bignum.h:67:21: error: ‘BN_init’ was not declared in this scope
        BN_init(this);
                    ^
bignum.h:68:30: error: cannot convert ‘CBigNum*’ to ‘BIGNUM* {aka bignum_st*}’ for argument ‘1’ to ‘BIGNUM* BN_copy(BIGNUM*, const BIGNUM*)’
        if (!BN_copy(this, &b))
                             ^
bignum.h:70:31: error: cannot convert ‘CBigNum*’ to ‘BIGNUM* {aka bignum_st*}’ for argument ‘1’ to ‘void BN_clear_free(BIGNUM*)’
            BN_clear_free(this);
.
.
.
.
.
.
bignum.h: In function ‘bool operator<(const CBigNum&, const CBigNum&)’:
bignum.h:719:83: error: cannot convert ‘const CBigNum*’ to ‘const BIGNUM* {aka const bignum_st*}’ for argument ‘1’ to ‘int BN_cmp(const BIGNUM*, const BIGNUM*)’
inline bool operator<(const CBigNum& a, const CBigNum& b)  { return (BN_cmp(&a, &b) < 0); }
                                                                                  ^
bignum.h: In function ‘bool operator>(const CBigNum&, const CBigNum&)’:
bignum.h:720:83: error: cannot convert ‘const CBigNum*’ to ‘const BIGNUM* {aka const bignum_st*}’ for argument ‘1’ to ‘int BN_cmp(const BIGNUM*, const BIGNUM*)’
inline bool operator>(const CBigNum& a, const CBigNum& b)  { return (BN_cmp(&a, &b) > 0); }
                                                                                  ^
makefile.unix:193: recipe for target 'obj/alert.o' failed
make: *** [obj/alert.o] Error 1
```

대략 이런 에러메세지...
이에 대한 해결책을 소개하겠다.

1. 현재 설치되어있는 libssl-dev ( 1.1.0f-3 ) 삭제
`sudo apt-get remove libssl-dev`

2. `sudo nano /etc/apt/sources.list`입력하면 Repository list가 나오는데 여기서 "stretch"를 "jessie"로 변경. 

3. jessie 패키지를 받기 위해 `sudo apt-get update` 실행

4. `sudo apt-get install libssl-dev` 실행

5. `cd src && make -f makefile.unix`

6. 완료되면 `sudo apt-mark hold libssl-dev` 실행. 추후 업그레이드 되는걸 막아준다.

7. 2번에서 했던 sources.list로 돌아가 "jessie"를 "stretch"로 바꿔준다.

8. `sudo apt-get update` `sudo apt-get upgrade`

9. 기존 가이드대로 계속 진행한다.


# 참조
- https://github.com/stratisproject/stratisX/issues/33