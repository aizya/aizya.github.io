---
layout: post
title:  服务器时间的同步方式
date: 2018-08-01
categories: Linux
tags: [Linux]
description: 如何同步服务器时间
published: true
---

# 为什么要进行服务器时间同步？

没有什么玩意就百分之百精确的，尤其是计量时间的玩意，都会存在误差。

对于服务器而言，时间误差就意味着失真，出现问题的风险就会增大。

# 如何更改服务器时间

一般情况下，如果服务器时间出现了问题，比如差了几个小时，那么，为了把时间调整回来，一般常常使用命令：

    date -s "yyyy-MM-dd HH:mm:ss"

为了完成上面的操作，就需要我们跟校对手表一样，掐准了标准时间进行设置，这是普通方法，误差较大。

并且，尽管更改了时候，还是会有问题，我们需要同步一下服务器硬件的时间。第一次听说硬件还有时间...

    hwclock -w

以上，便完成了一个普通的时间矫正方法。

# 如何对服务器时间进行同步？

使用ntpdate时间同步软件，对服务器时间进行同步。

1. 安装ntpupdate：

    yum install -y ntpdate

2. 同步网络时间：

    ntpdate time.nist.gov #参数为时间服务器

3. 调整硬件时间：

    hwclock -w

# 参考链接

1. <a href="https://www.cnblogs.com/itxiongwei/p/5556558.html">如何让linux时间与internet时间同步(centos)</a>

2. <a href="https://blog.csdn.net/u011391839/article/details/62892020">CentOS设置系统时间与网络时间同步</a>