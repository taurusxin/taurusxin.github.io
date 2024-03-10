---
title: "Windows 10 下使用 MSVC 2019 编译 Nginx 64位过程记录"
categories: [ "程序人生" ]
tags: [  ]
draft: false
slug: "nginx-windows-build"
date: "2021-06-16 23:56:00"
---

## 前言

Nginx 在 Windows 下还是很实用的，可以作为前端的 HTTP 部署测试工具，或者当然可以用 Python 自带的 http.server 来部署，都挺快的。除了部署前端项目之外，放一些自制的基于 Web 的小工具都是极好的。

## 下载源码包

### Nginx 源码

Nginx 在 Windows 下编译的源码与 Linux 下的不同，直接从官网的 download 里面下载是编译不过的，需要从下面的 Mercurial 链接下载，Mercurial 也是一种类似于 Git 的版本管理工具

下载地址: <http://hg.nginx.org/nginx>

注意，请选择一个带有 RELEASE TAG 的版本进行下载，如下图橙色方框所示

![下载页面][1]

截至发文， Nginx 的最新 release 版本为 **1.21.4**

然后在左侧选择你偏好的压缩包类型，点击下载即可

![选择压缩包][2]

将其解压至一个你喜欢的位置，例如我解压在 `D:\nginx\1.21.4`

### 依赖源码

在 Nginx 源码根目录下创建 objs/lib 文件夹

依次下载 [pcre][3] / [zlib][4] / [openssl][5] 的源码包，解压至 objs/lib 文件夹

**注意：pcre版本为1，即8.x，不要下载pcre2，即10.x开头的版本**

![依赖包][6]

### StrawberryPerl

因为需要编译 OpenSSL， 需要使用 perl 进行配置

<https://strawberryperl.com/>

### MSYS 2

msys2 在编译阶段仅用作配置 Makefile，因本文介绍的是在 MSVC 环境下的编译，所以不用 msys2 的 GNU 环境进行编译。

下载地址：<https://www.msys2.org/>

**Tip： Git 的 Bash 环境也可以用作后面开始编译的配置 Makefile**

## 开始编译

### 配置 64 位 OpenSSL

**重要！本文编译64位 Nginx，配置 Makefile 前需要将 OpenSSL 依赖库改为64位**
进入 nginx/auto/lib/openssl 使用编辑器打开makefile.msvc 将第 9 行的 `VC-WIN32` 改成 `VC-WIN64A`

### 配置 Makefile

安装完成 msys2 后，使用 msys2 的任意环境或 Git Bash 进入 nginx 源码目录，执行以下命令，用于生成适用于 MSVC 的 Makefile

**以下命令行配置了 nginx 在 Windows 下的常用模块的编译，请根据自己需要调整相关模块的加入或删除**

**加入的特性包括 HTTP 2，SSL/TLS HTTPS 支持，HTTP 流媒体支持，socket 流转发支持，mail 服务器支持**

```bash
auto/configure \
  --with-cc=cl \
  --prefix= \
  --conf-path=conf/nginx.conf \
  --pid-path=logs/nginx.pid \
  --http-log-path=logs/access.log \
  --error-log-path=logs/error.log \
  --sbin-path=nginx.exe \
  --http-client-body-temp-path=temp/client_body_temp \
  --http-proxy-temp-path=temp/proxy_temp \
  --http-fastcgi-temp-path=temp/fastcgi_temp \
  --http-scgi-temp-path=temp/scgi_temp \
  --http-uwsgi-temp-path=temp/uwsgi_temp \
  --with-cc-opt=-DFD_SETSIZE=1024 \
  --with-pcre=objs/pcre-8.45 \
  --with-zlib=objs/zlib-1.2.11 \
  --with-openssl=objs/openssl-3.0.0 \
  --with-openssl-opt='no-asm no-tests -D_WIN32_WINNT=0x0601' \
  --with-http_ssl_module \
  --with-http_v2_module \
  --with-http_realip_module \
  --with-http_addition_module \
  --with-http_sub_module \
  --with-http_stub_status_module \
  --with-http_dav_module \
  --with-http_flv_module \
  --with-http_mp4_module \
  --with-http_gunzip_module \
  --with-http_gzip_static_module \
  --with-http_auth_request_module \
  --with-http_random_index_module \
  --with-http_secure_link_module \
  --with-http_slice_module \
  --with-mail \
  --with-mail_ssl_module \
  --with-stream \
  --with-stream_ssl_module \
  --with-stream_ssl_preread_module
```

**请提前自行创建目录 `logs`、`temp\client_body_temp`、`temp\proxy_temp`、`temp\fastcgi_temp`、`temp\scgi_temp`，否则执行脚本会提示目录不存在**

若脚本执行如果提示 `auto/cc/msvc: line 117: [: : integer expression expected`

是因为读取不到 VC 编译器的版本，需要手动指定。

打开 VS2019 的 VC 命令行，输入 `cl` 就能查看编译器版本了。

![编译器版本][7]

打开 `auto/cc/msvc` 文件，在文件的 `NGX_MSVC_VER=...` 下方添加如下代码，即手动指定 MSVC 编译器的版本（采用截图原因是方便定位添加代码的位置）

![手动指定 MSVC 版本][8]

再次执行生成 Makefile 的命令，不报错即通过。

### 进行编译

![VC命令行][9]

打开 VS2019 的 VC 命令行，注意选择 Native Tools，不要选择 Cross Tools。

cd 到源码根目录后，执行编译命令

```shell
nmake –f objs/Makefile
```

编译过程较久，视 CPU 单核性能而定，耗时十分钟到半小时不等。

当提示 sed 不是内部命令或外部命令时就代表编译成功了，忽略 nmake 的报错，因为 sed 是 Linux 下的命令

没有其它报错退出时， Nginx 即编译完成。

## 整理文件

将源码目录下的`conf`、`contrib` `html` `logs` `temp` 和 `objs\nginx.exe` 整理到一个目录下，双击打开 nginx.exe，命令行一闪而过，打开浏览器输入 `http://localhost/` 或 `http://127.0.0.1/` 看到以下页面即成功。

![success.png][10]

输入以下命令结束运行 Nginx

```shell
nginx.exe -s stop
```

## 笔者编译的版本

版本：1.21.4
版本信息：

```shell
nginx version: nginx/1.21.4
built by cl 19.30.30706
built with OpenSSL 3.0.0 7 sep 2021
TLS SNI support enabled
configure arguments: --with-cc=cl --prefix= --conf-path=conf/nginx.conf --pid-path=logs/nginx.pid --http-log-path=logs/access.log --error-log-path=logs/error.log --sbin-path=nginx.exe --http-client-body-temp-path=temp/client_body_temp --http-proxy-temp-path=temp/proxy_temp --http-fastcgi-temp-path=temp/fastcgi_temp --http-scgi-temp-path=temp/scgi_temp --http-uwsgi-temp-path=temp/uwsgi_temp --with-cc-opt=-DFD_SETSIZE=1024 --with-pcre=objs/pcre-8.45 --with-zlib=objs/zlib-1.2.11 --with-openssl=objs/openssl-3.0.0 --with-openssl-opt='no-asm no-tests -D_WIN32_WINNT=0x0601' --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_addition_module --with-http_sub_module --with-http_stub_status_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_auth_request_module --with-http_random_index_module --with-http_secure_link_module --with-http_slice_module --with-mail --with-mail_ssl_module --with-stream --with-stream_ssl_module --with-stream_ssl_preread_module
```

下载地址：[点击这里](https://cdn.rhyland.cn/typecho/2021/12/06/nginx-1.21.4.zip)

## 结语

至此，Nginx 在 Windows 10 下，使用 MSVC 2019 的完整编译过程结束，任何报错，不明白请在下方评论区留言！

  [1]: https://cdn.rhyland.cn/typecho/2021/06/14/download.png
  [2]: https://cdn.rhyland.cn/typecho/2021/06/14/archive.png
  [3]: https://sourceforge.net/projects/pcre/files/pcre/
  [4]: https://zlib.net/
  [5]: https://www.openssl.org/source/
  [6]: https://cdn.rhyland.cn/typecho/2021/06/14/require-libs.png
  [7]: https://cdn.rhyland.cn/typecho/2021/06/14/cl-version.png
  [8]: https://cdn.rhyland.cn/typecho/2021/06/14/point-msvc-version.png
  [9]: https://cdn.rhyland.cn/typecho/2021/06/14/msvc-cli.png
  [10]: https://cdn.rhyland.cn/typecho/2021/06/14/success.png
