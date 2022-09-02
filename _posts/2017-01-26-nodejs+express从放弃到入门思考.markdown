---
layout:     post
title:      "nodejs+express 从放弃到入门思考"
subtitle:   ""
date:       2017-01-26 12:36:00
author:     "Smm"
header-img: "img/post-bg-js-module.jpg"
tags:
    - 教程
---
## 继续进行 nodejs
	
  从上次node 环境搭建和node学习，一直没有放弃过，只是少了一些代码的研究，只要看基础的东西，
  还有一些关于node 能做什么进行了思考。
  这次开始nodejs 是从ide中开始的。
  我使用的ied 工具
  
  
	WebStorm 2016.3.2
	Build #WS-163.9166.30, built on December 12, 2016
	Licensed to lan yu
	Subscription is active until November 23, 2017
	JRE: 1.8.0_91-b14 amd64
	JVM: Java HotSpot(TM) 64-Bit Server VM by Oracle Corporation
	
  这里面已经给了很多能用的东西了，少了一下命令了，多的是一下，代码的编写上了。
  主要根据
  http://www.tuicool.com/articles/JfqYN3I
  这篇文章上代码编写和研究，对于我写代码有了重新的认识了。
  
  这个主要可能能进行接口开发。所以我没有纠结，因为我从第一个项目中，得到的最多的是接口中的数据，
  所以只能向接口开发出发了，自己写接口，自己调用是不是很爽，每次都找后台，他们说先做其他功能之后在调试，
  现在说去你的吧。
   

## node 能做什么

  我也想知道node 能做什么，网上也说了一些，关于书籍的推荐，自己体会他的神奇之处。
  正如JavaScript 为客户端而生，Node.js 为网络而生。Node.js 能做的远不止开发一个网站那么简单，使用Node.js，你可以轻松地开发：
  具有复杂逻辑的网站； 
  1.基于社交网络的大规模Web 应用；
  2.Web Socket 服务器；
  3.TCP/UDP 套接字应用程序；
  4.命令行工具；
  5.交互式终端程序；
  6.带有图形用户界面的本地应用程序；
  7.单元测试工具；
  8.客户端JavaScript 编译器。
  Node.js 内建了HTTP 服务器支持，也就是说你可以轻而易举地实现一个网站和服务器的组合。这和PHP、Perl 不一样，因为在使用PHP 的时候，必须先搭建一个Apache 之类的HTTP 服务器，然后通过HTTP 服务器的模块加载或CGI 调用，才能将PHP 脚本的执行结果呈现给用户。而当你使用Node.js 时，不用额外搭建一个HTTP 服务器，因为Node.js 本身就内建了一个。这个服务器不仅可以用来调试代码，而且它本身就可以部署到产品环境，它的性能足以满足要求。

  Node.js 还可以部署到非网络应用的环境下，比如一个命令行工具。Node.js 还可以调用C/C++ 的代码，这样可以充分利用已有的诸多函数库，也可以将对性能要求非常高的部分用C/C++ 来实现。
  
  还可以做其他的吧，我也知道的有限。

## node 接口开发
  生产出来的文件，以及文件里面的具体是什么？
  项目创建成功之后，生成四个文件夹，主文件app.js与配置信息文件packetage.json

  bin是项目的启动文件，配置以什么方式启动项目，默认 npm start

  public是项目的静态文件，放置js css img等文件

  routes是项目的路由信息文件,控制地址路由

  views是视图文件，放置模板文件ejs或jade等（其实就相当于html形式文件啦~)

  express这样的MVC框架模式，是一个Web项目的基本构成。
  
  里面就是配置和代码接口的文件了。express 也公开了源码，也可以去看看。
  
  这里真不知道写教程呢还是写体会呢。
  
  接口返回来的json 数据即可。
  
  作为android 开发，得到的就是数据，另外想后天转，也想做有用的东西，并且不是说写个接口就完了，还得去学习新的东西，
  所以我真的要明白node 能做什么？以及我放弃的时候，想到是javaee 方向，对工作流和大数据以及分布式，进行悔恨的测试去。
  因为他们的环境不太好搭建。至少我觉得是这样的。
  
  如果真的想接口和界面开发的请看这个教程
  https://github.com/imwtr/nodejs_express_login_register/
  
  这个小的demo 也研究了不是时间，大体的教程就这样的。
  配置mysql 也好，配置mongodb 也行，得花时间去研究连接数据库东西。
  让我体会到数据库方法，基础还是有点弱，没有刚毕业写sql 会的多。



