---
title: "大幅度优化你的 VPS 网速"
categories: [ "程序人生" ]
tags: [ "vps" ]
draft: false
slug: "linux-network-optimize"
date: "2023-05-21 23:59:00"
---

## 网速优化脚本

我在买到手一台新的独立服务器或者是虚拟服务器(VPS)时，第一件事情就是先对服务器的网络进行优化，尤其是海外的服务器到国内的速度，将会大幅度提升。

我将要做的内容全都整合进一个脚本，使用以下命令行即可执行。

```shell
wget https://gist.githubusercontent.com/taurusxin/a9fc3ad039c44ab66fca0320045719b0/raw/3906efed227ee14fc5b4ac8eb4eea8855021ef19/optimize.sh -o optimize.sh && sudo bash optimize.sh
```

具体做的事情，有以下几点：

1. 更新软件源和软件包。
2. 安装 `haveged` 优化 Linux 随机数生成器，可以解决在某些情况下，系统熵过低的问题。
3. 最关键的，内核网络参数优化，具体就是对一些 IPv4 的 TCP 参数做了调整，另外还启用了 Linux 原版 BBR 拥塞控制算法，这个算法是 Google 开发的，可以大幅度提升网络速度。

### 具体测速

如图所示是我的一台位于香港的 VPS，在使用脚本之前的速度如下

![before](https://cdn.taurusxin.com/hugo/2023/05/22/before.jpg)

安装脚本后，测速如下

![after](https://cdn.taurusxin.com/hugo/2023/05/22/after.jpg)
