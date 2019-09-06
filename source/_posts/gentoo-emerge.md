---
title: gentoo的emerge选项
date: 2019-09-04 19:03:40
tags:
 - emerge
categories:
 - gentoo
---

[man-emerge](https://dev.gentoo.org/~zmedico/portage/doc/man/emerge.1.html)

``emerge --info`` 尤其是 USE 变量值，就是系统的一些功能

``-a`` 等同 ``--ask`` 有问题询问用户
``-v`` 等同 ``--verbose`` 详细输出
``-u`` 等同 ``--update`` 更新包为最新版本
``-U`` 等同 ``--changed-use`` 包括已安装包设置的USE
``-D`` 等同 ``--deep`` 整个依赖树
``-N`` 等同 ``--newuse`` USE 变量修改后
``-A`` 等同 ``--alert`` 命令完成后响声提示， Add a terminal bell character ('\a') to all interactive prompts
``-p`` 等同 ``--pretend`` 显示将要安装的包，并不安装，对不熟悉的包使用，what *would* have been installed
``-t`` 等同 ``--tree`` 显示依赖树，需要--update and --deep
``-c`` 等同 ``--depclean`` 后面跟包名就是卸载，没有就是删除系统内没有依赖，不用的包，如果有依赖没有解决，不做任何操作

``emerge -auAD @world``
``emerge -auAD @world``

``emerge --sync`` 更新软件包数据，就是``rsync`` 目录 ``/var/cache/distfiles``

``--with-bdeps`` 安装包时，默认启用，就是emerge的calculations时，把编译时依赖的包也安装上

``--autounmask-write`` 自动``unmask packages``, 自动更新USE，package.use

``--keep-going`` 出错后，尽可能执行其他，比如下载，编译其他的包

``emerge -pe @world`` 显示所有包，-p是显示，-e是empty tree，就是整个依赖树
