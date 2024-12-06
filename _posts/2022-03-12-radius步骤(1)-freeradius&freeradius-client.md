---
title: radius步骤(1)-freeradius & freeradius-client
date: 2022-03-12 10:29:01 +0800
categories: []
tags: [radius, pppoe]
comments: false
published: true
---

# freeradius
## freeradius安装步骤

```shell
sudo apt-get install freeradius
```


## freeradius常用命令
开启1812和1813端口
```shell
iptables -A INPUT -p udp --dport 1812 -j ACCEPT
iptables -A INPUT -p udp --dport 1813 -j ACCEPT
```


启动/关闭/重启 freeradius服务
```shell
systemctl start freeradius
systemctl stop freeradius.service
service freeradius restart
```

调试模式
```shell
freeradius -X
```

查看服务器状态
```shell
systemctl status freeradius.service
```



```shell
echo "User-Name=testuser,User-Password=123" | radclient 127.0.0.1:1812 auth 123456 -x
```

本地测试
```shell
radtest tian 123456 localhost 0 testing123
```


远程测试
```shell
radtest tian 123456 192.168.125.128 0 123456
```



## freeradius配置文件
`/etc/freeradius/3.0`{: .filepath}
 
 **clients.conf**
 
客户端ip地址
```shell
client 192.168.125.129/24 {
	ippadr = 192.168.125.129/24
	client = 123456
}
```

---
**users**

pppoe服务器配置的用户
```shell
tian Cleartext-Password := "123456"
```

---
# freeradius-client

## freeradius-client安装步骤

由于Ubuntu中自带的安装包中没有freeradius-client，所以需要手动下载，编译freeradius-client。

有两种获取方法获取freeradius-client的压缩包——git和weget。


**直接从Github pull [freeradius-client](https://github.com/FreeRADIUS/freeradius-client)**



```shell
apt install git, gcc, make
```

从freeradius-client远程仓库拉取。
```shell
git clone https://github.com/FreeRADIUS/freeradius-client.git
```

线面就是常规操作了。
```shell
cd freeradius-client
./configure
make
make install
```


至此，freeradius-client安装完成。


**wget安装freeradius-client-1.16**
	

```shell
wget https://freeradius.org/ftp/pub/freeradius/old/freeradius-client-1.1.6.tar.gz
```

接着解压、安装，然后和上面步骤一样。

```shell
tar -zxvf freeradius-client-1.1.6.tar.gz
```


## freeradius-client配置文件

`/usr/local/etc/radiusclient/`{: .filepath}

**servers**

radius服务器的ip地址，以及radius服务器配置的clients.conf对应secret。

```shell
192.108.1.107 123456
```

---
**dictionry**

如果是wget安装的freeradius-client-1.16这个不用更改。


如果是git安装的最新版本，需要更改，把关于ipv6的配置全部注释掉。（`/ipv6`搜索）



---

**radiusclient.conf**

注释 `radius_deadtime 0`



如果没有`seqfile  /var/run/radius.seq`，添加上。（1.16版有，git获取的最新版没有）


---
`/etc/ppp/`{: .filepath}

**pppoe-server-options**

第一个是radius插件的位置，第二个是freeradius-client的配置信息。
```shell
plugin /usr/lib/pppd/2.4.7/radius.so
radius-config-file /usr/local/etc/radiusclient/radiusclient.conf 
```




## freeradius-client命令
`radlogin`和`radexample`

可能能用，可能不能用？

---
# 参考链接

1. [FreeRadius-client配置_13035706的技术博客_51CTO博客](https://blog.51cto.com/u_13045706/3832248)
2. [ LINUX下PPPoe+Freeradius+Mysql配置_Openking的博客-CSDN博客](https://blog.csdn.net/Openking/article/details/42561819)
3. [freeradius server和 client 部署(实战) - 天地一体 - 博客园 (cnblogs.com)](https://www.cnblogs.com/qinshizhishi/p/14961964.html)
4. [使用FREERADIUS搭建EAP认证环境 - SVEN (lishiwen4.github.io)](https://lishiwen4.github.io/tool/setup-freeradius-for-eap-auth)
<br>
