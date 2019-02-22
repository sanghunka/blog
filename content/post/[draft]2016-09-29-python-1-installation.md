---
title: "p1"
date: 2017-11-09T17:42:06+09:00
draft: true
---




layout: post
title: "[파이썬 프로그래밍 무작정 따라하기] #1-설치"
description: "#1-파이썬 설치"
tags: [steem, python, programming]
image:
  feature: "Python_logo.png"


<center>
<img src="../images/python_rectangle.jpg">
</center>

 안녕하세요 **Kaang**입니다. 
 
 이번 글에선 파이썬 개발환경을 셋팅하는 방법을 알아보겠습니다. 윈도우 환경을 기본으로 설명하겠습니다.(mac과 linux에는 기본적으로 파이썬이 설치되어 있습니다. 터미널에서 **python**을 입력해 보세요!)
 
### 다운로드

먼저 파이썬 사이트에 접속해 파이썬 인스톨러를 다운받아봅시다.

> [https://www.python.org/downloads/](https://www.python.org/downloads/)

파이썬은 2버전과 3버전이 있습니다. 특별한 이유가 없다면 `파이썬3`을 다운받는걸 추천드립니다. `파이썬2`는 2020년에 지원이 끊길 예정입니다. 

<center>
<img src="../images/python-1/pic-1.png">
</center>

현재 파이썬3 최신 안정화 버전인 3.5.2버전을 다운받아주세요. 네모부분을 클릭하게 되면 다운로드가 진행됩니다. 만약 **64비트 운영체제**를 사용하고 계신다면 화살표 부분을 클릭해주세요. 

>[본인의 pc가 64비트인지 32비트인지 확인하는 방법](http://support.hp.com/kr-ko/document/c02020927)

그런다음 파이썬 3.5.2 섹션에서 `Windows x86-64 executable installer`를 클릭해 파이썬 64비트 버전 인스톨러를 다운받아 주세요.


<center>
<img src="../images/python-1/pic-2.png">
</center>

32비트나 64비트나 설치과정은 동일합니다. 다운받으신 인스톨러를 실행시켜주세요.

<center>
<img src="../images/python-1/pic-3.png">
</center>
 
아래 `Add Python 3.5 to PATH` 부분을 꼭 체크해주시고 <del>(체크를 하지 않고 진행하면 따로 환경변수를 잡아줘야 합니다. 체크하면 간편해요.)</del><del>`Customize installation`을 선택하면 설치경로나 옵션을 변경 할 수 있지만 권해드리지 않습니다. 복잡해요. 그러므로</del>`Install Now`를 클릭하면 설치가 진행됩니다. 

<center>
<img src="../images/python-1/pic-4.png">
</center>


<center>
<img src="../images/python-1/pic-5.png">
</center>


설치가 완료되었습니다. 

<center>
<img src="../images/python-1/pic-6.png">
</center>


작업표시줄에 Python 3.5 폴더가 생긴걸 확인할 수 있습니다. IDLE나 Python 3.5를 실행해 주세요.

<center>
<img src="../images/python-1/pic-7.png">
</center>

> 위: ILDE, 아래: python shell

IDLE(Integrated Development and Learning Environment)가 기능이 좀 더 많긴 하지만 사실 둘 다 잘 안씁니다 ㅎㅎ 더 좋은게 많거든요. 검은색 배경이 더 마음에 들어서 python shell을 가지고 설명을 진행하도록 하겠습니다.


<center>
<img src="../images/python-1/pic-8.png">
</center>


`copyright`라고 입력해보면 위와 같은 정보가 출력됩니다.

<center>
<img src="../images/python-1/pic-9.png">
</center>


`license()`라고 입력해보면 위와 같은 정보가 출력됩니다. Guido라는 이름이 눈에 띄네요. Guido는 파이썬 언어를 만든 개발자입니다. Guido가 말하길 크리스마스때 연구실은 닫혀있고 심심해서 시작한게 파이선 언어 만들기였다고 합니다.


파이썬은 1989년, 귀도 반 로썸이 '크리스마스에' 연구실은 닫혀있고 심심해서 만들었다.

<center>
<img src="../images/python-1/pic-10.png">
</center>


`help()`를 입력하면 도움말이 출력됩니다.

<center>
<img src="../images/python-1/pic-11.png">
</center>


이 상태에서 `print`를 입력하면 프린트 함수에 대한 설명이 나옵니다. 설명은 복잡하게 보이지만 사용하는건 쉽습니다. 직접 한번 해보죠. 엔터를 치면 help모드에서 빠져나올수 있습니다.

<center>
<img src="../images/python-1/pic-12.png">
</center>


`print(1)`을 입력하면 `1`이 그대로 출력되는걸 볼 수 있습니다.
>`print(1+1)``print(9*8)`이런것도 됩니다.
> 계산기대신 파이썬을! 

<center>
<img src="../images/python-1/pic-13.png">
</center>


자! 이제 프로그래밍을 해본 사람이라면 모두 출력해봤다는 전설의 헬로월드를 출력해보겠습니다. `print('Hello, World!')`를 입력하시면 그 결과로 `Hello, World!`가 출력되는걸 확인할 수 있습니다. 저는 처음에 헬로월드를 출력했을때 가슴속이 벅차오르더라구요.

> 1은 그냥 print() 안에 넣었는데 왜 Hello world는 따옴표로 감싼 다음 print() 안에 넣는거지?

혹시 이런 의문점이 드셨다면 관찰력에 박수쳐드리고 싶습니다. 오늘은 일단 따라했지만 차근차근 그 의문점 해결해가도록 하겠습니다. 

<center>
<img src="../images/python-1/pic-14.png">
</center>


`exit()`을 입력하면 파이썬 쉘이 종료됩니다. 물론 그냥 X를 눌러서 종료해도 됩니다.

<center>
<img src="https://image-proxy.namuwikiusercontent.com/r/http%3A%2F%2F38.media.tumblr.com%2Ftumblr_lz412ce5BK1r2yonh.jpg" width ="500">
</center>





제대로된 강의 첫 글인데 어떠셨나요? 의문점이나 피드백 있으시면 언제든지 댓글로 남겨주세요. 감사합니다 ^^




#### P.S.
[파이썬 홈페이지](https://www.python.org/)에서 노란색 버튼을 클릭하면 파이썬 설치 없이 웹브라우저에서 코딩을 즐기실 수 있습니다!
<center>
<img src="../images/python-1/pic-15.png">
</center>