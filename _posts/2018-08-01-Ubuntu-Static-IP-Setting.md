---
layout: post
title:  Linux静态IP设置
date: 2018-08-01
categories: Linux
tags: [Linux]
description: 如何配置静态IP
published: true
---

# 为什么要设置静态IP？ 

在工作中，为了防止IP经常性的发生变更，导致同事之间协作出现麻烦，一般都需要设置一个静态的IP。

有同学就会问了，我没有设置静态IP，而我的IP也一直都没有变化啊。我默认使用的DHCP自动分配了一个IP，是不是这种方式生成的IP也属于静态IP呢？

并不是。 

IP没有发生变化的主要原因是<a href="https://blog.csdn.net/wangdk789/article/details/27052505">DHCP的自动续约功能</a>。 

# Ubuntu设置静态IP

1. 使用vi修改以下文件：

    vi /etc/network/interfaces

2. 添加以下内容，根据实际要求来写，别闷着头乱抄：

<pre>
    auto eth0
    iface eth0 inet static
    address 192.168.6.84
    netmask 255.255.255.0
    gateway 192.168.6.1
    dns-nameserver 119.29.29.29 # 域名解析服务
</pre>

3. 重启网络，否则不生效

    sudo /etc/init.d/networking restart

    注意，如果出现了错误， 根据它的提示去做。查看它的日志。我碰到的问题是，未发现网卡eth0，因为我本地的网卡叫eno1.

# RedHat/CentOS设置静态IP的方式

1. 使用vi修改以下文件：

    vi /etc/sysconfig/network-scripts/ifcfg-eth0

2. 添加以下内容：

<pre>
    DEVICE=eth0    #网卡名
    BOOTPROTO=static #设置静态
    HWADDR=00:15:17:B2:DC:B5
    ONBOOT=yes
    IPADDR=192.168.6.84 #这个是设置的静态IP地址
    NETMASK=255.255.255.0
    GATEWAY=192.168.6.1 #网关
</pre>

3. 重启网络：

    service network restart

# 总结

设置静态IP的方式有的时候不可取，因为它会长期的占用IP资源，但是很多时候又不得不去设置静态IP。因为每次SSH连接同事的电脑，他的IP地址都再变化，很烦的。

至于设置的方法，都是从网上搬运的，如果存在问题，google一下。

# 参考链接

1. <a href="https://blog.csdn.net/xiaohuozi_2016/article/details/54743992">Ubuntu16.04下设置静态IP</a>

2. <a href="https://blog.csdn.net/qq_708912805/article/details/52107982">Linux设置静态ip后遇到的问题及其解决方案</a>

3. <a href="https://www.jb51.net/article/100311.htm">详解CentOs设置静态IP的方法</a>

3. <a href="https://blog.csdn.net/wangdk789/article/details/27052505">DHCP的IP地址租约、释放</a>

 