---
title: "【区块链】Dogecoin 狗狗币挖矿教程"
categories: [ "区块链" ]
tags: [  ]
draft: false
slug: "doge-mining"
date: "2021-05-21 10:57:00"
---

最近一段时间狗狗币很火，价格暴涨。狗狗币挖矿其实很简单，只需要下载一个狗狗币钱包，然后下载一个挖矿程序，配置一下钱包地址，就可以开始挖了。

### 下载官方钱包并获取钱包地址

#### 下载狗狗币钱包

前往狗狗币官网：

> https://dogecoin.com/

由于目前用的是 Windows，所以下面教程都以 Windows 为例。

官网首页，下载一个狗狗币钱包，如下图所示，建议下载 Dogecoin Core。

![dogecoin 官网][1]

下载之后进行安装，安装的时候安装到一个剩余空间比较大的硬盘，因为它需要同步数据，目前（截止2021-05-21）数据大概有 47.8 GB，并且会随时间一直增长。

安装之后打开钱包，第一次打开会有一个提示页面，点击隐藏即可。然后点击 Much Receive，可以新建一个钱包地址，如图所示，基本上什么都不用填，直接点请求付款即可。

![收款][2]

此外，也可以在 File -> Much receiving addresses 里面看到钱包地址。

![收款地址][3]

至此，我们就有了一个狗狗币钱包地址。

### 下载 lolMiner 挖矿软件

下载地址：
> https://github.com/Lolliedieb/lolMiner-releases/releases

### 修改挖矿软件配置开始挖矿

1、修改挖矿配置
下载之后解压 lolMiner，解压出来之后，新建一个 `mine_doge.bat` 文件，右键选择编辑，或者直接用记事本打开。

将以下内容粘贴进去

```shell
@echo off
 
setlocal enableDelayedExpansion
 
Rem #################################
Rem ## Begin of user-editable part ##
Rem #################################
 
set "POOL=ethash.unmineable.com:3333"
set "WALLET=DOGE:钱包地址.lolMinerWorker"										
 
Rem #################################
Rem ##  End of user-editable part  ##
Rem #################################
 
lolMiner.exe --algo ETHASH --pool ethash.unmineable.com:3333 --user DOGE:钱包地址.矿工名字#cbnm-tuds --4g-alloc-size 4024 --keepfree 8 
timeout 10
```

**将上面内容中，第10行和第16行的这两个钱包地址改成你的狗狗币钱包地址，矿工名字可以随意取，主要用于区分不同机器。**

**注意：仅修改上面的“钱包地址”这几个字，前面的 DOGE: 以及后面的 .lolMinerWoekr 等都不要改。**

### 开始挖矿
修改完成后保存，然后双击这个文件（mine_doge.bat），就可以开始挖矿了。一般来说会显示下面这样的提示信息，没有报错就行。

![正在挖矿][4]

### 前往矿池查看收益
耐心等待半小时到一小时后，我们就可以前往矿池查看收益了。

因为用的是 unmineable 这个矿池，所以前往下面地址：

> [https://unmineable.com](https://unmineable.com/?ref=cbnm-tuds)

然后搜索 DOGE，或者直接选择 Dogecoin，进入狗狗币页面。

![unmineable doge][5]

然后输入你的狗狗币钱包地址，如下图所示，就可以查看收益了。一般开始挖矿之后半小时到一小时之后才有收益。

![unmineable doge address][6]

祝各位挖矿愉快~


  [1]: https://cdn.rhyland.cn/typecho/2021/05/21/dogecoin.png
  [2]: https://cdn.rhyland.cn/typecho/2021/05/21/receive.png
  [3]: https://cdn.rhyland.cn/typecho/2021/05/21/address.png
  [4]: https://cdn.rhyland.cn/typecho/2021/05/21/mining.png
  [5]: https://cdn.rhyland.cn/typecho/2021/05/21/unmineable-doge.png
  [6]: https://cdn.rhyland.cn/typecho/2021/05/21/unmineable-doge-address.png