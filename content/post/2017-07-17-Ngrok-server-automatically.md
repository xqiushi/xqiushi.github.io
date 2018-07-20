---
layout:     post
title:      Ngrok服务器与客户端开机自动启动
subtitle:   现在改用frp了
date:       2017-07-17
author:     xqiushi
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - Ngrok
    - 技
---

### 前言

[上文](https://xiangqiushi.com/2017/07/15/myself-hosted-ngrokd.html)中实现了不使用teamviewer或anydesk等工具,便可访问内网远程桌面.但是有个缺点就是,这些工具不是自行化的.特别是windows客户端,你总不可能每次都先登陆一下,然后运行egrok.exe吧.这样有反自动化流程.经过探索,我找到了一个方法,就是利用window自带的计划任务为主要工具.
### 服务器端自动化
**CentOS自行化,非常的方便.**
> 设置开机启动, 将该命令行写入到/etc/rc.local.

```
/opt/ngrok/bin/ngrokd -tlsKey=/opt/ngrok/server.key -tlsCrt=/opt/ngrok/server.crt -domain=xiangqiushi.com -httpAddr=:8080 -httpsAddr=:8081 > /opt/ngrok/ngrok.log &
```
> 修复权限,这步非常必要,很多时候,开机不能启动的原因都是这文件的权限问题.

```
chmod +x /etc/rc.d/rc.local
```
### 客户端自动化
**window下自动化,流程有点多,今天只说重要内容.**
> 新建一个批处理文件,名如:ngrok.bat.内容如下:

```
ngrok -config=ngrok.cfg start mstsc  mysql http https
```
> 使用Bat_To_Exe_Converter(x64)工具将bat转成exe. 可见--隐形应用程序,如下图:

> 添加计划任务.

win+R,输入Taskschd.msc,创建基本任务,根据自己需要设置时间等,然后下一步.

- **关键1**:起始于(可选)必须指定上述转的exe所在目录.设置如图:

- **关键2**:完成后,双击才创建的任务,编辑属性:不管用户是否登录都要运行(W).设置如图:

这样,只要电脑一通电启动系统后,便可以使用远程桌面了.

### 小结
我最开始想的办法就是添加成服务便可以实现通电开机就可以远程访问桌面,结果服务死活不能正常启动.最终使用计划任务的试来完成.并在计划任务中加入了定时关机,结合BIOS的定时开机.基本上可以随时控制这个电脑.

**条条大路通罗马,而那些出生在罗马的人,无聊的时候就俯视我们的折腾**
