---
layout: post
title:  "Use of content in the ZSH env on Mac"
date:   2020-08-09 22:45:58 +0800
category: Tech

---

# Use of content in the ZSH env on Mac

------

## 前言：

在安装软件包时通常会要求写入环境变量信息到配置文件里，很多人多数时间就按照官方指导上的来。但是没有考虑研究为什么要放这里。时间长了就会导致配置文件混乱，且甚至有还有虚拟系统和本地系统冲突。这里浅谈一下各类文件，且内容主要以 MacOS 为主。

------

## 正文：

### ZSH 用户环境配置文件（多位于主目录）：

- **`.zshenv`：** 环境变量配置
- **`.zshrc`：** 交互 shell 配置
- **`.zlogin`：** 登陆 shell 配置
- **`.zprofile`：** 登陆 shell 配置
- **`.zlogout`：** 退出 shell 配置

`.zlogin` 和 `.zprofile` 基本上是同一件事。相比于 CSH 的衍生 `.zlogin` ，Bash 是 Mojave 之前所有组件的默认外壳，因此用 `.zprofile` 稳定性会更高。 `.zshenv` 在非交互式shell下运行，和你需要点开交互式 shell 的 `.zshrc` 以及设置并忘记，需要频繁使能的 `.zprofile` 不同。`.zlogout` 通常在退出交互式 shell 时执行。

### mac | Linux 系统下的文件夹：

- **`/bin`：**  **二进制 (`binary`)** 文件的可执行程序的命令，包含 引导启动 及 **用户操作 用到的命令**

```zsh
zs@zsxzy-MacBook-Pro /bin % ls
[		dd		launchctl	pwd		test
bash		df		link		rm		unlink
cat		echo		ln		rmdir		wait4path
chmod		ed		ls		sh		zsh
cp		expr		mkdir		sleep
csh		hostname	mv		stty
dash		kill		pax		sync
date		ksh		ps		tcsh
```

- **`/dev`：** 包括所有的**设备文件**

```zsh
...
# tty 物理设备连接
disk3s6				tty.debug-console
dtrace				tty.wlan-debug
dtracehelper			ttyp0
fbt				ttyp1
fd				ttyp2
...
```

- **`/etc`：** 'et cetera' 的缩写、**系统配置（网络配置）文件**

```zsh
# 启动的配置文件和脚本
/etc/rc.d
```

- **`/home`：** 用户主目录的基点 - user用户的主目录就是/home/user

- **`/sbin`：** 用于管理系统的一些必备命令

- **`/tmp`：** 公用的临时文件存储点

- **`/var`：** var目录存在的目的是把usr目录在运行过程中需要更改的文件或者临时生成的文件及目录提取出来

- **`/usr`:** 是个很有意思的目录：曾经我也以为是 user 的缩写，但是在浏览其他博客的时候发现其实并非如此。也与 `/home` 冲突。

```zsh
zs@zsxzy-MacBook-Pro /usr % ls
X11		bin		libexec		sbin		standalone
X11R6		lib		local		share
```

而是包含所有用户二进制文件、文档、库、头文件、程序等。那么此前是作为用户主目录的 `/usr` ，现在更多的是作为**用户端程序和数据（相对于“系统端”程序和数据）所在的位置。**名称没有改变，但已经从“与用户相关的所有内容”缩小并扩展到“**用户可用的程序和数据**”。为了方便记忆，个人理解为 'User Available System Resource'。

```zsh
# 本地安装的软件和其他文件
/usr/local

# 本地增加的命令
/usr/local/bin

# 本地增加的库
/usr/local/lib
```

------

## 结束：

对于 ZSH 用户环境更多用到的是 `./zprofile` 与`.zshrc` 而 Mac 系统下的文件夹则多关注 `/usr/local` 及其纵向文件。

### 以上：

本博客所有文章除特别声明外，均采用 [CC BY-SA 4.0 协议](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) ，转载请注明出处！

