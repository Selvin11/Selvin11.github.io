---
title: React Native 开发环境搭建
date: 2016-11-10
categories: React Native
tags: 
  - React Native
---

分别实践了Windows/OSX上的Android运行环境，以及OSX上的IOS运行环境，记录下了其中的坑洼。
<!-- more -->

> Windows 搭建Android运行环境

[React Native 官方搭建教程]: http://reactnative.cn/docs/0.38/getting-started.html#content

- 首先安装Java，直接在官方网站下载最新版本即可（1.8以上）
- 接下来，安装Python2、Node.js
- 使用npm全局安装react native cli 和 yarn(增加包管理下载速度)
- 下载Android Studio（安卓运行sdk以及安卓虚拟机），根据教程将所有必须项安装
- 点击SDK Manager中的Launch Standalone SDK Manager，找到ADV Manager，打开一个android虚拟机，没有的话就创建一个即可
- `react-native run-android`

执行`react-native run-android`命令后出现的问题

- post占用，使用命令行查找使用该端口的进程，并杀掉即可
- 安卓虚拟机创建，点击Android SDK，点击Launch Standalone SDK Manager，进入AVD Tools进行创建
- 下载Gradle过程出现中断，根据下载路径，用浏览器打开进行离线下载安装，下载之后放置与`/users/username/.gradle/wrapper/dists/`目录下
- 出现Java文件未能执行的问题，配置JAVA_HOME环境变量


> OSX 配置 Android环境

- 基本同上，遇到下载Gradle中断问题，离线下载至`~/.gradle/wrapper/dists`

> 分析各项安装包的作用

1. Android Studio（包含了运行和测试React Native应用所需的Android SDK和模拟器）


1. JDK(JAVA环境，Android Studio运行必备)
2. Android SDK(android运行必备,提供android各版本支持)``Google APIs`、`Android SDK Platform 23`
3. ``Intel x86 Atom System Image`、`Intel x86 Atom_64 System Image`以及`Google APIs Intel x86 Atom_64 System Image`以上几个是虚拟机内置环境
4. `Android SDK Build-Tools 23.0.1`（必须是这个版本）与`Android SDK Platform 23`对应



> 因此方案二：使用性能更好的Genymotion模拟器，放弃`Android Studio`自带模拟器

- 注册Genymotion账号，下载Genymotion和VirtualBox，下载一个版本的机型即可，下载中断问题，同样找到下载路径，进行离线下载，放置于路径`/Users/username/.Genymobile/Genymotion/ova/`即`~/.Genymobile/Genymotion/ova/`(Windows/OSX操作步骤一直)
- 因此关于Android Studio下载的必须项目即`Google APIs`、`Android SDK Platform 23`、`Android SDK Build-Tools 23.0.1`这三个即可，而且在安装界面也可以不勾选内置模拟器`Android Virtual Device `  

> 方案二同比方案一安装完全减少10G作用的容量=。= 环境搭建至此。