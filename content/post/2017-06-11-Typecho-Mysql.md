---
layout:     post
title:      Typecho博客利用Mysql语句批量替换内容
subtitle:   
date:       2017-06-11
author:     xqiushi
header-img: img/post-bg-os-metro.jpg
catalog: true
tags:
    - Typecho
    - 术
---

### 1.前言

我[博客][1]搬远无数次家,好多图片都是记录了不同网址。有ArcGIS.me xqiushi.cn等等。导致的问题就是图片都是原来的地址。

要想网站图片能正常打开不开。笨办法就是要一个一个编辑，对于一个抄或写了十年人来说（虽然不到100篇文章）一个一个的编辑也太Low了吧。

AI时代，一切问题都要考虑批量出发。此时便可以使用phpmyadmin的SQL语句来实现。

### 2.普通替换

> 实例1：将原xiangqiushi.cn的所有地址，替换成xiangqiushi.com

具体代码如下： 

    UPDATE `rd_contents` SET `text` = REPLACE(`text`,'xiangqiushi.cn','xiangqiushi.com'); 


<font color=#FF1493 size=3 >rd_contents为你自己的数据库表的名字。</font>

### 3.转义替换

如果全站添加了SSL，执行上述代码后，有可能还是不出现“绿小锁”是会报错的。需要进一步替换成https网址。此时要注意使用转义字符"\",如下操作：

> 实例2：将旧http://xiangqiushi.cn的所有地址，替换成新https://xiangqiushi.com

具体代码如下： 

    UPDATE `rd_contents` SET `text` = REPLACE(`text`,'\http://xiangqiushi.cn','\https://xiangqiushi.com'); 


<font color=#FF1493 size=3 >必须使用转义字符"\"。</font>

### 4.语句推广

> <font color=#FF1493 size=3 >UPDATE *表名* SET *字段名* = REPLACE (*字段名*, '原内容', '新内容');</font>

### 5.结束

<font color=#673FB4 size=4 >所以经验教训就是：博客要多写字，少放图片。</font>

[1]: https://xiangqiush.com
