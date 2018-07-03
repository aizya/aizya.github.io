---
layout: post
title: 记录一个Pike Box的编译问题
date: 2018-03-26
categories: PIKE
tags: [PIKE]
description: Pike Box的编译问题
published: true
---

在我将最新的编译好的Pike Box放入到Wildfly中执行运行的时候，系统无法正常执行，界面无法加载。 

后台提示警告如下: 

    22:06:02,854 INFO  [com.vaadin.server.VaadinServlet] (default task-60) Requested resource [/VAADIN/widgetsets/com.polelink.pike.box.widgetset.PikeWidgetset/com.polelink.pike.box.widgetset.PikeWidgetset.nocache.js] not found from filesystem or through class loader. Add widgetset and/or theme JAR to your classpath or add files to WebContent/VAADIN folder.

从其中的警告中得出问题出在pike-box下的widgetset。

解决方法: 使用如下命令重新编译pike-box:

    mvn clean install -P vaadin

其中"-P"表示使用名为"vaadin"的插件Plugin对项目进行编译。

查看pike-box项目的pom文件可以发现定义了如下的Plugin: 

    <profile>
		<id>vaadin</id>
			<build>
				<plugins>
					<plugin>
						<groupId>com.vaadin</groupId>
						<artifactId>vaadin-maven-plugin</artifactId>
						<configuration>
							<extraJvmArgs>-Xmx1024M ${params.charts} ${params.spreadsheet}</extraJvmArgs>
							<webappDirectory>${basedir}/src/main/resources/VAADIN/widgetsets</webappDirectory>
							<hostedWebapp>${basedir}/src/main/resources/VAADIN/widgetsets</hostedWebapp>
							<warSourceDirectory>${basedir}/src/main/resources</warSourceDirectory>
							<noServer>true</noServer>
							<draftCompile>false</draftCompile>
							<style>OBF</style>
							<compileReport>true</compileReport>
						</configuration>
						<executions>
							<execution>
								<configuration>
								</configuration>
								<goals>
									<goal>clean</goal>
									<goal>resources</goal>
									<goal>update-theme</goal>
									<goal>update-widgetset</goal>
									<goal>compile-theme</goal>
									<goal>compile</goal>
								</goals>
							</execution>
						</executions>
					</plugin>
				</plugins>
			</build>
		</profile>

先立个flag吧，找时间学学Maven的Plugin!!!