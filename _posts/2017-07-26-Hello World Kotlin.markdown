---
layout:     post
title:      "Kotlin Demo 第一次尝试"
subtitle:   " \"教程 \""
date:       2017-07-26 14:14:00
author:     "Smm"
header-img: "img/helloWorld.jpg"
tags:
    - 教程
---


1，ketlin 的 好处  看这里就可以了。
 http://www.jianshu.com/p/c4750300b862
2，代码尝试

①打开android studio 安装kotlin 插件。

如果慢，等。 打开 File -> Settings -> Plugins，点击右侧下方第一个按钮 “Install JetBrains plugins...” ，

搜索 kotlin ，选中 ‘Kotlin Extensions For [android](http://lib.csdn.net/base/android)’，

点击右侧 “Install plugin”，安装过程中会要求安装 Kotlin，确认就好。安装完成后重启生效。
②  先建项目。demo 项目。空的就行。

③ 完成后，在MainActivity中，Ctrl+Shift+Alt+K，

即可完成[Java](http://lib.csdn.net/base/java)文件向kotlin文件的自动转换。可以自己写。

④ build.gradle中的配置。直接附代码了.



		ext.kotlin_version = "1.0.4"
		buildscript {
		    ext.kotlin_version = '1.0.4'
		    repositories {
		        jcenter()
		        mavenCentral()
		    }
		    dependencies {
		        classpath 'com.android.tools.build:gradle:2.3.1'
		    }
		//    dependencies {
		//        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
		//    }
		
		}

	allprojects {
	    repositories {
	        jcenter()
	        mavenCentral()
	    }
	}
	
	task clean(type: Delete) {
	    delete rootProject.buildDir
	}

```

另一个
```
		
	//apply plugin: 'com.android.application'
	buildscript {
	    repositories {
	        jcenter()
	    }
	    dependencies {
	        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
	    }
	}

	apply plugin: 'com.android.application'
	apply plugin: 'kotlin-android'
	apply plugin: 'kotlin-android-extensions'
	
	android {
	    compileSdkVersion 25
	    buildToolsVersion "25.0.2"
	    defaultConfig {
	        applicationId "com.smm.ketlindemo"
	        minSdkVersion 15
	        targetSdkVersion 25
	        versionCode 1
	        versionName "1.0"
	        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
	    }
	    buildTypes {
	        release {
	            minifyEnabled false
	            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
	        }
	    }
	    sourceSets {
	        main.java.srcDirs += 'src/main/kotlin'
	    }
	}
	
	 
	dependencies {
	    compile fileTree(dir: 'libs', include: ['*.jar'])
	
	    compile 'com.android.support:appcompat-v7:25.3.1'
	    compile "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
	}


```


maniactivity.java 中代码。


 

	package com.smm.ketlindemo
	
	import android.support.v7.app.AppCompatActivity
	import android.os.Bundle
	
	class MainActivity : AppCompatActivity() {
	
	    override fun onCreate(savedInstanceState: Bundle?) {
	        super.onCreate(savedInstanceState)
	        setContentView(R.layout.activity_main)
	    }
	}


 

其他代码不动。

开始吧。有人说五分钟。
看你的表现了。。。