---
title: git使用
date: 2021-10-23 00:01:59
tags:
- Git
categories: Git
---

本文主要记录git中的一些常见概念原理和命令的使用。

<!--more-->

### 1. git相关设置

在完成git的安装后，需要进行一些设置才可以正常使用git的相关功能。

- **设置姓名和邮箱**

```bash
git config --global user.name "Your Name"
git config --global user.email "Your Email"
```

- **设置UI配色，提高命令的可读性**

```bash
git config --global color.ui auto
```

### 2. git仓库初始化

构建一个git仓库通常有两种形式，第一种是clone一个已有的git仓库，第二种是将自己的工作目录初始化为一个git仓库。clone一个已有的git仓库可以通过git clone命令加仓库地址来实现，例如：

```bash
$ git clone https://github.com/tensorflow/tensorflow.git    // clone TensorFlow远程仓库到本地
```

将自己的工作目录初始化为git仓库，通过git init命令实现。首先切换到自己的项目根目录下，然后运行git init命令即可。

```bash
$ cd "the root dir of your project"
$ git init
```

初始化为git仓库后，项目根目录下会多出一个名为**.git**的影藏文件夹，这个文件夹内记录了关于这个git仓库的所有信息。运行git init命令后，仅是初始化了一个本地git仓库，如果需要添加远程仓库，则需要使用git remote add命令：

```bash
git remote add [NAME] [remote URL]
```

通过上述命令可以添加一个远程仓库，并且指定该远程仓库的名字。

### 3. 工作区，暂存区，本地仓库和远程仓库

工作区，暂存区，本地仓库和远程仓库是git中非常重要的四个概念：

- 工作区（Workspace）：当前工作区域，可以理解为日常编辑工程文件的地方
- 暂存区（index/stage）：用于临时存放文件的改动情况
- 本地仓库（Repository）：安全存放数据的地方，这里面包含所有的提交版本的信息，其中HEAD指向最新放入仓库的版本
- 远程仓库（Remote Repository）：用于托管代码的远程服务器

这四者之间关系如下图所示：

<img src="https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/1090617-20181008211557402-232838726.png" alt="img" style="zoom: 67%;" />

git的工作流程一般有如下4步：

1. 在工作目录添加或者修改文件
2. 将需要进行版本管理的文件放入暂存区：通过git add命令完成
3. 将暂存区的文件提交至本地仓库：通过git commit命令完成
4. 将本地仓库同步至远程仓库：通过git push命令完成

在git工作流程中，一个文件通常具有如下4中状态：

- Untracked：未跟踪，文件尚未纳入版本管理中，可通过git add变为staged状态
- Unmodified：文件已在本地仓库中，但是未被修改，即当前的文件内容与本地仓库中的文件快照中的内容一致。如果对其进行修改，则其变为Modified状态；如果使用git rm移出本地仓库，则变为Untracked状态
- Modified：文件被修改了。如果对该文件进行git add，该文件变为staged状态；如果对该文件进行git checkout，则丢弃修改，返回Unmodified状态
- Staged：已暂存。执行git commit将修改同步至本地仓库中，文件变为Unmodified状态。执行git reset HEAD [name]取消暂存，文件状态变为Modified。

状态变化关系如下图所示：

![img](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/1090617-20181008212040668-1339848607.png)

可以使用git status查看文件状态，git status -s可以简略的状态信息，结果如下：

<img src="https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211023162808917.png" alt="image-20211023162808917" style="zoom:80%;" />

其中？表示文件尚未被跟踪，M表示文件已被修改，A表示文件被刚纳入版本管理并且已加入至暂存区。共有两列内容，第一列表示暂存区中的状态，第二列表示工作区中的状态。git_1.md为什么在暂存区和工作区都被显示为已修改呢？这是因为：我对git_1.md文件进行修改了之后，将其加入暂存区，然后又对其进行了修改，所以在暂存区和工作区都显示其为已修改状态。


### 4. 推送至远程仓库

- **添加远程仓库**

在将本地仓库推送至远程仓库前，需要为本地仓库添加远程仓库，这通过git remote add命令实现：

```bash
git remote add origin git@github.com:github-book/git-tutorial.git
```

执行上述命令后，将会添加远程仓库，并将远程仓库命名为origin。

- **推送至远程仓库**

使用git push命令将本地仓库中的分支推送至远程仓库中的分支，git push命令的语法如下：

```bash
git push [remote_name] [local branch name] : [remote brach name]
```

如果远程分支名与本地分支名相同，则可以省略远程分支名，例如将本地的master分支推送至origin远程仓库的master分支，可以使用如下命令：

```bash
git push -u origin master
```

其中，-u选项会将远程仓库的master设置为本地master分支的upstream，这样后续的推送和拉取会方便很多

### 5. git diff

git diff用于比较文件之间的差异，通常有如下几种用法：

- git diff [filename]：用于比较当前工作区和暂存区之间的差异
- git diff --cached [filename]：用于比较暂存区和上一次提交之间的差异
- git diff HEAD：查看工作区和暂存区与上一次提交之间的差异
- git diff commit1 commit2

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
