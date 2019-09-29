---
title: grep
date: 2019-09-29 17:18:16
tags:
 - grep
categories:
 - linux命令
---

grep 默认【区分】大小写，``-i`` 不区分

``grep -n zoo stack.yml`` 显示行号的查找

``grep  "Z\|z" stack.yml`` 同 ``grep  -E "Z|z" stack.yml`` 多个【或】字符查找

``grep "zoo1"  stack.yml |grep "zoo2"`` 同 ``grep -E "zoo1.*zoo2"  stack.yml``(这个有字符前后顺序，前面的那个没有) 多个【和】字符查找

``-A 1``,``-B 1``,``-C 1`` 多显示匹配行的下面一行，上面一行，上下一行，方便看日志
