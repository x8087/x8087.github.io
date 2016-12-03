title: Cocos2d-x实战c++卷
date: 2015-09-15 21:55:52
tags: [cocos2d-x,c++]
---
### 21.从win32到Android平台的移植 ###

#### 21.1 搭建交叉编译和打包环境 ####

CPU指令集不同、架构不同
  -->计算机x86架构CPU
  -->移动设备ARM架构CPU
操作系统：
 - windows：windows XP（32位）、Vista（32或64位）、Windows7（32或64位）、Windows8(32或64位）
 - mac：OSX 10.5.8及以上
 - Linux：Ubuntu 10.04LTS及以上
编译需要准备的软件：Cocos2d-x,JDK,Apache Ant,Python,Android SDK,Android NDK。

1. Cocos2d-x: 3.0以上版本。[官网](http://www.cocos2d-x.org/)
2. JDK：要求JDK6以上版本。[下载地址](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)需要设置系统环境变量JAVA_HOME，变量值为JDK路径。环境变量PATH追加bin路径，防止多个JDK版本对于环境的影响。
3. Apache Ant：[下载地址](http://ant.apache.org/)需要配置环境变量，将bin目录追加到PATH环境变量。
4. Python：[下载地址](https://www.python.org/)需要配置环境变量，Python目录追加到PATH环境变量。使用Python2

#### 21.1.1 安装Android SDK ####

[下载地址](http://developer.android.com/sdk/index.html)
Download the SDK Tools for windows提供Android SDK安装文件
Download the SDK ADT Bundle for windows提供一个工具包，包括安装了ADT插件的Eclipe工具和Android SDK。
交叉编译不需要ADT和Eclipe
使用ADT和Eclipe能够方便管理Android应用的发布、运行和调试。方便管理Android模拟器
C++工具插件CDT
配置NDK环境
Cygwin

##### 21.1.2 管理Android SDK #####

安装目录下SDK Manage文件

##### 21.1.3 管理Android开发模拟器 #####

AVD Manager工具创建和管理Android模拟器


