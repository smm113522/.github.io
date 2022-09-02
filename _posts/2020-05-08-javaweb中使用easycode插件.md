---
Xml中配置 
layout:     post
title:      "javaweb中使用EasyCode插件"
subtitle:   " \"java web 学习\""
date:       2020-08-08 21:37:00
author:     "Smm"
header-img: "img/post-bg-2015.jpg"
tags:
    - 教程
---

## 内容

idea 安装EasyCode 插件



在setting 中配置 mybatis plus 中选项

添加一个新的extendController 内容

```xml
##导入宏定义
$!define

##设置表后缀（宏定义）
#setTableSuffix("ExtendController")

##保存文件（宏定义）
#save("/controller/extend", "ExtendController.java")

##包路径（宏定义）
#setPackageSuffix("controller.extend")

##定义服务名
#set($serviceName = $!tool.append($!tool.firstLowerCase($!tableInfo.name), "Service"))

##定义实体对象名
#set($entityName = $!tool.firstLowerCase($!tableInfo.name))

import org.springframework.web.bind.annotation.*;
import $!{tableInfo.savePackageName}.controller.$!{tableInfo.name}Controller;


#tableComment("控制层扩张类，一般初次生成，后续不要覆盖")
@RestController
@RequestMapping("$!tool.firstLowerCase($!tableInfo.name)")
public class $!{tableName} extends $!{tableInfo.name}Controller {

}
```



为了扩展用，上面都是铺路。

配置mybatis plus 设置分页

```java
@Configuration
public class MybatisPlusConfig {
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }
}
```

```xml
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>mybatis-plus-boot-starter</artifactId>
  <version>3.1.2</version>
</dependency>
```

Xml中配置 



1，在idea 中 database 中添加数据库mysql 选择数据库，然后写上用户名和密码，然后选择使用easy code 产生相应 的代码。

2，idea 中配置 之后。选中表之后，开始创建实体类和controller mapper 之后就产生了。



这个输入mybatis 逆向工程。



