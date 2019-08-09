---
title: 修好积灰的U盘
categories:
- tutorial
tags:
- 硬件
---

>可能大家像我一样经历过U盘出现故障的情况，故障类型有很多，如`U盘写有保护`，`无法格式化`，`U盘的容量显示不出来`，像这样的U盘可能就放着放着就积灰了，那么这时候你可以尝试这种修理U盘的操作--`量产`（U盘出厂的最后一道工序，也可以用来解决上面的问题），本文旨在介绍一下关于量产的几个基本操作，以及写下我量产金士顿U盘的过程。


## 下载芯片无忧

`芯片无忧`，又叫`ChipEasy`，用途是用来查看U盘的详细信息，下载地址可以直接百度，或者使用以下链接[ChipEasy](http://dl.pconline.com.cn/download/91928-1.html)，选择任意一个`本地下载`就可以了，使用方式很简单，在插上U盘的前提下打开芯片无忧，选择你的U盘，接着就能可到具体的信息了，这里贴的是我的U盘的信息。

![ChipEasyDetail](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/181006RepairUdisk/ChipEasy_Detail.png?raw=true)

需要注意的是这里显示的`芯片制造商`，`芯片型号`，`闪存颗粒`,以及`固件版本`，你可以直接百度它找合适的工具，如群联PS2251-07海力士05.00.50，或者到[U盘量产网](http://www.upantool.com/),搜索`芯片型号`，如PS2251-07,找到符合你闪存颗粒和固件版本的量产工具。


## 使用量产工具

我这里使用成功的量产工具是这个[MPALL_F1_7F00_DL07_v503_0A](https://pan.baidu.com/s/1W8VMdVX2SZ8vwpiT8DP9Rw),使用的是这个[量产教程](http://www.upantool.com/jiaocheng/liangchan/Phison/11074.html),不过比较坑的是使用这个量产教程没办法成功，虽然U盘能显示出来（如果显示不出来的可以在配置好软件后试下插拔U盘，重启软件，或者点下软件的Update按钮），但是老是会爆红，后来查了下网上和我相同问题的情况，发现只要不勾选一个按钮就行，具体步骤如下。

![step1](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/181006RepairUdisk/step1.png?raw=true)
![step2](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/181006RepairUdisk/step2.png?raw=true)
![step3](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/181006RepairUdisk/step3.png?raw=true)
![step4](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/181006RepairUdisk/step4.png?raw=true)
![step5](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/181006RepairUdisk/step5.png?raw=true)
![step6](https://github.com/iAoe444/iAoe444.github.io/blob/master/assets/images/181006RepairUdisk/step6.png?raw=true)

## 最后几点需要注意的

* 量产有风险，量产有风险，量产有风险。
* 量产不一定能解决所有U盘问题，有些还需要拆解U盘来短接。
* 量产工具要对应 `芯片制造商`，`芯片型号`，`闪存颗粒`,以及`固件版本`，否则量产很大可能会失败。
* 量产工具比较难找，至少我花了几个小时试正确的量产工具QAQ，本人也只是走马观花了一下，可以到相关的群里，或者找大神询问，或者多百度。
* 量产过后的U盘可能会读写速度减慢。
* 本文旨在介绍量产，这里的量产教程只对应我的这款型号，具体可以找到在U盘量产网找到工具后在页面查看有无相关教程。
* 善用搜索工具