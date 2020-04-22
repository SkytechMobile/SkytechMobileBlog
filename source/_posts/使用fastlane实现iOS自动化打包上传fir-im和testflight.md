---
title: 使用fastlane实现iOS自动化打包上传fir.im和testflight
date: 2019-10-11 16:41:33
author: Fan Chason
tags: [自动化打包]
categories: 作者：Fan Chason       
comments: true
---

## 前言

*日复一日重复打包工作，是在浪费宝贵的时间*

*自动化打包工具应运而生，是我们解放双手的利器*

[fastlane官方文档](https://docs.fastlane.tools/)

[github地址](https://github.com/fastlane/fastlane/tree/master/fastlane/docs#fastfile)

## 安装fastlane

### 安装最新的Xcode命令行工具

> 可以[Developer Apple](https://developer.apple.com/download/more/)上手动下载对应的`Command Line Tools` 安装

*fastlane官方推荐的`xcode-select --install` 安装时最后结果是**不能安装该软件***

### 安装 fastlane

*放到`/usr/local/bin`下面*

`sudo gem install -n /usr/local/bin fastlane`

其他的安装方法：（扩展，可跳过）

![](1970838-d07d8f2b55853319.png)

> 第一种是通过 Homebrew 软件包管理器来进行安装，<br>第二种就是我们最熟悉的方式，下载安装包来进行安装,<br>第三种是通过 RubyGems 来进行，RubyGems 是一个方便的 Ruby 程序包管理器，可以将一个 Ruby 应用程序打包到一个 Gem 里，作为一个安装单元。 一个  Gem 就是一个封装起来的应用程序或代码库

## 配置环境变量

添加用户环境变量 

`vim ~/.bash_profile`

添加

```
...
export PATH=/usr/local/bin:$PATH
...
```

保存退出，使用 `source ~/.bash_profile` 来使配置立即生效

现在在项目根目录下可以使用`fastlane`了

## fastlane使用

### 在项目根目录下初始化

`fastlane init`

- 四个选项

  ```
  What would you like to use fastlane for?
  
  1.Automate screenshots
  2.Automate beta distribution to TestFlight
  3.Automate App Store distribution
  4.Manual setup - manually setup your project to automate your tasks
  ```

  解释：

  ```
  1 自动截屏。（帮助我们截取App的显示到appstore上的 截图）
  2 自动发布beta到TestFlight上，用于内测。
  3 自动打包发布到AppStore上。
  4 手动设置。
  ```

  

  上传fir选择的4

- 初始化成功会生成如下目录

![](Xnip2019-10-09_14-40-37.jpg)

### 配置相关文件

#### 配置Gemfile

如果使用cocoapods要加上（*注意带上当前安装的pod的版本号*，否则会报错）

`gem 'cocoapods', '~>1.8.3'` 

执行 

`bundle install`

> 第一次运行 bundle install 时自动生成 Gemfile.lock 文件。以后每次运行 bundle install 时,如果 Gemfile 中的条目不变 bundle 就不会再次计算 gem 依赖版本号，直接根据 Gemfile.lock 检查和安装 gem。如果出现依赖冲突时可以通过 bundle update 更新 Gemfile.lock

#### 配置Fastfile

```ruby
default_platform(:ios)

platform :ios do
  
  	before_all do
    	# 如果你用 pod install
    	cocoapods 
  	end
  
  	desc "打包上传ipa到fir.im"
  	lane :fir do
    
  	# 如果你没有申请adhoc证书，sigh会自动帮你申请，并且添加到Xcode里
  	#	sigh(adhoc: true)
      
  	# 以下两个action来自fastlane-plugin-versioning，
  	# 第一个递增 Build，第二个设定Version。
  	# 如果你有多个target，就必须指定target的值，否则它会直接找找到的第一个plist修改
  	# 在这里我建议每一个打的包的Build都要不一样，这样crash了拿到日志，可以对应到ipa上
  
    increment_build_number_in_plist(
        target: 'TestDemo',
 				build_number: '5'
    )
    increment_version_number_in_plist(
        target: 'TestDemo',
        version_number: '1.0'
    )
    # gym用来编译ipa
    gym(
        scheme: 'TestDemo',
        export_method: "ad-hoc", # 指定打包方式 ["app-store", "ad-hoc", "package", "enterprise", "development", "developer-id"]
        #teamID: "xxxxxx",  # developer.apple.com 上查看
        xcargs: "-allowProvisioningUpdates",
        output_directory: './firim',
        output_name: 'TestDemo.ipa'
    )
    # 上传ipa到fir.im服务器，在fir.im获取firim_api_token
    firim(firim_api_token: "xxxxxxxxxxxxx")  # token 在fir 上查看。
  end
  
  desc "打包上传testflight/app-store"
  lane :tf do

    increment_build_number_in_plist(
        target: 'TestDemo'
    )
    increment_version_number_in_plist(
        target: 'TestDemo',
        version_number: '1.0'
    )
    # gym用来编译ipa
    gym(
        scheme: 'TestDemo',
        export_method: "app-store", # 指定打包方式
        #teamID: "xxxxxx",  # developer.apple.com 上查看
        xcargs: "-allowProvisioningUpdates",
        output_directory: './testflight',
        output_name: 'TestDemo.ipa'
    )
    #upload_to_testflight
    appstore       # 上传你的App iTunes Connect
  end
end
```

> 关于build_number与version_number
>
> 1,version_number、build_number都没有设置，会自动获取项目的version和build版本号，并且都自动加1。例如，fastlane打包前后版本号变化：ver1.0.2（Build 11）-> ver1.0.3（Build 12）；
> 2,version_number设置了、build_number没设置，会自动获取项目build版本号，build版本号+1；
> 3,version_number、build_number都设置了，那打包出来的版本号就是设置的版本号，不会自动+1；

#### 添加两个插件

```
fastlane add_plugin versioning
fastlane add_plugin firim
```

### 执行打包

#### 上传fir.im

`fastlane fir`

> `fir`为Fastfile文件中`lane :fir do`处设置的名字，可以为别的名字

执行打包成功如下图所示：

<img src="Xnip2019-10-10_15-01-27.jpg" style="zoom:5%" />

- 根目录下`firim`文件夹下可看到`ipa`文件

  ![](Xnip2019-10-10_15-22-43.jpg)

- fir.im应用列表，多了刚上传的项目

  ![](Xnip2019-10-10_15-01-59.jpg)

#### 上传testFlight

`fastlane tf`

## 问题

1. Could not find action, lane or variable 'increment_build_number_in_plist'...

![](Xnip2019-10-10_11-39-52.jpg)

- 解决：

  `fastlane add_plugin versioning`

参考：https://github.com/SiarheiFedartsou/fastlane-plugin-versioning/issues/20

## 参考

1. [fastlane 自动打包到 fir.im 的踩坑之路](https://www.jianshu.com/p/2918cf082b9d)
2. [fastlane ios快读入门文档](https://docs.fastlane.tools/getting-started/ios/setup/)
3. [fastlane使用说明书](https://www.jianshu.com/p/19ae8cc865b0)
4. [fastlane 在mac上配置iOS自动化上架](https://www.jianshu.com/p/48343a655f75)
5. [iOS 自动打包 - fastlane （一）](https://www.jianshu.com/p/eab3e1ded17d)
6. [和重复劳动说再见-使用fastlane进行iOS打包](https://juejin.im/post/5a7d51986fb9a063435ece35)
7. [macOS/Linux 环境变量设置](https://zhuanlan.zhihu.com/p/25976099)