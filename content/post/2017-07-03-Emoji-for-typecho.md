---
layout:     post
title:      Emoji表情，美化Typecho
subtitle:   不再折腾的Typecho
date:       2017-07-03
author:     xqiushi
header-img: img/post-bg-hacker.jpg
catalog: true
tags:
    - Typecho
    - 术
    - Emoji
---


### 1.前言

我天真的以为Emoji表情都是那可爱呢。天天哭着闹着想在微信或QQ里发Emoji表情。
![Mobie_Emoji.jpg][1]
### 2.typecho支持Emoji
于是通过修改据库Mysql数的编码，然后修改config.inc.php文件中'charset' => 'utf8mb4',修改后的代码如下：
```
/** 定义数据库参数 */
$db = new Typecho_Db('Pdo_Mysql', 'typecho_');
$db->addServer(array (
  'host' => '127.0.0.1',
  'user' => 'typecho',
  'password' => 'xqiushi',
  'charset' => 'utf8mb4',  # 关键位置修改为：utf8mb4
  'port' => '3306',
  'database' => 'xqiushi',
), Typecho_Db::READ | Typecho_Db::WRITE);
Typecho_Db::set($db);
```
### 3.如梦初醒
先看这个几个“可爱的”表情：😂😭😪一大波非苹果系统生态用户懵逼了，这是什么鬼，这么丑，和微信、QQ里的emoji表情简直不是一会事。可是你换成IOS或MacOS看一下呢，原来不同的环境Emoji表情是有差别的。
> MacOS或IOS环境下，无论是Chrome、Safari还是Firefox都是都是这样子：

![MacOS_emoji.png][2]

> Windwos下是这样子的：
+ IE下是这样子的：
![Emoji_IE.png][3]
+ Chrome使用[Chomoji插件](https://chrome.google.com/webstore/detail/chromoji-emojis-for-googl/negakbijaemdgbhklopmghphgaeadmpo)(ps:无插件跟IE一样丑):
![chromenoemjiPuls.png][4]
+ Firefox原生就是这样子:
![window_firefox.png][5]
### 4.其他Emoji资源
+ 对比不同环境下的Emoji表情图示：

  [Face With Tears of Joy](http://emojipedia.org/face-with-tears-of-joy/)
  ![emojipedia.png][6]

+ 大图标方式显示，可复制粘贴：[✂ Copy and 📋 Paste Emoji 👍](http://getemoji.com)

  ![Copy and Paste Emoji.png][7]
### 5.小结
同样的Emoji表情，在不同的环境中，表现出了距大的差别。其实人也是一样，硬件条件相同，不同的行业或岗位，可能发挥的力量是完全不一样。
如果大环境不好的时候，我们应该更加努力地提高软件来弥补缺陷。


[1]: https://p.Reddy.Wang:9227/usr/uploads/2017/07/978631445.jpg
[2]: https://p.Reddy.Wang:9227/usr/uploads/2017/07/885146341.png
[3]: https://p.Reddy.Wang:9227/usr/uploads/2017/07/2633218064.png
[4]: https://p.Reddy.Wang:9227/usr/uploads/2017/07/2143562261.png
[5]: https://p.Reddy.Wang:9227/usr/uploads/2017/07/2888114532.png
[6]: https://p.Reddy.Wang:9227/usr/uploads/2017/07/1381933928.png
[7]: https://p.Reddy.Wang:9227/usr/uploads/2017/07/1053496030.png
