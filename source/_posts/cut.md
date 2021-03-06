---
title: Linux Bash之cut命令
date: 2021-10-18 00:14:47
tags:
- Linux
- Bash
categories: Linux
---

cut是Linux中的一个重要数据选取工具，cut以“行”单位，从中提取出我们想要的信息。cut命令非常适用于处理格式上非常具有规律的数据。

<!--more-->

### 1. 指定分隔符

通过指定分隔符来选取我们想要的信息是cut命令的第一种用法，命令格式如下：

```bash
$ cut -d "分隔符" -f field
```

通过-d来指定分隔符（**如果不指定，默认的分割符为tab**）,通过-f后添加数字，来选取分割后的第几段内容。以环境变量PATH为例，它是以“:”来分隔每个字段的，我们可以使用cut来截取其中的部分字段。例如：

![image-20211019230857154](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211019230857154.png)

cut是以行为单位进行处理的，即对每一行都进行相同的处理，例如：

```bash
$ export | cut -d " " -f 3
```

这个示例内容过长，就不截图展示了。

需要注意的是：使用cut命令的时候不建议使用空格符作为分隔符，当有多个连续的空格符时，比较容易出问题。

### 2. 以字符的形式截取

cut可以通过-c以字符为单位截取信息，例如：

```bash
$ export | cut -c 12-         # 截取从第12个字符开始到结尾的所有字符
$ export | cut -c 10-15       # 截取第10个字符到第15个字符
```



