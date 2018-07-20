---
layout:     post
title:      Windows Server 2016中文版评估版转VOL版
subtitle:   服务器已改用PVE了
date:       2018-02-23
author:     xqiushi
header-img: img/post-bg-hacker.jpg
catalog: true
categories: [ Note ]
tags:
    - Windows
    - 技
---

### 1.前言
约六个月前，我在[微软官网](https://www.microsoft.com/zh-cn/evalcenter/evaluate-windows-server-2016)申请了评估版（valuation版）。申请过程也非常简单，自己打开官网随便填一填资料。然而真的只可以用180天。我把软件路由和NAS都放在上面的，结果年三十那天机器就自动断网。我远程桌面登陆一看，显示评估版到期了。纠结于每180天都要重新申请，还不如直接转成正式版。
### 2.操作
虽然网上到处都有类似的资料，有时候你不一定能找到那些网站。所以我在此记录一下操作过程，就算是一个备忘录。

+ 按win+R，输入cmd(注意是管理员权限)；
+ 查看Edition ID，输入“DISM /online /Get-CurrentEdition”。例如Datacenter，一般就DatacenterEval。把后面Eval那四字母去了就是你的Edition ID。
+ 接着输入“DISM /online /Set-Edition:*<Edition ID>* /ProductKey:*XXXXX-XXXXX-XXXXX-XXXXX-XXXXX* /AcceptEula”

>  注意：Edition ID出现第二步中，ProductKey后面是序列号，也就是VOL密钥。

#### 转数据中心版的代码如下：
`
DISM /online /Set-Edition:ServerDatacenter /ProductKey:CB7KF-BWN84-R7R2Y-793K2-8XDDG /AcceptEula
`
### 3.KMS激活
KMS服务器这里就不多介绍了，自行找一下。假如巨硬件全家桶工具箱的IP地址就是192.168.2.1。那么如下操作就可以。
> 激活windos

```
cd C:\Windows\System32
CSCRIPT /NOLOGO SLMGR.VBS /SKMS 192.168.2.1
CSCRIPT /NOLOGO SLMGR.VBS /ATO
CSCRIPT /NOLOGO SLMGR.VBS /XPR

```
> ~~随手送上~~激活Office2016（以64位为例子）

```
cd C:\Program Files\Microsoft Office\Office16
CSCRIPT OSPP.VBS /SETHST:192.168.2.1
CSCRIPT OSPP.VBS /ACT
CSCRIPT OSPP.VBS /DSTATUS

```
### 4.小结
其中一定要注意2中的CMD是管理员权限，输入命令后会删除原来的评估版相关的信息，这时需要耐心等上一些时间。其他的交给KMS服务器会自动自理就好了。
