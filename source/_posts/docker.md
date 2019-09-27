---
title: docker的使用
date: 2019-09-27 10:56:42
tags:
categories:
 - 软件
---

安装后的测试 ``sudo docker run hello-world``

加入docker组，避免每次sudo，需要重新登录
```
sudo usermod -aG docker your-user  # user docker without sudo, need relogin
```

``docker run`` 是``create``与``start``的别名，每次回新建一个container

``docker images`` 镜像列表

``docker ps`` 运行中的container

``docker ps -a`` 所有container，container是基于image创建的

``docker logs d1c445661242`` 运行的container的输出，后面是``docker ps -a``后的``CONTAINER ID``或者``NAMES``

``docker start``与``docker stop``

``docker exec -it myzk zkCli.sh`` 进入bash
