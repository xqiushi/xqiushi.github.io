---
layout:     post
title:      介绍Google神器之Brotli
subtitle:   好像有些CND不支持
date:       2017-08-07
author:     xqiushi
header-img: img/post-bg-android.jpg
catalog: true
tags:
    - Google
    - 术
---



### 前言

相信大家对Gzip非常地熟悉,它是一种流行的文件压缩算法，现在在Linux平台下的应用十分广泛。它对纯文本文的压缩率可高达70％以上.因此,在nginx服务器中,除了缓存技术可以加速外,就算是压缩技术了.Gzip是应用较多的一种
### brotli简介
brotli是以瑞士烘培产品命名，它在瑞士德语中的意思是小面包。它最初是用于网络字体的离线压缩,后来Google工程师们重写了编码器并改进为http服务下的压缩利器.这是继Zopfli之后Google官方博客宣布的新的开源压缩算法:Brotli.它通常压缩的更多的是0.5-1MB的文件,对于超过16M的文件,也做特殊地处理编码,并降低了编码和解码速度.将来可能成为主要浏览器能支持的数据压缩格式.
### brotli安装<a name="-1"></a><sup>[[1]](#1)</sup>

CentOS7下军哥维护的LNMP(A)1.4环境的的安装方法:

+ 安装需要用到开发工具

```
yum groupinstall 'Development Tools'
```

+ 安装libbrotli

```
cd /usr/local/src/
git clone https://github.com/bagder/libbrotli
cd libbrotli
./autogen.sh
./configure
make && make install
```

+ 安装ngx_brotli

```
cd /usr/local/src/
git clone https://github.com/google/ngx_brotli
cd ngx_brotli && git submodule update --init
```

+ 下载Nginx

```
cd /usr/local/src
wget http://nginx.org/download/nginx-1.13.2.tar.gz
tar -xvzf nginx-1.13.2.tar.gz && rm -rf nginx-1.13.2.tar.gz
```

+ 获取Nginx以前的编译参数

```
nginx -V
```

+ 配置新参数并编译

> PS:在nginx -V获得的的参数后然后再加上 --add-module=/usr/local/src/ngx_brotli 就为新参数
>

```
cd /usr/local/src/nginx-1.13.2
./configure --user=www --group=www --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-http_v2_module --with-http_gzip_static_module --with-ipv6 --with-http_sub_module --with-openssl=/root/lnmp1.4/src/openssl-1.0.2l --with-ld-opt='-ljemalloc' --add-module=/usr/local/src/ngx_brotli

make && make install

```

	ipv6报错,我无视了
	openssl报错,我重新下载了

+ 检查nginx

```
 nginx -t
```

+ 配置Nginx文件

在/usr/local/nginx/conf/nginx.conf的http{}内的gzip后插入下面代码:

<font color=#FF1493 size=3 >PS:LNMP1.4中的proxy.conf参数默认为(**proxy_set_header   Accept-Encoding '';**),添加如下代码便可以与gzip共存</font>

```
http
    {
       …
        #gzip Compression
        gzip on;
        gzip_static on;
        gzip_min_length  1k;
        gzip_buffers     4 16k;
        gzip_http_version 1.1;
        gzip_comp_level 2;
        gzip_types     text/plain application/javascript application/x-javascript text/javascript text/css application/xml application/xml+rss;
        gzip_vary on;
        gzip_proxied   expired no-cache no-store private auth;
        gzip_disable   "MSIE [1-6]\.";

        
        #Brotli Compression
        brotli on;
        brotli_static on;
        brotli_comp_level 11;
        brotli_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/javascript image/svg+xml text/html;
        brotli_buffers 16 8k;
        brotli_window 512k;
        brotli_min_length 20;
        …
	}
```

+ 再次检测是否正常并重启

```
nginx -t
systemctl restart nginx
```

+ 检查是否生效

打开你的网页，用chrome开发者工具调试，在Network那，发现有

```
content-encoding:br
```

+ 做个软链接,防止报错
把对应的库文件在 /lib(64) 或者 /usr/lib(64) 中做上软链接：

```
$ ln -s /usr/local/lib/libbrotlienc.so.1 /lib64
```

### 浏览器支持情况<a name="-2"></a><sup>[[2]](#2)</sup>

Mozilla Firefox在Firefox 44中实现Brotli。

Google Chrome从Chrome 49开始支持Brotli。

Opera从Opera 36开始支持Brotli。

windwos 10中的Microsoft Edge 开始支持Brotli。

### 小结
虽然安装成功了,并没有感受到速度有什么明显地提升.压缩的确有点提高以前的某篇文章是7.9K现在是7.6K😱.
    
​    
参考与注释
---
<a name="1"></a>[1]. [把Gzip换成Brotli的Nginx配置教程](https://www.linpx.com/p/replace-the-gzip-nginx-configuration-tutorial-brotli.html) Chakhsu Lau[↩](#-1)

<a name="2"></a>[2]. [Brotli](https://zh.wikipedia.org/zh-hans/Brotli) 维基百科[↩](#-2)
    
​     