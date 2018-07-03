---
layout: post
title: java.util.zip.ZipException:invalid LOC header (bad signature)
date: 2018-01-30
categories: Java
tags: [Java]
description: 记Vaadin的一个异常问题
published: true
---

今天碰到一个非常奇怪的问题， 我使用Eclipse创建一个Vaadin的项目，然后使用jetty运行的时候，出现了下面的错误提示：

            Suppressed: java.lang.RuntimeException: Error scanning entry com/vaadin/shared/ui/splitpanel/HorizontalSplitPanelState.class from jar file:///home/xiaoxiong/.m2/repository/com/vaadin/vaadin-shared/7.7.13/vaadin-shared-7.7.13.jar
            at org.eclipse.jetty.annotations.AnnotationParser.parseJar(AnnotationParser.java:937)
            ... 6 more
        Caused by: java.util.zip.ZipException: invalid LOC header (bad signature)
            at java.util.zip.ZipFile.read(Native Method)
            at java.util.zip.ZipFile.access$1400(ZipFile.java:60)
            at java.util.zip.ZipFile$ZipFileInputStream.read(ZipFile.java:717)
            at java.util.zip.ZipFile$ZipFileInflaterInputStream.fill(ZipFile.java:419)
            at java.util.zip.InflaterInputStream.read(InflaterInputStream.java:158)
            at java.io.FilterInputStream.read(FilterInputStream.java:133)
            at java.io.FilterInputStream.read(FilterInputStream.java:133)
            at org.objectweb.asm.ClassReader.a(Unknown Source)
            at org.objectweb.asm.ClassReader.<init>(Unknown Source)
            at org.eclipse.jetty.annotations.AnnotationParser.scanClass(AnnotationParser.java:1003)
            at org.eclipse.jetty.annotations.AnnotationParser.parseJarEntry(AnnotationParser.java:984)
            at org.eclipse.jetty.annotations.AnnotationParser.parseJar(AnnotationParser.java:933)
            ... 6 more
    Caused by: java.lang.RuntimeException: Error scanning entry elemental/json/Json.class from jar file:///home/xiaoxiong/.m2/repository/com/vaadin/vaadin-shared/7.7.13/vaadin-shared-7.7.13.jar
        at org.eclipse.jetty.annotations.AnnotationParser.parseJar(AnnotationParser.java:937)
        ... 6 more
    Caused by: java.util.zip.ZipException: invalid LOC header (bad signature)
        at java.util.zip.ZipFile.read(Native Method)
        at java.util.zip.ZipFile.access$1400(ZipFile.java:60)
        at java.util.zip.ZipFile$ZipFileInputStream.read(ZipFile.java:717)
        at java.util.zip.ZipFile$ZipFileInflaterInputStream.fill(ZipFile.java:419)
        at java.util.zip.InflaterInputStream.read(InflaterInputStream.java:158)
        at java.io.FilterInputStream.read(FilterInputStream.java:133)
        at java.io.FilterInputStream.read(FilterInputStream.java:133)
        at org.objectweb.asm.ClassReader.a(Unknown Source)
        at org.objectweb.asm.ClassReader.<init>(Unknown Source)
        at org.eclipse.jetty.annotations.AnnotationParser.scanClass(AnnotationParser.java:1003)
        at org.eclipse.jetty.annotations.AnnotationParser.parseJarEntry(AnnotationParser.java:984)
        at org.eclipse.jetty.annotations.AnnotationParser.parseJar(AnnotationParser.java:933)
        ... 6 more
    [INFO] Started ServerConnector@5ab4f1d9{HTTP/1.1,[http/1.1]}{0.0.0.0:9999}
    [INFO] Started @8840ms
    [INFO] Started Jetty Server

服务虽然起来了，但是界面上并不会出现想要的界面。 根据错误提示：

    Caused by: java.util.zip.ZipException: invalid LOC header (bad signature)

于是我便去Google，发现了网上已经有人碰到过相同的问题：<a href="http://www.fatalerrors.org/a/java.util.zip.zipexception-invalid-loc-header-bad-signature.html">java.util.zip.ZipException: invalid LOC header (bad signature)</a>

这篇文章最后给了个总结：

    Replace the corresponding jar in the local Maven repository, then problem is solved.

**在本地仓库中替换对应的jar文件，问题自然解决了。**

让我很疑惑的是，我根据错误日志输出：

     Caused by: java.lang.RuntimeException: Error scanning entry elemental/json/Json.class from jar 
     file:///home/xiaoxiong/.m2/repository/com/vaadin/vaadin-shared/7.7.13/vaadin-shared-7.7.13.jar

在本地仓库中对应的位置找到了提示中的jar文件，但是我少做了一步，**没有使用反编译工具查看，错误信息中对应的class文件是否真的如提示的一般，没有存在。**

而我最后的处理方式是： 直接把我本地仓库中对应的vaadin-shared-7.7.13.jar删除了，然后执行命令：**mvn jetty:run -Djetty.port=9999**, 发现它会自动将我们需要的jar包下载到本地，也没有了之前的错误提示。

#### 那么为什么会出现java.util.io.ZipException呢？

通过Google我发现有这样一篇文章：<a href="https://cn.mathworks.com/matlabcentral/answers/313286-why-do-i-see-a-java-util-zip-zipexception-error-in-my-installer-log-file-when-i-try-to-install-the?requestedDomain=true#">why invked java.util​.zip.ZipEx​ception?</a>

里面有这样一句话：

    This error was due to incomplete third-party downloads.

这个问题是因为出现了不完整的第三方jar包。
