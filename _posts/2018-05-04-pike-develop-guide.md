---
layout: post
title: PIKE常见问题以及细节
date: 2018-05-04
categories: PIKE
tags: [PIKE]
description: 关于PIKE的一些常见问题以及细节
published: true
---

1. jetty 指定端口运行

    mvn jetty:run -Djetty.port = 8888

1. 创建PIKE用户和数据库

    mysql> create user 'pike'@'localhost' identified by 'pike';
    mysql> create database PIKE DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;

1. 用户赋权

    mysql> grant all privileges on PIKE.* to 'pike'@'localhost';
    mysql> flush privileges;