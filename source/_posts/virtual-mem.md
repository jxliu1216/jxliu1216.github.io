---
title: 虚拟内存空间分布
date: 2021-11-13 18:53:03
tags:
- 内存管理
- 虚拟内存
- Linux
- 操作系统
categories: 操作系统
---

虚拟内存可以为正在运行的进程提供独立的内存空间，使得每一个进程都认为自己独立拥有全部的内存。Linux虚拟内存空间分布如下图所示，其中处于高地址的1G空间为内核空间，剩余的3GB为用户空间。

<!--more-->

![](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/1458743340-5ddbcaa6525a5_articlex.png)

用户空间从低地址空间到高地址空间包含如下5个部分：

- 代码段（text segment）：存放程序的可执行二进制代码
- 数据段（data segment）：存放程序中已经初始化且初值不为0的全局变量和静态局部变量，数据段属于静态内存分配
- BSS段：存放未初始化的全局变量和静态局部变量；初值为0的全局变量和静态局部变量
- 堆（heap）：用于存放程序运行时动态分配的内存段，可动态扩张或者缩减
- 栈（stack）：由编译器自动分配释放，它存放如下信息：
  1. 函数内部声明的非静态局部变量
  2. 记录函数调用过程的相关维护信息（成为栈帧）
- 内存映射段（mmap）：内核将硬盘文件直接映射到内存，是一种高效的文件I/O方式
