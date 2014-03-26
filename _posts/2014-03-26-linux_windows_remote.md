---
layout: post
title: "Linux与Windows间相互远程访问"
description: "远程访问remote"
category: linux
tags: [linux]
---
{% include JB/setup %}

windows访问linux
-------

#### ssh访问

* putty
* xshell
* SecureCRT

#### 图形界面

* TightVNC
（linux上面要装vncserver）


linux访问windows桌面
------

* rdesktop
```bash
rdesktop -a 16 172.16.13.88:3389 -u jiangjia -g 1024*768 
```
