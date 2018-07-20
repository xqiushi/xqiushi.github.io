---
layout:     post
title:      CentOS 7下LNMPA介绍以及在防火墙规则中添加端口方法 
subtitle:   CentOS中有时候命令不好用就用这个方法
date:       2017-07-05
author:     xqiushi
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - CentOS 7
    - 术
    - LNMPA
---



### 1.前言
以前介绍过了，我的网站使用人称“军哥”维护的[LNMP一键安装包](http://lnmp.org)(当然还有几个LNMP一键包）中的LNMPA。今天写个备忘录，关于LNMPA以及无法添加端口的处理办法方法。

### 2.名词术语

> Nginx:（发音同engine x）是一个网页服务器软件，它能反向代理HTTP, HTTPS, SMTP, POP3, IMAP的协议链接，以及一个负载均衡器和一个HTTP缓存。<a name="-1"></a><sup>[[1]](#1)</sup>

> Apache:（Apache HTTP Server的简称）是一个开放源码的网页服务器软件。由于其跨平台和安全性，被广泛使用。是最流行的Web服务器软体之一。可通过简单的API扩充，将Perl／Python等直译器编译到服务器中。<a name="-2"></a><sup>[[2]](#2)</sup>


Linux与Mysql就不复制粘贴了。自己查一下。下面说重主要的

> LAMP：是指Linux下Apache、MySQL、PHP这种网站服务器架构；

> LNMP：是指Linux下Nginx、MySQL、PHP这种网站服务器架构；

> LNMPA：是指Linux下Nginx、MySQL、PHP、Apache这种网站服务器架构
>

### 3.工作模式<a name="-3"></a><sup>[[3]](#3)</sup>
相信军哥已经说明白了这种结构的安装要求。可以简单的理解工作模式如下：
> Nginx年轻，身材苗条，反应快但不喜欢动脑。

> Apache年长，身材微胖，稳重喜欢脑力活动。

所以，我们安排年轻身材好的Nginx在前线工作，与客户交流，能满足客户需要的js，jpg静态类的请求，并能同时搞定多个客户的类似简单请求。这就类似于人脑中的快思考，但是可能出现速则不达的情况。当超级大量的客户，并提出需要一些动态的php,asp等类的请求，她只级交给成熟稳重、经历丰富在后台工作的Apache。

所以窃以为，只是她们同时好好工作，才算一个好网站，于是采用这样的结构。
	
### 4.CentOS7的防火墙
CentOS7下的防火墙已经由iptables改为firewall，
> 启动防火墙服务

```
systemctl start firewalld.service
```
> 启动防火墙服

```
systemctl enable firewalld
```

> 开启80端口（http）、443端口（https）、开启22端口（ssh）：

```
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --zone=public --add-port=22/tcp --permanent
```

> <a name="firewall-reload">重新加载防火墙</a>

```
firewall-cmd --reload
```
> 查看防火墙状态

```
firewall-cmd --list-all
```

### 5.其他端口
因为80、443和22都是默认开启的。当是我有时候要用其他端口，比如：Redis的6379，他会报错：Error: INVALID_SERVICE: 'nginx' not among existing services

```
[root@CentOS ~]# firewall-cmd --zone=public --add-port=6379/tcp --permanent
Error: INVALID_SERVICE: 'nginx' not among existing services
```
我试着关掉nginx也不行。

```
[root@CentOS ~]# lnmp nginx stop
+-------------------------------------------+
|    Manager for LNMP, Written by Licess    |
+-------------------------------------------+
|              https://lnmp.org             |
+-------------------------------------------+
Stoping nginx...  done
[root@CentOS ~]# sudo firewall-cmd --zone=public --add-port=6379/tcp --permanent
Error: INVALID_SERVICE: 'nginx' not among existing services
```
我试着关掉LNMPA还是不行。

```
[root@CentOS ~]# lnmp stop
+-------------------------------------------+
|    Manager for LNMP, Written by Licess    |
+-------------------------------------------+
|              https://lnmp.org             |
+-------------------------------------------+
Stoping LNMPA...
Stoping nginx...  done
Shutting down MySQL. SUCCESS! 
stop apache...  done
[root@CentOS ~]# sudo firewall-cmd --zone=public --add-port=6379/tcp --permanent
Error: INVALID_SERVICE: 'nginx' not among existing services
```
### 6.解决办法
最终使用cyberduck（MacOS下的WinSCP）登陆服务器，编辑/etc/firewalld/zones下的public.xml。如下：

```<?xml version="1.0" encoding="utf-8"?>
<zone>
  <short>Public</short>
  <description>For use in public areas. You do not trust the other computers on networks to not harm your computer. Only selected incoming connections are accepted.</description>
  <service name="dhcpv6-client"/>
  <service name="http"/>
  <service name="ssh"/>
  <service name="https"/>
  <port protocol="tcp" port="6379"/> # 关键修改的位置
  <service name="nginx"/>
</zone>
```
> 别忘了[重新加载防火墙](#firewall-reload) 

如果您有更好的方法，回帖告知。

参考与注释
---
<a name="1"></a>[1]. [nginx](https://zh.wikipedia.org/zh-hans/Nginx) 维基百科[↩](#-1)

<a name="2"></a>[2]. [Apache HTTP服务器](https://zh.wikipedia.org/zh-hans/Apache_HTTP_Server) 维基百科[↩](#-2)

<a name="3"></a>[3]. [LNMPA](https://lnmp.org/lnmpa.html) LNMP一键包官方[↩](#-3)




