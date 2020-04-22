# SkytechMobileBlog

1. 在`source/_posts/`目录下创建`.md`博客文件（推荐`Typora`软件）

有两种创建新博客的方法：

1> 直接创建md文件或者拷贝md文件到`source/_posts/`目录下

2> 根目录下使用命令行创建
`hexo new 博客文件名字`

2. 添加文章属性（例如）：

```
---
title: 搭建技术博客 // 博文标题
tags: [hexo,github]  // 标签      
comments: true               
---
```
3. 设置作者

   `categories: 作者：Fan Chason`
   目前看，只有这个方法添加作者名字

4. 插入图片
   在`source/_posts/`目录下创建与md文件同名的文件名
   图片资源放在文件夹中md中通过`![](xxx.png)` 引用图片

5. 写正文

6. 上传博客，提交到远程
`hexo g -d`
