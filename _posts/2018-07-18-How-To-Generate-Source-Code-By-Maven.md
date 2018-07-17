---
layout: post
title: Maven源码构建插件 
date: 2018-07-18
categories: Maven
tags: [Maven]
description: 如何将项目代码构建成JAR形式
published: true
---

官方文档： 

http://maven.apache.org/plugin-developers/cookbook/attach-source-javadoc-artifacts.html


Generate source code jar for Maven based project：

https://www.mkyong.com/maven/generate-source-code-jar-for-maven-based-project/

    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-source-plugin</artifactId>
        <!--<configuration>-->
            <!--<skipSource>true</skipSource>-->
        <!--</configuration>-->
        <executions>
            <execution>
                <id>attach-sources</id>
                <goals>
                    <goal>jar</goal>
                </goals>
            </execution>
        </executions>
    </plugin>



