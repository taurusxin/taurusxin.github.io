---
title: "【区块链】Dogecoin 狗狗币挖矿教程"
categories: [ "区块链" ]
tags: [ '挖矿', 'doge', '狗狗币', 'eth', 'dogecoin' ]
draft: false
slug: "doge-mining"
date: "2021-05-21 10:57:00"
lastmod: '2024-03-27T18:49:00+0800'
---

最近一段时间狗狗币很火，价格暴涨。狗狗币挖矿其实很简单，只需要下载一个狗狗币钱包，然后下载一个挖矿程序，配置一下钱包地址，就可以开始挖了。

## 更新说明

（更新于2024年3月27日）这篇文章第一次写于2021年5月，已经过去了很久。由于最近这篇文章的关注度比较高，这里针对一些问题进行更新。

首先连不上矿池的问题，这个问题可能是矿池的问题，也可能是网络问题，可使用 **Clash的 TUN 模式** 或者 Proxifier 等软件可将挖矿软件走全局代理，不是那个系统的全局代理，因为挖矿软件不遵循系统代理。

其次是挖矿没有算力的，这个问题首先尝试更新你的显卡驱动，不要使用特别旧的版本，另外可以自行尝试一些其它的算法，比如新出的 EthashB3 等，可以拉到底下看新的算法教程，或者点[这里](#bzminer)。

在点进矿池首页，会有很多算法和软件，只要根据配置配置即可。

![Unmineable 矿池首页](https://cdn.taurusxin.com/hugo/2024/03/27/unmineable-index.png)

## 获取钱包地址

### 使用手机钱包

你可以在**非国区**的应用商店下载一个狗狗币钱包，比如 Trust Wallet，然后在里面创建一个通用的钱包地址，并找到狗狗币的地址。

### PC版官方全节点钱包

前往狗狗币官网：

> <https://dogecoin.com/>

由于目前用的是 Windows，所以下面教程都以 Windows 为例。

官网首页，下载一个狗狗币钱包，如下图所示，建议下载 Dogecoin Core。

![Dogecoin 官网](https://cdn.taurusxin.com/hugo/2024/03/27/dogecoin-core.png)

下载之后进行安装，安装的时候安装到一个剩余空间比较大的硬盘，因为它需要同步数据，目前（截止2024-03-27）数据大概有 141 GB，并且会随时间一直增长，如果需要全节点的数据，可以在下方评论区留言，我会提供一个全节点数据的下载地址。

安装之后打开钱包，第一次打开会有一个提示页面，点击隐藏即可。然后点击 Much Receive，可以新建一个钱包地址，如图所示，基本上什么都不用填，直接点请求付款即可。

![收款](https://cdn.rhyland.cn/typecho/2021/05/21/receive.png)

此外，也可以在 File -> Much receiving addresses 里面看到钱包地址。

![收款地址](https://cdn.rhyland.cn/typecho/2021/05/21/address.png)

至此，我们就有了一个狗狗币钱包地址。

## 开始挖矿

钱包准备好了，可以开始挖矿了

### 下载 lolMiner 挖矿软件

下载地址：
> <https://github.com/Lolliedieb/lolMiner-releases/releases>

### 修改软件配置

1、修改挖矿配置
下载之后解压 lolMiner，解压出来之后，新建一个 `mine_doge.bat` 文件，右键选择编辑，或者直接用记事本打开。

将以下内容粘贴进去

```shell
lolMiner.exe --algo ETHASH --pool ethash.unmineable.com:3333 --user DOGE:钱包地址.矿工名字#cbnm-tuds --ethstratum ETHPROXY
timeout 10
```

**将上面内容中，将【钱包地址】改成你的狗狗币钱包地址，后面的【矿工名字】可以随意取，主要用于区分不同机器。**

**注意：仅修改上面的“钱包地址”这几个字，前面的 DOGE: 以及后面的 .lolMinerWoekr 等都不要改。**

### 启动挖矿

修改完成后保存，然后双击这个文件（mine_doge.bat），就可以开始挖矿了。一般来说会显示下面这样的提示信息，没有报错就行。

![正在挖矿](https://cdn.taurusxin.com/hugo/2024/03/30/doge_mining.png)

**如果没有速率，例如显示的 Average speed 为 0 Mh/s，那么可能是你的显卡驱动问题，可以尝试更新显卡驱动。**

**或者，你可以选择更换一个软件试一下，见下面的 BzMiner。**

## BzMiner

如果用 lolMiner 没有速率，考虑换一个软件试试，这里推荐 BzMiner。

首先去官网，下载 BzMiner：

> <https://www.bzminer.com/>

点击右上角的 Release vxx.xx，下载最新版本的 BzMiner。

解压后同样的在文件夹中新建一个 `mine_doge.bat` 文件，然后将下面的内容粘贴进去：

```shell
bzminer.exe -a rethereum -p stratum+ssl://ethashb3.unmineable.com:4444 -w DOGE:钱包地址 --pool_password 矿工名字#cbnm-tuds
```

这里我们用的是新的 EthashB3 算法，所以要选择 rethereum，然后将 YOUR_ADDRESS 改成你的狗狗币钱包地址，WORKER_NAME 改成你的矿工名字。

保存之后双击这个文件，就可以开始挖矿了。

![BzMiner 挖矿界面](https://cdn.taurusxin.com/hugo/2024/03/30/doge_mining_bzminer.png)

会显示你的显卡型号、算力(图中的miner hr)等信息，如果显示的算力不是 0，那么就是在挖了。

## 查看收益

耐心等待半小时到一小时后，我们就可以前往矿池查看收益了。

因为用的是 unmineable 这个矿池，所以前往下面地址：

> [https://unmineable.com](https://unmineable.com/?ref=cbnm-tuds)

然后搜索 DOGE，或者直接选择 Dogecoin，进入狗狗币页面。

![unmineable doge](https://cdn.rhyland.cn/typecho/2021/05/21/unmineable-doge.png)

然后输入你的狗狗币钱包地址，如下图所示，就可以查看收益了。一般开始挖矿之后半小时到一小时之后才有收益。

![unmineable doge address](https://cdn.rhyland.cn/typecho/2021/05/21/unmineable-doge-address.png)

祝各位挖矿愉快~
