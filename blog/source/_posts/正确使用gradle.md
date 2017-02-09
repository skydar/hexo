---
title: 重新认识Gradle
date: 2017-02-09 20:32:32
tags: 构建工具 gradle 
---

# 重新认识Gradle
标签： 构建工具 gradle 
------

之前所有新建的Android Gradle项目都是COPY原有的gradle配置，在没有彻底弄清楚之前，难免出现各种各样的疑问和异常，所以非常有必要重新认识下这个老朋友了。

*首先从以下几个概念出发*
> * gradle wrapper
> * gradle plugin
> * gradle
> * gradlew

*最后给出正确使用的姿势@。@*

## Gradle wrapper

英文文档原文是这样说的：[地址](https://docs.gradle.org/current/userguide/gradle_wrapper.html)
>Most tools require installation on your computer before you can use them. If the installation is easy, you may think that’s fine. But it can be an unnecessary burden on the users of the build. Equally importantly, will the user install the right version of the tool for the build? What if they’re building an old version of the software?
The Gradle Wrapper (henceforth referred to as the “Wrapper”) solves both these problems and is the preferred way of starting a Gradle build

* *在编译的时候，如果还需要安装其他的辅助工具，这对于开发者来说是一个负担*
* *选择一个正确的版本安装辅助工具更加不容易*

gradle wrapper就是为了解决这两个版本而出现的。
白话说就是：团队中人员A,B,C，每个人都有自己的电脑和配置，如何能保证A,B,C使用同一个版本得gradle来编译项目呢，gradle wrapper就用来干这个。

那视觉上的gradle wrapper是什么呢？
在IDEA或者android studio中，选中你的项目，切换到project files视图，会发现就是`gradle/wrapper`这个目录

其中gradle-wrapper.jar ，是使用gradle-wrapper的一个辅助库
gradle-wrapper.properties 内容大体如下:
```
#Tue Sep 27 19:31:03 CST 2016
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-2.10-all.zip
```

分别说明如下：

```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
```
规定解压后的gradle包放在哪里

```
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```
规定gradle zip包放在哪里
GRADLE_USER_HOME 一般情况下代表的路径是:
```
C:\Users\Administrator\.gradle
distributionUrl=https\://services.gradle.org/distributions/gradle-2.10-all.zip
```
其中distributionUrl规定了使用哪个版本的gradle编译项目，这个地址可以配置成服务器地址或者本地地址
项目在编译开始的时候，会检测gradle-wrapper，是否存在已经下载好的gradle版本，如果没有，下载并解压，这样就保证了对于项目的所有使用者，都会使用相同的gradle版本进行编译。

------

## Gradle plugin

实际上，如果不是使用IDEA或者android studio（以下特称IDE），那么我们是可以不用关心gradle plugin版本的。

Gradle plugin ，俗称gradle插件，是IDE为了方便使用gradle进行配置和编译而开发的插件，它跟随gradle版本的变迁而变迁.

在IDE中，项目的根目录下的build.gradle中会配置如下代码
```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.1.0'
    }
}
```
这个dependencies中的gradle:2.1.0代表的就是使用gradle 插件版本 2.1.0。在编译过程中，如果gradle插件版本与gradle版本不匹配，编译就会失败。

目前在使用的gradle与gradle插件版本的对应关系如下:

| grale插件版本| 发布日期 | gradle最低版本 | 依赖的SDK build tools最低版本 |
| -------- | -----: | :----:  | :----: |
| 2.1.3 | 2016.08 | 2.14 | 23.0.2 |
| 2.1.0 | 2016.04 | 2.10 | 23.0.2 |
| 2.0.0 | 2016.04 | 2.10 | 21.1.1 |
| 1.5.0 | 2015.11 | 2.2.1 | 21.1.1 |
| 1.3.1 | 2015.08 | 2.2.1 | 21.1.1 |
| 1.3.0 | 2015.07 | 2.2.1 | 21.1.1 |
| 1.2.0 | 2015.04 | 2.2.1 | 21.1.1 |


------

## Gradle 和 Gradlew
### gradle

由于本文是介绍gradle版本的管理，对于gradle本身的语法和优势就暂不做介绍，实际上到本文为止，作者对于gradle版本了解并不比你多。

用大白话来说一下gradle：

一个基于groovy的项目打包工具
能复用很多的打包过程（tasks）
有一个中央仓库能找到你打包过程中需要依赖的库，并且申明使用很简单
更多深入的理解，读者可自行领悟参阅

### gradlew

gradlew： W意思是wrapper，它是一个用bash命令包装过的gradle编译启动脚本，里面会进行环境变量检测和设置。最终进行编译的还是gradle
```
"%JAVA_EXE%" %DEFAULT_JVM_OPTS% %JAVA_OPTS% %GRADLE_OPTS% "-Dorg.gradle.appname=%APP_BASE_NAME%" -classpath "%CLASSPATH%" org.gradle.wrapper.GradleWrapperMain %CMD_LINE_ARGS%
```
------

## 正确使用gradle（IDEA、AS）
### 修改build.gradle 插件版本号
其实就是配置IDE中gradle plugin
```
dependencies {
    classpath 'com.android.tools.build:gradle:2.1.0'
}
```
### 修改使用的gradle版本
*对应IDE两种Gradle配置的方式*
> **Use default gralde wapper** 
gradle wrapper 中 distributionUrl 修改 gradle版本与插件版本匹配(版本匹配关系上文已给出)
```distributionUrl=https\://services.gradle.org/distributions/gradle-2.10-all.zip```

> **Use local gralde distribution** 
指定你本地的gradle文件夹，同样要注意匹配关系

[1]: http://hucaihua.cn/2016/09/27/Gradle_upgrade/