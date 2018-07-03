---
layout: post
title: 关于Activiti版本6与版本5区别
date: 2018-03-26
categories: Activiti
description: Activiti的版本差异
published: true
---

### 概念上的变化

之所以将当前版本称之为Activiti6最主要的一个原因就是，核心的引擎对象被完全的重写了。 核心引擎操作执行的步骤发生了巨大的变化，它可以直接执行BPMN。 然而在V5的时候，在Activiti与BPMN之间有一个intermediate model 中间层。

同样的，运行时执行(执行树 execution tree)也发生了巨大的变化。 上述所有发生变化的区域都进行了重要的简化，使得执行更加的简单/简洁清晰，此外，用户自定义的操作更加的简单和直接。

### 严重的变化

下述的变化称之为breaking changes，换而言之，它很有可能会造成编译错误。

1. PVM类

org.activiti.engine.impl.pvm 包下以及它的子包下的所有的类都被移除了。 这样做的原因是**PVM(Process Virtual Machine)模块已经被移除了，**取而代之的是一个更加简单以及轻量集的模块。

这也就意味着，关于**ActivitiImpl，ProcessDefinitionImpl，ExecutionImpl，TransitionImpl**等类的用法usage都是不合法的。

通常，在V5中上述清单中的类的作用就是为了获取**流程定义**中的包含的一些信息。在V6中，所有的流程定义的信息都可以通过BpmnModel对象获得。BpmnModel对象是使用Java表示BPMN2.0 XML的流程定义。 

这样的好处就是**使得操作更加清晰，搜索起来很方便**。 不同与之前那样，使用多个类去对流程定义进行处理。

通过使用org.activiti.engine.impl.util.ProcessDefinitionUtil类可以快速获取流程定义表示的BpmnModel对象。

    // 获取整个模型
    ProcessDefinitionUtil.getBpmnModel(String processDefinitionId);
    // 只获取某个指定的流程定义对象
    ProcessDefinitionUtil.getProcess(String processDefinitionId);


### 参考文档

<a href="https://www.activiti.org/migration.html"> Activiti Migraton Guide : Activiti v5 to Activiti v6</a>



