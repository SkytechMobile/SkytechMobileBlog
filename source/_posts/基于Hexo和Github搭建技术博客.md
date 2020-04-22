---
title: 基于Hexo和Github搭建技术博客
date: 2019-09-26 14:40:33
author: Fan Chason
tags: [搭建博客]
categories: 作者：Fan Chason       
comments: true


---

## 准备:

`nodejs`、`git`、`npm`、`hexo`

验证有没有敲命令，eg: `npm -v`，没有装先去装

- npm可通过homebrew安装

  `brew install npm` 

- Hexo安装

  `npm install hexo-cli -g`

## 开始搭建博客

- 本地创建一个存放博客文件夹

  eg: SkytechMobileBlog

- 切到`SkytechMobileBlog`目录下执行`初始化`

  `hexo init`

- 然后安装依赖

  `npm install`

- 生成静态文件

  `hexo g`

- 创建服务

  `hexo s`

- hexo常用命令：

  | 命令        | 含义                                           |
  | ----------- | ---------------------------------------------- |
  | `hexo init` | 初始化一个文件夹，它会为这个文件夹配置所有骨架 |
  | `hexo g`    | 即hexo generate，生成静态文件                  |
  | `hexo s`    | 即hexo server，创建服务                        |
  | `hexo d`    | 即hexo deploy,用于将本地文件发布到github上     |
  | `hexo n`    | 即hexo new,用于新建一篇文章                    |

  现在只是在本地搭建了一个技术博客，

  要想把博客内容让所有人都能看到，就得借助Github了，把博客内容部署到Github上去

<!--more-->

## 部署到github

1. GitHub上登录/注册一个账号，创建仓库

- 如果是团队博客

  先`New organization`，再`New repository`（名字为`xxx.github.io`，`xxx`为organization名称）

- 如果是个人博客：

  `New repository`（名字为`xxx.github.io`，`xxx`为github账号）

2. 博客的根目录的`_config.yml`文件的底部`deploy`设置为：

```
deploy:
  type: git
  repository: https://github.com/SkytechMobile/SkytechMobile.github.io.git
  branch: master
```

注意：冒号（:）后都有一个空格。你可以把此段代码复制过去，并把`https://github.com/SkytechMobile/SkytechMobile.github.io.git`替换为你自己的Github仓库路径

3. 部署

   根目录下执行

   `hexo g`

   `hexo d`

   或者直接

   `hexo g -d`

   如果此时报错你可以输入`npm install hexo-deployer-git --save`来解决

4. 以上都没什么问题的话，访 http://SkytechMobile.github.io/ 就能看到自己的博客了

## 主题配置

Eg: 把主题设置为`next`，最火的hexo的主题。

1. 下载主题

   在跟目录文件夹下输入如下命令下载next主题

   `git clone https://github.com/iissnan/hexo-theme-next themes/next`

2. 设置主题

   把`SKytechMobileBlog/_config.yml`文件称为`站点配置`

   把`SKytechMobileBlog/_config.yml`文件称为`主题配置`

下载完成后，将`站点配置`文件的`theme`字段的值改为`next`，重新编译并上传到Github上后，访问博客链接，你就会发现主题已经改成next了

next的默认语言为英文，你可以更改为简体中文，找到并打开`站点配置`文件，找到`language`字段，将值改为`zh-Hans`

3. 这里介绍下`站点配置`文件部分字段的含义：

```
title: SkytechMobile团队博客 // 博客名称
subtitle:			   // 博客副名称
description:           // 描述
author: 晨之科SDK团队    // 作者
language: zh-Hans      // 语言
```

## 关于写博客

### 一、如何新建博客

新建命令

`hexo new xxx`

执行上面的命令即在`source/_posts/`目录下新建了名为`xxx的md文件`和`资源文件夹`

### 二、将本地博客发布到线上

生成静态文件，并将本地文件发布到github上，合并执行

`hexo g -d`

### 三、写博客注意点

1. 如何上传带图片的博客？

![](useImage.png)

2. 团队博客设置作者

   可在博客md文件`分类`字段设置

   `categories：作者：Fan Chason`

![](common_on.png)

