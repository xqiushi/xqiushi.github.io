---
layout:     post
title:      synergy共享鼠标和键盘
subtitle:   有大神告诉我：现在蓝牙鼠标自带切换功能
date:       2017-09-07
author:     xqiushi
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - synergy
    - 技
---

#### 前言

自从有了共享单车后，“共享”这一词频繁出现在生活，共享床，共享汽车、共享车位等都属于共享经济的新概念。但是今天说的共享鼠标和键盘而不是这么一会事。
#### 多台电脑
这里说的是共享鼠标和键盘，不是指的多人共享，而是指的一个人的多台电脑之间共享鼠标和键盘。比如，很多专业软件（如ArcGIS）必须运行在windows系统上，但是为了更提高视觉体验和阅读效果（Safari的阅读模式）你得必须使用MacOS。因此常常会使用几台电脑来处理不同的工作生活需求。那么问题就来了，如果每一个电脑都要各自一套鼠标和键盘，那岂不是你的桌子上至少两个鼠标和键盘，这样换来换去使用，非常的麻烦，效率非常低，有时候还容易因为拿错了而进行了错误的操作。这时候我们需要将鼠标与键盘减少到一套，而不论有几台电脑。
#### synergy共享
其实网上的软件非常多，目前我主要使用的就是synergy，主要有这些特点：
- 从一台电脑中拖放文件到另一台电脑(Windows和macOS)；
- 网络基础(IP)，切换时延时较小；
- 分享您的剪贴板(复制与粘贴)。

切换操作也非常简单，只要你向不同方向的电脑移动指针就可以在不同的电脑屏幕之间工作，感觉就像是只有一台主机和两个显示器一样。而且synergy可以台支持 Win/Mac/Linux。正如[官方](https://symless.com/synergy)所说的：“让您的电脑结合升华成为综合性的体验”。
#### synergy下载
Synergy现已正式改为收费软件了！以前还是[nightly版](https://symless.com/nightly)可以下载，但那是以前。好在软件是开源的，可以自己编译。如果操作困难，可以先试试这个：[Brahma Dev](https://github.com/brahma-dev/synergy-stable-builds)编译的。
#### synergy配置
略
![synergy.png][1]

#### 声明
Synergy源码所有权归[Synergy官网](https://symless.com)所有；编译版本所有权归[Brahma Dev](https://github.com/brahma-dev/synergy-stable-builds)所有。如有侵犯,请告之！


[1]: https://p.Reddy.Wang:9227/usr/uploads/2017/09/153489841.png
