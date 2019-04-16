---
draft: true
---

# Install the RabbitMQ Server

- http://www.rabbitmq.com/install-homebrew.html
- brew install rabbitmq

# Run RabbitMQ Server

## path 설정
- `PATH=$PATH:/usr/local/sbin` to your .bash_profile

## 실행
- `rabbitmq-server`
- 기본 credential user명: `guest`, password: `guest`
    - localhost로 연결시에만 기본 user가 사용 가능하며 다른 연결을 사용할 경우 미리 credential을 설정해야 한다.

## Controlling System Limits on OS X
- 대부분의 OS에서, Max number of open files는 messaging broker로 쓰기엔 턱없이 적다.
- rabbitmq를 이용하기 위해, production에선 최소한 65536이상, 개발환경에선 4096이상의 **Max number of open files**를 지정해주는걸 추천.
- 두 가지 제약이 있다.
    - the maximum number of open files the OS kernel allows (kern.maxfilesperproc)
    - per-user limit(ulimit -n)
    - 전자가 후자보다 무조건 높아야만 한다.
- 몇가지 설정 방법이 있다.
    - Invoke ulimit -S -n 4096 before starting RabbitMQ in foreground.
    - Edit the[rabbitmq-env.conf][1] to invoke ulimit before the service is started.
    - [Configure max open files limit][2] via launchctl limit /etc/launchd.conf.

그런데 `sudo launchctl limit maxfiles 200000 900000`이렇게 설정하는게 제일 간편하더라.


## Verifying the Limit

- `rabbitmqctl status` 또는 `launchctl limit`로 확인

[1]: http://www.rabbitmq.com/configure.html
[2]: https://github.com/basho/basho_docs/blob/master/content/riak/kv/2.2.3/using/performance/open-files-limit.md#mac-os-x-el-capitan


----웹ui부분하면서 막힌거--------

The management plugin is included in the RabbitMQ distribution. To enable it, use rabbitmq-plugins:
- rabbitmq-plugins enable rabbitmq_management
- The Web UI is located at: http://server-name:15672/
- The Web UI uses an HTTP API provided by the same plugin. Said API's documentation can be accessed at http://server-host:15672/api/ or our latest HTTP API documentation here).
Download rabbitmqadmin at: http://server-name:15672/cli/
- guest/guest

# RabbitMQadmin 설치
- http://localhost:15672/cli
- http://www.rabbitmq.com/management-cli.html


# rabbitmqctl 사용
- Listing queues
    - sudo rabbitmqctl list_queues
--------------------------------

----튜토리얼1

# Hello World!
- The simplest thing that does something

Listing queues

You may wish to see what queues RabbitMQ has and how many messages are in them. You can do it (as a privileged user) using the rabbitmqctl tool:

`sudo rabbitmqctl list_queues`



튜토2

# Work queues (aka: Task Queues) 
- Distributing tasks among workers(the competing consumers pattern)


---
Forgotten acknowledgment

It's a common mistake to miss the basic_ack. It's an easy error, but the consequences are serious. Messages will be redelivered when your client quits (which may look like random redelivery), but RabbitMQ will eat more and more memory as it won't be able to release any unacked messages.

In order to debug this kind of mistake you can use rabbitmqctl to print the messages_unacknowledged field:

`sudo rabbitmqctl list_queues name messages_ready messages_unacknowledged`
---

튜토3

# Publish/Subscribe
- sending messages to many consumers at once.
<!-- - how to deliver the same message to many consumers. -->



