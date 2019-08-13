---
title: sfdisk可以方便备份与恢复分区表
date: 2019-08-13 15:06:54
tags:
 - linux命令
 - sfdisk
categories:
 - linux命令
---

硬盘的分区表有时候很容易丢失，一般在硬盘开始扇区，分区表丢失后，lsblk就只能看到一块没有分区的硬盘，所以分好区后，备份分区表，比备份系统更重要

```
lsblk  # print all disk and partition
sfdisk -d /dev/sda > sda.dump   # Backup partition table
sfdisk /dev/sda < sda.dump      # restore partition table
```