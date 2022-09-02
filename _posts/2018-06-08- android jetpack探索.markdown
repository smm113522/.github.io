---
layout:     post
title:      "android jetpack 探索"
subtitle:   " \" 学习 \""
date:       2018-06-08 14:19:10
author:     "Smm"
header-img: "img/008.jpg"
tags:
    - 学习
---


## 前言

从google 到大会里面我们只知道了ai 的到来，里面确实有了一些demo和新的说明。
仔细看了一下android studio 工具3.2版本的使用，也看到了一种新的工作模式。
activity+fragment+ViewModel 模式。 我重新创建了一下。
https://developer.android.google.cn/jetpack/
里面有创建和一些说明。
突然发现和ios 开发的StoryBoard 和像

里面的东西都在jetpack 中。jetpack 中主要讲的是

![](http://bp.googleblog.cn/-dwL58chu7wo/WvD1RrHln3I/AAAAAAAAFUg/cRTc0IZga_wMPTWr3CI53IZ5BwtnZMeYACLcBGAs/s1600/Screen%2BShot%2B2018-05-05%2Bat%2B11.49.30%2BAMimage1.png)
 
其中new 是添加的

才发现里面有这么多东西。

### navigation 

![](http://shimengmeng.win/img/navigation.png)

说的是ios 中 StoryBoard 差不多，每次问ios 开发者，你们用StoryBoard开发吗？ 他们说团队开发都不用，

控件都是在代码里面，有时候他们直接复制过来就可以，我很诧异，对于这个，我只能说好看不好用，

不过对于 fragment 跳转到fragment 和activity 会更加简单了，例如，某个模块可以使用，你说所有的功能从首页中进入开始

这样写，未免太单一了，变的无法扩展性，个人建议。

### paging

这个是为了recycleview paging 中viewmodel中 分页处理。

### workmanager

这个一直是我郁闷的，因为我看官方的文档和api 中 和我本地现实的代码 中不匹配，有些方法我看不到和找不到。

例如PeriodicWorkRequestBuilder 中启动和普通的启动时不一样的，但是代码 给的api 中和所给代码是没有的。
我只是能知道有这个东西就行了，等待google 出现新的再用吧。。

不过不可否认的是，他对多个action 的处理，是可以进行的。。。


### android ktx

主要是Kotlin 中google 做出积极的 回应，主要是对Android原始的Api做了一些扩展，方便开发调用，使代码更加自然和简单。

kotlin 中单例的应用 ，实体类 的应用还很陌生。

### android studio 3.2 版本

新版本 3.2 我创建了中的几种demo 形式，都测试了一遍，也都开始新的demo 重新创建了一遍。。

![](http://shimengmeng.win/img/newdemo.png)

从viewpager 到 ad 广告地图等基本的看了一下。

这次出发主要对kotlin 的学习，和新功能探索，还设有对老的知识不作假的进行不同程度的回顾和自己去写。



