---
title: ubuntu放wifi(热点)给安卓手机
date: 2016-11-30 09:19:38
tags: [ubuntu,wifi]
categories: [瞎折腾]
photos: ['http://p1.bpimg.com/567571/b7236abd9eaa68b0.png']
---

![ubuntu](/img/ubuntu.png)

# 前言

由于宿舍路由器坏了，手机流量急剧下降（刷twitter...)，所以只能出此下册（个人不太喜欢电脑放wifi）。

# ubuntu16.04lts软件中心安装kde5-nm-connection-editor

> 注：ubuntu14.04lts该软件为kde5-nm-connection-editor。

![install kde](/img/kdeInstall.png)

# 在终端打开kde5-nm-connection-editor

> 注：ubuntu14.04lts:在终端输入kde-nm-connection-editor

![open kde](/img/openKde.png)

# 添加无线(共享)连接

![add wireless(shared)](/img/shareWIFI.jpg)

# 填写连接信息：连接名称，SSID,模式。设置热点密码

![fill blank](/img/fillBlank.png)

![set passwd](/img/setPasswd.png)

# **记得连接有线网络**，不然是释放不了wifi(热点的),点击右上角网络连接的icon,选择连接隐藏网络，选择你设置的连接名称

![connect to wifi](/img/connectToWIFI.jpg)

![connect to blue'wifi](/img/connectToBlueWIFI.jpg)

> 点击连接

![blue'wifi](/img/blueWIFI.png)

**这里有点小问题**：好像不能设置wifi安全性，连不上**blue'wifi**。没关系，我们可以另辟蹊径。

![set wifi](/img/setWIFI.jpg)

![set blue'wifi](/img/setBlueWIFI.png)

> 打开之后切换到Wi-Fi安全性的tab那里

![set wifi security](/img/setWIFISec.png)

# 都设置好了之后就拿去你手机连接你设置的热点吧～

> 可能刚设置完，需要等几分钟才能生效的。

![mobile connect to hot point](/img/mobileWIFI.jpg)

![success](/img/succeedConnect.jpg)

# 如果还是设置不好，那就参考下面的链接

- [Share Internet Connection With Android in Ubuntu 14.04](http://ubuntuhandbook.org/index.php/2014/06/share-internet-with-android-ubuntu-1404/)

- [ubuntu14.04怎么建立wifi热点？](http://www.jb51.net/os/Ubuntu/344088.html)
