---
layout:     post
title:      "title"
subtitle:   "sub-title"
date:       2019-10-28T15:52:47+08:00
banner_img: /img/post_banner_common.jpg
tags:
    - tag1
    - tag2
---
> 摘自 https://docs.oracle.com/javase/specs/jvms/se8/jvms8.pdf


## JVM 运行时数据区域
Java虚拟机在程序执行期间定义了许多运行时数据区域.  一些数据区域在虚拟机启动的时候创建, 在Java虚拟机退出的时候销毁. 其它数据区是每个线程.  每个线程数据区域在当线程创建是创建,  线程退出时销毁.
The Java Virtual Machine defines various run-time data areas that are used during execution of a program. Some of these data areas are created on Java Virtual Machine start-up and are destroyed only when the Java Virtual Machine exits. Other data areas are per thread. Per-thread data areas are created when a thread is created and destroyed when the thread exits.

1. PC寄存器
```
Java虚拟机可以同时支持多个线程执行(JLS§17)。每个Java虚拟机线程都有自己的pc(程序计数器)寄存器。在任何时候，每个Java虚拟机线程都在执行单个方法的代码，即该线程的当前方法(§2.6)。如果该方法不是native的，则pc寄存器包含当前正在执行的Java虚拟机指令的地址。如果线程当前执行的方法是native的，则Java虚拟机的pc寄存器的值是undefined的。Java虚拟机的pc寄存器足够宽，可以容纳特定平台上的returnAddress或本机指针。
```
3. JVM栈
4. 堆
5. 方法区
6. 运行时常量池
7. 本地方法栈


## 栈帧




时区
JVM 参数
监控 & 调试工具