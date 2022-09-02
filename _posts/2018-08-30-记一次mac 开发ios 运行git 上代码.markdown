---
layout:     post
title:      "记一次mac 开发ios 运行git 上代码"
subtitle:   " \" 总结 \""
date:       2018-08-30 13:12:10
author:     "Smm"
header-img: "img/011.jpg"
tags:
    - 总结
---


## 前言 
最近在看mac 开发。 自从买了mac 一直都在 mac开始工作了，但是没有在实际中使用，去年十月1号买的。
在之前的公司里面进行开发，项目紧张，所以一直在家使用，没有全投入的进行。又换了新工作到至今才感觉有点上道了。

从git 下载下来的代码我一直都运行不起来，但是有几个可以

是没有pod 使用的demo 可以运行。加上oc或swift 版本更新所以有些版本还需要修改一些代码。

但是swift 代码我居然可以看懂一部分。


最近的ios demo 在github 上已经上传上去，demo 也会更新。

[https://github.com/smm113522/IosDemo!](https://github.com/smm113522/IosDemo! "地址")

[演示](https://github.com/smm113522/IosDemo/blob/master/sexDemo/demoapp1/%E6%95%88%E6%9E%9C%E5%9B%BE.gif)

## 开始swift 的练习

从git 上下载过教程，都是茫茫懂，现在可以看了。感觉轻松了不少。。

这些写一些 关于 swift 语法方法

学习 50集基础swift 基础教程
http://www.iqiyi.com/v_19rrnnvmbg.html?flashvars=videoIsFromQidan%3Ditemviewclkrec#vfrm=5-7-0-1

从头看到尾，最让我头疼的tabview 那块，哪里有很多理解和android 不一样的地方。

Storyboard 的使用，以及约束的添加。

语法包括 if 判断 == != + -

For 语法 函数的使用 里面涵盖很多。。


## 项目出发

https://github.com/smm113522/IosDemo/tree/master/sexDemo/demoapp1

项目地址里面有一个图片可以看一下效果。

里面涉及到StoryBoard 的使用
共享设置
页面跳转和返回效果
动态列表显示
数据传递
监听的添加，Android叫监听。ios称为代理

很多时候是swifi3和swift4差别很多，里面的函数不一样。。增加学习难度。

## cocoapod 的使用。。。

Xcode9 安装cocoapod 插件很费劲。我同事也没有安装上，我试了好久也没有。。

接下来是pod 的安装 步骤总结

Cocoa pod 的使用
1，创建新的项目
2，用命令进入该项目的文件夹内 cd demoapp2
3，touch Podfile 是为了创建文件Podfile 的用的
4，用命令打开文件 open -e Podfile  或者用xcode 打开文件
5，Podfile 进行编辑 内容如下


source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '10.0'
use_frameworks!

target 'demoapp2' do
    pod 'Alamofire' 
end

6.然后保存后，进行pods install 安装成功。

7，重新打开项目，这次用demoapp2.xcworkspace 这样的后缀名字打开项目。进行对alamofie里面的方法就可以使用了。


## 后期

会继续写一些tab 类的。涵盖多demo 应用吧。也会写使用第三方的。



