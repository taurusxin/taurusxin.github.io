---
title: "计算机专业学生的 MacBook 软件指南（一）"
categories: [ "macOS" ]
tags: [ "macos", "terminal" ]
draft: false
slug: "mac-soft-1"
date: "2022-01-26 20:31:00"
---

## 前言

本文仅介绍对于 macOS 系统的软件体系，不包含对 Mac 电脑的选购与推荐。

本文以下所有内容均属于个人观点，你完全可以参照自己的使用方法来进行各种配置，我仅提供参考性意见，你的电脑是属于你的。

## 开机

开机后，macOS 会引导用户进行相关设置并激活 macOS，在此过程中，可以通过设置 iCloud 将所有 Apple 的设备“连接起来”，形成 Apple 生态链，这也是苹果全家桶的价值和优势所在。

![我的 iCloud 在 MacBook Pro 2018](%E8%AE%A1%E7%AE%97%E6%9C%BA%E4%B8%93%E4%B8%9A%E5%AD%A6%E7%94%9F%E7%9A%84%20MacBook%20%E8%BD%AF%E4%BB%B6%E6%8C%87%E5%8D%97%EF%BC%88%E4%B8%80%EF%BC%89%20b242776b0a704c17beec1c6993851330/Untitled.png)

我的 iCloud 在 MacBook Pro 2018

## 软件包管理器

### Homebrew 安装

macOS 下最出名的包管理器莫过于 `Homebrew` 了，几乎每一个程序员的 Mac 上都会安装这个包管理器，很多软件的官方教程、技术网站、个人博客等也都推荐使用 Homebrew 来安装 cli 程序或者是 GUI 程序。

安装 Homebrew 的方法很简单，打开其官网：[https://brew.sh](https://brew.sh/) 就可以看到使用一句命令即可安装

Homebrew 到你的 Mac 电脑。

打开启动台，找到其它，打开终端，粘贴以下命令并按回车，即可在短时间内安装 Homebrew

```jsx
/bin/bash -c "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh))"
```

安装完成后，就可以在终端内输入 `brew -v` 来查看当前 Homebrew 的版本 

![查看 Homebrew 的版本（你的终端可能会和我长的可能会不一样，后面会详细阐述终端美化）](https://cdn.rhyland.cn/hugo/2022/01-26/brew-version.png)

### Homebrew 使用

macOS 下有一种类似于 Windows 里的软件安装包格式 `pkg` 。这是 macOS 下的一键安装包，倘若在学习 Python 时，想安装最新的 Python 3，一种办法就是去 Python 的官方网站下载官方提供的 pkg 安装包，这样可以一键安装，这种方法的优点是安装便捷，缺点就是日后想升级一下 Python 的小版本，就无从下手，先需要卸载旧的版本，这也是 pkg 格式最大的缺点——没有对应的卸载程序。直接安装新的版本，就要解决新旧版本冲突问题，例如 Java 的环境变量。除非使用一些第三方工具，例如卸载 pkg 专用的 `UninstallPKG` 。

使用 Homebrew 吧！依旧是打开你的终端，可以先对你想安装的软件包进行搜索，使用命令

`brew search name`  

来进行软件的搜索，name 为该软件名称

![使用 Homebrew 搜寻软件包 Python](https://cdn.rhyland.cn/hugo/2022/01-26/brew-search.png)

可以看到，brew 的软件库里面有许多和关键词 “python” 相关的软件包，写本篇文章时，Python 的 3.10 版本才刚发布没多久，brew 就收录了，brew 的另一个优点就是收录和更新软件包的速度，每当新版本发布，brew 基本都在第一时间进行更新。

接下来可以使用命令 `brew info name` 来查看一个软件包的详细信息，包含了发行商、使用协议、依赖软件包、使用方法和安装次数等。

![查看 python@3.9 软件包的信息](https://cdn.rhyland.cn/hugo/2022/01-26/brew-info.png)

确定要安装的软件包之后，使用命令 `brew install name` 来安装你的软件包，brew 会自动下载软件包，并且安装到其指定的位置下面。

安装完成之后，可以查看一下 Python 的版本信息。

![查看 Python 的版本](https://cdn.rhyland.cn/hugo/2022/01-26/python-version.png)

以我当前版本举例，一段时间之后，Python 发布了 3.9.10 版本，想对其进行升级，首先要进行 brew 的软件列表更新，因为 brew 的原理就是从网络上将整个 brew 软件库的列表下载下来，然后在安装软件包时在本地搜寻该软件包的信息，如果没有更新 brew 的软件库列表，那么搜寻到的还是 python 的 3.9.9 版本，更新 brew 软件包列表的命令为 `brew update` 该命令同时也包含了对 brew 包管理器本身的更新。

更新完成之后，brew 会提示有哪些软件包可以进行升级，下图中，输出了 brew 收录了什么新的软件包，更新了哪些软件包，移除了哪些软件包以及我当前电脑中有一个软件包可以升级。

![升级 brew 软件库列表](https://cdn.rhyland.cn/hugo/2022/01-26/brew-update.png)

随后可以针对单一软件进行升级，使用命令（注意上面的 update 和这里 upgrade 的区别）

`brew upgrade name`

如果你想对所有可以升级的软件升级，则命令最后不用加软件包名称，brew 会对当前所有可以升级的软件包进行升级

`brew upgrade`

卸载软件包也很简单，使用命令 `brew remove name` 即可卸载该软件包

以上就是 Homebrew 的一些基本使用方法，更多用法可以前往官网 [https://docs.brew.sh/](https://docs.brew.sh/) 查看文档

## 终端美化

### 概念

终端对于一个使用 Mac 的程序员来说是必不可少的，打造一个适合自己使用习惯的终端能够节省大量时间。

首先阐述一下 Shell、Terminal 之间的区别，这是很多人都会搞混淆的两个概念。

Shell 是一种外壳程序，我们所有输入的命令都将由 Shell 进行语法解析，并最后送往系统的内核运行，执行命令的主体是内核而不是 Shell。Shell 的作用就是解释命令。

常见的 Shell  有

- BourneShell (sh)
- BourneAgain Shell (bash)
- ash (ash)
- CShell (csh)
- KornShell (ksh)
- Z Shell (zsh)

在 Linux / macOS 下使用命令 `echo $SHELL` 就可以看到当前系统内置的 Shell 了

Terminal 则是一种终端模拟器，它提供了一种输入输出的环境，所以你才能与 Shell 进行交互，具体取决于你当前用户使用的默认 Shell。

### Z Shell

著名的 zsh 莫过于它的一个插件 oh-my-zsh，从 macOS 10.15 (Catalina) 之后，用户的默认 Shell 就是 zsh 了，如果你还在用这之前的系统，赶快升级或者使用 brew 安装一个 zsh

oh-my-zsh 的安装和配置将在下面的内容讲解

### iTerm2

macOS 下我个人最推荐使用的是 **iTerm 2** 这个终端模拟器了，具有非常强大的自定义功能，界面做的也非常好看，符合我的审美。

官网：[https://iterm2.com/](https://iterm2.com/index.html)

![iTerm2 官网](https://cdn.rhyland.cn/hugo/2022/01-26/iterm2.png)

下载、安装并打开 iTerm2 之后，进行简单的配置，首先使用 `Command + ,` 来打开首选项，进入 Appearance 选项卡，将 Theme 设置为 Minimal，标题栏就消失了，整个界面变的沉浸式

![设置沉浸式主题](https://cdn.rhyland.cn/hugo/2022/01-26/theme-minimal.png)

打开 [https://iterm2colorschemes.com/](https://iterm2colorschemes.com/) 选择你喜欢的配色，下载下来，它应该是一个后缀为 itermcolors 的文件，进入 `Profiles - Colors` 选项卡，在右下角的 `Color Poesets` 选择 `Import...` 导入你刚才下载的主题，我选择的是 Dracula 主题。

![主题颜色配置](https://cdn.rhyland.cn/hugo/2022/01-26/color-presets.png)

![Dracula 主题](https://cdn.rhyland.cn/hugo/2022/01-26/dracula.png)

### Oh My Zsh

如果只使用 zsh 那么其实和用 bash 差不多，这时候就要借助 zsh 强大的插件系统了，平时在网上看到的各种美观的终端截图基本都是 zsh + oh-my-zsh 截屏出来的。

打开官网 [https://ohmyz.sh/](https://ohmyz.sh/) 有安装说明，直接一键安装，安装完成后会询问是否设置默认，当然是要设置的。

```bash
# 使用 curl 安装
sh -c "$(curl -fsSL [https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh](https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh))"

# 使用 wget 安装
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

安装完成后，你的终端就会发生一些变化，有了一个默认的主题。接下来我们进行一些配置。

首先使用 vim 或者你偏好的编辑器，打开主目录下的 .zshrc 文件

```bash
# 使用 vim
vim ~/.zshrc

# 使用 VSCode
code ~/.zshrc
```

![使用 VSCode 编辑配置文件](https://cdn.rhyland.cn/hugo/2022/01-26/zshrc-theme.png)

默认的主题是 robbyrusseell，你可以打开 [https://github.com/ohmyzsh/ohmyzsh/wiki/Themes](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes) 来挑选喜欢的自带主题，然后将双引号内容修改

更改并保存后，需要重开 iTerm2 或者输入命令 `source ~/.zshrc` 来应用更改

### 语法高亮和自动完成插件

在输入命令时难免会输入错误，这时候需要一个插件开进行命令的检测，那就是语法高亮插件，而自动完成则是一个很强大的命令补全插件，可以帮助你补全需要的命令。

那就两个一起安装吧，复制以下命令到终端然后回车

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

然后还是打开刚才的 .zshrc 配置文件，往下拉找到 plugin 选项，在括号内加上这两个插件的名字

![添加插件](https://cdn.rhyland.cn/hugo/2022/01-26/zshrc-plugin.png)

同样需要重启 Shell 或者使用命令来应用更改

语法高亮↓

![绿色为存在该命令，红色为不存在](https://cdn.rhyland.cn/hugo/2022/01-26/syntax-highlighting.png)

自动补全↓

![输入一个使用过的命令开头，按下方向键→即可补全](https://cdn.rhyland.cn/hugo/2022/01-26/auto-suggestions.png)

或者，还可以这样补全文件夹（动图）

![输入文件夹开头，按 Tab 补全](https://cdn.rhyland.cn/hugo/2022/01-26/auto-suggestions-folder.gif)

总之，还有很多有意思的功能等待你去发现。