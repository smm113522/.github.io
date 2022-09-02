---
layout:     post
title:      "nodejs+express从入门到放弃"
subtitle:   ""
date:       2017-01-25 16:36:00
author:     "Smm"
header-img: "img/post-bg-js-module.jpg"
tags:
    - 教程
---
 
## nodejs 中web 项目的开始

###前言

   通过 https://github.com/liumingmusic/react-native_toiletApp 这篇文字中 app源码和后台相关的，想原来后台如此简单
   它让我心动了，我感觉到我的血液在向他流去，我的工作是android 6.0 调试阶段，太慢了，安装android各个版本的系统
   真的很卡，所以我就挤出来的时间来等待了调试了，没有办法，node 恰恰就是命令行，我没有开启ide 去编译它，
   我就用普通的方式去进行吧，从而也提高了我的记忆和我的兴趣。
   
## node 基础
   我从入门就很杂了，在这个 https://github.com/damonare/node-lessons 课程让我继续走下去，
   不管怎么样，基础始终还是很薄弱的，操作能力，还是有的，毅力是复杂的也不怕。
   基本上走了几个爬虫脚本，走了一般上面的项目。感觉还是没有怎么开始。
   
## nodejs+express 的理解
   
   一个服务器，一个后台开发框架，还是很多，知识了解了一下，之后在慢慢去应对吧。
   感觉里面还有很多java web 也能遇到的东西。看着眼熟而已。
   
   express 是什么?
   百度是最好的老师。
   
## express 环境搭建
   从这个基础教程中可以看到。http://blog.csdn.net/sindlly/article/details/52680828
   但是都是模模糊糊的。过程变化很复杂了。
   
   
   
    1、express开发
	1.1 npm环境设置

	1.安装node软件：https://nodejs.org/en
	2.安装淘宝滤镜：npm install -g cnpm --registry = https://registry.npm.taobao.org
	3.设置全局的npm从国内下载资源， npmrc添加配置。mac下面地址为 /Users/liumingming/.npmrc，修改strict-ssl=true 和 registry=https://registry.npm.taobao.org

	1.2 express环境搭建

	1.安装express-generator：npm install -g express-generator，用户快速创建express项目
	2.生成项目模块：进入到项目目录 /User/liumm/A_study/app/toiletApp 下面，执行命令 express -e service，其中-e为ejs模块简写
	3.在服务端项目安装依赖：进入服务端项目 /User/liumm/A_study/app/toiletApp/service 执行命令 cnpm install，安装依赖类库
	4.启动项目：使用在当前目录中使用 npm start启动项目，其中start命令在package.json已经配置
	5.预览：启动已经开发本地的 localhost:3000，访问地址即可看见启动的页面
	6.修改预览：项目中app.js 文件为服务启动入口路径。修改项目下面 views/index.ejs文件，重启服务进行查看
	7.express修改热加载：安装supervisor，npm install supervisor -g，修改项目自动更新

	2、根据环境之后基本步骤：http://blog.sina.com.cn/s/blog_a29eae2b0102vuey.html

	2.1.工程的创建， 目录为three express three -s
		
	2.2.cd three 打开文件夹

	2.3.express -e three 编译程序


	2.4.npm install npm 添加
	从而可以添加mysql 等其他工具了

	**这个必须进行
	2.5.npm init 然后初始化， **这个必须进行

	2.6.进行数据添加 然后添加json 里的数据了 yes 必须写上

	2.7.npm start 就运行了。
	就可以运行浏览器了，上面就有欢迎界面了。

	8.http://localhost:3000/ 就有了welcome 了。

成功现象
![成功现象](https://smm113522.github.io/img/project/express.png "成功现象")


## 总结
   里面有很多坑，要想进步，还得自己去实践代码，这里没有贴代码，我的代码都是复制来的，就是看到懂，也能进行修改，添加。
   关于怎么添加样式，怎么添加数据库，这就是之后要做的了。
	





 
