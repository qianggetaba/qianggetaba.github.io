---
title: linux 方便看日志-具体行查看
date: 2019-08-14 13:37:49
tags:
 - tail
 - head
 - sed
categories:
 - linux命令
---

```
cat my.log |grep -n "error"  # 显示行号
sed -n '425,435p' my.log  # 425-435行
tail -n +425 my.log | head -n 11  #从425(含)行往下11行
sed -n '435p' my.log  # 435行
tail -200 my.log  # 末尾200行
```
