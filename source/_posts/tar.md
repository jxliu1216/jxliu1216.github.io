---
title: Linux命令之tar
date: 2021-10-10 00:40:48
tags:
- Linux
- Bash
categories: Linux
---

tar命令可以将多个文件和目录打包成一个大的文件，并结合gzip/bzip2进行压缩。本文首先介绍常用的压缩命令，然后介绍tar命令。

<!--more-->

### 1.常用压缩命令

Linux上的压缩命令非常多，不同的压缩命令对应不同的扩展名。因此，需要知道常见的扩展名所对应的压缩命令，才能使用相应的压缩命令对其进行解压缩。

| 扩展名   | 描述                                 |
| -------- | ------------------------------------ |
| .gz      | gzip程序的压缩文件                   |
| .bz2     | bzip2程序的压缩文件                  |
| .tar     | tar程序的打包文件，并没有经过压缩    |
| .tar.gz  | tar程序的打包文件，并经过gzip的压缩  |
| .tar.bz2 | tar程序的打包文件，并经过bzip2的压缩 |

#### 1.1 gzip

gzip命令的语法如下：

```bash
$ gzip [-cdtv#] filename
```

各个选项的含义如下：

- -c：将压缩后的数据输出的屏幕上，可以结合重定向进行使用
- -d：解压缩
- -t：校验压缩文件是否正确无误
- -v：对文件进行压缩，并显示压缩比
- -#：通过数字指定压缩等级，-1最快，但是压缩比最低；-9最慢，但是压缩比最高。默认为6

示例：

```bash
$ gzip test.txt             # 压缩test.txt，并生成test.txt.gz
$ gzip -c test.txt          # 输出test.txt压缩后的信息，并不产生test.txt.gz
$ gzip -d test.txt.gz       # 解压test.txt.gz
$ gzip -v test.txt          # 对test.txt进行压缩，并显示压缩比
$ gzip -9 test.txt          # 以压缩等级9对test.txt进行压缩
```

需要注意的是，使用gzip对文件压缩或者解压缩时，均不会保留原文件或者压缩文件，如果想要保留原始文件，可以结合-c选项和重定向，命令如下：

```bash
$ gzip -c test.txt > test.txt.gz  # 将屏幕输出重定向至test.txt.gz，从而保留原文件 
$ gzip -cd test.txt.gz > test.txt # 将解压信息重定向至test.txt，从而保留原文件
```

上述选项也可结合使用，实现综合功能，如：

- 比较不同压缩等级下的压缩比

  ```bash
  $ gzip -cv1 test.txt     # 显示压缩信息和压缩等级，并不产生压缩文件
  $ gzip -cv9 test.txt
  ```

需要注意的是，gzip是对单个文件的压缩，并不能对整个目录进行压缩。虽然可以通过-r选项来对目录进行操作，但这也只是对目录里的文件分别进行压缩。

![image-20211010163234547](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211010163234547.png)

#### 1.2 bzip2

bzip2的用法几乎与gzip相同，bzip2命令的语法如下：

```bash
$ bzip2 [-cdkzv$] filename
```

各个选项的作用如下：

- -c：将压缩过程产生的数据输出至屏幕上
- -d：解压缩
- -k：保留原文件
- -z：压缩的参数
- -v：显示压缩比信息
- -#：与gzip相同，压缩参数，1最快，9压缩比最高

相比gzip，bzip2提供了-k选项，在压缩文件的同时，可以保留原文件。当然也可以使用-c选项和重定向来实现，不过这显得有些麻烦了。

和gzip一样，bzip2也仅能对单个文件进行压缩，而无法对多个文件或者文件夹进行压缩。

### 2. tar命令

tar是Linux中的归档命令，用于打包多个文件，并且可以结合压缩命令进行使用，tar命令的语法如下：

```bash
$ tar [-j|-z] [cv] [-f new_filename] object1 object2 ...    # 压缩
$ tar [-j|-z] [tv] [-f new_filename]         # 查看文件名
$ tar [-j|-z] [xv] [-f new_filename] [-C folder]            # 解压
```

这些常用选项的作用如下：

- -c：新建打包文件，结合-v可以查看打包过程中的文件
- -t：查看打包文件中包含的文件名，结合-v可以显示文件的详细信息
- -x：解打包或者解压缩，注意：-c，-t，-x不可能在命令中同时出现
- -j：通过bzip2的支持进行压缩和解压缩，此时文件扩展名为*.tar.bz2
- -z：通过gzip的支持进行压缩和解压缩，此时文件扩展名为*.tar.gz
- -f：后面跟打包文件的名字，建议单独出来写
- -C：指定解打包的目录

实际比较常用的打包，查看和解打包涉及到的命令如下：

```bash
$ tar -jcv -f all_test.tar.bz2 test*.txt      # 使用bzip2进行压缩
$ tar -zcv -f all_test.tar.gz test*.txt       # 使用gzip进行压缩
$ tar -jtv -f all_test.tar.bz2                # 查看打包文件名
$ tar -ztv -f all_test.tar.gz                 # 查看打包文件名
$ tar -jxv -f all_test.tar.bz2                # 解打包*.tar.bz2
$ tar -zxv -f all_test.tar.gz                 # 解打包*.tar.gz
$ tar -zxv -f all_test.tar.gz -C target_folder/    # 指定目录解打包
```

tar还有两个比较常用的功能：

- 解打包某一个文件

  使用上述的解打包命令时，会把所有的文件都解出来，但有时候我们可能只需要其中的某个文件，这时候可以指定需要的文件，用法如下：

  ```bash
  $ tar -jxv -f filename.tar.bz2 -C targer_folder file1
  ```

  其中，filename.tar.bz2为需要解打包的文件，file1为需要的目标文件，targer_folder为目标目录。

  ![image-20211010174744834](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211010174744834.png)

- 打包时，排除某些文件

  可以使用--exclude来排除不需要打包的文件，示例如下：

  ![image-20211010175823219](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211010175823219.png)

