---
title: "Linux 自我运维（一） - Nginx 1.25.2 源码编译"
categories: [ "程序人生", "Linux" ]
tags: [ "nginx", "linux", "运维" ]
draft: false
slug: "linux-ops-1-nginx"
date: "2023-10-12T22:44:00+0800"
---

## 前言

Nginx 1.25.0 实验性地将 HTTP/3 的支持合并到 mainline 中，现在可以通过`--with-http_v3_module` 来开启对这一功能的加入，本文将手把手带你从源码开始编译一个可用于学习环境的 Nginx。

![Roadmap for QUIC and HTTP/3 Support in NGINX](https://cdn.taurusxin.com/hugo/2023/10/12/nginx-quic-http3-roadmap.png)

在阅读本文之前，你至少会基础的 Linux 指令，知道根目录下的每个文件夹的作用是什么，知道 Linux（或类 Unix）环境下的文件系统、文件权限等，知道 GNU 编译器套件，以及之前使用过面板现在想自己手动地从头来的读者。

## 准备

首先是操作系统的准备，学习 Linux 运维的你当然需要准备一个 Linux 的环境，那么对于初学者来说，我最推荐的系统是 Ubuntu Server，每个人都有自己的偏好，Debian 也是个不错的选择，但是相对来说 Ubuntu 遇到的问题会少一些，Debian 则更纯净，也很适合折腾。

本系列文章的工作都基于 Ubuntu 的最新长期支持版 22.04.3，你可以在你的物理机上以双系统的方式安装，或者使用虚拟机，更方便的话，你可以使用云服务器来完成。

## Nginx 编译

终于来到正文内容，首先 Nginx 作为一个高性能的 Web 服务器，这里就不过多介绍了，因为它的使用广泛已经受到了很多运维工程师的青睐，而基于 Nginx 的许多衍生版本也会在后面的文章介绍，例如 `openresty`，`Tengine` 等。从现在开始，我们从零编译原始版本的 Nginx。

截止这篇文章的发表，Nginx 的最新 mainline 版本是 1.25.2，在 1.25 之后，官方将 QUIC 分支的部分代码合并到了 mainline，正式支持了 HTTP3 协议，本文将加入 HTTP3 模块编译的支持，感受 HTTP3 的魅力。

### 准备源码

建立新用户

```shell
sudo useradd -M -r -s /bin/false -c 'Web server' www
```

更新系统并安装编译器套件

```shell
sudo apt update
sudo apt install build-essential
```

首先准备必要的开发库，包括 `pcre2` `zlib`，以及`openssl`，其中前两者使用包管理器安装，`openssl`使用最新版源码，需要注意的是，Nginx 在 1.21.5 及之后的版本启用了默认 pcre2 的支持，所以在此使用 pcre2

```shell
sudo apt install libpcre2-dev zlib1g-dev
```

建立一个工作环境，我们在`/opt`目录下完成我们的工作

```shell
mkdir /usr/local/src && cd /usr/local/src
```

然后将 `openssl` 下载下来，放到一个合适的位置，截止发文，最新版为 3.1.3

```shell
wget https://www.openssl.org/source/openssl-3.1.3.tar.gz
tar zxf openssl-3.1.3.tar.gz
```

下载 Nginx 源码

```shell
wget https://nginx.org/download/nginx-1.25.2.tar.gz
tar zxf nginx-1.25.2.tar.gz
```

配置 Makefile

```shell
cd nginx-1.25.2
./configure --with-openssl=/usr/local/src/openssl-3.1.3 --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=www --group=www --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-http_v3_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-g -O2 -ffile-prefix-map=/usr/local/src/nginx-1.25.2=. -flto=auto -ffat-lto-objects -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -fPIC' --with-ld-opt='-Wl,-Bsymbolic-functions -flto=auto -ffat-lto-objects -Wl,-z,relro -Wl,-z,now -Wl,--as-needed -pie'
```

依照 CPU 核心数，多线程编译，我的测试在四线程下 2 分钟不到就跑完了

```shell
make -j4
```

编译完成后，安装到系统

```shell
sudo make install
```

测试版本信息，若打印下面的内容，则说明编译安装成功

```shell
$ nginx -V
nginx version: nginx/1.25.2
built by gcc 11.4.0 (Ubuntu 11.4.0-1ubuntu1~22.04)
built with OpenSSL 3.1.3 19 Sep 2023
TLS SNI support enabled
configure arguments: --with-openssl=/usr/local/src/openssl-3.1.3 --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-http_v3_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-g -O2 -ffile-prefix-map=/usr/local/src/nginx-1.25.2=. -flto=auto -ffat-lto-objects -fstack-protector-strong -Wformat -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -fPIC' --with-ld-opt='-Wl,-Bsymbolic-functions -flto=auto -ffat-lto-objects -Wl,-z,relro -Wl,-z,now -Wl,--as-needed -pie'
```

若想要守护运行，添加一条服务项到 `systemd`，输入以下命令来给 Nginx 添加一个守护进程

```shell
cat > /etc/systemd/system/nginx.service << EOF
[Unit]
Description=nginx - high performance web server
Documentation=https://nginx.org/en/docs/
After=network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
User=www
Group=www
Type=forking
PIDFile=/var/run/nginx.pid
ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx.conf
ExecReload=/usr/sbin/nginx -s reload
ExecStop=/usr/sbin/nginx -s stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target
EOF
```

使用对应命令启动就行啦

```shell
# 首先刷新 systemd 配置文件
sudo systemctl daemon-reload

# 设置开机启动并且立即启动
sudo systemctl enable nginx.service --now

# 关闭开机启动
sudo systemctl disable nginx.service

# 停止服务
sudo systemctl stop nginx.service

# 查看服务状态
sudo systemctl status nginx.service
```

