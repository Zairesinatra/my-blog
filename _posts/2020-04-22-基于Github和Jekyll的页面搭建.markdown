---
layout: post
title:  "Build a blog based on github-page and jekyll"
date:   2020-04-22 20:58:08 +0800
categories: zairesinatra
---

# 基于 Github 和 Jekyll 的页面搭建

------

## 搭建过程

- **Github 仓库操作**

登陆 GitHub 账号后，创建一个新的 Repository ，注意在 Repository name 的位置填写格式是 `usr.GitHub.io`域名，并在 Settings 的 Github Pages 中选择一个 theme。

在命令行中切换到你自定义的路径下，然后 Clone 下来你的项目（操作需要在 Mac 的 Terminal 中完成，Windows 系统可以使用 Git-bash。）

这里可能遇见[报错](https://stackoverflow.com/questions/6842687/the-remote-end-hung-up-unexpectedly-while-git-cloning)：

```zsh
LibreSSL SSL_read: SSL_ERROR_SYSCALL, errno 60
```

问题原因：由于大文件造成的提交或者拉取失败，curl的postBuffer默认值太小，增大缓存配置即可

```
git config --global http.postBuffer 1048576000
```

- **生成工具安装**

由于 Github Page 需要在仓库存放静态模板， 那么需要静态网站生成器。这里考虑 GitHub 官方推荐的 [Jekyll](https://jekyllcn.com/docs/quickstart/) 。不用部署静态页面，仅将 markdown 文件放在 _ post 目录下项目源码即可。

Jekyll 是基于 Ruby 的静态网页生成系统，首先安装 Ruby 环境：

```zsh
brew install ruby
```

Ruby 安装完毕后再执行 Jekyll 和 bundler 的安装命令：

```zsh
gem install jekyll bundler
```

这里可能遇见[报错](https://stackoverflow.com/questions/51126403/you-dont-have-write-permissions-for-the-library-ruby-gems-2-3-0-directory-ma)：

```zsh
You don't have write permissions for the /Library/Ruby/Gems/2.3.0 directory. (mac user)
```

问题原因：存在 macOS 附带的版本。

如果确定不需要同时使用多个 Ruby 版本（除了 macOS 附带）则输入代码：

```zsh
echo 'export PATH="/usr/local/opt/ruby/bin:/usr/local/lib/ruby/gems/2.7.0/bin:$PATH"' >> ~/.zshrc
```

该`2.7.0`在上面的命令假定 Homebrew 安装与启动一个Ruby版本`2.7`。如果使用的是不同的版本（可以使用 进行检查`ruby -v`），则替换`2.7`为查看的 Ruby 版本的前两位数字。

```zsh
# 刷新
source ~/.zshrc
```

检查现在是否使用的是非系统版本的 Ruby，运行以下命令：

```zsh
which ruby
```

应该是： `/usr/bin/ruby`

```zsh
ruby -v
# xzy@zairesinatra-MBP ~ % ruby -v
# ruby 3.0.0p0 (2020-12-25 revision 95aff21468) [x86_64-darwin20]
```

现在则可以通过非系统版本的 Ruby 安装 bundler：

```zsh
gem install bundler
```

- **验证 theme 状态**

进入 Clone 下来的 GitHub Pages 项目的路径并执行以下命令：

```zsh
jekyll new . --force
```

这里可能遇见[报错](https://stackoverflow.com/questions/51126403/you-dont-have-write-permissions-for-the-library-ruby-gems-2-3-0-directory-ma)：

```zsh
You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.
```

问题原因：[Ruby 3.0 不再带有 webrick](https://github.com/jekyll/jekyll/issues/8523) ：

```zsh
bundle add webrick # 补 webrick 库
```

完成后，Jekyll 会在刚才执行代码的指定的目录下生成 output 文件，使用如下命令可以通过访问 `127.0.0.1:4000` 查看初始界面：

```zsh
bundle exec jekyll serve
or
bundle exec jekyll serve --livereload
```

<img src = "https://res.cloudinary.com/dzb9ldnvl/image/upload/v1622892582/blog/buddle-add-webrick_nxxqbq.png" width="50%" />

此时可以再更改其他主题：https://jekyllthemes.io/free。下载主题包解压后完整的，复制到的 GitHub Pages 的项目目录里，并覆盖之前的文件即可，当然有些特殊的主题要参考作者给的安装步骤。主题里的所有关键性配置都在 _config.yml 文件中。

需要注意的是，文章的内容和标题需要按照 Jekyll 的格式进行写作。



## 结束

- 提一下 `brew`安装工具操作：

```zsh
# 搜索软件
brew search software
# 安装软件
brew install software
# 卸载软件
brew uninstall brew install 
```

-  Jekyll 之外静态模板系统 ：

1. Node.js 编写的 Hexo（ jekyll 运行速度较慢，所以才开发出 Hexo，Hexo 是把静态页面部署到 GitHub 上，每次更新的内容需要先生成静态页面，再 push 。易因 markdown 语法错误导致错误。 ）

2. Go 编写的 Hugo

3. Python 编写的 Pelican

这里要知道也是使用Ruby作为核心语言的 kramdown 是 markdown 的超集，在 Jekyll 中也是支持的，可用于 Github 搭建博客。有很多一般 markdown 所没有的语法特点，包括和 [GFM](https://help.github.com/articles/github-flavored-markdown/) 也有差异。

```zsh
xzy@zairesinatra-MBP zairesinatra.github.io % cat Gemfile
source "https://rubygems.org"
# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!
gem "jekyll", "~> 4.2.0"
# This is the default theme for new Jekyll sites. You may change this to anything you like.
gem "minimal-mistakes-jekyll", "~> 4.23.0"
# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
# gem "github-pages", group: :jekyll_plugins
# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]


gem "webrick", "~> 1.7"
```

