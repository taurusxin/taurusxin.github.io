---
title: "Windows 技巧分享（二）正版激活及一些配置"
categories: [ "操作系统" ]
tags: [ "Windows 10 技巧" ]
draft: false
slug: "windows-skill-02"
date: "2021-03-21 13:26:00"
---

### 激活

我目前有2台 Windows 电脑，一台是高中自己配的4代i7台式，放在家里，还有一台就是我的日常主力笔记本，**神舟 G8-CU7NA**。

现在市售笔记本默认出厂的系统都是家庭版，作为一个专业开发者用户，家庭版不能满足需求，比如 Docker 必备的 Hyper-V 虚拟化技术等。而且很多厂商的出厂家庭版系统自带了一堆没用的软件和功能。

于是我就自行购买了真正的官方 Windows 10 专业版的正版，**绝对不是淘宝5元-20元左右的激活码**，没有可比性。如果有小伙伴需要，也可以从博客的关于我联系到我。

我在安装时就指定了正版的秘钥，安装完即为正版激活。

如果安装时选择了我暂时没有秘钥，那就可以在系统的设置页面手动输入秘钥激活。

正版激活的系统，会在页面显示**与 Microsoft 账户关联的数字许可证激活**，此后，在不更换重要硬件（主板、CPU）的前提下，无限次重装都不必输入秘钥，Windows 10 会在安装完成之后自动匹配机器与账户。

![Windows 10 激活（设置页面）][1]

此为正版激活的办法，我们公司目前所有人用的都是正版的专业版，工作所需。

---

若手头能力有限，对于正版的需求度不高，这里直接贴2个方法及其工具，步骤见隐藏内容。

[collapse status="false" title="点击展开"]
#### 1.KMS激活

KMS激活地址：kms.xingez.me

**脚本**：[button color="info" icon="" url="https://cdn.rhyland.cn/typecho/2021/03/21/Win10%20Pro-d22afa.bat" type=""]下载[/button]
下载之后右键**必须管理员运行**，弹出三个对话框即可。

**手动**：

使用**管理员启动**的PowerShell（开始菜单右键），手动输入脚本中的三个命令，此秘钥为激活专业版，其他版本见 https://docs.microsoft.com/zh-cn/windows-server/get-started/kmsclientkeys

```shell
slmgr /ipk W269N-WFGWX-YVC9B-4J6C9-T83GX
slmgr /skms kms.xingez.me
slmgr /ato
```

---

#### 2. 伪数字权益激活

工具：[button color="info" icon="" url="https://cdn.rhyland.cn/typecho/2021/03/21/HWIDGen%2066.01-968a4c.zip" type=""]下载[/button]

工具启动后，右侧选择 HWID，然后 Start即可。这是一个伪造数字激活的工具，且是永久。
[/collapse]

### 配置

#### 隐藏搜索框
安装完系统且激活完成之后，我做的第一件事情就是先调整底部的任务栏，这是我最不能忍的一点，很多人的电脑底下都会有一个搜索框，对我来说这个用不太上，当然也因人而异

![隐藏搜索框][2]

#### 添加美式键盘

在早期的 Windows 10，还可以为中文语言添加美式键盘，在某一个版本更新后就不行了，必须通过注册表的方式手动添加，美式键盘打游戏，中文输入法写代码和码字。

![中文输入法和美式键盘][3]

这种方法并不是添加一个英文语言，因为添加语言是用 Win + Space 来切换的，我从XP时代就习惯使用 Ctrl + Shift 切换中文输入法和美式键盘，所以使用脚本**为中文语言添加美式键盘**。

![语言选项][4]

注意，我的语言只有一个中文。

注册表：[button color="info" icon="" url="https://cdn.rhyland.cn/typecho/2021/03/21/uskey-b35ad7.reg" type=""]下载[/button]

双击导入之后，需要注销用户，或者重启系统方可生效。

#### 更多的配置内容

以后想到什么在这里添加。

### GNU程序及常用工具

用过 Linux 发行版的同学们应该都知道很多实用的工具都是原生开发在 Linux 下的，因为他们都是用 ANSI C 写的，所以移植到 Windows 下面就成为了一种可能性。

首先就是在自己的数据盘目录下新建一个文件夹，用于存放这些命令行工具，日后添加环境变量也比较方便。
我日常开发、调试会用到包含但不仅限于以下这些工具

 - curl（强大的web请求工具）
 - iperf3（测试多线程公网或局域网带宽吞吐量，Windows下因为网络API不同可能会有性能问题）
 - nali（Golang 版本的nali，离线查询IP归属地，支持 IPv6）
 - wget（web文件下载工具）
 - tcping（TCP Ping 延迟检测工具）
 - ffmpeg（多媒体音视频工具）
 - Nginx（web服务器，部署自己的前端项目）
 - OpenSSL（开源加密库，我个人常用于哈希校验，base64编码等）
 - SpeedTest-cli（测网速，经常到一个地方先看看网速怎么样）

程序都是去官网下载，能下到 Windows 版本就最好。

Nginx 和 OpenSSL 都是自己源码编译的，OpenSSL 在 Windows 下的编译教程之前有发过，详见[这里][5]，以后会写一篇 Nginx 的源码编译教程，为什么要自己写，因为感觉百度或Google出来的文章针对版本都过时了，甚至还看到Nginx用0.9的OpenSSL。

所有工具都解压到文件夹，然后依次添加到 Windows 的环境变量，这样随时右键 Git Bash 就能调用了。还有一些没有列举出来，日后看需求添加。


  [1]: https://cdn.rhyland.cn/typecho/2021/03/21/windows-activate.png
  [2]: https://cdn.rhyland.cn/typecho/2021/03/21/hide-search.png
  [3]: https://cdn.rhyland.cn/typecho/2021/03/21/input-type.png
  [4]: https://cdn.rhyland.cn/typecho/2021/03/21/language.png
  [5]: https://blog.xingez.me/openssl_win_build