---
title: "Linux 下 Node Source 安装的 npm 权限问题"
categories: [ "Linux" ]
tags: [ "nodejs", "npm", "linux" ]
draft: false
slug: "npm-prefix"
date: "2022-03-20 17:16:02"
---

## 前言

近期安装了 Linux 系统作为主力机器的第二系统，双启动引导。

系统是 `Pop!OS 20.04 LTS`，基于 `Ubuntu 20.04`

由于前端开发及部署需求，需要安装 node.js，于是使用 node source 提供的 node 发行版。

由于安装完成后，npm 存储包的目录为 `/usr`，使用时必须加 `sudo`，为了避免这种情况和权限安全问题，需要将 npm 的包存储到用户目录下，并且强烈不建议使用 sudo 来全局安装包到 `/usr` 目录下

## 方法

检查当前 `npm` 安装的包路径前缀

```bash
npm config get prefix
```

如果返回 `/usr` 则执行以下命令

```bash
mkdir ~/.npm-global
export NPM_CONFIG_PREFIX=~/.npm-global
export PATH=$PATH:~/.npm-global/bin
```

这个步骤创建了一个 npm 目录在用户主目录下并且令 `npm` 指向这里

想要永久生效，执行以下命令将其导出至用户 Shell 的配置文件

```bash
echo -e "export NPM_CONFIG_PREFIX=~/.npm-global\nexport PATH=\$PATH:~/.npm-global/bin" >> ~/.bashrc
```