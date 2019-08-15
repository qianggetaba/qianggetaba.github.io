---
title: 获取tar压缩包tarball的根目录文件名
date: 2019-08-15 17:36:10
tags:
---

使用下面命令可以得到tar压缩包解压后的根目录文件名
tar 会自动根据文件后缀选择解压算法
```
tar tf  /mnt/lfs/sources/m4-1.4.18.tar.xz | sed -e 's@/.*@@' | uniq
m4-1.4.18
```

z for tar.gz,gzip
```
tar tzf nginx-1.0.0.tar.gz
```