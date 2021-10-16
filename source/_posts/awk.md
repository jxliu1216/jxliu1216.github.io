---
title: Linux命令之awk
date: 2021-10-15 23:07:18
tags:
- Linux
- Bash
categories: Linux
---

awk是Linux中的一个非常强大的数据编辑处理工具，说它是一门编程语言也毫不过分（的确有介绍awk变成语言的书籍）。awk的功能太过于强大，以至于无法对其进行全面的总结。本文目前只总结介绍一些常见的基本功能，在后续的学习过程中，会不断总结更新对awk工具的认识，并更新本文。

<!--more-->

### 1. awk简介

Linux中的很多命令是将其功能所对应的英文进行缩写而得到的，那么awk这个名字是怎么由来的呢？awk是由该工具的三位创作者（Alfred Aho，Peter Weinberger和Brain Kernighan）的family name的首字母构成。

相比于sed对一整行数据进行处理，awk则是将一整行分解为若干个“数据段”进行处理。其使用方法可以表示如下：

```bash
$ awk 'condition 1 {action 1} condition 2 {action 2} ...' filename
```

根据上述使用方法，我们可以知道：

- awk可以执行多个处理动作（action）
- 可以根据不同的条件（condition）执行不同的动作（action）

前文说到awk是将每一行分解成若干个数据段进行处理，那么是如何进行数据段分解的呢？awk默认使用空格或者tab作为分割符，当然也可以通过命令指定分割符，有如下两种方式：

```bash
$ awk -F ":" 'condition 1 {action 1} condition 2 {action 2} ...' filename # 使用:作为分隔符，method 1
$ awk 'BEGIN {FS=":"} contition 1 {action 1} condition 2 {action 2} ...' filename # 使用:作为分隔符，method 2
```

那么对数据进行分段后，如何表示每一个分段呢？awk使用$符号加数字来表示每一个分段，$1表示第一个分段，$2表示第二个分段，以此类推。

### 2. awk的若干常见用法

#### 2.1 结合print打印相关字段

我们此处以/etc/passwd为例，/etc/passwd文件记录了用户信息，每一行都使用“:”将各个字段隔离开，如果我们只想获取用户名，则可以使用如下命令：

```bash
$ awk 'BEGIN {FS=":"} {print $1}' /etc/passwd
```

BEGIN {FS=":"}用于指定分隔符为“:“，{print $1}表示打印第一个字段，即用户名。也可以打印多个字段，如：

```bash
$ awk 'BEGIN {FS=":"} {print $1 "\t" $3}' /etc/passwd
```

打印第1和第3个字段，并且中间使用tab作为分隔符。

还可以结合grep进行过滤，再用awk筛选相关字段，例如：我想要输出登录shell为/bin/bash的所有用户名，可以使用如下命令：

```bash
$ grep "/bin/bash" /etc/passwd | awk awk 'BEGIN {FS=":"} {print $1}'
```

