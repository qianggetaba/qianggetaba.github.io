---
title: fdisk 完全命令行执行分区
date: 2019-08-13 15:01:54
tags:
 - linux命令
 - fdisk
categories:
 - linux命令
---

fdisk交互式，命令行分区工具，有时候需要完全在脚本里自动执行分区，可以用下面的脚本

```
sed -e 's/\s*\([\+0-9a-zA-Z]*\).*/\1/' << EOF | fdisk /dev/sdals
  o # clear the in memory partition table
  n # new partition
  p # primary partition
  1 # partition number 1
    # default - start at beginning of disk 
    #
  w
EOF
```
就是模拟交互式，输入命令给fdisk，一个空行就是回车，使用默认选项，分区大小在输入结束分区的扇区数时，可以使用数字如 +200M 就是这个分区200m，或者 G，有提示的

或者看看 sfdisk 从分区表恢复分区