---
title: "p3"
date: 2017-11-09T17:42:06+09:00
draft: true
---




layout: post
title: "[파이썬 프로그래밍 무작정 따라하기] #3-제어문 part 1. 조건문"
description: "#3-Jupyter 너로 정했다 + 기본 자료형"
tags: [steem, python, programming, howto]
image:
  feature: "Python_logo.png"


- 비교연산자(Comparison operator)
- 조건문
    - if문
    - if-else문
    - if-else if문
    - 중첩 if문
- 마무리

<center>
<img src="http://www.sanghun.xyz/images/python_rectangle.jpg">
</center>

 안녕하세요 **Kaang**입니다.
 안녕하세요. 프로그램은 항상 동일하게 실행되는건 아닙니다. 상황에 따라 다르게 실행되는데요, 이를 제어문을 통해 제어합니다. 제어문에는 조건문과 반복문이 있습니다. 이번 글에선 조건문을 학습 해보겠습니다. 저번 글과 마찬가지로 Jupyter를 이용해 학습을 진행하겠습니다. 이 글에 사용된 코드를 작성한 [Jupyter notebook](https://github.com/sanghkaang/steemit-python-lecture/blob/master/%233-%EC%A0%9C%EC%96%B4%EB%AC%B8%20part%201.%20%EC%A1%B0%EA%B1%B4%EB%AC%B8/main.ipynb)을 이미 github에 업로드 해두었습니다. 링크를 클릭하시면 확인할 수 있습니다. 따라하시면서 학습해 주세요.

# 비교연산자

조건문을 공부하기 전에 비교연산자를 학습해야 합니다. 쉬워요.

| 비교연산자|설명|
|:-:|:-:|
|a>b|a가 b보다 크다(초과))|
|a>=b|a가 b보다 크거나 같다(이상)|
|a<b|a가 b보다 작다(미만)|
|a<=b|a가 b보다 작다(이하)|
|a==b|a와 b가 같다|
|a!=b|a와 b가 같지 않다.|

```python
a = 2
b = 1

print(a>b)  #True
print(a>=b) #True
print(a<b)  #False
print(a<=b) #False
print(a==b) #False
print(a!=b) #True
```

이런식으로 a와 b에 값을 대입한 후 비교를 할 수도 있지만 input()함수를 이용하여 사용자로부터 직접 값을 입력받을수도 있습니다.

```python
a = input()
print(a)
```
<center>
<img src="http://www.sanghun.xyz/images/python-3/pic-1.png" width="250">
</center>

input함수에 입력 메세지를 주고 싶다면 아래처럼 하면 됩니다.

```python
a = input("a를 입력하세요:")
print(a)
```

<center>
<img src="http://www.sanghun.xyz/images/python-3/pic-2.png" width="250">
</center>

위의 예제를 입력값을 통해 다시 작성해 봅시다.

```python
a = input("a를 입력하세요:")
b = input("b를 입력하세요:")

print(a>b)
print(a>=b)
print(a<b)
print(a<=b)
print(a==b)
print(a!=b)
```

# 조건문

ATM기에서 돈을 출금하는 상황을 생각해봅시다. 돈을 출금하려 했을때, 잔고를 초과하는 금액은 출금할 수 없습니다. 오직 잔고 이하의 금액만 출금할 수 있다. `'잔고'와 '출금액'의 비교`를 통해 `출금``이라는 행위가 결정됩니다.

우리가 사용하는 ATM기도 조건문으로 프로그래밍 되어있습니다. 간단한 ATM기 프로그래밍을 해보겠습니다.

## if문

if문의 기본 문법은 아래와 같습니다. 조건이 참(True)면 명령어가 실행되는 구조입니다.
```python
if 조건:
  명령어
```
if문의 문법에서 주의해야할 사항은 두가지가 있습니다.

1. 조건 뒤에 콜론(:)을 무조건 적어줘야 합니다.
2. if문을 작성한 뒤 아래의 `명령어` 부분은 들여쓰기 해야 합니다. 들여쓰기는 주로 `Tab`키를 사용하거나 `space` 4번을 사용합니다. 이 들여쓰기 부분을 정확하게 맞춰야 에러 없이 코드가 실행됩니다.

잔고(balance)가 10000원이고 출금하고자 하는 금액(withdrawl)이 2000원이라고 해봅시다.

```python
balance = 10000  # 잔고 10000원
#withdrawl = input("출금액을 입력하세요:") 를 사용한다면 에러가 발생합니다.
#input()함수를 다시 int()함수로 감싸서 형변환 해줘야 합니다.
withdrawl = int(input("출금액을 입력하세요:")) # 출금액 2000원 입력

if balance >= withdrawl:  #조건 1
    print(str(withdrawl)+"원을 출금합니다.")
    balance = balance - withdrawl #출금액만큼 잔고에서 빼주기
    #들여쓰기가 끝났습니다. 이 이후부터는 if문 밖입니다.

print("잔고: " + str(balance))
```

> 1.input()은 모든 입력을 str형태로 받습니다. 그렇기에 if절에서 balance(정수형 데이터)와 withdrawl(문자열 데이터)의 비교를 위해 withdrawl을 int()함수를 통해 정수로 형변환 해 줍니다.

> 2.print함수 내에서 withdrawl을 str()로 감싼건 다시 한번 형변환을 위해서입니다. withdrawl은 정수(int)이므로 뒷 부분의 '을 출금합니다.'와의 문자열 접합(+)을 위해 str()함수를 통해 문자열(string)로 형변환 해주었습니다.

`2000원을 출금합니다.`라는 문장이 출력됨을 확인할 수 있습니다. 위 문장을 해석해보겠습니다. 먼저 if뒤의 조건을 확인합니다. `balance >= withdrawl` 이 조건이 참이면 아래 문장이 실행되고 거짓이면 실행되지 않습니다. 10000 >= 2000, 즉 10000은 2000보다 크니까 아래 문장은 실행됩니다. 그럼 만약 잔고보다 큰 금액을 출금하려고 한다면 어떻게 될까요? 출금액으로 잔고보다 큰 20000원을 입력해 봅시다.

```python
balance = 10000  # 잔고 10000원
withdrawl = int(input("출금액을 입력하세요:")) # 출금액 20000원 입력

if balance >= withdrawl:
    print(str(withdrawl)+"원을 출금합니다.")
    balance = balance - withdrawl #출금액만큼 잔고에서 빼주기
    #들여쓰기가 끝났습니다. 이 이후부터는 if문 밖입니다.

print("잔고: " + str(balance))
```

이 코드를 실행한다면 "~원을 출금합니다." 출력을 볼 수 없습니다. balance >= withdrawl의 결과가 False이기에 아래의 print문이 실행 되지 않는거죠. 그렇다면 if문이 실행되지 않을 때도 뭔가를 출력해서 사용자에게 정확한 상황을 알려 주면 어떨까요?

## if-else문

단일 if문은 조건이 참인 경우만을 커버합니다. 조건이 거짓일때도 실행하고자 하는 문장이 있을 경우 if-else문을 사용합니다.

```python
if 조건:
  명령어 # 조건이 참일때 실행
else:
  명령어 # 조건이 거짓일때 실행
```

if문과 마찬가지로 else뒤에 콜론(:)을 붙여줘야 하고 명령어 부분은 들여쓰기를 해줘야 합니다.

```python
balance = 10000  # 잔고 10000원
withdrawl = int(input("출금액을 입력하세요:")) # 입력한 금액에 따라 결과가 달라짐

if balance >= withdrawl:
    print(str(withdrawl)+"을 출금합니다.")
    balance = balance - withdrawl
else:
    print("잔고가 부족합니다.")

print("잔고: " + str(balance))
```
입력한 금액에 따라 결과가 달라짐을 확인할 수 있습니다. 10000원 이상 금액을 입력하면 if부분이 실행되고 10000원 미만 금액을 입력한다면 else부분이 실행됩니다.

## if-else if문

여러가지 조건을 사용하고 싶을때 사용하는 구문입니다.

```python
if 조건1:
  명령어 # 조건1이 참일때 실행
elif 조건2:
  명령어 # 조건1이 거짓이고 조건2가 참일때
else:
  명령어 # 모든 조건이 거짓일때 실행
```

elif는 else if의 줄임말입니다. else부분은 필요 없다면 작성하지 않아도 됩니다.(생략 가능합니다.)

```python
balance = 10000  # 잔고 10000원
withdrawl = int(input("출금액을 입력하세요:")) # 입력한 금액에 따라 결과가 달라짐

if balance > withdrawl:
    print(str(withdrawl)+"원 을 출금합니다.")
    balance = balance - withdrawl
elif balance == withdrawl:
    print("전액 출금합니다.")
    balance = balance - withdrawl
else:
    print("잔고가 부족합니다.")

print("잔고: " + str(balance))
```
조건 2를 추가하면서 조건 1을 >=(이상)에서 >(초과)로 수정했습니다.
입력값이 10000보다 작을 경우 if절이, 10000일 경우 elif절이, 10000보다 클 경우 else절이 실행되는걸 확인할 수 있습니다. 원하는 만큼 조건을 추가 할 수 있습니다. 원하는 만큼 elif절을 추가 할 수 있단 뜻이죠.

```python
balance = 10000  # 잔고 10000원
withdrawl = int(input("출금액을 입력하세요:")) # 입력한 금액에 따라 결과가 달라짐

if balance > withdrawl:     # 조건 1
    print(str(withdrawl)+"원 을 출금합니다.")
    balance = balance - withdrawl
elif balance == withdrawl:  # 조건 2
    print("전액 출금합니다.")
    balance = balance - withdrawl
elif 0 == withdrawl:        # 조건 3
    print("0원을 출금할 수 없습니다.")
else:
    print("잔고가 부족합니다.")

print("잔고: " + str(balance))
```
0을 입력하면 "0원을 출금할 수 없습니다."가 출력되겠죠? 그러나 예상과는 달리 "0원을 출금합니다."라는 문장이 출력됩니다. if문은 위에서부터 순서대로 실행됩니다. 만약 위쪽의 조건에 해당된다면 아래의 elif나 else는 무시되는 구조이죠. 예시를 들어 보겠습니다.

```python
a=100

if a>1:
  print('조건 1 참')  # 출력됨
elif a>2:
  print('조건 2 참')  # 무시됨
```

예시를 간단히 하기 위해 else문은 생략했습니다. a는 100이므로 조건1(a>100)과 조건2(a>2) 둘 다 참입니다. 이 경우 둘 다 실행되는 것이 아니라 가장 위에 있는 참인 조건에 해당하는 부분만 실행이 됩니다. 위의 ATM예시를 보자면, 출금액이 0이라서 `조건3`이 참이긴 하지만 더 위쪽의 `조건1`이 참이라 이 부분이 실행되고 나머지 부분은 무시되는 것이죠. 그렇기에

1. 조건을 설정할땐 조건들의 실행 순서를 고려해야 합니다.
2. 또는 조건들끼리 겹치는 부분이 없도록 배타적으로 설정하는겁니다. 조건 1~3이 주어진다면 단 하나만 참이 되도록 말이죠.


조건 순서를 다시 고려하며 겹치지 않도록 코드를 짜 보겠습니다.

```python
balance = 10000  # 잔고 10000원
withdrawl = int(input("출금액을 입력하세요:")) # 입력한 금액에 따라 결과가 달라짐

if 0 > withdrawl:  #0보다 작은 경우
    print("0원보다 큰 금액만 입력 가능합니다.")
elif 0 == withdrawl:  #0인 경우
    print("0원을 출금할 수 없습니다.")
elif balance > withdrawl:  #0보다 큰 경우
    print(str(withdrawl)+"원을 출금합니다.")
    balance = balance - withdrawl
elif balance == withdrawl:
    print("전액 출금합니다.")
    balance = balance - withdrawl
else:
    print("잔고가 부족합니다.")

print("잔고: " + str(balance))
```

입력값이
- 0보다 작은 경우
- 0인 경우
- 0보다 큰경우
로 겹치는 경우가 없도록 코드를 작성했습니다.

## 중첩 if문

if문 내부에 또 if문을 작성할 수 있습니다. 출금이 일어나는 경우에 비밀번호를 물어보는 코드를 추가해 보겠습니다. 비밀번호는 1111이라고 가정하겠습니다.

```python
balance = 10000  # 잔고 10000원
withdrawl = int(input("출금액을 입력하세요:")) # 입력한 금액에 따라 결과가 달라짐

#if문 시작
if 0 > withdrawl:  #조건 1
    print("0원보다 큰 금액만 입력 가능합니다.")
elif 0 == withdrawl:  #조건 2
    print("0원을 출금할 수 없습니다.")
elif balance >= withdrawl: #조건 3
    password = input("비밀번호를 입력하세요:")
    # 중첩 if문 시작
    if password == "1111": #1111이 아니라 "1111"과 비교
        print(str(withdrawl)+"원을 출금합니다.")
        balance = balance - withdrawl
    else:
        print("비밀번호가 일치하지 않습니다.")
    # 중첩 if문 끝
else:
    print("잔고가 부족합니다.")
#if문 끝

print("잔고: " + str(balance))
```

```
비밀번호를 맞게 입력했을 경우:
    출금액을 입력하세요:1000
    비밀번호를 입력하세요:1111
    1000원을 출금합니다.
    잔고: 9000
비밀번호를 틀리게 입력했을 경우:
    출금액을 입력하세요:1000
    비밀번호를 입력하세요:2222
    비밀번호가 일치하지 않습니다.
    잔고: 10000
```

`조건 3`내부에 비밀번호를 체크하는 if-else문이 삽입된 형태입니다. 비밀번호를 입력받을떄 input()함수가 키보드입력을 문자열로 읽어들이기에 password를 비교할때 1111(정수형)이 아닌 "1111"(문자열)과 비교를 진행해야 합니다.

# 마무리
어떠셨나요? 조건문과 비교연산자를 학습했습니다. 그리고 사용자로부터 입력을 받는 input()함수도 학습했군요. 이정도만 해도 어느정도 형태를 갖춘 프로그램을 만들수 있습니다. 다음 글에선 또다른 제어문인 반복문에 대해 알아보겠습니다. 이해가 안가거나 궁금하신 부분 있으면 언제든 댓글남겨주세요. 다음 글로 찾아뵙겠습니다. 감사합니다!
