---
title: Hexo学习
date: 2021-11-04 01:14:36
sticky: 1
categories: 
  - 博客
tags:
  - 博客
---
# Hexo操作指南

## Hexo安装

```sh
npm install -g hexo
```

## Hexo初始化

```sh
# 创建一个文件夹，进入并初始化，里面hexo 会自动下载一些文件到这个目录
cd /d/myblog
hexo init
```

### 初始化后的结构

```text
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

**注意**：hexo 有 2 个config.yml 文件，一个是站点根目录下的config.yml，一个是 theme（主题）下的_config.yml 文件。

## 博客生成、预览

```sh
hexo g # 生成
hexo s # 启动服务
```

- 执行以上命令之后，hexo 就会在 public 文件夹生成相关 html 文件，这些就是你博客的静态文件，后续需要把这些提交到 GitHub 上。
- hexo s 是开启本地预览服务，打开浏览器访问 [http://localhost:4000](https://link.zhihu.com/?target=http%3A//localhost%3A4000/) 即可看到内容，很多人会碰到浏览器一直在转圈但是就是加载不出来的问题，一般情况下是因为端口占用的缘故。

## 修改主题

首先下载 hexo-theme-yilia 这个主题：

```sh
cd /d/document/GitHub/hexo/
git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```

下载的主题文件都在 theme 目录下。

然后我们将 Hexo 根目录下的_config.yml 中的 theme: landscape 改为 theme: yilia，然后重新执行 hexo g 来重新生成。

注意：如果出现一些莫名其妙的问题，可以先执行

```text
hexo clean
```

来清理一下 public 的内容，然后再重新生成。

## 部署到 GitHub

如果你一切都配置好了，接下来就是把博客部署到 GitHub 上：

```sh
hexo d
```

注意：

- ssh key 配置好(**如果是https方式可以不配**)。
- 配置_config.yml 中有关 deploy 的部分（注意缩进）。

```text
deploy:
  type: git
  repository: git@github.com:dta0502/dta0502.github.io.git # 用https或者ssh均可
  branch: master
```

直接执行 hexo d 的话一般会报如下错误：

```text
Deployer not found: git
```

这是因为缺少了一个插件，我们可以通过如下命令安装：

```text
npm install hexo-deployer-git --save
```

然后输入 hexo d 就会将本次有改动的代码全部提交。

# 常用的 hexo 命令

## 收集一

```sh
hexo new "postName"      # 新建文章
hexo new page "pageName" # 新建页面
hexo generate            # 生成静态页面至public目录
hexo server              # 开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy              # 部署到GitHub
hexo help                # 查看帮助
hexo version             # 查看Hexo的版本
缩写命令：
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
组合命令：
hexo s -g   # 生成并本地预览
hexo d -g   # 生成并上传
```

## 收集二

```sh
npm install hexo -g #安装Hexo
npm update hexo -g #升级
hexo init #初始化博客

命令简写
hexo n "我的博客" == hexo new "我的博客" #新建文章
hexo g == hexo generate #生成
hexo s == hexo server #启动服务预览
hexo d == hexo deploy #部署

hexo server #Hexo会监视文件变动并自动更新，无须重启服务器
hexo server -s #静态模式
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP
hexo clean #清除缓存，若是网页正常情况下可以忽略这条命令
```

# 写文章

我们可以在 hexo 根目录下执行命令：

```sh
hexo new 'my-first-blog'
```

hexo 会帮我们在_posts 下生成相关 md 文件，我们只需要打开这个文件就可以开始写博客了，用这个命令的好处是帮我们**自动生成了文章创建时间**。