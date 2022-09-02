---
layout:     post
title:      "android studio 运行报错收集"
subtitle:   " \" 总结 \""
date:       2018-06-29 15:41:10
author:     "Smm"
header-img: "img/009.jpg"
tags:
    - 总结
---


## 前言 

android开发过程中总会遇到工具和代码中的错误。这里将收集一部分。

## 案例

1，从git 下载下来的如何在本地运行起来。

	首先导入看哪里报错。
	修改本地已经有的build 和Gradle 版本 然后在运行 会有一段下载过程。

2，Android出现：Your project path contains non-ASCII characters.

	把项目目录改成英文，去掉中文路径。

3，Error: java.lang.RuntimeException: Some file crunching failed, see logs for details
	
	是你的图片本身不是png格式，只是名字被改成png结尾了，比如一张jpg的被改成png就会报错
	或者直接关闭Android Studio的PNG合法性检查的，直接不让它检查！！！
	添上这段代码就可以了：
    aaptOptions {
                 cruncherEnabled = false
                 useNewCruncher = false
    }
	http://www.cnblogs.com/jym-sunshine/p/5644449.html；

4，finished with non-zero exit value 1


	http://stackoverflow.com/questions/35990995/com-android-dx-command-main-unsupported-major-minor-version-52-0
	
	在Android中Studeio 2.1转到文件 - >项目结构 - >应用程序 - >构建工具版本。将其更改为23.0.3
	我已经能够通过应用程序设置的gradle降级buildToolsVersion来解决此问题。
	
	
	参考内容：
	
	http://stackoverflow.com/questions/29045129/android-java-exe-finished-with-non-zero-exit-value-1
	
	网上其他的方法介绍：(我的情况是不可以)

    defaultConfig {
        // Enabling multidex support.
	//        multiDexEnabled true
    }

    dexOptions{
        javaMaxHeapSize"1g"
		//一开始设置1g  不行，2g 还是报错，，后面果断4g,就正常了。
    }





5，报错：finished with non-zero exit value 2

	jar包的冲突，
	最后，我是直接在libs里面，引用的jar包，直接通过gradle添加的话，不清楚是什么错误。



他山之石，可以攻玉