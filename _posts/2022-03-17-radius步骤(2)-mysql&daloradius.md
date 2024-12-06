---
title: radius步骤(2)-mysql & daloradius
date: 2022-03-17 15:23:56 +0800
categories: []
tags: [pppoe, radius]
comments: false
published: true
---

# MySQL & daloRADIUS

>[daloRADIUS](https://www.daloradius.com/) is an advanced RADIUS web management application aimed at managing hotspots and general-purpose ISP deployments. It features user management, graphical reporting, accounting, a billing engine and integrates with GoogleMaps for geo-locating.
>
>daloRADIUS is written in PHP and JavaScript and utilizes a database abstraction layer which means that it supports many database systems, among them the popular MySQL, PostgreSQL, Sqlite, MsSQL, and many others.
>
>It is based on a [FreeRADIUS](https://www.freeradius.org/) deployment with a database server serving as the backend. Among other features it implements ACLs, GoogleMaps integration for locating hotspots/access points visually and many more features.

## [](https://github.com/lirantal/daloradius#contributors)

在原来的基础增加数据库管理，采用daloradius进行图形化管理。

> I can only say that step by step.
{: .prompt-info }

[Install FreeRADIUS & daloRADIUS on Ubuntu 20.04 + MySQL/MariaDB - ByteXD](https://bytexd.com/freeradius-ubuntu/)

```sql
CREATE DATABASE radius;
CREATE USER 'radius' IDENTIFIED BY 'secret';
GRANT ALL PRIVILEGES ON radius.* TO 'radius';
FLUSH PRIVILEGES;
quit;
```

其中`secret`更改为自己配置的密码。

---
在完成上述操作之后，还要更改数据库中radacct表格。

```sql
use radius;  
DROP TABLE radacct;  
CREATE TABLE radacct (  
radacctid bigint(21) NOT NULL auto_increment,  
acctsessionid varchar(64) NOT NULL default '',  
acctuniqueid varchar(32) NOT NULL default '',  
username varchar(64) NOT NULL default '',  
groupname varchar(64) NOT NULL default '',  
realm varchar(64) default '',  
nasipaddress varchar(15) NOT NULL default '',  
nasportid varchar(15) default NULL,  
nasporttype varchar(32) default NULL,  
acctstarttime datetime NULL default NULL,  
acctupdatetime datetime NULL default NULL, 
acctstoptime datetime NULL default NULL,  
acctinterval int(12) default NULL,  
acctsessiontime int(12) unsigned default NULL,  
acctauthentic varchar(32) default NULL,  
connectinfo_start varchar(50) default NULL,  
connectinfo_stop varchar(50) default NULL,  
acctinputoctets bigint(20) default NULL,  
acctoutputoctets bigint(20) default NULL,  
calledstationid varchar(50) NOT NULL default '',  
callingstationid varchar(50) NOT NULL default '',  
acctterminatecause varchar(32) NOT NULL default '',  
servicetype varchar(32) default NULL,  
framedprotocol varchar(32) default NULL,  
framedipaddress varchar(15) NOT NULL default '',  
framedipv6address varchar(15) NOT NULL default '',
framedipv6prefix varchar(15) NOT NULL default '',
framedinterfaceid varchar(15) NOT NULL default '',
delegatedipv6prefix varchar(15) NOT NULL default '',
PRIMARY KEY (radacctid),  
UNIQUE KEY acctuniqueid (acctuniqueid),  
KEY username (username),  
KEY framedipaddress (framedipaddress),  
KEY acctsessionid (acctsessionid),  
KEY acctsessiontime (acctsessiontime),  
KEY acctstarttime (acctstarttime),  
KEY acctinterval (acctinterval),  
KEY acctstoptime (acctstoptime),  
KEY nasipaddress (nasipaddress)  
) ENGINE = INNODB;  
exit;
```

# 参考链接

1. [Install FreeRADIUS & daloRADIUS on Ubuntu 20.04 + MySQL/MariaDB - ByteXD](https://bytexd.com/freeradius-ubuntu/)

<br>
