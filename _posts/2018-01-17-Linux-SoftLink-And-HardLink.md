---
layout: post
title: 关于软链接以及硬链接的使用
date: 2018-01-17
categories: Linux
tags: [Linux]
description: 关于软链接以及硬链接的使用
published: true
---

最近下载了两个IDEA, 一个社区版的(Community),一个扩展版的(Ultimate), 只有扩展版的才可以很好进行企业级的开发.... 悲催啊.

说白了其实用惯了Eclipse和STS, 用IDEA真的很不习惯....

刚开始的时候,一点小东西要摆弄很久,搞得我很烦...

言归正传,今天聊一聊Linux当中的链接问题. 其实就是对Linux一个命令的使用而已.

IDEA的Linux版本和Eclipse不同,它的下载文件中没有可执行的程序,启动程序是一个Shell脚本,这样,每次想要运行IDEA的时候,都需要在终端上执行路径+idea.sh这个文件.

因此,我就想到了软链接的方式,想能不能为这个文件创建一个软链接,指向另一个脚本呢?

### Linux中,可以使用命令ln创建对应的链接

使用man ln, 进入帮助文档, ln: make links between files

- ln [OPTION] ... [-T] TARGET  LINK_NAME
解释: 为TARGET创建一个链接名为LINK_NAME的链接
- ln [OPTION] ... TARGET
解释: 在当前目录下,为TARGET创建一个链接,链接名与TARGET的文件名相同.
- ln [OPTION] ... TARGET ... DIRECTORY
- ln [OPTION] ... -t DIRECTORY TARGET ...
解释: 最后两个表示的皆是在指定的目录下为TARGET创建链接

### 下面展示一些常用的ln创建链接的方式(cheat命令安装Python的一个库得到的)

    xxx@xxx-Latitude-E5440:~/Downloads$ cheat ln

    #To create a symlink:
    
    ln -s path/to/the/target/directory name-of-symlink
    
    #Symlink, while overwriting existing destination files:
    
    ln -sf /some/dir/exec /usr/bin/exec

### 关于软链接

比如现在有这样一个文件A,使用ln -s A B 命令,给文件A创建了一个软链接指向文件B.
此时,只要你操作B,修改B,对应的A文件都会得到修改. 相反的,修改A文件,对应的B文件也会被修改.

此时,如果你想要删除B文件时,不会影响到A文件. 但是如果将TARGET文件A删除了,链接B文件就相当于失去了链接对象,内容清空. (不会被删除,但是里面的内容都没有了).

### 关于硬链接

默认生成的都是硬链接, 如 ln A B, 将会得到B就是A的一个硬链接,二者没有冲突,修改谁都另外一个都没有任何影响.

### 参考

<a href="https://www.tecmint.com/create-hard-and-symbolic-links-in-linux/">How to Create Symbolic Links in Linux</a>

<a href="https://github.com/chrisallenlane/cheat">cheat能够帮助你更好的理解常用的Linux命令</a>
