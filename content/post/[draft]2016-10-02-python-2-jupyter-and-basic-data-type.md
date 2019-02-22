---
title: "p2"
date: 2017-11-09T17:42:06+09:00
draft: true
---



layout: post
title: "[파이썬 프로그래밍 무작정 따라하기] #2-Jupyter 너로 정했다 + 기본 자료형"
description: "#2-Jupyter 너로 정했다 + 기본 자료형"
tags: [steem, python, programming, howto]
image:
  feature: "Python_logo.png"


#목차 

- 개발도구 설치
    - Jupyter 설치하기
        - pip를 통해 설치하기
        - notebook 생성
        - 실행 경로 변경(선택 사항)
- 드디어 코딩 시작
    - 변수(Variable)
    - 주석(Comment)
    - 자료형(Data Type)
        - 숫자(Number)
            - 정수(Integer)
            - 실수(float)
        - 문자열(string)
    - 자료형간 형변환
- 마무리

<center>
<img src="../images/python_rectangle.jpg">
</center>

 안녕하세요 **Kaang**입니다. 이번 글에선 개발도구인 Jupyter와 파이썬의 기본 자료형에 대하여 알아보겠습니다. 코딩이 처음인 경우 글이 어렵고 잘 이해가 되지 않을 수 있습니다. 그럴땐 완벽하게 이해하려고 머리를 혹사시키기 보단 이 글의 제목처럼 무작정 따라하며 먼저 몸으로 체득하는걸 추천드립니다.
 
# 개발도구 설치
-- 
- 저번 글에서 파이썬 쉘이나 ILDE를 통한 간단한 코딩을 진행해 보았습니다. 하지만 이들은 극히 기본적인 기능만을 제공하기에 대부분의 사람들은 저마다의 개발환경을 셋팅합니다. 생산성에서 차이가 나기 때문이죠.

<center>
<img src="https://d3nmt5vlzunoa1.cloudfront.net/pycharm/files/2015/12/PyCharm_400x400_Twitter_logo_white.png">
</center> 
 
- 파이참은 훌륭한 파이썬 개발 도구입니다. 그렇지만 수많은 기능들을 탑재하고 있어 초보자에게 무겁게 다가오는 점도 있습니다. 이 강좌에선 Jupyter를 주 개발환경으로 이용하고자 합니다.

- 먼저 명령 프롬프트를 실행시켜 보겠습니다. `윈도우 10`의 경우 작업표시줄의 검색창에서, `윈도우 7이하`버전에서는 시작 -> 실행에서 `명령 프롬프트` 또는 `cmd`를 검색해 명령 프롬프트를 실행시켜 주세요.
 
<center>
<img src="../images/python-2/pic-1.png">
</center>
 
- 명령 프롬프트에서 'python'을 입력했을때 파이썬 쉘이 실행된다면 '환경변수'가 잘 설정되어 있는 경우입니다. [#1번 글](https://steemit.com/kr/@sanghkaang/1)대로 잘 진행하셨다면 자동으로 '환경변수'가 설정됩니다.

<center>
<img src="../images/python-2/pic-2.png">
</center>
  
- `print('Hello, World!')`를 입력하니 잘 작동하네요. 이제 Jupyter 설치를 위해 파이썬 쉘을 빠져나와 다시 명령 프롬프트로 가봅시다. **`exit()`를 입력하거나 창 종료 후 명령 프롬프트를 다시 실행시켜 주세요.**
 
##Jupyter 설치하기

<center>
<img src="https://avatars3.githubusercontent.com/u/7388996?v=3&s=200">
</center>

- Jupyter는 머신러닝이나 데이터분석 용도로 파이썬을 사용하시는 분들이 주로 사용하는 툴로써 가벼우며 코드를 실행하고 수정하기가 간편합니다. 또한 notebook형태로 파일이 공유가 가능합니다. 강의 노트를 jupyter notebook 형태로도 공유할 예정입니다. 무슨 소리인지 모르시겠다구요? 일단 따라와 주세요!


### pip를 통해 설치하기

- pip는 파이썬 패키지를 관리하는 소프트웨어입니다. 최신버전의 파이썬 인스톨러에 포함되어있어 자동으로 설치가 됩니다. 명령 프롬프트에서 아래의 명령어를 입력해 주세요.

```
pip install jupyter
```
> 만약 에러가 발생한다면 명령어를 '명령 프롬프트'상에서 입력한건지 확인해주세요. 파이썬 쉘에서 입력하면 pip 명령어가 작동하지 않아요. 또한 pip install은 인터넷이 연결된 환경에서만 작동합니다.


<center>
<img src="../images/python-2/pic-3.png">
</center>

- 이게 끝입니다! 간단하죠? 설치가 완료되었다면 명령 프롬프트에 아래의 명령어를 입력해 Jupyter를 실행해봅시다.

```
jupyter notebook
```
- 명령어를 입력하면 웹브라우저를 통해 Jupyter가 실행됩니다. 실행이 안 될 경우 웹브라우저 주소창에 [http://localhost:8888](http://localhost:8888)를 입력하면 됩니다.

### notebook 생성

- Jupyter에선 notebook단위로 코드가 관리됩니다.

<center>
<img src="../images/python-2/pic-4.png">
</center>

- 우측 `New`를 클릭하면 노트북을 만들 수 있습니다.

<center>
<img src="../images/python-2/pic-5.png">
</center>

- Notebooks 아래의 `Python 3`보이시나요? 저같은 경우에는 이것저것 설치해 놔서 다른 선택지들도 보이네요. `Python 3`을 클릭해주세요! 
 
<center>
<img src="../images/python-2/pic-6.png">
</center> 

- 새로운 탭에서 위와 같은 화면이 실행됩니다.
다시 [http://localhost:8888](http://localhost:8888)로 가보면 Untitled.ipynb라는 파일이 생성된 걸 확인할 수 있습니다. ipynp는 Jupyter에서 사용되는 Ipython notebook파일의 확장자 입니다. Untitled.ipynb파일을 체크해보시면 다양한 선택지가 나타납니다.

    - Duplicate: 노트북 복제
    - Shutdown: 노트북 종료
    - 휴지통 아이콘: 노트북 삭제

<center>
<img src="../images/python-2/pic-7.png">
</center>
 
- 위쪽 Running탭을 클릭하면 현재 실행되고 있는 노트북을 확인할 수 있습니다.

<center>
<img src="../images/python-2/pic-8.png">
</center>

### 실행 경로 변경(선택 사항)
- **건너뛰어도 추후 내용 진행에 지장이 없는 섹션입니다.**
- `jupyter notebook`명령어를 입력하기전의 경로가 jupyter의 home이 됩니다. 명령 프롬프트를 실행시켰을때의 기본 경로는 `C드라이브\Users\사용자명` 입니다. 이 경로에서 계속 Jupyter를 사용해도 무방합니다만 본인이 원하는 폴더에서 Jupyter를 실행시키고자 한다면 이 섹션을 참조해 주세요.

- 아래는  C드라이브 아래 steemit 폴더를 만들고 거기서 Jupyter notebook을 실행하는 명령 흐름입니다.

<center>
<img src="../images/python-2/pic-9.png">
</center>

- cd: (Change Directory)디렉토리를 이동을 할때 사용됩니다. 한단계 위의 경로로 가고 싶은 경우 `cd ..`를 입력합니다.
    - `cd`만 입력하면 현재의 경로를 출력합니다.
- mkdir: (MaKe DIRectory)디렉토리를 만드는 명령어입니다. 그냥 탐색기에서 원하는 경로에 폴더를 만들어줘도 됩니다.
    - ex.현재 경로에 `steemit` 디렉토리를 만들고 싶은 경우: `mkdir steemit`
- 다른 드라이브로 이동하고 싶은 경우, 해당 드라이브의 문자를 입력
    - ex. `D:`, `E:`  

<center>
<img src="../images/python-2/pic-10.png">
</center> 

- 위의 명령 흐름으로 Jupyter를 실행시킨 뒤 New를 통해 노트북을 만들어 봅시다. 기본 파일명은 Untitled입니다. Jupyter는 notebook을 자동저장합니다. 자동저장이 안되었다면 File에서 Save and Checkpoint를 선택하면 됩니다.

<center>
<img src="../images/python-2/pic-11.png">
</center>

- 탐색기에서 C드라이브의 steemit폴더로 들어가면 아까전 생성했던 Untitled노트북이 있는걸 확인할 수 있습니다.

<br>
<br>
<br>
<br>
<br>

#드디어 코딩 시작
-- 
- 파이썬은 직관적인 언어입니다. 너무 머리아프게 이해하고 암기하려 하기 보다 직관적으로 받아들이는게 더 도움이 되기도 합니다. 
- `print(1)`: 1을 출력하라! 쉽지 않나요?
 
##변수(Variable)

- 새로운 Jupyter notebook을 생성해 cell 안에 아래와 같이 입력후 실행시켜주세요.

```python
x = 2
print(x)
```

- 세가지 방법으로 실행시킬 수 있습니다.

    - 상단 `run cell`버튼 클릭
    - 명령어 입력 후 `ctrl + enter` 눌리기 (Mac 의 경우엔 `command + return`)
    - 명령어 입력 후 `shift + enter` 눌리기

- 첫번째, 두번째는 단일 cell을 실행시키고 마지막 `shift + enter`는 셀을 실행 시킨 뒤 아래쪽에 새로운 셀을 생성하는 입력입니다.

<center>
<img src="../images/python-2/pic-12.png">
</center>

> 위와같이 코드가 실행되는걸 확인할 수 있습니다.

- **변수**란 값을 담는 그릇입니다. `'변수명'='변수에 저장할 값'`의 형태로 사용할 수 있습니다. 
- 위의 코드는 x란 이름의 변수에 2라는 값을 저장하는 코드입니다.
- 변수는 언제든지 변경 가능합니다. x에 10이란 값을 저장 후 x를 출력해 어떤 값이 저장되었는지 확인해 봅시다.

```python
x = 10
print(x)
```
- 새로운 셀을 추가하려면 왼쪽 상단의 `+` 버튼을 눌러주세요.

##주석(Comment)

- **주석은 코드에서 무시되는 부분입니다.** 코드가 길어지고 복잡해지면 타인이 작성한 코드를 이해하기 힘듭니다. 심지어 본인이 작성한 코드라도 몇달이 지나면 기억이 가물가물해지기도 하죠. 주석은 코드를 더 쉽게 이해할 수 있도록 설명을 작성할 때 사용됩니다. 파이썬에선 #기호를 주석에 사용합니다. #이후에 작성된 코드는 무시됩니다

```python
x = 2 # x에 2 저장
print(x) # 2 출력
print(x+1) # x+1 출력, 2+1=3이므로 3 출력
#print(x+2) 전체 줄이 주석처리되어있으므로 무시됨
print(x+3) # x+3 출력, 2+3=5이므로 5 출력
```

<center>
<img src="../images/python-2/pic-13.png">
</center>
> 결과

- #은 한줄에만 적용됩니다. 한 줄이 아닌 여러 줄을 주석처리하고 싶으면 '''(따옴표 세개)로 무시하고 싶은 부분을 감싸주면 됩니다.

```python
print(1)
print(2)
''' 
print(3)
print(4)
print(5)
''' 
print(6)
print(7)
```
- 위 코드를 실행시켜보면 주석으로 감싸진 부분은 실행 되지 않는걸 확인할 수 있습니다.

<center>
<img src="../images/python-2/pic-14.png">
</center>
> 현재까지 결과

##자료형(Data Type)

### 숫자(Number)

#### 정수(Integer)

- 정수는 '양의 정수', '음의 정수', '0'으로 이루어져 있습니다. 간단하게 이해하자면 소수점이 없는 수라고 보셔도 무방합니다. 1,2,100, 0, -1, -20 등 이런게 정수입니다. 한번 정수를 출력해 보겠습니다.

```python
print(1)    # 출력 1
print(2)    # 출력 2
print(100)  # 출력 100
print(0)    # 출력 0
print(-1)   # 출력 -1
print(-20)  # 출력 -20
```

- type()이란 함수가 있습니다. (함수에 대해선 추후에 다뤄보겠습니다. 함수가 무엇인지에 대한 호기심도 좋지만 이번 시간에는 사용법에 주목해주세요.) 이 함수 안에 값을 넣으면 그 값의 자료형(data type)을 알려줍니다.

```python
type(1)   # int
```
- int는 정수를 의미하는 integer의 약자입니다. type()안에 0을 넣어도, -1을 넣어도 'int'가 출력되는걸 확인할 수 있습니다.

```python
x = 3
print(type(x)) # 출력 "<class 'int'>"
print(x)       # 출력 "3"
print(x + 1)   # 덧셈; 출력 "4"
print(x - 1)   # 뺄셈; 출력 "2"
print(x * 2)   # 곱셈; 출력 "6", *는 곱셈 연산자입니다. 프로그래밍에선 x기호 대신 * 기호를 사용합니다.
print(x ** 2)  # 제곱; 출력 "9", **는 제곱 연산자입니다.
```

- 여기서 다시 x를 출력하면 어떤 값이 나올까요?

```python
print(x)
```

- x+1, x-1, x*2, x**2인 값을 출력했을 뿐 x 값 자체는 변경시키지 않았기에 x값은 그대로 3입니다.
- x에 1을 더한 값을 다시 x에 저장하려면 아래처럼 다시 변수에 값을 할당 하면 됩니다.

```python
x = x + 1
print(x)
```
- `x = x +1`는 x에 1을 더한 후 그 값을 다시 x에 대입하란 뜻입니다. 이 과정을 줄인 축약연산자가 존재합니다.

```python
x = 3    # x에 3 대입
x += 1   # x = x + 1 과 동일
print(x) # 4 출력
x -= 1   # x = x - 1 과 동일
print(x) # 3 출력
x *= 2   # x = x * 2 과 동일
print(x) # 6 출력
```

- 변수를 추가로 할당하여 변수간 연산도 가능합니다.

```python
x=3
y=2
print(x+y)   # 덧셈, "8" 출력
print(x-y)   # 뺼셈, "1" 출력
print(x*y)   # 곱셈, "6" 출력
print(x**y)  # 거듭제곱, "9" 출력
print(x/y)   # 나눗셈, "1.5" 출력
print(x%y)   # 나눗셈 나머지, "1" 출력
```

#### 실수(float)

- 프로그래밍에서의 실수는 소수점 이하의 값이 있는 수를 의미합니다. 정수와 동일한 연산들이 가능합니다.

```python
y = 2.5
print(type(y)) # 출력 "<class 'float'>"
print(y, y + 1, y * 2, y ** 2) # 출력 "2.5 3.5 5.0 6.25"
```

#### 문자열(string)

- 파이썬은 문자열을 다루기 쉬운 프로그래밍 언어입니다.

```python
x = 'hello'    # 문자열을 표현할 땐 따옴표나
y = "world"    # 쌍따옴표가 사용됩니다; 어떤걸 써도 상관없습니다.
print(x)       # hello
print(y)       # world
print(type(x)) # <class 'str'>; str은 string을 의미합니다.
print(len(x))  # len()은 문자열의 길이(length)를 반환하는 함수입니다.
               # hello는 5글자니까 5를 반환합니다.
hw = x + y     # 문자열간의 + 연산은 문자열을 연결하는 겁니다.
               # 이를 문자열 접합(concatenation)이라고 합니다.
print(hw)      # helloworld
hw2 = x + " " + y
print(hw2)     # 중간에 공백도 함께 접합
```

## 자료형간 형변환
- 자료형간 형변환이 가능한 경우가 있습니다.

```python
x=1              # x에 1 할당. 1은 정수
print(x)         # 1
print(type(x))   # <class 'int'>

x = float(x)     # float(): 실수 자료형으로 형변환
print(x)         # 1.0
print(type(x))   # <class 'float'>

x = int(x)       # int(): 정수 자료형으로 형변환
print(x)         # 1
print(type(x))   # <class 'int'>

x = str(x)       # str(): 문자열 자료형으로 형변환
print(x)         # 1
print(type(x))   # <class 'str'>
```

- 제일 마지막 부분의 1은 정수형인지 실수형인지 출력만 해선 구분이 가지 않죠? 그럴땐 type()으로 확인하면 됩니다. 그런데 왜 이런게 필요할까요? 예시를 들어 설명하겠습니다.

```python
x = 10000
print("잔고: " + x)  # TypeError: Can't convert 'int' object to str implicitly
```
- 위 코드를 실행하면 에러가 납니다. 문자열("잔고: ")과 정수(10000)을 더하려(+) 했기 때문이죠. 동일한 자료형으로 맞춰주면 에러가 나지 않습니다.

```python
x = 10000
x = str(x)
print("잔고: " + x)  # 잔고: 10000
```

- print함수 내부에서 바로 형변환 함수를 사용할수도 있습니다.

```python
x = 10000
print("잔고: " + str(x))  # 잔고: 10000
```

--
# 마무리

이번글부터 본격적으로 코딩을 시작했군요. 어떻게 보셨나요? 조금 어렵게 느껴질수도 있지만 차근차근 따라오다보면 못할건 아니지 않나요? ㅎㅎ
따라해 보시면서 잘 이해가 안가거나 궁금하신 부분 있으면 언제든 댓글남겨주세요. 다음 글로 찾아뵙겠습니다. 감사합니다!






--
# **이전 글 보기**
    
- [[파이썬 프로그래밍 무작정 따라하기] #0-Intro](https://steemit.com/kr/@sanghkaang/0-intro)
- [[파이썬 프로그래밍 무작정 따라하기] #1-파이썬 설치하기](https://steemit.com/kr/@sanghkaang/1)




