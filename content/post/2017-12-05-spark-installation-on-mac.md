---
title: "[Mac] Apche Spark 설치"
metaAlignment: center
date: 2017-12-05
categories:
- dev
tags:
- apache
- spark
- installation
- pyspark
- mac
thumbnailImage: https://www.onlinebooksreview.com/uploads/blog_images/2017/11/27_file.png
thumbnailImagePosition: left
coverSize: full
---

<!--more-->

<!--toc-->


# Java 설치

> 이미 설치된 java의 경로를 찾고 싶다면 `/usr/libexec/java_home` 명령어를 이용하면 된다.

```sh
brew tap caskroom/versions
brew cask search java
# brew cask install java 이렇게하면 자바9가 설치됩니다.
brew cask install java8
```

- 2017-12-05 현재 Spark는 Java9를 지원하지 않는다.
- 그러므로 java8을 설치해야한다.
- 아래처럼 본인의 version에 맞는 path를 .bashrc(또는 .zshrc)에 지정해준다.
- `export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk1.8.0_152.jdk/Contents/Home"`


# Scala 설치

```sh
brew install scala
```

- java와 마찬가지로 본인의 version에 맞는 path를 .bashrc(또는 .zshrc)에 지정해준다.

```sh 
export SCALA_HOME=/usr/local/Cellar/scala/2.12.4/libexec
export PATH=$PATH:$SCLAL_HOME/bin
```

# Spark 설치

```sh
# brew install spark가 아니다.
brew install apache-spark
```

- java, scala와 마찬가지로 본인의 version에 맞는 path를 .bashrc(또는 .zshrc)에 지정해준다.

```sh
export SPARK_HOME=/usr/local/Cellar/apache-spark/2.2.0/libexec
export PATH=$PATH:$SPARK_HOME
# export SPARK_LOCAL_IP=127.0.0.1 # 로컬에서 spark를 실행시키는경우 필요한 부분.
```

# Pyspark를 위한 환경설정

- 본인의 version에 맞는 path를 .bashrc(또는 .zshrc)에 지정해준다.

```sh
export PYTHONPATH=$SPARK_HOME/python:$SPARK_HOME/python/build:$PYTHONPATH
export PYTHONPATH=$SPARK_HOME/python/lib/py4j-0.10.4-src.zip:$PYTHONPATH
```

> py4j의 하드코딩을 피하고 싶다면
> export PYTHONPATH="${SPARK_HOME}/python/:${SPARK_HOME}/python/lib/py4j-*-src.zip):${PYTHONPATH}"


# 샘플 코드

## Scala
- `spark-shell` 명령어를 통해 spark shell을 실행시킨다.
- 아래 코드가 잘 실행되면 설치 성공.

```java
val count = sc.parallelize(1 to 100).filter { _ =>
  val x = math.random
  val y = math.random
  x*x + y*y < 1
}.count()

println(s"Pi is roughly ${4.0 * count / 100}")
```

## Python
- `pyspark` 명령어를 통해 spark shell을 실행시킨다.
- 아래 코드가 잘 실행되면 설치 성공.

```py
# for Python2
import random
NUM_SAMPLES = 10
def inside(p):
    x, y = random.random(), random.random()
    return x*x + y*y < 1

count = sc.parallelize(xrange(0, NUM_SAMPLES)) \
             .filter(inside).count()
print "Pi is roughly %f" % (4.0 * count / NUM_SAMPLES)

# for Python3
import random
NUM_SAMPLES = 10
def inside(p):
    x, y = random.random(), random.random()
    return x*x + y*y < 1

count = sc.parallelize(range(0, NUM_SAMPLES)) \
             .filter(inside).count()
print ("Pi is roughly %f" % (4.0 * count / NUM_SAMPLES))
```

# Spark Web UI

- Spark shell을 실행시키면 web UI 주소가 나온다.
- 기본값은 **http://127.0.0.1:4040**

## [Optional] Jupyter에서 pyspark 사용하기

### Pyspark의 default 실행환경을 Jupyter notebook으로 강제설정하는법

- .bashrc(또는 .zshrc)에 아래 내용을 추가

```sh
export PYSPARK_DRIVER_PYTHON=jupyter
export PYSPARK_DRIVER_PYTHON_OPTS='notebook'
```

- `pyspark` 명령어를 실행했을때 jupyter notebook형태로 실행된다.
- 기존의 shell방식으로 실행하고 싶다면 위에서 추가한 내용을 제거해야한다.

### 기존 Jupyter에서 pyspark 실행시키기

#### 방법1. findspark 활용하기
```
pip install pyspark findspark
```

```py
import findspark
findspark.init()

import pyspark
sc = pyspark.SparkContext(appName="myAppName")
```

#### 방법2. 경로 잡아주기

```py
# for python2
import os
execfile(os.path.join(os.environ["SPARK_HOME"], 'python/pyspark/shell.py'))

# for python3
import os
exec(open(os.path.join(os.environ["SPARK_HOME"], 'python/pyspark/shell.py')).read())
```

# 참조

> https://github.com/minrk/findspark
> https://gist.github.com/ololobus/4c221a0891775eaa86b0