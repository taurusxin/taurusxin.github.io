---
title: "Windows 技巧分享（一）安装篇"
categories: [ "操作系统" ]
tags: [ "Windows 10 技巧" ]
draft: false
slug: "windows-skill-01"
date: "2021-03-21 13:18:00"
---

### 回忆

在介绍本系列文章之前，先谈一谈我对于 Windows 系统的了解。

对于 Windows 系统来说，我是一个忠实的老用户了，我记忆中，小学上电脑课的时候，那时我对于计算机知识是几乎为0的，但我记忆最清楚的就是那时候机房的操作系统是 **Windows 2000**。那个界面，怎么说，就永远也不可能忘记的，直接上个图!
![Windows 2000][1]

那时候我学习的第一个软件就是 Windows 的经典画图。

直到2010年，5年级，学校机房迎来一次重大升级，从原先的老奔腾，换成了酷睿2，系统也换成了那时候最流行的 **Windows XP**，Windows 7 那时候还不流行，因为 Windows 7 是2009年发布的。记得那时候我是电教委员，每次上课前帮老师开机，开机后机房内一片绿色壁纸亮起。现在我知道，我的青春不会再回来了QWQ。

### 装系统

人生苦短，我用 Ghost。这句话放在五六年前，可能还会有人用 Ghost 去安装 Windows 7，甚至XP系统。一键安装，驱动自带，常用软件，360全家桶……

在过去的时间里，我一直都是用 Windows PE安装，我用过的PE有大白菜、老毛桃等，后来改为极度简洁的微PE，直到前两年，我换成了IT天空的**优启通**，应该是目前市面上最好用的PE了。不过直到我上了大学之后，我发现PE无非也就是安装系统用，这么多功能都不常用到，至于说清除密码，我想也不太会有人现在还会中当年特别流行的改系统密码勒索病毒了吧。

---

##### 于是我就投向了微软官方原版镜像的怀抱之中

#### 启动工具制作

这也是官方最推荐的做法，就是使用官方的制作工具制作启动盘

首先要准备一个U盘，16G 为最佳，这里推荐我自己用的闪迪 CZ73 16G，日常就作为给同学、朋友或老师的系统安装盘。

然后去微软官方网站（https://www.microsoft.com/zh-cn/software-download/windows10），下载制作工具，截止发稿，目前官方释出最新版为 `Windows 10 2020 年 10 月更新`（20H2-October Update）

选择你的U盘制作就可以了，注意一定要选择 Windows 10 的 **不带家庭版 **和 **64位系统**。

#### 镜像下载制作

当然也可以使用原版ISO进行安装，这个方法与使用制作工具同理，都是将ISO写入U盘，不过一个是官方工具写入，这个是自助写入。强迫症总规是跟着官方的办法来嘛。

依旧是打开官网下载链接（https://www.microsoft.com/zh-cn/software-download/windows10），看到如下界面。

![Windows 10 下载页面 1][4]

那么问题来了，如果是 Windows 操作系统，打开该网站则会让你下载升级工具，没办法进入如图所示页面，该怎么办

这时候就需要插件来辅助了，如果不想用插件，可以用 Chrome 自带的 UA 修改，具体方法见链接（https://www.imooc.com/article/253385）

插件则使用 User-Agent Switcher and Manager，它长这样，在 Chrome 应用商店和 Edge 应用商店都能下载到。

![User-Agent Switcher and Manager][6]

只要将你的访问UA切换为 macOS 就行了。
![Windows 10 下载页面 2][7]

选择 Windows 10，选择你喜欢的语言，选择64位版本下载即可。

下载完即会得到ISO的原版镜像，可以使用软碟通（UltraISO）写入你的U盘，这是第二种方法。

#### 安装

U盘插入电脑，开机进入BIOS，一路下一步之后，选择你的空白硬盘，格式化即可。

- 不用担心4K对齐，微软帮你考虑到了

- 不用管磁盘分区表类型，默认就是GPT，现在电脑也只推荐GPT。

如果是重装系统，则用工具将你作为系统盘的硬盘的所有分区（包括主分区、ESP分区、MSR分区等统统删光）删除，留下未分配空间即可。
![Windows 10 安装][8]

（图示为32G大小系统盘，格式化后的最终形态）

### 结尾

本章介绍了最基础的，也是人人必会的原版安装系统篇。后面的系列文章将会介绍一些我对于 Windows 10 的日常使用技巧及我是如何管理我自己的系统内容的。


  [1]: https://cdn.rhyland.cn/typecho/2021/03/21/windows2000.jpg
  [4]: https://cdn.rhyland.cn/typecho/2021/03/21/windows10-download.png
  [6]: https://cdn.rhyland.cn/typecho/2021/03/21/ua-switcher.png
  [7]: https://cdn.rhyland.cn/typecho/2021/03/21/windows10-download-2.png
  [8]: https://cdn.rhyland.cn/typecho/2021/03/21/windows10-install.png