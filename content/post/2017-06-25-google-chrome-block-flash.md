---
layout:     post
title:      Google Chrome 59 浏览器macOS版及禁用flash插件的方法
subtitle:   Chrome是好，可惜内存受不了
date:       2017-06-25
author:     xqiushi
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - Google
    - Chrome
    - MacOS
    - 技
---

### 1.前言
众所周知，Chrome可以算是当今天最优秀的浏览器之一，其实几乎可以省略后面的之一。但是由于第三方插件Flash是Chrome的朋友，同时一直是苹果的敌人。很多时候，敌人的朋友就是敌人。
### 2.化敌为友
"A country does not have permanent friends, only permanent interests." 这是十九世纪英国首相帕麦斯顿的一句话。用来今天说，Chrome也不会是“苹果生态链”永恒的敌人。

至少在我的使用环境中：
MacOS  | Chrome
------------- | -------------
10.12.6 Beta (16G18a) | 59.0.3071.109

时间 | 地点 | 人物 | 
----|-----|----
3月5日 | 北京 | 姚明
3月7日 | 上海 | 韩寒



已经有非常明显的改善，不论是占CPU的情况和内存的情况，都在降低。如果感觉Safari不给力的，一定要试试Chrome59版。
### 3.禁用Flash
Chrome基本上高耗能，换取高性能。而Falsh与Mac的矛盾一直存在。为了更好的能量体验，我们要自己手动禁用一下Flash，这个方法比较原始，但是直观。

+ chrome://components 查看当前插件及版本更新情况。

#### 方法一：

+ 由于<del>chrome://plugins</del> 已经用了。所以chrome://settings/content/flash这里如图设置为禁用。

#### 方法二：
- Chrome://flash，如图可以看到插件安装位置
  /Users/*youname*/Library/Application Support/Google/Chrome/PepperFlash/*26.0.0.131*/。并复制该位置
- Finder下输入 ⌘（ Command 键）+ ⇧（Shift 键）+ G，粘贴上面位置。
- 删除 PepperFlashPlayer.plugin
- 重启Chrome浏览器
- 方法一与方法二任意选一个就可以
### 4.其他测试
例如如：处于实验阶段的效果：
+ chrome://flags
其他需要的效果使用查看：
+ Chrome://about
### 5.纠结
我现在有点纠结了，到底用Safari还是Chrome呢？

