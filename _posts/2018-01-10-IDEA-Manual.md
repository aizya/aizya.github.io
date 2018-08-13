---
layout: post
title: IDEA入门小tips
date: 2018-01-06
categories: JAVA
tags: [JAVA]
description: IDEA的一些基本使用
published: true
---

# 感觉真的不适应IDEA怎么办

用习惯了Eclipse，再使用IDEA真的感觉自己快把键盘给砸了。很多东西都用着不顺手，感觉效率一下子低了很多。但是没办法，必须强迫自己使用IDEA。也不知道为什么。所以只好把不会的东西都记录下来。再简单都不能放过。否则下次找起来太麻烦了。

# 如何设置IDEA的注释模板

IDEA的注释模板比之Eclipse相对繁琐，类的注释以及方法的注释设置方式都不一样。

参考链接： <a href="https://blog.csdn.net/xiaoliulang0324/article/details/79030752">IDEA类和方法注释模板设置</a>
# 如何开启IDEA的项目自动编译

很多时候，尤其是在使用热部署的时候，如果没有开启自动编译，即便是设置了热部署，还是不会生效。 在IDEA中，可以Build菜单栏的"Rebuild Project"对项目进行重新构建。当然也可以自行设置项目的自动编译。
 
参考链接： <a href="http://blog.csdn.net/aqzwss/article/details/45667885">IDEA自动编译</a>

# IDEA的代码如何设置设置Flat平铺

在Project菜单栏中，有一个锯齿形状的设置，点击可以看到"Flatten Packages"，可以通过点击这个进行调节。

参考链接： <a href="https://blog.csdn.net/liuhailiuhai12/article/details/53822394">项目目录设置与包的显示</a>

# IDEA 快速回到左侧Project栏

如果你想要快速显示/隐藏IDEA的左侧项目栏,可以使用快捷键 **Alt+1(数字1)**。 当然，IDEA中其它菜单栏对应的数字的也可以使用这种方式快速打开/关闭。

# IDEA项目栏中的第一个图标Scroll from Source是什么意思

这个就有点类似与Eclipse中关联代码了，你每次打开一个类，它都会自动帮你链接到它的源文件位置。很方便你编辑。

参考链接： <a href="http://blog.csdn.net/luonanqin/article/details/41088171">Intellij IDEA插件 - Scroll From Source</a>

# IDEA中的Modules是个什么东西

我简单的介绍一下，在IDEA中，Project就类似与Eclipse中的Workspace，而Module就像Eclipse中的每一个项目。

参考链接： <a href="https://blog.csdn.net/qq_35246620/article/details/65448689">>IntelliJ IDEA 中 Project 和 Module 的概念及区别</a>

# IDEA如何忽略大小写,完成提示

问题: 在使用IDEA的时候,我发现如果我输入了test就无法导入Junit的Test注解,一定要首字母在匹配(同样是大写的情况下),才能补全该注解.

解决方式: <a href="http://blog.csdn.net/u012934325/article/details/70755539">IDEA更改大小写设置</a>

# IDEA的方法提示

使用CTRL+P快捷键，快速得到方法提示。再也不用ALT+/，傻乎乎的搞半天。

参考链接： <a href="https://blog.csdn.net/nan_cheung/article/details/79487267">IDEA设置eclipse一样的方法自动显示参数提示</a>