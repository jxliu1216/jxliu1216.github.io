---
title: 操作系统相关概念
date: 2022-02-13 16:56:32
tags: operating system
categories: operating system
---

本篇博客记录在学习操作系统过程中，总结的一些概念和相关知识点。

<!--more-->

### 1.用户态和内核态

用户态运行的是用户程序，而内核态下运行的是操作系统程序。从特权级的角度来说，内核态运行在0级特权级（最高特权级），用户态运行在3级特权级（最低特权级）。

![](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com//img/20220213165004-2022-02-13.png)