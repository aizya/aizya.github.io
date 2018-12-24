---
layout: post
title:  在任意目录下运行自定义脚本
date: 2018-12-24
categories: Shell
tags: [Shell]
description: 如何在任意目录下运行自定义脚本
published: true
---

# 如何在任意目录下运行自定义脚本?

一般而言,新建的脚本,需要先通过chmod命令,增加可执行权限. 但是运行的时候,一般都是通过"./脚本名"的方式运行. 

如果想要在电脑的任意目录下运行, 我之前的做法是将脚本放到"$HOME/bin"目录下. 今天突然想, 如果没有放到这个目录下,貌似也能运行..

于是在网上查了下, 确实可以, 前提是需要配置一下环境变量, 将脚本存放的目录加入到\$PATH路径下,其实这样一想就明白了, 比如我们安装JAVA的时候,也需要将\$JAVA_HOME/bin目录加入到\$HOME目录下. 这样全局就能使用JAVA中自带的一些脚本命令了. 

方式如下(可以修改/etc/profile或者是某个用户下的~/.bashrc或者 ~/.profile文件): 

    export PATH=$PATH:/PATH/OF/SHELL

最后在source一下这个配置文件即可.

这样就能够直接使用这个目录下的所有的脚本.

以上.

# 参考链接

1. <a href="https://blog.csdn.net/a394268045/article/details/51985970">在任意目录下使用某个shell脚本
</a>