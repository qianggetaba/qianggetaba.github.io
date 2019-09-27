---
title: docker-zookeeper
date: 2019-09-27 10:54:08
tags:
categories:
 - docker
---

[docker--zookeeper](https://hub.docker.com/_/zookeeper)

``docker pull zookeeper``

``docker run --name myzk -d zookeeper:latest``

``docker logs -f myzk``

``docker inspect myzk | grep IPAddress`` 默认启动的没有绑定本机端口，只能在本地访问, 可以iptables转发

``docker exec -it myzk zkCli.sh`` 进入container的bash，myzk是名字，后面是进入后执行的命令

``docker stop myzk``

zookeeper命令
```
ls /
ls /zookeeper
stat /zookeeper
quit
```

docker-compose的zk集群，3台
```
//stack.yml
version: '3.1'

services:
  zoo1:
    image: zookeeper
    restart: always
    hostname: zoo1
    ports:
      - 2181:2181
    environment:
      ZOO_MY_ID: 1
      ZOO_SERVERS: server.1=0.0.0.0:2888:3888;2181 server.2=zoo2:2888:3888;2181 server.3=zoo3:2888:3888;2181

  zoo2:
    image: zookeeper
    restart: always
    hostname: zoo2
    ports:
      - 2182:2181
    environment:
      ZOO_MY_ID: 2
      ZOO_SERVERS: server.1=zoo1:2888:3888;2181 server.2=0.0.0.0:2888:3888;2181 server.3=zoo3:2888:3888;2181

  zoo3:
    image: zookeeper
    restart: always
    hostname: zoo3
    ports:
      - 2183:2181
    environment:
      ZOO_MY_ID: 3
      ZOO_SERVERS: server.1=zoo1:2888:3888;2181 server.2=zoo2:2888:3888;2181 server.3=0.0.0.0:2888:3888;2181
```
```
COMPOSE_PROJECT_NAME=zk_test docker-compose -f stack.yml up -d
docker ps |grep zoo
COMPOSE_PROJECT_NAME=zk_test docker-compose -f stack.yml ps
COMPOSE_PROJECT_NAME=zk_test docker-compose -f stack.yml down

docker exec -it zk_test_zoo1_1 zkServer.sh status  # 状态

echo stat | nc 127.0.0.1 2181 # 测试端口绑定，会显示stat信息或者stat is not executed because it is not in the whitelist

docker exec -it zk_test_zoo1_1 zkCli.sh # 进入zk命令
```

