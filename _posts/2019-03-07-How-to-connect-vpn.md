---
layout: post
title:  VPN基础教学
date: 2019-03-06
categories: VPN
tags: [VPN]
description: VPN的基础教学
published: true
---

# 什么是VPN

VPN又称之为虚拟专用网络，它的主要功能是在公用网络上建立专用网络，进行加密通讯。VPN又分为不同种类的,在使用客户端连接之前,需要询问**VPN的类型**. 然后采用不同的连接方式.

本文介绍常见的Cisco's AnyConnect SSL VPN, 如果是此类VPN,那么该怎么连接???

1. 使用Cisco's AnyConnect客户端

    通过 https://uci.service-now.com/kb_view.do?sysparm_article=KB0010201 下载客户端,然后安装.

2. 使用OpenConnect连接

    脚本如下: 

    #!/bin/bash
    sudo openconnect ip地址:端口

3. 使用Vpnc连接

# 常见问题

我在用1和3方案使用时都存在问题...方案1,无法连接,提示错误:

    The AnyConnect package on the secure gateway could not be located. You may be experiencing network connectivity issues. Please try connecting again

原因还未可知.

采用方案三: 看不太懂,很多项客户没有提供. 详细情况查看链接,有时候再试试看.

# 参考链接

1. <a href="https://www.jianshu.com/p/e66198ac22cb">ubuntu上安装anyconnect步骤</a>

2. <a href="https://blog.zzstudio.net/system/article_1228.html">Centos系统使用vpnc连接cisco的vpn服务</a>