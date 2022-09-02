---
layout:     post
title:      "记一次失败的utuntu开发android使用"
subtitle:   " \"ubuntu 使用\""
date:       2017-02-28 10:52:00
author:     "Smm"
header-img: "img/post-bg-2015.jpg"
tags:
    - 生活
---


## 前言 
由于工作环境改变，所以我就把项目切换到ubuntu 中
至今我都不知道为啥要切换到ubuntu中去，就是一直没有去放弃吧。
安装的是
ubuntu 16.04 LTS 7.7 GiB Intel® Core™ i5-4460 CPU @ 3.20GHz × 4 Intel® Haswell Desktop
 
我的安装步骤 和一些解决的办法，互相学习吧。

### jdk 安装
作为开发的开始，java 编译必用功能。
<http://sddhn.blog.163.com/blog/static/128187792013103014453434/>
### svn 安装
<http://www.cnblogs.com/cocowool/archive/2008/11/10/1330932.html>

### android studio安装

	1.先android jdk
	2.安装过程<http://www.open-open.com/lib/view/open1456659628734.html>
	一般为下载压缩文件，解压，然后在运行，sh文件，直接就安装了adk 都帮你装好了。
	
### sdk 下载地址
	<http://gmirror.org/#android-sdk-tools-only> 单独的地址下载
	
### git 安装
	<http://blog.csdn.net/abclixu123/article/details/46464089>
	git 的使用
	切换分支
	git checkout test
	git pull
	git status
	git add .
	git commit -m ""
	git push origin master

### android 安装虚拟机
	虚拟机较为费劲，
	<http://www.jianshu.com/p/b3b4c9ac3f04>
	android studio 中找到 genymotion 用于启动，配置 路径即可。
	genymotion 下载 下来，然后安装 bin 文件安装，直接在命令在目录下genymotion-2.6.0-linux_x64.bin即可。
	就会有文件，找到genymotion启动即可。

	还有其他办法
	<http://www.jianshu.com/p/ce4b1fede738>
	
	genymotion 的运行以来  Virtualbox
	下载安装就可以了。会牵扯到deb 文件怎么安装
	安装deb 文件  dpkg -i
	安装如何在Ubuntu 14.04中安装Virtualbox 4.3.20
	导入下载好的ova文件，在genymotion 中可以看到了。
	在android studio 中安装genymotion插件，可以快速启动。
	插件安装搜一搜就很多了，当然会看到很多好的插件。
	
### svn 的使用
	1.android studio 中 svn 的使用
	2.svn 界面 <http://jingyan.baidu.com/article/215817f7e255bc1edb142355.html>

### ubuntu 使用技巧
	<http://blog.csdn.net/kevinhg/article/details/5934462>
	ubuntu 中其他命令
	进程 gnome-system-monitor
	apt install nodejs-legacy
	apt install npm
	还有很多命令，是自己去看的。
	记录一下吧 <http://www.cnblogs.com/gaopeng527/p/4318034.html>
	dpkg -i 
	关机 重启 进入 不会了搜一搜就可以了。

### 最后

	还有一些命令，ubunte还有一些提示，感觉还是不错的。

	就是输入法不是很理想，目前一切开始ok了。
	唉，由于每次工作都要看百度hi ，web端还上不去，我来回看手机看得我眼疼。
	加上公司事很多，消息收不到，还。。。只好切换到window中去，我只想说我还会回来的。
