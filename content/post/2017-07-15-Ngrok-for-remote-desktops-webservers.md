---
layout:     post
title:      Ngrok实现访问内网的远程桌面和web服务器
subtitle:   frp更方便、快捷
date:       2017-07-15
author:     xqiushi
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - CentOS
    - Ngrok
    - 术
---

### ngrok介绍

看名字就知道跟nginx是同辈的,具有是一个反向代理的功能,但是它又有很多特别地方:当它在外网服务器上,便可以通过外网的公共的端点和内网(本地)之间建立一个安全的通道.
### ngrok功能
由于能在公网与内网建议一个安全通道,那么它最大的功能就是实现内网穿透.

比如,我台电脑建了一个typecho博客网站用于平常的测试.这个电脑在内网的IP地址是10.1.102.4,并且我没有网络管理权限,不能做端口映射.当我不在这台电脑旁边但想浏览一下这个电脑上的web内容,这时ngrok就是便可以大显身手.可以将10.1.102.4:80映射到xiangqiushi.com:8080下.便可以在任何公网下输入xiangqiushi.com:8080,便可以访问到10.1.102.4的web服务器内容.从此便可以将博客放在家中电脑,然后在阿里云的ECS上安装ngrok服务.以后搬家就不用备份资料.

那么,映射远程桌面端口也就不成问题了.
### ngrok安装<a name="-1"></a><sup>[[1]](#1)</sup><a name="-2"></a><sup>[[2]](#2)</sup>
**服务器端安装,以CentOS 7 64位为例**

> SSH到服务器,下载源码

```
cd /opt
git clone https://github.com/inconshreveable/ngrok.git ngrok
```
> 配置域名与证书

```
cd /ngrok
NGROK_DOMAIN="xiangqiushi.com"
openssl genrsa -out base.key 2048
openssl req -new -x509 -nodes -key base.key -days 10000 -subj "/CN=$NGROK_DOMAIN" -out base.pem
openssl genrsa -out server.key 2048
openssl req -new -key server.key -subj "/CN=$NGROK_DOMAIN" -out server.csr
openssl x509 -req -in server.csr -CA base.pem -CAkey base.key -CAcreateserial -days 10000 -out server.crt
```

> 替换默认证书

```
cp server.crt assets/server/tls/snakeoil.crt
cp server.key assets/server/tls/snakeoil.key
cp base.pem assets/client/tls/ngrokroot.crt

```
> 开始编译,完成后便可以在/opt/ngrok/bin/中看到ngrok、ngrokd 两个可执行文件

```
sudo make release-server release-client
```
> 编译Mac版客户端,完成后便可以在/opt/ngrok/bin/darwin_amd64/中找到ngrok可执行文件

```
GOOS=darwin GOARCH=amd64 make release-client
```

> 编译Windows64位版,完成后便可在生/opt/ngrok/bin/windows_amd64/中打到ngrok.exe可执行文件

```
GOOS=windows GOARCH=amd64 make release-client
```

> 开启ngrok服务

```
sudo ./opt/ngrok/bin/ngrokd -tlsKey=server.key -tlsCrt=server.crt -domain="xiangqiushi.com" -httpAddr=":8080" -httpsAddr=":8081"
```

> 设置防火墙规则

httpAddr和httpsAddr的端口分别是ngrok用来转发http和https服务的端口。但是ngrokd还会默认运行4443端口用来跟客户端通讯（可通过 -tunnelAddr=":xxx" 指定），因此,在防火墙中放行这三个端口。

```
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --zone=public --add-port=8081/tcp --permanent
firewall-cmd --zone=public --add-port=4443/tcp --permanent
```
如果添加不了,可参考[CentOS 7下LNMPA介绍以及在防火墙规则中添加端口方法](https://reddy.wang/2017-07-05-LNMPA-firewall-rules-in-CentOS-7.html)

### ngrok配置
**客户端设置,以windows为例**

下载/opt/ngrok/bin/windows_amd64/ngrok.exe到本地电脑(10.1.102.4).新建ngrok.cfg,内容如下:

```
server_addr: xiangqiushi.com:4443
trust_host_root_certs: false
tunnels:
    mstsc:
         remote_port: 33891
         proto:
             tcp: 3389
    mysql:
         remote_port: 33061
         proto:
             tcp: 3306
    http:
         subdomain: web
         proto:
             http: 80
    https:
         subdomain: blog
         proto:
             https: 443

```

此处不要复制粘贴,最好自己写一下:因为配置文件不能有制表符也就是tab,缩进只能用空格！

在ngrok.exe所在目录,运行:

```
ngrok -config=ngrok.cfg start mstsc  mysql http https
```
这样就可以同时开启web、数据库和远程桌面的端口穿透.

任一公网电脑上输入web.xiangqiushi.com:8080和blog.xiangqiushi.com:8081便可以10.1.102.4的80和443端口.

用远程访问工具,输入地址xiangqiushi.com:33891就可以实现远程访问了.

### 小结
真是有网络就有天下的感觉,当然,这些都是人类智慧的结晶.网络不断的缩小物理距离感,减小世界的延时,加速经济的发展速度,促进文化的交融.

参考与注释
---
<a name="1"></a>[1]. [搭建属于自己的ngrok服务器 实现内网穿透](http://www.chenweikang.top/2016/11/20/%E6%90%AD%E5%BB%BA%E5%B1%9E%E4%BA%8E%E8%87%AA%E5%B7%B1%E7%9A%84ngrok%E6%9C%8D%E5%8A%A1%E5%99%A8-%E5%AE%9E%E7%8E%B0%E5%86%85%E7%BD%91%E7%A9%BF%E9%80%8F/) 左手代码右手诗[↩](#-1)

<a name="2"></a>[2]. [搭建 ngrok 服务实现内网穿透](https://imququ.com/post/self-hosted-ngrokd.html) Jerry Qu[↩](#-2)
