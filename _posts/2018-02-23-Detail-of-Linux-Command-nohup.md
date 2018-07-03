---
layout: post
title: nohup命令详解
date: 2018-02-23
categories: Linux
description: nohup命令详解
published: true
---

### 关于nohup命令

当你使用命令行的时候，在命令的前面增加一个nohup能够有效的防止中断，即便是你关机或者退出shell。

对于这个命令，man给我的解释是这样的：

    nohup - run a command immune to hangups, with output to a non-tty

nohup表示 no hangup, 中译：无挂起。

nohup它能帮助我们运行一个命令，怎么运行呢？ 它能immune(免疫) hangups(挂断)，且在一个非tty的地方输出相应的日志。

**我的理解：这个nohup命令能够帮助我们在后台运行命令。** 这样，我们就不用担心在本机运行Shell中跑的某些处理远程服务器的页面关闭之后，会影响服务器的正常使用。 

**并且，nohup命令能够将命令的运行日志输出在一个文件中，而不是直接在tty(暂时理解成控制台)中显示出来。**

当需要你远程连接公司服务器，并且需要启用某些长时间运行的程序时，比如jboss，jetty等等。

你就会意识到nohup这个命令的好处。

即便是你本地关机了之后，服务器上的jboss依旧是在努力的运行中。 而如果没有使用nohup命令，直接运行jboss，关机之后，**shell中的所有的东西都会关闭，包括启动服务器的那个页面。**

### 关于nohup的使用

很简单，直接在命令前增加nohup即可：

    nohup COMMAND [ARG]... &

### 参考文章

<a href="https://linux.101hacks.com/unix/nohup-command/">1. Unix Nohup: Run a Command or Shell-Script Even after You Logout</a>
