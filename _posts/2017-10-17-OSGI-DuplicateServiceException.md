---
layout: post
title: 详解DuplicateServiceException
date: 2017-10-17
categories: Jboss
tags: [Wildfly]
description: org.jboss.msc.service.DuplicateServiceException
published: true
---

公司使用的是Jboss公司的Wildfly，基于OSGI的规范，多个模块之间在编译部署的时候经常会出现DuplicateServiceException异常，导致模块无法动态部署(Dynamic deployment)。

出现这类异常的原因是什么呢？

先看错误提示：

    10:30:54,177 ERROR [org.jboss.msc.service.fail] (MSC service thread 1-4) MSC000001: Failed to start service jboss.deployment.unit."MESFramework-0.0.1-SNAPSHOT.jar".CONFIGURE_MODULE: org.jboss.msc.service.StartException in service jboss.deployment.unit."MESFramework-0.0.1-SNAPSHOT.jar".CONFIGURE_MODULE: WFLYSRV0153: Failed to process phase CONFIGURE_MODULE of deployment "MESFramework-0.0.1-SNAPSHOT.jar"
    at org.jboss.as.server.deployment.DeploymentUnitPhaseService.start(DeploymentUnitPhaseService.java:154)
    at org.jboss.msc.service.ServiceControllerImpl$StartTask.startService(ServiceControllerImpl.java:1948)
    at org.jboss.msc.service.ServiceControllerImpl$StartTask.run(ServiceControllerImpl.java:1881)
    at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
    at java.lang.Thread.run(Thread.java:745)
    Caused by: org.jboss.msc.service.DuplicateServiceException: Service jboss.module.spec.service."deployment.MESFramework-0.0.1-SNAPSHOT.jar".main is already registered
    at org.jboss.msc.service.ServiceRegistrationImpl.setInstance(ServiceRegistrationImpl.java:158)
    at org.jboss.msc.service.ServiceControllerImpl.startInstallation(ServiceControllerImpl.java:235)
    at org.jboss.msc.service.ServiceContainerImpl.install(ServiceContainerImpl.java:768)
    at org.jboss.msc.service.ServiceTargetImpl.install(ServiceTargetImpl.java:223)
    at org.jboss.msc.service.ServiceControllerImpl$ChildServiceTarget.install(ServiceControllerImpl.java:2401)
    at org.jboss.msc.service.ServiceBuilderImpl.install(ServiceBuilderImpl.java:317)
    at org.jboss.as.osgi.service.ModuleLoaderIntegration$FrameworkModuleLoaderImpl.addModuleSpec(ModuleLoaderIntegration.java:208)
    at org.jboss.osgi.framework.internal.ModuleManagerImpl.createHostModule(ModuleManagerImpl.java:325)
    at org.jboss.osgi.framework.internal.ModuleManagerImpl.addModule(ModuleManagerImpl.java:207)
    at org.jboss.osgi.framework.internal.FrameworkResolverImpl.addModules(FrameworkResolverImpl.java:304)
    at org.jboss.osgi.framework.internal.FrameworkResolverImpl.applyResolverResults(FrameworkResolverImpl.java:249)
    at org.jboss.osgi.framework.internal.FrameworkResolverImpl.resolveInternal(FrameworkResolverImpl.java:164)
    at org.jboss.osgi.framework.internal.FrameworkResolverImpl.resolveAndApply(FrameworkResolverImpl.java:109)
    at org.jboss.as.osgi.deployment.BundleResolveProcessor.resolveBundle(BundleResolveProcessor.java:81)
    at org.jboss.as.osgi.deployment.BundleResolveProcessor.deploy(BundleResolveProcessor.java:68)
    at org.jboss.as.server.deployment.DeploymentUnitPhaseService.start(DeploymentUnitPhaseService.java:147)
    ... 5 more

由错误提示可知，DeploymentUnitPhaseService.start方法出现了问题，找到问题出现的源头:

    private static boolean shouldRun(final DeploymentUnit unit, final RegisteredDeploymentUnitProcessor deployer) {
        Set<String> shouldNotRun = unit.getAttachment(Attachments.EXCLUDED_SUBSYSTEMS);
        if (shouldNotRun == null) {
            if (unit.getParent() != null) {
                shouldNotRun = unit.getParent().getAttachment(Attachments.EXCLUDED_SUBSYSTEMS);
            }
            if (shouldNotRun == null) {
                return true;
            }
        }
        return !shouldNotRun.contains(deployer.getSubsystemName());
    }

其中，unit参数为 deployment "MESFramework-0.0.1-SNAPSHOT.jar"， deployer为org.jboss.as.server.deployment.RegisteredDeploymentUnitProcessor@3d3ade1d,先暂时不管这个，这里主要的表示的是看shouldNotRun这个集合是否有值。

Attachments.EXCLUDED_SUBSYSTEMS 为 org.jboss.as.server.deployment.SimpleAttachmentKey<java.util.Set>

此处的shouldNotRun返回结果为 [batch-jberet, batch] ，不为空。 所以，应该直接运行最后一段代码。

最后一段代码，判断shouluNotRun是否包含deployer.getSubsystemName() [server], 返回false ，取反为真。

注意方法saftUndeploy():

     private static void safeUndeploy(final DeploymentUnit deploymentUnit, final Phase phase, final RegisteredDeploymentUnitProcessor prev) {
        try {
            if (shouldRun(deploymentUnit, prev)) {
                prev.getProcessor().undeploy(deploymentUnit);
            }
        } catch (Throwable t) {
            ServerLogger.DEPLOYMENT_LOGGER.caughtExceptionUndeploying(t, prev.getProcessor(), phase, deploymentUnit);
        }
    }

实话实说，说代码的角度想要找到问题的所在是比较困难的。比如，出现了DuplicateServiceException重复服务的异常，我可以大胆的假设一下，为什么部署其他的模块，譬如ProductionManagement/RoutingManagement，都不会出现这样的问题。 那么为什么，部署MESFramework就会出现异常呢？

热部署就是在不需要停止服务器的情况下，自动部署最新的代码。这方面，可以看看JRebel。那么，在部署之前，模块应该是被注册了的，换句话，模块已经部署了。那么，要部署新的模块，则需要先取消部署，然后在进行部署。

下面以ProductionManagement为例，在热部署时它会先停止bundle模块：

![]({{ site.url }}/assets/Selection_305.png)

而之所以MESFramework热部署出现问题，则是因为这里停止bundle模块出现了问题，导致多次部署同一个bundle。

那么，为什么无法停止模块了，我大胆猜测一下，应该是RoutingManagement以及ProductionManagement等模块依赖它，因此它无法被停止stop。

这就是为什么会出现org.jboss.msc.service.DuplicateServiceException异常。。。

仅仅是我的猜测。具体为什么要看代码。

另外，还有一点可以证明我的猜测，在系统配置，插件里，停止MESFramework时:

![]({{ site.url }}/assets/Selection_306.png)

在点击RoutingManagement和ProductionManagement模块内容时，后台会出现异常，直接crash了：

![]({{ site.url }}/assets/Selection_308.png)

间接证明了，一旦自身模块被其他模块所依赖时，热部署会出现问题。

解决方法：停掉Wildfly服务器，重新运行。