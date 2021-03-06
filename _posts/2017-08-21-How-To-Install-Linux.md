---
layout: post
title: 在VirtualBox上安装CentOS
date: 2017-08-21
categories: Linux
tags: [Linux]
description: 记录一下安装CentOS的过程.
published: true
---

### 熟能生巧,古人诚不欺我 

来公司也四个多月了,依稀记得刚入职那会,同事递了台电脑,顺带还给了个U盘,说,自己装个Linux系统吧...

虾米?我之前好歹也是在自己Windows上玩过Ubuntu的男人,这点小事我还没有点数吗?

不过,实话实说,装Linux图形系统比Windows简单了很多,基本上就是傻瓜操作...下一步...下一步...

这反倒还是Windows的一贯特色...

说实在的,网上有很多人抨击Windows,我觉得其实不大可取..

毕竟二者都是工具,相对于大部分开发者而言,Windows几乎满足他们所有的工作要求...

再不济,在电脑上跑个Linux虚拟机也就是了...

其实也还巧,刚入职那会,正好赶上了新人培训RedHat,所以顺带学习了下基本的一些使用方法..

不过现在都已经忘记了,什么磁盘分区,什么ip划分等等...

这也是为什么要花一些心思,做一些微小的工作...做做笔记,写写废话,吐吐槽...

Linux还是很重要的,最好的学习方法就是多练习,你看我,练着练着不就把傻逼无限鼠标右键刷新的臭毛病改了嘛~~~

### 初来乍到,请多多包涵

话不多说,直接入正题,如何在Virtual Box上安装CentOS呢?

首先,先把手头上的工具都准备一下:

1. 下载一个Virtual Box, Windows与Linux的安装方式不一致,无非下载安装与命令行安装.自行Google,另官网上也有各种系统的Virtual,下载一个deb的,直接使用:sudo gdebi xxx.deb 进行安装. 使用命令行进行<a href="https://askubuntu.com/questions/367248/how-to-install-virtualbox-from-command-line">安装下载</a>.

2. 下载一个CentOS镜像文件,iso结尾的,我用的是7.0版的. <a href="https://www.centos.org/download/">官网可以下载各种版本的CentOS</a>

3. 下载安装好了这两个最基本的玩意之后,启动Virtual Box,我使用的是命令行的方式.

4. 点击NEW,创建一个新的虚拟机,然后选择好系统的版本并且取个名字,这名字不能重复. ![]({{ site.url }}/assets/Selection_235.png)

5. 给虚拟机分配指定的内存空间,按照每台电脑性能不同,分配不同的内存,当然也是越大越快咯. ![这里分配给它1G的内存:]({{ site.url }}/assets/Selection_236.png)

6. 然后各种下一步,到了给虚拟机分配硬盘空间的时候,选择不同大小,按照需求设置,然后直接Create创建. ![]({{ site.url }}/assets/Selection_237.png)

7. 刚创建的时候,没有加入ISO文件,这里显示的为空. ![]({{ site.url }}/assets/Selection_238.png)

8. 选择设置,存储,然后点击那个光盘处进行添加ISO. ![]({{ site.url }}/assets/Selection_239.png)

9. 添加完之后,直接start启动.进入安装界面,选择第一个,直接安装CentOS7. ![]({{ site.url }}/assets/Selection_240.png)

10. 点击完之后,会经过一系列的内部处理,可以通过ESC键进行查看切换,进入安装界面之后,首先会选择语言,这个语言可以拉到最下面选择中文,下一步.

11. 进入了系统安装信息摘要之后,首先设置日期和时间,我设置的是上海.**软件选择**选择带GUI的服务器,而不是最小安装(这个对于新手友好).安装位置选择自动分区. **网络和主机名**默认就可以,配置一下常规,设置为可用时自动链接到这个网络. **基本上都是要原有的默认进行安装...** ![]({{ site.url }}/assets/Selection_241.png)

12. 在安装的时候要花费一段时间,正好可以设置Root密码,注意密码过短时需要按两下确认.在创建一个普通用户,同理...然后就等着吧... ![]({{ site.url }}/assets/Selection_242.png)

13. 安装完成之后会提示重启电脑,还需要设置一下接受许可...就OK了.. ![安装成功啦:]({{ site.url }}/assets/Selection_243.png)

**附1:** 注意,需要在安装之后,设置一下这里为:使用硬盘进行登录,如果使用的是光盘,那么会出现安装成功之后,下次进入的时候,还是要求进行安装...

**附2:** 在文档中插入图片的时候,如果缺少了一个!且图片描述未添加时,则该图片不会显示,若添加了图片描述,则图片描述会变成一个链接.(与本文无关,仅仅只是博客本身语法有关,做个记录.)

**附3:** 其实在虚拟机上安装其他的系统真不难,难的是如何使用好这个系统,无论是Windows还是Linux,需要我们去学习的太多太多了.加油,共勉...

**附4:** 此文仅仅是个人的理解,主要为了下次安装的时候不需要再去看一遍视频,或者查其他资料,因此不保证其正确性...