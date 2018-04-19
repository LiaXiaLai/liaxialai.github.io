---
title: Android 介绍
date: 2018-03-07 11:54:05
tags: [Android]
published: true
---

![](http://xuesheblog.oss-cn-beijing.aliyuncs.com/images/mmhx8.png)

[Android](https://www.android.com) 最初是由安迪·鲁宾领导开发的一个项目。目前，Android 凭借着开源的优势，在智能手机、智能手表、智能电视等领域占有越来越大的份额，成为全球第一的智能操作系统。

# 一、Android 体系结构

Android 体系结构采用了分层的、自顶向下的思想。从上层到底层共包括四层：应用层、应用框架层、系统运行库层和 Linux 内核层。

![](http://xuesheblog.oss-cn-beijing.aliyuncs.com/images/w522a.png)

**（1）应用层**

Android 平台不仅仅是操作系统，也包括了许许多多的应用。所有安装在手机上的应用程序都是属于这一层的，比如自带的联系人、照相机、浏览器等等程序，或者是从 Google Play 上下载的游戏，当然，也包括自己开发的程序。这些应用程序都是由 Java / Kotlin 语言编写的。

**（2）应用框架层**

应用框架层是 Android 应用开发的基础。该层主要提供了构建应用程序可能用到的各种 API。Android 自带的一些核心应用就是使用这些 API 完成的，开发者也可以根据这些 API 来构建属于自己的程序。

**（3）系统运行库层**

系统运行库通过 C/C++ 库来为 Android 系统提供主要的特性支持。该层可以分成两部分：

+ 系统库: 应用程序框架的支撑，用来连接应用框架层与 Linux 内核层的重要纽带。如 SQLite、OpenGL|ES、Webkit等。
+ Android 运行时库: 包括了核心库和 Dalvik 虚拟机（5.0 只有改为 ART 虚拟机）。核心库允许了开发者使用 Java / Kotlin 来编写 Android 应用。Dalvik 虚拟机允许每个应用程序运行在独立的进程中，并且拥有自己的虚拟机实例。并且虚拟机完成了对生命周期的管理、堆栈管理、线程管理、安全和异常的管理以及垃圾回收等重要功能。

**（4）Linux 内核层**

Android 系统是基于 Linux 内核的，这一层为 Android 设备提供了各种硬件的底层驱动，如显示驱动、音频驱动、蓝牙驱动、照相机驱动、WiFi 驱动、电源管理等。

# 二、Android 程序结构

 ![](http://xuesheblog.oss-cn-beijing.aliyuncs.com/images/fi2r1.png)

 **（1）java 目录**

java 目录是放置我们所有 Java / Kotlin 代码的地方。

**（2）res 目录**

res 目录资源目录，可以存放应用程序使用到的各种资源。如 XML 界面文件、图片、数据等。

+ drawable: 存放图片资源。
+ layout: 存放界面资源。
+ values: 存放字符串资源。
+ mipmap: Google 建议只存放程序启动图片。

**（3）manifests 目录**

该目录一般只存在 **AndroidManifest.xml** 文件，该文件是 Android 项目的配置文件，列出了应用程序提供的功能。我们在程序中定义的所有四大组件都需要在这个文件里注册，还可以在这个文件中声明所需要的权限等。后续将详细说明。

**（4）build.gradle 文件**

build.gradle 是 app 模块的 gradle 构建脚本，会指定很多项目构建的相关配置，后续将详细说明。

**（5）proguard-rules.pro 文件**

proguard-rules.pro 文件用于指定项目代码的混淆规则，主要为了防止代码被别人破解，将代码进行混淆，从而让破解这难以阅读。

**（6）其他文件目录**

其余文件及目录一般为 Android Studio 的配置文件及自动生成。

# 三、Android 应用的四大组件介绍

## 1. Activity: 应用表示层（基类 Activity）

Activity 表示可视化的界面，一个 Activity 表示一个界面。虽然多个 Activity 一起工作形成了一个整体的用户界面，但是同一应用程序中的每个 Activity 是相互独立的。

每一个 Activity 都是作为 Activity 基类的子类实现。

## 2. Service: 没有可见的用户界面，但能够长时间运行于后台（基类 Service）

Service 是一个没有用户界面的在后台运行执行消耗操作的应用组件。例如，当用户在其他应用中时，服务可能在后台播放音乐或者通过网络获取数据，但是不会阻断用户与当前的 Activity 交互。Service 与 Activity 一样都存在于当前进程的主线程中，所以，一些阻塞 UI 的操作，不能放在 Service 中进行。

每一个 Service 作为 Service 基类的子类实现。

## 3. Broadcast Receiver: 用户接受广播通知的组件（基类 BroadcastRecevier）

Broadcast Provider 仅是接收广播并作出相应的响应。例如，通知屏幕关闭、电池电量不足等广播。尽管 Broadcast Provider 不提供用户界面，但它们可以**创建状态栏通知**，在发生广播事件时提醒用户。

Broadcast Recevier 作为 BroadcastRecevier 基类的子类实现，并且每条广播都作为 **Intent** 对象进行传递。

## 4. Content Provider: 应用程序间数据通信、共享（基类 ContentProvider）。

Content Provider 管理一组共享的应用数据，可以将数据存储在文件系统、SQLite 数据库、网络上或可访问的任何其他永久存储位置。为应用间数据的共享提供了可能，并且能够实现自我数据的存储。

Content Provider 作为 ContentProvider 基类的子类实现。

# 附

下图为 Android 系统版本及其详细信息。

![](http://xuesheblog.oss-cn-beijing.aliyuncs.com/images/ta9vu.png)

# 参考链接

+ [Wikipedia: Android](https://zh.wikipedia.org/wiki/Android), by Wikipedia
+ [Android Developer](https://developer.android.com/index.html?hl=zh-tw), by Google
+ [第一行代码 第二版](http://blog.csdn.net/guolin_blog/article/details/52032038), by Guolin

（完）
