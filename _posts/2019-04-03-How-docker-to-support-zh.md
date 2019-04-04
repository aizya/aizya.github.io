---
layout: post
title:  Docker容器怎么支持中文
date: 2019-04-03
categories: Docker
tags: [Docker]
description: Docker无法正确显示中文,皆为乱码
published: true
---

# Docker显示中文乱码

Docker CentOS服务器不支持中文字符集, 所有的中文皆显示????乱码. 那么应该怎么办呢?

第一步, 使用命令:

    locale -a    // 查看系统所有支持的语言包

如果没有发现**C.UTF-8**或者**zh_CN.utf8**, 那么就直接运行下面命令安装语言包: 
    
    yum -y -q reinstall glibc-common

再运行locale -a,发现并没有任何变化.

问题的根本原因出在Centos默认镜像的/etc/yum.conf里面有一句: 

    override_install_langs=en_US.utf8 

删除这句再运行 

    yum -y -q reinstall glibc-common 
    
就好了.

# 参考链接

1. <a href="https://www.jianshu.com/p/d0c0140ce9e9">docker centos 容器不支持中文字符集</a>

2. The <a href="https://www.rpmfind.net/linux/rpm2html/search.php?query=glibc-common">glibc-common package </a> includes common binaries for the GNU libc libraries, as well as **national language (locale) support**.