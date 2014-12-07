---
layout: post
title: "自己动手做个机顶盒"
description: "XBMC on Respberry Pi"
category: tech
tags: [respberrypi xbmc tv]
---
{% include JB/setup %}

不知N年后，还有人知道[机顶盒]()是啥东西不。就像当年的VCD

### 基础知识
- [Respberry Pi](http://www.raspberrypi.org/)
- [XBMC](www.xbmc.org/)，现在改名叫[Kodi](http://kodi.tv/)了
- [xbmc-addons-chinese](https://github.com/taxigps/xbmc-addons-chinese)
- [XBMC Remote for Android](https://code.google.com/p/android-xbmcremote/)


### 硬件准备
- Respberry Pi一个， 淘宝200-300元间
- SD卡（2G或4G），装系统用
- 网线一根（我的Pi只有LAN口， 没有WIFI模块）
- 5V micro USB电源一个(Android手机的充电器就可以用)
- Android手机一个（旧的也行，反正我的HTC G1能用）
- HDMI线一根(老电视没有HDMI接口，需要三色AV线1根，3.5mm一分二音频线公头转双母头1根，AV解析度只能到720×480i）

### 软件准备
- RASPBMC, raspberrypi网站上已经有XBMC的镜像，直接用就可以
- ssh客户端（最好Linux系统）

### 过程

### 遇到问题
- 代理配置改后未生效，GUI上也看不到
