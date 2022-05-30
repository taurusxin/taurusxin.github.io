---
title: "2022 年程序员一定要做的一件事情 - 更换你的 SSH 公钥算法"
categories: [ "Linux" ]
tags: [ "ssh", "linux" ]
draft: false
slug: "ssh-key-ed25519"
date: "2022-05-30 19:50:00"
---

## 前言

常见的 SSH 登录密钥使用 RSA + SHA-1 算法。RSA 经典且可靠，但性能不够理想。

只要你的服务器上 OpenSSH 版本大于 6.5（2014 年的古早版本），就可以利用 Ed25519 算法生成的密钥对，减少你的登录时间。如果你使用 SSH 访问 Git，那么就更值得一试。

Ed25519 的安全性在 RSA 2048 与 RSA 4096 之间，且性能数十倍于 RSA 算法。

## 起因

自 2022 年 4 月 21 日 Canonical 发布了 Ubuntu 22.04 版本，并且将 Ed25519 加入了 SSH 登录密钥算法，禁用了 RSA 公钥算法，导致我在 iOS 上 ServerCat 应用程式的 RSA 密钥无法连接到服务器，经过排查之后发现 Ubuntu 22.04 将 OpenSSH 版本升级到 8.9，且 OpenSSH 团队宣布`ssh-rsa`在 8.8 版本中将默认禁用签名方案。

![SSH 服务端守护进程版本](https://cdn.rhyland.cn/hugo/2022/05-30/image-20220530200309584.png)

这是导致我无法使用 RSA 秘钥连接到我所有已经升级到 Ubuntu 22.04 版本的虚拟机

## 解决方案

解决方案有两种，启用过时的方案，或者是使用 Ed25519 公钥算法，**后者**是最为推荐的，数十倍的性能及不低于 RSA 2048 的安全性。

### 启用抛弃的 RSA 算法

使用编辑器打开远程主机上的 sshd 的配置文件，配置公钥接收 RSA 算法

```shell
echo 'PubkeyAcceptedKeyTypes +ssh-rsa' >> /etc/ssh/sshd_config
systemctl restart sshd.service
```

### 换用 Ed25519 公钥算法

更换算法意味着所有的机器都将要更换公钥，需要替换所有机器的 authorized_keys 文件中的可信公钥。

```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
cat ~/.ssh/id_ed25519.pub
```

将新的公钥添加到你目标远程主机上的 ~/.ssh/authorized_keys 文件末尾

## 结语

强烈建议替换公私钥体系的算法，提高安全性的同时也换来了高性能。
