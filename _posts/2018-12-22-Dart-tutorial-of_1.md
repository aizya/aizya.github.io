---
layout: post
title:  Dart初识
date: 2018-12-22
categories: Dart
tags: [Dart]
description: Dart初识第一章如何安装
published: true
---

# 从Flutter到Dart

最近Flutter非常的火,作为一个热度只有三分钟的人, 肯定要看看这到底是个什么玩意...

其实,移动端开发和我真没有什么交集,没有真正开发过APP, 但是依稀还记得自己步入这个行业的原因....都是那该死的Android...

在我深入了解Flutter之前, 我了解到,它使用的主要语言是.. Dart..

Dart.. 听都没听过.... 网上查了下,大概是Google开发的一门语言,起初的目的是为了替代JS, 但是目前看来,是没有可能了...

其实我觉得这些多数都是噱头... 什么替代不替代,能帮助解决实际问题就是好的... 

好了不说废话了... 主要讲讲今天遇到的一些坑吧...

# 安装配置Dart

其实网上有一大堆配置Dart的, 根本没有必要专门写一篇博客...但是今天,,,真的被坑死了...

Dart的安装包实在太难找了... 

首先,第一点需要确定的, Dart目前有两个稳定版本, 版本1和版本2. 这二者之间的区别很大, 下午就是一直在找Dart2的安装包..

参照这个链接安装并不行,,, 可能是因为我本机的Ubuntu有问题...

所以我是直接下载安装包: 

    https://www.dartlang.org/tools/sdk/archive

在浏览器中输入:

    https://storage.googleapis.com/dart-archive/channels/stable/release/2.0.0/sdk/dartsdk-windows-ia32-release.zip
    https://storage.googleapis.com/dart-archive/channels/stable/release/1.24.3/sdk/dartsdk-macos-x64-release.zip
    https://storage.googleapis.com/dart-archive/channels/dev/release/2.0.0-dev.69.5/sdk/dartsdk-linux-x64-release.zip

然后解压缩,配置环境变量即可:

    vi /etc/profile

    export PATH="$PATH:/opt/dart-sdk-2/bin"

    source /etc/profile

冒号后跟着你本机上dart-sdk的bin路径.... 你看,这个安装,太蠢了...

至于如何使用Dart,我后面看看自己能坚持看下去么, 如果没崩,就会继续往下更...

我使用IDEA开发, 以下教我们怎么在其中安装使用Dart:

    https://www.jianshu.com/p/fa275a08b083

