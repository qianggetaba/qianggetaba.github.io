---
title: heredoc使用cat写入大段内容到文件,脚本生成配置文件
date: 2019-08-13 16:06:46
tags:
 - linux命令
 - cat
categories:
 - linux命令
---


使用heredocument可以写入大段内容到文件，比如在一个脚本里，生成多个脚本，配置文件等等

```
cat <<EOF >testd.sh
#!/bin/bash
ls -al
time ls -al
EOF
```
EOF只是一个标记，但是一般就用这个避免与内容混淆