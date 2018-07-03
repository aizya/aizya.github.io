---
layout: post
title: Java守护线程
date: 2017-10-16
categories: Java
tags: [Java]
description: Java守护线程
published: true
---

Java中线程分为，用户线程和守护线程。

守护线程的英文为Daemon Thread.

其中Daemon为守护神的意思，什么是守护神，只要但凡有一个需要保护的人(用户线程)， 守护神(守护线程)都会与他同在。

最为常见的守护线程就是我们常常提到的GC线程。(Garbage Collection，垃圾回收线程)。

顺带简单提一句，垃圾回收工作，一般在堆区域(heap，也是我们俗称的'垃圾堆')中进行。

具体关于Java虚拟机的详细介绍，我现在也是模棱两可的。

#### 关于用户线程与守护线程

区别: 用户线程与守护线程的不同之处在于，用户线程能直接的影响虚拟机的关闭，当程序中所有的用户线程结束之后，虚拟机就会自动的关闭/终止(terminate)。当然，守护进程也就相应的结束了。**但是，只要程序中有一个用户线程存在，守护进程都会存在，且虚拟机会正常运行。**

日常情况下，无论是继承Thread类还是实现Runnable接口，生成的线程都是用户线程。

    public class DaemonThreadDemo {
        public static void main(String[] args) {
            YueThread yueThread = new YueThread();
            yueThread.start();
            System.out.println("判断生成的线程是否是守护线程: " + yueThread.isDaemon());
        }
    }

    class YueThread extends Thread {
        @Override
        public void run() {
            System.out.println("sorry yue~");
        }
    }

简单的写上一个Thread的实现类，运行结果发现其并不是守护进程:

    判断生成的线程是否是守护线程: false
    sorry yue~

另外，main线程是否是守护线程呢？

    public class MainThreadDemo {
        public static void main(String[] args) {
            System.out.println(
                    "main主线程" + Thread.currentThread().getName() + " 是否是守护线程: " + Thread.currentThread().isDaemon());
        }
    }

输出结果: 

    main主线程main 是否是守护线程: false

从上面两个例子，你可以看出，判断一个线程是否是守护线程的方法，就是使用Thread的isDaemon()方法。

