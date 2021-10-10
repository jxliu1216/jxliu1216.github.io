---
title: Linux命令之sed
date: 2021-09-26 23:15:30
tag:
- Linux
- Bash
categories: Linux
---
### 1. sed简介

​		sed是Linux中的流编辑器，与传统的编辑器（如Vim）相比，它没有交互式的界面进行操作，而是通过命令来处理数据流中的数据。sed会对数据做如下操作：

1. 一次从输入中读取一行数据

2. 根据提供的命令对数据进行处理（选取，删除，替换，新增）

3. 将处理后的结果输出至stdout

<!--more-->

​		sed对一行处理完毕后，会读取下一行并继续进行上述操作，直至将所有的行处理完毕。sed命令的使用格式如下：

```bash
$ sed options 'actions' file
```

其中，options为sed命令支持的选项，actions为编辑动作。

#### 1.1 options

sed命令支持的选项和对应的功能如下所示：

| 选项 | 作用                                                    |
| :--: | :------------------------------------------------------ |
|  -n  | silent模式，只有经过sed处理的行会被输出至stdout         |
|  -e  | 允许对输入数据应用多条sed命令编辑                       |
|  -i  | 用sed修改的结果直接修改读取数据的文件，而不是由屏幕输出 |
|  -f  | 从文件中读取编辑命令                                    |

#### 1.2 actions

sed命令支持的编辑动作如下所示：

| 编辑动作 | 功能                                            |
| :------: | ----------------------------------------------- |
|    a     | 追加，在当前行后添加一行或多行                  |
|    c     | 行替换，用c后面的字符串替换原数据行，可替换多行 |
|    i     | 插入，在当前行前插入一行或多行                  |
|    d     | 删除，删除指定的行                              |
|    p     | 打印，输出指定的行                              |
|    s     | 字符串替换，用一个字符串替换另一个字符串        |

#### 1.3 sed寻址方式

在默认情况下，sed命令会对所有行进行相同的操作，但是，某些时候，我们只想对输入数据的部分行进行操作，这个时候则需要进行寻址。sed命令支持两种寻址方式：

- 以数字形式表示行区间
- 用文本模式进行匹配过滤

**A. 以数字形式表示行区间**

我们可以使用数字来指定处理的行，可以是指定特定的某一行，也可以是指定某个行区间。例如：

```bash
# 打印第三行
$ sed -n '3p' file.txt

# 打印第三到五行
$ sed -n '3,5p' file.txt
```

指定行区间，则是用起始行号，逗号以及结尾行号来表示一定区间范围内的行。需要注意的是，sed中对于行的编号是从1开始的。

**B.以文本模式进行匹配过滤**

sed命令也允许指定文本模式来过滤出命令要作用的行，格式如下：/pattern/actions。sed命令会过滤出包含pattern的行，并用actions对这些行进行处理

例如，只想打印出/etc/passwd文件中包含/bin/bash的行，则可以使用如下的sed命令

```bash
$ sed -n '/\/bin\/bash/p' /etc/passwd
```

当然，使用grep命令可以更简单的实现相同的效果。此处，举这个例子来演示sed命令的文本匹配过滤功能。

### 2. sed之打印(p)

sed进行打印时，常与-n一同使用，只打印符号条件的行。

```bash
$ sed -n '2p' test.txt           # 打印第2行
$ sed -n '2,4p' test.txt         # 打印第2到4行
$ sed -n '/hello/p' test.txt     # 打印包含hello的行
```

### 3. sed之追加(a)，插入(i)，删除(d)

使用a进行追加时，可以追加1行，也可以使用“ \ ”追加多行。

```bash
$ sed '2a hello world' test.txt      # 在第2行后添加一行hello world
$ sed '2a hello\
> world' test.txt                    # 在第二行后添加2行，分别为hello和world
```

i的用法与a的用法类似，不同的是i是在指定的行前进行插入，a是在指定的行后追加。

d可以用来删除指定的某一行，或者某个范围内的行，或者使用模式进行匹配删除。

```bash
$ sed '2d' test.txt                 # 删除第2行
$ sed '3,5d' test.txt               # 删除第3到5行
$ sed '/hello/d' test.txt           # 删除匹配到hello的行
```

### 4. sed之行替换(c)

使用c可以对指定的行进行替换，可以替换一行，也可以替换多行，替换的内容可以是一行，也可以是多行。

```bash
$ sed '2c hello world' test.txt          # 将第2行替换为hello world
$ sed '2,3c hello world' test.txt        # 将第2到第3行替换为hello world
$ sed '2,3c hello world1\
> hello world2' test.txt                 # 将第2到3行替换为两行内容
$ sed '/hello/c c_hello' test.txt        # 对包含hello的行进行替换
```

### 5. sed之字符串替换(s)

使用s进行字符串替换时的格式如下：s/pattern/replacement/flags

有4中可用的标记（flag），作用分别如下：

- 数字：表明新文本将替换第几处模式匹配的地方
- g：表明新文本将会替换所有匹配的文本
- p：表明将进行了替换操作的行打印出来
- w：将行替换的结果写到文件中

```bash
$ sed 's/test/trail/2' test.txt     # 只替换每行第2次匹配到的文本
$ sed 's/test/trail/g' test.txt     # 替换所有匹配到的文本
$ sed -n 's/test/trail/p' test.txt  # 将被替换的行打印出来
$ sed 's/test/trail/w result.txt' test.txt # 将行替换的结果写入文件
```

注意：可以将上述flags进行组合使用，达到效果的叠加。
