

title: Viper架构探索实践（一）
date: 2019-10-17 19:35:00
author: Fan Chason
tags: [架构]
categories: 作者：Fan Chason       
comments: true

---

# Viper架构图

![image](http://upload-images.jianshu.io/upload_images/1432381-6aa65d17f5ed033f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![1865432-580872920986b640.png](https://upload-images.jianshu.io/upload_images/1432381-0b5691a0070fbabd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## VIPER的主要部分

### 简介
- View: (视图）： 显示Presenter告知的内容，并将用户输入中继回Presenter。
- Interactor: (交互器）：包含用例指定的业务逻辑。
- Presenter: (表示层，也可称主持人）：包含用于准备显示内容（如从Interactor接收的）和用于对用户输入做出反应（通过从Interactor请求新数据）的视图逻辑。
- Entity: (实体）：包含Interactor使用的基本模型对象。
- Routing: (路由）：包含用于描述按哪个顺序显示哪些屏幕的导航逻辑。

>这种分离也符合[单一责任原则](http://www.objectmentor.com/resources/articles/srp.pdf)。
>Interactor负责业务分析师
>Presenter代表交互设计师
>View负责视觉设计师

### 相比MVX
相比之前的MVX架构，VIPER多出了两个东西：Interactor（交互器）和Router（路由）。

各部分职责如下：
```
- View
提供完整的视图，负责视图的组合、布局、更新
向Presenter提供更新视图的接口
将View相关的事件发送给Presenter

- Presenter
接收并处理来自View的事件
向Interactor请求调用业务逻辑
向Interactor提供View中的数据
接收并处理来自Interactor的数据回调事件
通知View进行更新操作
通过Router跳转到其他View

- Router
提供View之间的跳转功能，减少了模块间的耦合
初始化VIPER的各个模块

- Interactor
维护主要的业务逻辑功能，向Presenter提供现有的业务用例
维护、获取、更新Entity
当有业务相关的事件发生时，处理事件，并通知Presenter

- Entity
和Model一样的数据模型
```
# Viper模版代码生成工具

## 推荐两个模版

### [Viperit](https://github.com/ferranabello/Viperit)
- 支持Swift、SwiftUI
- 可创建viper架构模版

### [Generamba](https://github.com/strongself/Generamba)
- 支持OC和Swift
- 可以创建mvvm、viper模版

## 了解模版语言Liquid

github源码：**[liquid](https://github.com/Shopify/liquid)**
[*Liquid* 模板语言中文文档](https://www.baidu.com/link?url=fK4IqIz8Jd_NwiiLfMbodfFYy5521QApXPJBhxB5yD_ACOge0kdPzMlm5_3B_tT9&wd=&eqid=df8a26640004378f000000045a6a8ac1)

>Liquid 是一门开源的模板语言，由 [Shopify](https://www.shopify.com/) 创造并用 Ruby 实现。它是 Shopify 主题的骨骼，并且被用于加载店铺系统的动态内容。<br>
>从 2006 年起，Liquid 就被 Shopify 所使用，现在更是被大量 web 应用所使用

# Viper架构实践
基于Viperit写了一个简单的新闻的demo
**[XCViperitDemo](https://github.com/FanChason/XCViperitDemo)**


# 参考：
1. viper原作者 By [Jeff Gilbert](mailto:jeff.gilbert@mutualmobile.com) and [Conrad Stoll](https://twitter.com/conradstoll) 著
 [Architecting iOS Apps with VIPER](https://www.objc.io/issues/13-architecture/viper/)
2. [iOS VIPER架构实践(二)：VIPER详解与实现](https://juejin.im/post/599a43035188252432172045)
3. [iOS架构模式-VIPER](https://www.jianshu.com/p/340b39c6d256)