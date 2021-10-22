---
title: git使用（一）
date: 2021-10-23 00:01:59
tags:
- Git
categories: Git
---

本文主要记录git中的一些常见命令的使用，包含如下命令：

- [git diff](#git_diff)

<!--more-->

### <a name="git_diff">1. git diff </a>

git diff用于比较文件之间的差异，通常有如下几种用法：

- git diff [filename]：用于比较当前工作区和暂存区之间的差异
- git diff --cached [filename]：用于比较暂存区和上一次提交之间的差异

git diff后的结果如下图所示，下面解释各个部分的含义

![image-20211023003548174](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211023003548174.png)

git diff的输出结果大致可以分为两个部分，第一部分是进行对比的两个文件的基本信息，第二部分是对比的结果。

```bash
diff --git a/backup.sh b/backup.sh
```

这一行的意思是使用git版本的diff进行文件对比，对比的两个文件为a版本的backup.sh（即修改前），和b版本的backup.sh（即修改后）。

```bash
index 7cf56ad..14ff882 100644
```

这一行表示的是git哈希值，意思是index区域的7cf56ad对象与工作区域的14ff882对象进行对比，100644代表的含义暂不清楚。

```bash
--- a/backup.sh
+++ b/backup.sh
```

这两行表示对比的两个文件，--- a/backup.sh表示修改前的文件，+++ b/backup.sh表示修改后的文件。

```bash
@@ -12,5 +12,6 @@ ...
```

从这行开始就是对差异信息的总结。两个@@之间的数字的意思是：原文件从12行开始的5行和目标文件从12行开始的6行存在差异，且差异信息显示在后面：

- 以空格开头的白色的行是两个文件中都有的内容
- 以-开头的的红色的行是原文件中有的内容
- 以+开头的绿色的行是目标文件中有的内容
