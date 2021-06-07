---
layout: post
title:  "Deploy Ghost based on Github Page"
date:   2020-04-22 21:58:08 +0800
category: Tech
---

# 基于 Github Page 部署 Ghost

## 前言：

在腾讯云部署过基于 Nginx 反向代理的个人博客，而 github.io 也是热门的博客驿站。注定浪迹网络的开发者，难免也要在这留下印迹。

## 正文：

### 部署过程：

- 安装 Ghost-CLI

```
sudo npm install -g ghost-cli@latest
```

- 为了后续创建 Ghost 项目，在自定义空文件夹安装 Ghost

```
ghost install local
```

这里大概率可会出现 Node.js 与 Ghost 版本不兼容的报错问题：

```
xieziyi@xieziyis-MBP GhostCLI % ghost install local
✔ Checking system Node.js version - found v14.15.4
✔ Checking current folder permissions
✔ Checking memory availability
✔ Checking free space
✔ Checking for latest Ghost version
✔ Setting up install directory
✖ Downloading and installing Ghost v4.6.4
A SystemError occurred.

Message: Ghost v4.6.4 is not compatible with the current Node version.

Debug Information:
    OS: macOS, v11.4
    Node Version: v14.15.4
    Ghost-CLI Version: 1.17.3
    Environment: development
    Command: 'ghost install local'

Try running ghost doctor to check your system for known issues.

You can always refer to https://ghost.org/docs/ghost-cli/ for troubleshooting.
xieziyi@xieziyis-MBP GhostCLI % ghost doctor
Working directory is not a recognisable Ghost installation.
Run `ghost doctor` again within a folder where Ghost was installed with Ghost-CLI.
```

- 选择当前 Github 上的特定 Ghost [版本](https://ghost.org/docs/faq/node-versions/)号安装：

```
ghost install 4.3.3 --local
```

完成如下：

```
Try running ghost doctor to check your system for known issues.

You can always refer to https://ghost.org/docs/ghost-cli/ for troubleshooting.
xieziyi@xieziyis-MBP Ghost % ghost install 4.3.3 --local
✔ Checking system Node.js version - found v14.15.4
✔ Checking current folder permissions
✔ Checking memory availability
✔ Checking free space
✔ Checking for latest Ghost version
✔ Setting up install directory
✔ Downloading and installing Ghost v4.3.3
✔ Finishing install process
✔ Configuring Ghost
✔ Setting up instance
✔ Starting Ghost

Ghost uses direct mail by default. To set up an alternative email method read our docs at https://ghost.org/docs/config/#mail

------------------------------------------------------------------------------

Ghost was installed successfully! To complete setup of your publication, visit: 

    http://localhost:2368/ghost/

xieziyi@xieziyis-MBP Ghost % 
```

同时在安装 Ghost 的文件夹下会生成相应源代码文件，如下图：

![img](https://res.cloudinary.com/dzb9ldnvl/image/upload/v1622839055/blog/ghostfile_ml4rkw.png)

为托管在 Github Pages ，需要完全静态部署，所以必须解析为纯 HTML、CSS、JavaScript 文件。

- 安装 Wget ，成功出现 `wget -help` 如下：

```zsh
# macm1 使用 x86 版本 homebrew即可
brew install Wget
```

![img](https://res.cloudinary.com/dzb9ldnvl/image/upload/v1622839739/blog/wgethelp_dieotm.png)

- 在 casper 目录下创建 docs 文件夹，并通过 VSCode 打开 capser 根路径终端输入代码：

```zsh
wget -r -nH -P docs -E -T 2 -np -k http://localhost:2368/
```

完成后 Docs 文件夹会出现解析成功的静态文件，这里注意端口号的正确，否则出现如下报错：

```
--20xx-0x-0x xx:12:22--  http://localhost:2368/portal/
正在连接 localhost (localhost)|127.0.0.1|:2368... 已连接。
已发出 HTTP 请求，正在等待回应... 404 Not Found
20xx-0x-0x xx:12:22 错误 404：Not Found。
```

- Github 创建 new Repository，在 Repository name 的位置填写格式为 `usr.GitHub.io` 的域名，并选择 Public 类型。
- 建立 docs 与远程仓库连接后，登陆 Github 仓库界面选择 Settings 中 Pages 选项推入 Source 即可，不要选择下面的 Theme Chooser 。

![img](https://res.cloudinary.com/dzb9ldnvl/image/upload/v1622840894/blog/ghostsettings_iwrryy.png)

- 登陆前文中设置的 Github Repository name 即完成。

## 结束

请肆意享受 Ghost 的乐趣。

### 以上

本博客所有文章除特别声明外，均采用 [CC BY-SA 4.0 协议](https://creativecommons.org/licenses/by-sa/4.0/deed.zh) ，转载请注明出处！