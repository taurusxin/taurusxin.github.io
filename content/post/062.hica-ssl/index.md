---
title: "使用 HiCA 签发免费的半年 IP / 通配符域名 SSL 证书"
categories: [ "程序人生" ]
tags: [ "SSL证书", "IP证书", "通配符证书" ]
draft: false
slug: "hica-ssl"
date: "2022-07-24 12:11:00"
---

## HiCA

官网：<https://www1.hi.cn/>

![HiCA 官网](https://cdn.rhyland.cn/hugo/2022/07-27/image-20220727235855829.png)

2022 年 7 月 22 日，国产数字证书品牌 HiCA 正式上线，提供免费的半年 IP 或者通配符域名 SSL 证书，非常实用。

7.27 更新：
官网文档已上线，移步 <https://www1.hi.cn/docs/intro> 获得更多教程

## acme.sh

HiCA 默认只提供 acme 协议签发证书，并且禁止任何 GUI 的形式对外公开服务，我们使用最简单的 acme.sh 脚本进行自动签发证书和更新证书。

使用以下命令行安装 acme.sh 自动化脚本，并且替换 `my@example.com` 为你的邮箱

```shell
curl https://get.acme.sh | sh -s email=my@example.com
```

## IP 证书

免费的 IP SSL 证书非常少，现在能有一家国产公司站出来，真的为之感到骄傲，并且支持 IPv4 / IPv6 的 IP 证书

签发 IP SSL 使用 HTTP 文件验证的方式，首先需要启动一个 Web 服务器，要求是访问要签发证书的 IP 能够直接访问到该站点，可以使用宝塔面板，或者直接安装 nginx 的方式，简单快捷。

### 宝塔面板

![宝塔面板新建站点](https://cdn.rhyland.cn/hugo/2022/07-24/image-20220724121747259.png)

使用宝塔面板新建一个站点，域名填写你的 IP，然后测试直接访问 IP，成功看到页面即可。

记住此站点的根目录 `/www/wwwroot/你的IP`

### 任意 HTTP 服务器

直接安装 nginx，以 Ubuntu 为例

```shell
sudo apt install nginx
```

安装完成之后即可访问 IP，记住必须是 80 端口。

Ubuntu 默认的网站根目录位于 `/var/www/html`

### 签发

进入到你的服务器终端，输入以下命令行

```shell
acme.sh --issue \
    -d [IPv4/IPv6] \
    --webroot [网站根目录] \
    --server https://acme.hi.cn/directory
```

例如为 IP 1.1.1.1 申请证书，则执行

```shell
acme.sh --issue \
    -d 1.1.1.1 \
    --webroot /www/wwwroot/1.1.1.1 \
    --server https://acme.hi.cn/directory
```

稍等片刻，提示签发成功即可下载证书，位于 `~/.acme.sh/xx.xx.xx.xx`

若提示 acme.sh 命令不存在，直接 cd 到 /root 目录下的 `.acme.sh` 目录下只用相对路径执行命令即可。

## 通配符证书

### 配置 DNS 自动验证

通配符证书需要进行 DNS 验证来确定域名持有人，本文以阿里云 DNS 为例。

首先在阿里云申请一个 AccessKey，用于 API 操作阿里云服务，可以使用子用户的方法，并且只授权 `AliyunDNSFullAccess` 权限

![image-20220724123219633](https://cdn.rhyland.cn/hugo/2022/07-24/image-20220724123219633.png)

使用以下命令添加环境变量

```shell
export Ali_Key="你申请的Key"
export Ali_Secret="你申请的Secret"
```

acme.sh 还提供了很多其它云服务厂商的 DNS API，详细可以参照官网 GitHub 的 Wiki

### 签发

进入到你的服务器终端，输入以下命令行

```shell
acme.sh --issue \
    -d \*.[域名] \
    -d [域名] \
    --dns dns_ali \
    --server https://acme.hi.cn/directory
```

例如为域名 taurusxin.com 申请通配符证书，则执行

```shell
acme.sh --issue \
    -d \*.taurusxin.com \
    -d taurusxin.com \
    --dns dns_ali \
    --server https://acme.hi.cn/directory
```

稍等片刻，提示签发成功即可下载证书，同样位于 `~/.acme.sh/你的域名`

若提示 acme.sh 命令不存在，直接 cd 到 /root 目录下的 `.acme.sh` 目录下只用相对路径执行命令即可。

![image-20220724124717718](https://cdn.rhyland.cn/hugo/2022/07-24/image-20220724124717718.png)
