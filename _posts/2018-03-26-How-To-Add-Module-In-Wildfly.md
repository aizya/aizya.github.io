---
layout: post
title: 记录Wildfly添加新模块
date: 2018-03-26
categories: PIKE
tags: [PIKE]
description: 记录Wildfly添加新模块
published: true
---

好记性不如烂笔头。 本章主要介绍Wildfly新增module(结合OSGI)以及一些遇到的坑。

由于当前系统正好需要集成activiti，因此就暂时以此为例，将整合的过程都详细的列一遍。

1. 首先，需要先在<a href="http://mvnrepository.com/">maven repository</a>中查找当前需要集成的框架。这里我们需要进入网站，搜索activiti，选择我们需要的版本。

	```
	<!-- https://mvnrepository.com/artifact/org.activiti/activiti-engine -->
	<dependency>
		<groupId>org.activiti</groupId>
		<artifactId>activiti-engine</artifactId>
		<version>6.0.0</version>
	</dependency>
	```

2. 搜索到我们需要的依赖之后，注意观察在该依赖的下面，会包含一些compile依赖以及provided依赖。关于二者之间的区别，可以看<a href="https://stackoverflow.com/questions/6646959/difference-between-maven-scope-compile-and-provided-for-jar-packaging">这篇文章。</a> 总而言之，只需要关注这两种类型的依赖。

3. 根据需要加入的框架的groupId，在wildfly的module中，选择创建对应的路径，比如以此例，org.activiti为groupId，那么就在wildfly/modules/system/layers/base/org路径下创建activiti目录，若无org目录，则创建。具体格式如下:wildfly/modules/system/layers/base/xxx/xxx/main/，其中xxx为它的groupId。此处为org/activiti。

4. 创建好了目录之后，需要在main下面添加一个module.xml，一般都可以从其它的module中拷贝过来。编辑此module.xml。 

	```
	<?xml version="1.0" encoding="UTF-8"?>

	<!--
	~ JBoss, Home of Professional Open Source.
	~ Copyright 2010, Red Hat, Inc., and individual contributors
	~ as indicated by the @author tags. See the copyright.txt file in the
	~ distribution for a full listing of individual contributors.
	~
	~ This is free software; you can redistribute it and/or modify it
	~ under the terms of the GNU Lesser General Public License as
	~ published by the Free Software Foundation; either version 2.1 of
	~ the License, or (at your option) any later version.
	~
	~ This software is distributed in the hope that it will be useful,
	~ but WITHOUT ANY WARRANTY; without even the implied warranty of
	~ MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
	~ Lesser General Public License for more details.
	~
	~ You should have received a copy of the GNU Lesser General Public
	~ License along with this software; if not, write to the Free
	~ Software Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA
	~ 02110-1301 USA, or see the FSF site: http://www.fsf.org.
	-->
	<module xmlns="urn:jboss:module:1.1" name="org.activiti">
		<resources>
			<resource-root path="activiti-bpmn-converter-6.0.0.jar"/>
			<resource-root path="activiti-bpmn-model-6.0.0.jar"/>
			<resource-root path="activiti-dmn-api-6.0.0.jar"/>
			<resource-root path="activiti-dmn-model-6.0.0.jar"/>
			<resource-root path="activiti-engine-6.0.0.jar"/>
			<resource-root path="activiti-form-api-6.0.0.jar"/>
			<resource-root path="activiti-form-model-6.0.0.jar"/>
			<resource-root path="activiti-image-generator-6.0.0.jar"/>
			<resource-root path="activiti-process-validation-6.0.0.jar"/>
			<resource-root path="mybatis-3.4.2.jar"/>
			<!-- Insert resources here -->
		</resources>
		<dependencies>
			<module name="com.fasterxml.jackson.core.jackson-databind"/>
			<module name="de.odysseus.juel"/>
			<module name="javaee.api"/>
			<module name="javax.api"/>
			<module name="org.apache.commons.lang3"/>
			<module name="org.codehaus.groovy"/>
			<module name="org.joda.time"/>
			<module name="org.slf4j"/> 
		</dependencies>
	</module>
	```

其中**resources**标签存放的是本地的依赖文件，是属于main文件夹中与xml文件同路径的，而**dependencies**则是引用其他的一些已经存在的modules。 注意，此二者都是根据**第二点**中提到的compile以及provided依赖获得，但是并不一定是全都需要。 你只需要添加你自己需要的依赖。 **此处会出现很多问题。**

4. 一切准备就绪之后，需要在standalone/configuration/目录下修改standalone-osgi.xml文件，在capability中增加咱们刚才添加的module:

	<capability name="org.activiti"/>

其中，你可以发现，capability有一个startlevel属性，该属性用于定义改模块的激活顺序，如果没有设置该属性，默认启动之后是**resolve**状态，而设置了该属性，则为active状态。 

基本上完成以上几点，配置就成功了。

但是需要注意的事，在过程当中会遇到很多的问题。下面列举我遇到的一个问题:

	18:13:05,302 ERROR [org.jboss.msc.service.fail] (MSC service thread 1-1) MSC000001: Failed to start service jbosgi.BootstrapBundles.INSTALL: org.jboss.msc.service.StartException in service jbosgi.BootstrapBundles.INSTALL: JBAS011955: Failed to process initial capability: org.activiti
	at org.jboss.as.osgi.service.BootstrapBundlesIntegration.start(BootstrapBundlesIntegration.java:141)
	at org.jboss.msc.service.ServiceControllerImpl$StartTask.startService(ServiceControllerImpl.java:1948)
	at org.jboss.msc.service.ServiceControllerImpl$StartTask.run(ServiceControllerImpl.java:1881)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
	at java.lang.Thread.run(Thread.java:745)
	Caused by: org.jboss.osgi.repository.RepositoryResolutionException: java.lang.IllegalArgumentException: JBOSGI020521: Cannot obtain module resource: org.activiti:main
		at org.jboss.as.osgi.service.ModuleIdentityRepositoryIntegration.findProviders(ModuleIdentityRepositoryIntegration.java:114)
		at org.jboss.as.osgi.service.BootstrapBundlesIntegration.installInitialModuleCapability(BootstrapBundlesIntegration.java:181)
		at org.jboss.as.osgi.service.BootstrapBundlesIntegration.start(BootstrapBundlesIntegration.java:135)
		... 5 more
	Caused by: java.lang.IllegalArgumentException: JBOSGI020521: Cannot obtain module resource: org.activiti:main
		at org.jboss.osgi.repository.spi.ModuleIdentityRepository.loadModule(ModuleIdentityRepository.java:81)
		at org.jboss.osgi.repository.spi.ModuleIdentityRepository.findProviders(ModuleIdentityRepository.java:99)
		at org.jboss.as.osgi.service.ModuleIdentityRepositoryIntegration.findProviders(ModuleIdentityRepositoryIntegration.java:108)
		... 7 more
	Caused by: org.jboss.modules.ModuleNotFoundException: de.odysseus.juel:main
		at org.jboss.modules.Module.addPaths(Module.java:1093)
		at org.jboss.modules.Module.link(Module.java:1449)
		at org.jboss.modules.Module.relinkIfNecessary(Module.java:1477)
		at org.jboss.modules.ModuleLoader.loadModule(ModuleLoader.java:225)
		at org.jboss.osgi.repository.spi.ModuleIdentityRepository.loadModule(ModuleIdentityRepository.java:79)
		... 9 more

	18:13:05,313 ERROR [org.jboss.as.controller.management-operation] (Controller Boot Thread) WFLYCTL0013: Operation ("add") failed - address: ([
		("subsystem" => "datasources"),
		("xa-data-source" => "local_workflow_ds")
	]) - failure description: {
		"WFLYCTL0180: Services with missing/unavailable dependencies" => undefined,
		"WFLYCTL0288: One or more services were unable to start due to one or more indirect dependencies not being available." => {
			"Services that were unable to start:" => ["org.wildfly.data-source.local_workflow_ds"],
			"Services that may be the cause:" => [
				"jbosgi.BootstrapBundles.COMPLETE",
				"jbosgi.PersistentBundles.COMPLETE",
				"jboss.security.security-domain.encrypted_local_workflow_ds"
			]
		}
	}

下面打印的日志告诉我们具体出现了什么事:

	Caused by: org.jboss.modules.ModuleNotFoundException: de.odysseus.juel:main

这个模块没有找到，这就意味着，在我们的wildfly中，名为de.odusseus.juel:main的模块不存在，我们需要认真的检查一下，这个目录下面是否有这个模块，没有就创建出来。

而给我的警告就是，**看输出日志，一定要看仔细一点，不能只看前面一小部分。** 直接导致我花了大量的时间在查为什么会出错，我明明定义好了activiti的module。

