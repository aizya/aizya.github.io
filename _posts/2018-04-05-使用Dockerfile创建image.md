---
layout: post
title: 使用Dockerfile创建image
date: 2018-04-05
categories: Docker
description: Dockerfile的使用
published: true
---

### 创建一个Dockerfile文件

    FROM node
    MAINTAINER xionghongzhi@polelink.com
    RUN git clone -q https://github.com/docker-in-practice/todo.git
    WORKDIR todo
    RUN npm install > /dev/null
    EXPOSE 8000
    CMD ["npm","start"]

里面填什么内容自己看着办吧，我这也是从哪里抄过来的，关于这个文件，我就讲两点:

1. 文件名必须为Dockerfile，不能是其他的。
2. 这个文件有点想shell脚本，每一步就写的很清楚，一步一步往下走。

### 执行这个文件创建Docker image镜像

1. 使用命令: docker build . (其中.表示Dockerfile所在的目录)
2. 使用1中提到的命令，很有可能会报错，因为墙的原因。

### 问题概述

错误提示: 

    xiaoxiong@xiaoxiong-Latitude-E5440:~/playground/docker$ docker build .
    Sending build context to Docker daemon  2.048kB
    Step 1/7 : FROM node
    latest: Pulling from library/node
    f2b6b4884fc8: Pulling fs layer 
    4fb899b4df21: Pulling fs layer 
    74eaa8be7221: Pulling fs layer 
    2d6e98fe4040: Waiting 
    452c06dec5fa: Waiting 
    7b3c215894de: Waiting 
    7387f894fec9: Waiting 
    ab02176cc6fc: Waiting 
    error pulling image configuration: Get https://dseasb33srnrn.cloudfront.net/registry-v2/docker/registry/v2/blobs/sha256/42/42e85254dd8f17a23c85de881f6d76dbe71f25ee80b25ca064b4fc13b8a9333c/data?Expires=1522954920&Signature=Svj5U4CWihT~ySOd519thXB0Emx9GLz247iIawykV2JHVGnOPT7KlrUqQhC5VXyjTFuFlGwIbWMJF35CvKUxl0iU5-HxdaYI9~DkIa-y7wT13LD2cCux3UM0y~XM1bhsTfdwvLsdeWPzMZaxi3sQZ1TtPQNgsnau~idakD8DMNw_&Key-Pair-Id=APKAJECH5M7VWIS5YZ6Q: net/http: TLS handshake timeout

那么该怎么办呢? 网上尽管一大堆的解决方法，但是还是花费了我大量的时间，我爬了一个又一个坑，根本的原因就是我之前提到的一样，**我的电脑无法访问国外的docker默认的镜像地址**，然后我需要做的就是换一个国内的镜像。

### 解决方案

我先是找到了这样的一个<a href="https://blog.csdn.net/sfdst/article/details/69336273">博客</a>，上面的思路就是未docker设置一个新的镜像。

1. 先停止docker: systemctl stop  docker

2. 使用如下命令:

    echo "DOCKER_OPTS=\"\$DOCKER_OPTS --registry-mirror=http://f2d6cb40.m.daocloud.io\"" | sudo tee -a /etc/default/docker

tee命令的作用接收标准输入，然后写入到标准输出或者是文件中，格式一般是: tee [-OPTION]... [FILE]... ， 其中-a 表示append到末尾，不会覆盖。详情可参照man命令查看。
 
3. 最后重启docker: service docker restart

经过我刻苦的尝试，上面这种情况对我而言并不奏效，但是我觉得**这种做法是没有错的**，所以我也很无语。

然后又是一通连滚键盘的搜索，找到了当前国内用的比较多的两个镜像公司，一个是阿里爸爸，另外一个是<a href="https://www.daocloud.io/">DaoCloud</a>。

然后我尝试着注册了一下，我真的是死马当成活马医了，所以码农真的很酷么。。

然后发现了这样一行代码:

    curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://be7a63b0.m.daocloud.io

该脚本的备注是:

    该脚本可以将 --registry-mirror 加入到你的 Docker 配置文件 /etc/docker/daemon.json 中。适用于 Ubuntu14.04、Debian、CentOS6 、CentOS7、Fedora、Arch Linux、openSUSE Leap 42.1，其他版本可能有细微不同.

然后重启一下docker服务，发现问题解决了。

真是九九八十一难，我一直觉得我很笨，基本上不管能不能遇上的问题，到头来，都会被我遇上。 真是见鬼了。

#### 参考链接

1. <a href="http://blog.daocloud.io/how-to-master-docker-image/">DaoCloud:玩转Docker镜像</a>

2. <a href="https://www.daocloud.io/mirror#accelerator-doc">DaoCloud:配置Docker加速器</a>

(完)