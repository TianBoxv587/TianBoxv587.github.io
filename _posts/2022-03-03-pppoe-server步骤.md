---
title: pppoe-server步骤
date: 2022-03-03 +0800
categories: [pppoe实验]
tags: [pppoe] # TAG names should always be lowercase
comments: true
---

## pppoe-server

![upgit_20220303_1646303746.png](https://cdn.jsdelivr.net/gh/TianBoxv587/Obsidian-img@main/2022/03/upgit_20220303_1646303746.png)

顺序随便，记得那张是仅主机模式，下面开启服务端会用到。

[ ubuntu上搭建pppoeServer_点滴积累成就技术梦想-CSDN博客_ubuntu搭建pppoe服务器](https://blog.csdn.net/leiming32/article/details/50462838)

**以下内容更改**⭐

1.

![upgit_20220303_1646303971.png](https://cdn.jsdelivr.net/gh/TianBoxv587/Obsidian-img@main/2022/03/upgit_20220303_1646303971.png)
```
auth
require-chap   #chap认证方式
lcp-echo-interval 10
lcp-echo-failure 2
logfile /var/log/pppd.log
```
这个感觉改不改都行

2.

![upgit_20220303_1646303351.png](https://cdn.jsdelivr.net/gh/TianBoxv587/Obsidian-img@main/2022/03/upgit_20220303_1646303351.png)
```
iptables -A POSTROUTING -t nat -s 192.168.5.0/24 -j MASQUERADE 
```

3.

![upgit_20220303_1646303638.png](https://cdn.jsdelivr.net/gh/TianBoxv587/Obsidian-img@main/2022/03/upgit_20220303_1646303638.png)
```
sudo pppoe-server -I ens33 -L 192.168.5.1 -R 192.168.5.200 -N 10
```
开启服务端，**ens33**改成**仅主机**网卡


## pppoe-client
![upgit_20220303_1646304903.png](https://cdn.jsdelivr.net/gh/TianBoxv587/Obsidian-img@main/2022/03/upgit_20220303_1646304903.png)

安装pppoeconf
```
sudo apt install pppoeconf
```
配置pppoeconf
```
sudo pppoeconf
```
配置pppoeconf之后默认开启，记得关闭，不行的话，就加上-a，全部关闭
```
sudo poff
```
再次启用pppoeconf
```
sudo pon dsl-provider
```
