---
layout: post
title:  使用NSSM将bat或者exe注册为Windows服务
date: 2020-08-09
categories: Windows
tags: [Windows]
description: 使用NSSM将bat或者exe注册为Windows服务
published: true
---

# NSSM - the Non-Sucking Service Manager

在Windows中,如果想要将某个bat或者exe作为Windows服务,允许其自启动, 有很多方法, 比如使用sc命令,或者搜索关键字instsrv/srvany等. 

有很多方法,本文简单介绍NSSM, 步骤如下, 访问链接: http://nssm.cc/download 下载最新版的NSSM. cmd命令行启动nssm.exe, 注意需要命令行启动.

    nssm install 服务名

然后就是傻瓜式操作, 选择我们需要注册的bat/exe文件, 点击Install Service即可.

![nssm](https://raw.githubusercontent.com/aizya/aizya.github.io/master/assets/nssm.png)

最后打开Windows管理服务,就可以看到我们新添加的服务了. 如果需要设置自启动,那么可以在管理界面自行配置. 

具体配置参照官网.

# 参考链接

1. http://nssm.cc/usage
2. http://www.chengwenit.com/blog/index.php/archives/159/