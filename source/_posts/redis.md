---
title: redis经验
date: 2019-08-15 20:06:11
tags:
 - redis
categories:
 - 软件
---

显示\xe5\xb7\x9d的中文
```
redis-cli --raw
```

字符串使用单引号
```
LPUSH carQueueByList '{"num":"一号","timestamp":1559792585000}'
```

