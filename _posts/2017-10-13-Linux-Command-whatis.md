---
layout: post
title: 每天一个Linux命令-whatis
date: 2017-10-13
categories: Linux
tags: [Linux]
description: 每天一个Linux命令-whatis
published: true
---

对于Linux初学者而言，帮助与查找是基础。

很多时候，一个命令可能有多种不同的使用方式，如果你是第一次接触这个命令，你肯定会想到先去man一下，看看具体的用法。

了解过man的都知道，man的基本使用: man [-n] command，n的范围1~9。

换而言之，同一个命令，有不同的使用方法。可以做不同的事, **whatis**命令是用于查询一个命令执行什么功能，并将查询结果打印到终端上。

![]({{ site.url }}/assets/Selection_300.png)

那么今天这个whatis命令，就告诉你，你所使用的这个命令所有可用的功能，**所有可以做的事**

    whatis - display one-line manual page descriptions

whatis命令与man -f的作用完全一致:

![]({{ site.url }}/assets/Selection_299.png)


了解一下，知道有这么个东西就可以了。