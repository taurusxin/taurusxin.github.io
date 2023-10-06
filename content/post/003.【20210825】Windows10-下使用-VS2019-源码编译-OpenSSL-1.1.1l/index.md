---
title: "Windows 11 下使用 VS2022 源码编译 OpenSSL 3.1.3"
categories: [ "程序人生" ]
tags: [  ]
draft: false
slug: "openssl_win_build"
date: "2019-08-20 13:00:00"
lastmod: '2023-10-06T18:34:00+0800'
---

**本文持续更新，跟进当前日期最新的 OpenSSL 版本，最后更新日期见文章末**

### 安装 Strawberry Perl

前往 [Strawberry Perl][1] 官网下载并安装 `Strawberry Perl`
这里选择的是目前最新版本 `Strawberry Perl` 5.32.1.1
安装过程中务必选择完整安装
安装完成之后会自动添加环境变量，无需手动添加

检查是否已安装

```shell
perl -v
```

### ~~安装 NASM~~

**2021/03/25 更新：以后教程不编译汇编代码，此步骤省略**

~~前往 [NASM][2] 官网下载并安装 NASM~~
~~这里选择的是 Stable 的 2.15.05~~

~~**安装完成后要将NASM的路径添加到环境变量，安装程序不会自动添加**~~

### 下载源代码

前往 [OpenSSL][3] 官网下载 openssl-3.1.3.tar.gz 并解压

### 编译

#### VC编译环境

 Visual Studio 在安装时必须勾选 Windows C++ 的开发环境，不然没法打开开发人员命令提示符

![Snipaste_2020-04-23_14-08-13.png][4]

#### 启动命令行提示工具

【2020年5月25日更新】
 使用 VS2022 的开发人员提示工具切换到源码目录，注意选择要编译的位数所对应的版本，不要选择cross版本。所有的选项都要统一，即：编译32位的就启动x86，配置Makefile时为命令行为VC-WIN32，64位同理，本文以编译64位为例。


**根据下图打钩的图标，确定需要编译的位数，选择合适的命令提示符。**
![选择合适的命令行版本][5]

#### 进入源码目录

```shell
cd /d C:\openssl-3.1.3
```

#### 配置 Makefile

| 版本   | 对应架构命令行 |
| ----- | -------------- |
| 64位 | VC-WIN64A      |
| 32位 | VC-WIN32       |

本文采用64位

```shell
perl Configure VC-WIN64A no-asm
```

如果编译静态链接版本的二进制程序，加一个 `no-shared` 选项即可。

```shell
perl Configure VC-WIN64A no-asm no-shared
```

根据自己需要，二者选一即可

默认编译后安装在以下路径：
| 版本 | 默认安装路径             |
| ---- | ------------------------------ |
| 64位 | C:\\Program Files\\OpenSSL       |
| 32位 | C:\\Program Files (x86)\\OpenSSL |

若想要自定义安装路径，添加 `--prefix` 选项即可

```shell
perl Configure VC-WIN64A no-asm --prefix=D:\OpenSSL\3.1.3
```

#### 开始编译及安装

 执行 nmake 开始编译
整个过程视机器配置而定，单线程编译持续约5-10分钟

```shell
nmake
```

如果想加速编译，使用 Qt 的 jom 工具即可使用 `-j` 选项来指定构建的线程数， jom 可以在[这里](https://download.qt.io/official_releases/jom/)下载，将它解压然后添加到环境变量即可，下面的命令使用 8 线程编译

```shell
jom -j 8
```

安装二进制和库

```shell
nmake install_sw
```

**使用 install_sw 代替 install 是因为默认 install 会生成 40M 左右的 HTML 文档，若不需要就使用 install_sw 仅安装二进制文件和库**

#### 添加环境变量

将编译后的安装目录下的 bin 文件夹添加到系统 Path 目录下
打开 cmd 使用 `openssl version -a` 测试

```shell
$ openssl version -a
OpenSSL 3.1.3 19 Sep 2023 (Library: OpenSSL 3.1.3 19 Sep 2023)
built on: Fri Oct  6 10:55:51 2023 UTC
platform: VC-WIN64A
options:  bn(64,64)
compiler: cl /Zi /Fdossl_static.pdb /Gs0 /GF /Gy /MD /W3 /wd4090 /nologo /O2 /FS -DL_ENDIAN -DOPENSSL_PIC
OPENSSLDIR: "C:\Program Files\Common Files\SSL"
ENGINESDIR: "C:\Program Files\OpenSSL\lib\engines-3"
MODULESDIR: "C:\Program Files\OpenSSL\lib\ossl-modules"
Seeding source: os-specific
CPUINFO: OPENSSL_ia32cap=0x7ef8320b078bffff:0x400004219c91a9
```

#### 清理生成的中间文件

```shell
nmake clean
```

### 下载

附上我编译的动态链接版及静态链接版，已经移除了 pdb 调试符号文件，确保文件大小

[动态链接版](https://cdn.taurusxin.com/softwares/openssl/openssl-3.1.3-windows-amd64-dynamic.zip)

[静态链接版](https://cdn.taurusxin.com/softwares/openssl/openssl-3.1.3-windows-amd64-static.zip)

### 结尾语

如果在配置或者编译有任何问题或者报错，请在下方留言或直接在关于页面联系我~

  [1]: http://strawberryperl.com/
  [2]: https://www.nasm.us/
  [3]: https://www.openssl.org/source/
  [4]: https://cdn.rhyland.cn/usr/uploads/2020/04/504569709.png
  [5]: https://cdn.rhyland.cn/usr/uploads/2020/05/1835595688.jpg
  [6]: https://cdn.rhyland.cn/typecho/2021/09/01/OpenSSL-1.1.1l-windows_x86-64.zip
