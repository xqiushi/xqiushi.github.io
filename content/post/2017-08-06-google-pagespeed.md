---
layout:     post
title:      介绍Google神器之pagespeed
subtitle:   转github,留给需要的人
date:       2017-08-06
author:     xqiushi
catalog: true
tags:
    - 2017
    - 术
    - Google
---



## 前言
google,也许你还没有听过这个公司呢.简单地说她是以互联网搜索引擎为著名的公司.搜索引擎网站与门户网站的区别就在于: **搜索引擎公司不产生内容,只是内容的搬运工**.你是不是会觉得搬运工会有什么了不起的吧.那是当然,天下没有相同的两片叶子,同样是搬砖的,当别人一天只能搬3万块砖,而你却可以搬2亿吨砖😱.她的神器真的不少.我以前也不有听过agespeed,是[Stargazer](https://blog.gazer.win/)告诉我的.
## pagespeed介绍
工欲善其事，必先利其器.你要是觉得搬2亿吨砖是手工搬的,那你就SB了.BBR、Brotli等利器外,今天就说说pagespeed.顾名思义:pagespeed,是关于网页速度的.打开[PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/?hl=zh-CN)先测试一下你的网站.然后根据提示更改,我个人觉得特别好用的是:有些图片帮你压缩后好了,下载替换就可.
## pagespeed安装<a name="-1"></a><sup>[[1]](#1)</sup>

经过我多次的折腾,发现安装都非常方便.基本上三个步骤就搞定:

> 1 下载源码

一般在github上下载源码到本机

> 2 编译

主要是在nginx以前的参数上加上新的参数,然后重新编译.偶尔下载的源码需要单独编译成小模块.

> 3 配置参数

对新添加到的程序进行参数配置,让他个性化的工作起来.主要位置是xiangqiushi.com.conf.

今天在CentOS7下,安装ngx_pagespeed也是这个过程.

### 下载源码

```
NPS_VERSION=1.12.34.2-stable
cd
wget https://github.com/pagespeed/ngx_pagespeed/archive/v${NPS_VERSION}.zip
unzip v${NPS_VERSION}.zip
cd ngx_pagespeed-${NPS_VERSION}/
NPS_RELEASE_NUMBER=${NPS_VERSION/beta/}
NPS_RELEASE_NUMBER=${NPS_VERSION/stable/}
psol_url=https://dl.google.com/dl/page-speed/psol/${NPS_RELEASE_NUMBER}.tar.gz
[ -e scripts/format_binary_url.sh ] && psol_url=$(scripts/format_binary_url.sh PSOL_BINARY_URL)
wget ${psol_url}
tar -xzvf $(basename ${psol_url})  # extracts to psol/
```
### 编译nginx
在以前的参数上加上"--add-module=[你的ngx_pagespeed位置]"例如:
--add-module=/root/ngx_pagespeed-1.12.34.2-stable
```
cd /usr/local/src/nginx-1.13.3
./configure --user=www --group=www --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-http_v2_module --with-http_gzip_static_module --with-ipv6 --with-http_sub_module --with-openssl=/root/lnmp1.4/src/openssl-1.1.0f --with-ld-opt='-ljemalloc' --add-module=/usr/local/src/ngx_brotli --add-module=/root/ngx_pagespeed-1.12.34.2-stable
make && make install
```

### 配置参数
nginx的配置文件server{}中增加
xiangqiushi.com.conf


```
		# on 启用，off 关闭
        pagespeed on;
        # 重置 http Vary 头
        pagespeed RespectVary on;
        # html字符转小写
        pagespeed LowercaseHtmlNames on;
        # 压缩带 Cache-Control: no-transform 标记的资源
        pagespeed DisableRewriteOnNoTransform off;
        # 相对URL
        pagespeed PreserveUrlRelativity on;
        # X-Header 值，用于判断是否生效
        pagespeed XHeaderValue "Powered By xqiushi";

        # 配置服务器缓存位置和自动清除触发条件（空间大小、时限）
        pagespeed FileCachePath "/var/ngx_pagespeed_cache/";
        pagespeed FileCacheSizeKb 2048000;
        pagespeed FileCacheCleanIntervalMs 43200000;
        pagespeed FileCacheInodeLimit 500000;
        # 过滤规则
        pagespeed RewriteLevel PassThrough;

        # 过滤WordPress的后台(可选配置，可参考使用)
        pagespeed Disallow "*/admin/*";
        pagespeed Disallow "*/login.php*";

        # 移除不必要的url前缀，开启可能会导致某些自动加载功能失效
        pagespeed EnableFilters trim_urls;
        # 移除 html 空白
        pagespeed EnableFilters collapse_whitespace;
        # 移除 html 注释
        pagespeed EnableFilters remove_comments;
        # DNS 预加载
        pagespeed EnableFilters insert_dns_prefetch;
        # 压缩CSS
        pagespeed EnableFilters rewrite_css;
        # 合并CSS
        pagespeed EnableFilters combine_css;
        # 重写CSS，优化加载渲染页面的CSS规则
        pagespeed EnableFilters prioritize_critical_css;
        # google字体直接写入html 目的是减少浏览器请求和DNS查询
        pagespeed EnableFilters inline_google_font_css;
        # 压缩js
        pagespeed EnableFilters rewrite_javascript;
        # 合并js
        pagespeed EnableFilters combine_javascript;
        # 优化内嵌样式属性
        pagespeed EnableFilters rewrite_style_attributes;
        # 压缩图片
        pagespeed EnableFilters rewrite_images;
        # 不加载显示区域以外的图片
        pagespeed LazyloadImagesAfterOnload off;
        # 图片预加载
        pagespeed EnableFilters inline_preview_images;
        # 移动端图片自适应重置
        pagespeed EnableFilters resize_mobile_images;
        # 图片延迟加载
        pagespeed EnableFilters lazyload_images;
        # 雪碧图片，图标很多的时候很有用
        pagespeed EnableFilters sprite_images;
        # 扩展缓存 改善页面资源的可缓存性
        pagespeed EnableFilters extend_cache;
        # 启用JavaScript库卸载 
        pagespeed EnableFilters canonicalize_javascript_libraries; 
        # 删除带默认属性的标签 
        pagespeed EnableFilters elide_attributes; 
        # 更换被导入文件的@import，精简CSS文件 
        pagespeed EnableFilters flatten_css_imports; 

        # 将 meta 转换为 header 
        location ~ "\.pagespeed\.([a-z]\.)?[a-z]{2}\.[^.]{10}\.[^.]+" { add_header "" ""; }
        location ~ "^/ngx_pagespeed_static/" { }
        location ~ "^/ngx_pagespeed_beacon$" { }
        location /ngx_pagespeed_statistics { allow 127.0.0.1; deny all; }
        location /ngx_pagespeed_message { allow 127.0.0.1; deny all; }
```
### 重启nginx
再次检测是否正常并重启
```
nginx -t
systemctl restart nginx
```
chrome打开网站某页.打开"发者工具",netwoek中查看X-Header有如下,说明已经安装好了:

	x-page-speed:Powered By xqiushi
或者,使用如下命令查看:
```
curl -I  https://xiangqiushi.com  | grep X-Page-Speed
```

同样会返回值:

```
X-Page-Speed: Powered By xqiushi
```
## Redis支持
如果有Redis,也可以将pagespeed添加到Redis中.这样的好处在于图片也可以写入到内存中,大大的提高访问速度.(PS:实际上这个并不完美,有BUG).

在上述位置,也就是xiangqiushi.com.conf的server{}中添加.

```
# redis支持
pagespeed RedisServer "127.0.0.1:6379";
```

## 小结
其实呢,提速效果还是不明显,而且,图片到加速方面还有许多bug,暂时我的能力解决不了.
所以我们 **有时候的折腾并不是为了有一个明显增益的结果,而是享受这个折腾的过程,人生的一切就是这样无聊地折腾着,我们只需要评价,某次折腾是否是愉快的.**
参考与注释
---
<a name="1"></a>[1]. [Build ngx_pagespeed From Source](https://modpagespeed.com/doc/build_ngx_pagespeed_from_source) PageSpeed [↩](#-1)

