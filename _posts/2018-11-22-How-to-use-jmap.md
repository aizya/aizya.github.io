---
layout: post
title:  如何使用jmap进行文件dump操作
date: 2018-11-22
categories: JVM
tags: [JVM]]
description: 从服务器中获取运行的jvm的dump文件进行分析
published: true
---

# 前提概述

最近, 客户的服务器一直崩溃, 查看日志,只能知道是内存溢出, 但是并不知道具体是哪里出现了溢出.

从网上找了一下, JDK本身就提供了一系列的分析工具,在Linux上,就是一些脚本. 

譬如: jps, jstack , jmap等等. 

以公司的服务器举个例子, 将jmap的dump过程记录下来. 其实还是有很多坑的. 

第一步,登录服务器, 直接运行jps. 发现命令没有找到, 这是因为环境变量没有配置好,所以命令找不到..  没关系, 先找找jdk安装在哪?

于是,引出了第一个问题, jdk安装在哪?

    1. which java  -> /usr/bin/java
    2, ll /usr/bin/java  -> /etc/alternatives/java
    3. ll /etc/alternatives/java -> /opt/jdk1.8.0_91/bin/java 

这样就找到了jdk的安装位置...然后也就知道jmap的位置了. 

# jmap 初识

jmap的使用,直接使用jmap --help就可以查看. 

一般使用的最多的方式:

    jmap -dump:format=b,file=heap.hprof PID

其中PID表示需要dump的java的ID, 这个ID可以使用命令jps进行查看:

    jps -l

    [root@pldv ~]# /opt/jdk1.8.0_91/bin/jps -l
    27169 org.apache.zookeeper.server.quorum.QuorumPeerMain
    27250 /data/app/wildfly/jboss-modules.jar    (这个是我们能够用上的,应用程序的PID)
    27494 kafka.Kafka
    9277 sun.tools.jps.Jps

下面开始了:

    jmap -dump:format=b,file=heap.hprof 27250

然后突然就报错了:

    27250: well-known file is not secure

网上一搜,发现需要使用该进程的创建者进行dump操作,所以要去找找,这个PID对应的进程是谁开启的.

问题又来了,怎么根据进程PID,查看进程的创建用户. 比较笨的方式就是,使用top命令,去查看top命令显示的PID对应的USER名称.

    [root@pldv ~]# top
    PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                           
    27250 wildfly   20   0 18.892g 2.878g  38972 S   2.3 18.6  58:02.58 java   

还有一种方式, 可以通过查看: /tmp/hsperfdata_$USER/$PID, 在我这里,$USER可能表示root或者wildfly,

所以我需要查看 /tmp/hsperfdata_root/27250 或者/tmp/hsperfdata_wildfly/27250 这个文件是否存在,

如果存在,就表明这个PID是属于哪个用户的,其实道理都相同的.

知道用户是谁之后,我又自然而然的切换到对应的用户下面去执行dump操作:

    cd /opt/jdk1.8.0_91/bin/ 
    ./jmap -dump:format=b,file=heap.hprof 27250

发现如下错误:

    Dumping heap to /opt/jdk1.8.0_91/bin/heap.hprof ...
    Permission denied

然后我想当然的在命令前面加了个sudo: 

    sudo ./jmap -dump:format=b,file=heap.hprof 27250
    ==>>>
    [sudo] password for wildfly: 
    wildfly 不在 sudoers 文件中。此事将被报告。

提示我输入wildfly的密码, 为此,我又修改了一下wildlfy的密码... 不知道会不会影响hudson的自动编译.

然后我根据提示,又在/etc/sudoers文件中,添加了wildfly用户,具体添加方式看文章最后的相关链接. 

需要注意一点: 在保存的时候,需要使用: wq! 而我习惯了使用: x 进行保存. 所以一直失败....

添加完成之后,我继续执行上一个命令,发现这个问题又出现了:

    27250: well-known file is not secure

于是我心想,这他妈不是一个死结吗, 不加sudo,提示权限不允许, 加了,又说要使用wildfly进行操作. 草... 

在这里卡了很久, 半夜突然想起来,,,这其实是个很蠢的问题,我为什么一定要在/opt/目录下做这些操作呢?????.....

/opt属于root组,本来就拒绝访问... 

切换到wildfly家目录, /data/app/,执行一下命令:

    root用户下:
    sudo -u wildfly /opt/jdk1.8.0_91/bin/jmap -dump:format=b,file=heap.hprof 27250
    wildfly用户下:
    /opt/jdk1.8.0_91/bin/jmap -dump:format=b,file=heap.hprof 27250
    
直接ok了...

我的天,困扰我这么久的问题,就一下解决了.... 剩下的就是如何分析jmap生成的dump文件...

睡觉....


# 相关链接

1. <a href="http://lkf520java.iteye.com/blog/1560686">为什么会"well-known file is not secure" ?</a>

2. <a href="https://stackoverflow.com/questions/9100149/jstack-well-known-file-is-not-secure?rq=1">jstack - well-known file is not secure</a>

3. <a href="https://www.cnblogs.com/mrcln/p/6117267.html">linux用户不在sudoers文件中</a>

4. <a href ="https://zhidao.baidu.com/question/1819360191513980668.html">为什么会"well-known file is not secure</a>