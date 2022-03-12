---
title: C/C++编译注意事项
date: 2022-03-05 21:53:55
tags:
- C++
- C
categories: C/C++
---


### 1. 头文件搜索路径

**#include "headfile.h"**，通过双引号包含头文件，搜索顺序为：

1. 当前目录
2. -I指定的目录
3. GCC/G++的环境变量指定的目录，C程序使用的是C_INCLUDE_PATH，C++程序使用的是CPP_INCLUDE_PATH
4. GCC/G++内定目录（在编译GCC/G++时指定的目录）

**#include <headfile.h>**，通过<>包含头文件，搜索顺序为：

1. -I指定的目录
2. GCC/G++的环境变量指定的目录，C程序使用的是C_INCLUDE_PATH，C++程序使用的是CPP_INCLUDE_PATH
3. GCC/G++内定目录（在编译GCC/G++时指定的目录）


当上述目录均存在目标头文件时，先搜索到哪一个就用哪一个。

### 2. 静态库搜索路径

1. -L指定的目录
2. gcc/g++的环境变量LIBRARY_PATH
3. 内定目录（编译GCC/G++时指定的目录）


### 3. 动态库搜索路径

1. 编译目标代码时指定的动态库搜索路径
2. 环境变量LD_LIBRARY_PATH指定的路径
3. 配置文件/etc/ld.so.conf中指定的动态库路径
4. 默认的动态库搜索路径/lib
5. 默认的动态库搜索路径/usr/lib

