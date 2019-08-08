---
title: 极路由1s锐捷配置教程
categories:
- tutorial
tags:
- 路由器
---


>这款极路由内置一个简易的锐捷认证，只需要几步配置就能完成认证操作，原理可以类似`mentohust`这款软件，下面，我将通过几步简单的步骤来介绍极路由1s的配置过程，网络环境为`2018年的广师大网络`，`锐捷客户端版本为4.96`，路由器系统版本为`HC5661A - 1.3.5.18462s `



## 下载一个小文件

可以通过 [百度网盘](https://pan.baidu.com/s/1XBUWX4TrOeWUQkcDnH91ug) 下载`template.mpf`这个小文件。

如果链接失效，可以通过 [google code](https://code.google.com/archive/p/mentohust/downloads?page=2) 这个页面（需科学上网）下载抓包工具，解压后就有这个小文件了。

![1537188175774](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/2018-08-12-%E6%9E%81%E8%B7%AF%E7%94%B11s%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/1537188175774.png?raw=true)



## 第一次使用路由器

> 这里的说明适合`未用过路由器的人`，配置过路由器的可以用其他方法或者直接跳过

1. **第一次开机**

   先给路由器通电，开机后用网线连接`路由器黄色的口`和`电脑的网线接口`，电脑的wifi最好关掉

   ![1](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/2018-08-12-%E6%9E%81%E8%B7%AF%E7%94%B11s%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/1.jpg?raw=true)

2. **打开浏览器**

   如果是第一次开机的话浏览器后直接跳转，如果不是的话就登陆`hiwifi.com`,直接进入下面一步

   ![1537186425054](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/2018-08-12-%E6%9E%81%E8%B7%AF%E7%94%B11s%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/1537186425054.png?raw=true)

   ***

   ![1537186461019](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/2018-08-12-%E6%9E%81%E8%B7%AF%E7%94%B11s%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/1537186461019.png?raw=true)

   ***

   ![1537186526713](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/2018-08-12-%E6%9E%81%E8%B7%AF%E7%94%B11s%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/1537186526713.png?raw=true)

   ***

   ![1537186590336](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/2018-08-12-%E6%9E%81%E8%B7%AF%E7%94%B11s%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/1537186590336.png?raw=true)

   ***

   ![1537186623248](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/2018-08-12-%E6%9E%81%E8%B7%AF%E7%94%B11s%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/1537186623248.png?raw=true)

## 设置路由器

   1. **登陆路由器**

      如果刚刚配置成功后会进入这么一个页面，填入刚刚设置的密码后点击ok就行

      ![1537186810185](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/2018-08-12-%E6%9E%81%E8%B7%AF%E7%94%B11s%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/1537186810185.png?raw=true)

   2. **mac地址克隆**

      在首页的地方点击mac地址克隆

      ![2](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/2018-08-12-%E6%9E%81%E8%B7%AF%E7%94%B11s%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/2.png?raw=true)

      克隆你电脑的mac地址后点击保存

      ![3](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/2018-08-12-%E6%9E%81%E8%B7%AF%E7%94%B11s%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/3.png?raw=true)

   3. **配置锐捷认证**

      点击页面左侧的互联网，再点击锐捷认证

      ![1537187072343](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/2018-08-12-%E6%9E%81%E8%B7%AF%E7%94%B11s%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/1537187072343.png?raw=true)

      配置信息如下

      ![1537187202939](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/2018-08-12-%E6%9E%81%E8%B7%AF%E7%94%B11s%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/1537187202939.png?raw=true)

      选择mentohust.mpf里的`选择文件` 按钮，选择我们一开始下载的文件，点击保存后即可

      ![1537187302731](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/2018-08-12-%E6%9E%81%E8%B7%AF%E7%94%B11s%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B/1537187302731.png?raw=true)

   4. **连接内网**

      将之前内网连接电脑的网线接到路由器的`黄色的口`,也就是WAN口上，等待一段时间，打开浏览器上随便一个网站，能连上就是成功了。

