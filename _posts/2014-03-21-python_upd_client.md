---
layout: post
title: "Python UDP客户端"
description: "Python UDP client"
category: python
tags: [network, python]
---
{% include JB/setup %}

第一次写UDP程序，超乎寻常的简单

```python
>>> import socket
>>> s = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)

>>> import time
>>> s.sendto("test%s" % time.strftime("%Y-%m-%d_%H-%M-%S"), ("172.16.12.28", 7301))
23
>>> s.recvfrom(1024)
('test2014-03-21_08-35-57', ('172.16.12.28', 7301))
```