---
layout: post
title:  CentOS设置定时任务
date: 2019-04-27
categories: Linux
tags: [Linux]
description: CentOS怎么设置定时任务
published: true
---

# 如何安装crontabs

第一步，安装crontabs：

    yum -y install crontabs

这一步在Docker上可能会出现问题，如果出现了 Errno 14 Couldn't resolve host xxxx 的情况，那么可能是服务器的DNS设置的问题，可以尝试在/etc/resolv.conf中添加不同的nameserver， 诸如: 8.8.8.8 

虽然不一定能解决问题，但是至少是一种方向。 具体问题具体看待吧。

安装完成之后，可以使用以下命令：

    service crond start //启动服务
    service crond stop //关闭服务
    service crond restart //重启服务
    service crond reload //重新载入配置
    查看crontab服务状态：service crond status
    手动启动crontab服务：service crond start

# 如何使用crontabs

设置定时任务的方式有以下几种：

1. 直接使用命令： crontab -e 添加对应的任务

2. 编辑 /etc/crontab文件，然后更改，在编辑该文件时，有具体crontabs的使用方式

    <pre>
        # For details see man 4 crontabs

        # Example of job definition:
        # .---------------- minute (0 - 59) 【*表示每分钟执行】 
        # |  .------------- hour (0 - 23) 【*表示每小时执行】
        # |  |  .---------- day of month (1 - 31) 【*表示每天执行】
        # |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ... 【*表示每月执行】
        # |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat 
        # |  |  |  |  |
        # *  *  *  *  * user-name  command to be executed 【表示待执行的脚本或者是命令】
    </pre>

那么，针对我而言，需要定时删除服务器上的/tmp文件目录，脚本放在/usr/local/bin/下：

    #!/bin/bash
   
    rm -rf /tmp/*

crontabs书写方式：

    # 表示每天早上4：25 定时执行这个rmtemp脚本
    25 4 * * * /usr/local/bin/rmtemp

    # 25 4 * * * rm -rf /tmp/*

然后重新加载定时任务即可。

# 总结

定时任务如果涉及到删除操作，一定要事先检查一下脚本，一定要写的十分规范。 并且针对不同版本的Linux服务器，crontabs的使用上可能会存在些许差异。 因此，需要根据具体情况，查找具体的方案。 本文只是为了记录crontabs的基本使用，其实很简单。

# 参考链接

1. <a href="https://www.cnblogs.com/wt645631686/p/6868672.html">CentOS Crontab(定时任务)</a>
2. <a href="https://yq.aliyun.com/articles/553597">yum 安装时错误 Errno 14 Couldn't resolve host 解决办法</a>
