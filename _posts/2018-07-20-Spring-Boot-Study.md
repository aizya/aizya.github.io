---
layout: post
title:  SpringBoot学习笔记
date: 2018-07-18
categories: SpringBoot
tags: [SpringBoot]
description: SpringBoot学习笔记
published: true
---

#### 官方文档 

    https://docs.spring.io/spring-boot/docs/2.1.0.BUILD-SNAPSHOT/reference/htmlsingle/#boot-features-spring-application

#### 如何添加启动的Banner 

    https://blog.csdn.net/ztx114/article/details/78137243

这个玩意其实没啥作用，就是好玩而已，可以去看看，GitHub上别人写的<a href="https://github.com/IonicaBizau/image-to-ascii">imageToAscii</a> 

#### 关于Spring的Application事件

事件的作用就是做一些初始化的操作， 具体的用法可以看这里：

    https://www.jianshu.com/p/2502a5103c4f

#### 支持WebApplicationType

在2.0中，支持响应式编程，虽然不知道是什么东西，但是目前在这里，可以简单的理解成，可以替换运行环境模式，比如不使用容器的，使用servlet容器，或者是响应式的。 只需要使用SpringApplication去设置：

    application.setWebApplicationType(WebApplicationType.NONE); // 常使用在Junit上。

#### 在属性中使用占位符

在配置文件中：

    app.name=MyApp
    app.description=${app.name} is a Spring Boot application

#### 使用YAML替代Properties

YAML的意思YAML Ain't Markup Language， 官方文档： http://yaml.org/spec/1.2/spec.html#id2708649


#### 使用@Value注解，操作properties

#### 使用多个yml配置

通过spring.profile.active选择正式激活某个配置文件。

#### yml文件缺陷

不能使用PropertySource

#### ConfigurationProperties