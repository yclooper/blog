---
title: "使用 Hugo+Git Page+Actions 搭建博客"
date: 2021-03-13T23:59:43+08:00
draft: false
---

虽然一直知道，程序员需要有一个博客，方便自己学习和记录，前前后后也折腾过 Jekyll 和 Hexo，但是最后都是几天
的新鲜劲，博客是搭好了，但是文章却没有怎么写。趁着这次使用 Hugo 重新搭建博客，做一下记录，希望后面能坚持下去。

Hugo 是基于 Go 语言的一个静态网站生成器。相比基于 node.js 的 Hexo，天然具有更快的编译速度，并且因为自己是后端
程序员，也更喜欢 Go 一些。

博客的搭建过程大致如下:

### 1. 安装 Go

先去[Go 官网](https://golang.org/dl/)下载对应系统安装包，当然也可以源码编译安装，
安装成功后可以通过 ```go version``` 查看是否安装成功

### 2. 安装 Hugo

我的是 mac 系统，可以直接使用 brew 安装
```$xslt
brew install hugo
```

### 3. 生成 site 和配置主题
安装好 hugo 之后，就可以直接生成site了
```$xslt
# 在sites目录生成blog站点
hugo new site /sites/blog

# 进入到创建的blog目录下
cd /sites/blog
```
我们可以看到站点的目录
```$xslt
  ▸ archetypes/ 
  ▸ content/      # 博客文章目录
  ▸ layouts/      # 布局目录
  ▸ static/       # 静态文件目录
    config.toml   # 配置文件
```
创建第一篇博客，我们会发现 content 目录下多了一个 first.md 文件，这就是我们创建的博客文章啦
```
hugo new fist.md
```
这时，我们先别急着运行站点，因为 hugo 自身没有配置主题，我们需要配置一下主题才能看到博客的内容，
在 hugo 提供的[皮肤列表](https://www.gohugo.org/theme/)中挑选一个主题并配置，这里我使用的 是 LoveIt，
一个简洁、美观又活泼的主题，然后修改 config.toml 配置文件

```$xslt
theme = "LoveIt"
```
这时，我们再运行 ```hugo server --theme=hyde --buildDrafts```，这会启动站点，
在终端会显示本地的访问地址，打开就能访问到我们的站点了

### 4. 发布到 Git Page 并且集成 Github Actions 自动部署
我们运行 ```hugo```命令之后，会在站点目录下生成一个 public 文件夹，
这个就是我们的静态网站文件了，一般我们要把 public 上传到对应的 git page 仓库，
我们就能通过访问例如 ```yclooper.github.io```这样的地址访问到自己的博客站点了（网上有很多通过git page 搭建博客的教程），
麻烦的一点是，每次我们写完博客都要使用 ```hugo``` 指令，重新生成静态文件，
然后 push 到仓库，有没有更方便的方法呢？有的，我们可以使用 Github Actions 功能。

大概逻辑就是:
* 我们有两个仓库上传到 git 仓库，一个是blog源码仓库（就是上面创建的项目），
一个是静态网站仓库（存放 public 静态文件的仓库）
* 每次我们向源码仓库 push 博客的时候，通过 git actions 会自动触发 hugo 构建并把生成 public
静态文件 push 到静态网站仓库
这样可以省去了我们手动构建的过程。现在已经有人
写好了这样的 github action 去部署 hugo 静态网站，
我是使用的[actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)

