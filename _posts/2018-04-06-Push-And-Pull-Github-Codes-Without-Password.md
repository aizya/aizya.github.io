---
layout: post
title: git push免输入账号和密码方法
date: 2018-04-06
categories: Github
tags: [Github]
description: 免密码push/pull远程git代码
published: true
---

### Ubuntu 设置免密push代码到Git

一直使用Visual Studio Code写代码，然后直接用它的terminal上传博客。

为了防止每次都要重复性的输入用户名和密码，于是就想到了免密码push/pull代码的方式。

网上很多资源，我就直接搬运过来了，省得我以后再去别的地方找，费时间。

只需要三个步骤: 

1. 创建文件，进入文件，输入一下内容:

    cd ~
    touch .git-credentials
    vim .git-credentials
    https://{username}:{password}@github.com

2. 在终端中输入:

    git config --global credential.helper store

3. 打开~/.gitconfig文件中，会发现多了一项:

    [credential]
    helper = store

### 参考链接

1. <a href="https://segmentfault.com/a/1190000008435592">解决向github提交代码不用输入帐号密码</a>

2. <a href="http://www.cnblogs.com/zhonghuasong/p/5975952.html">git push免输入账号和密码方法</a>