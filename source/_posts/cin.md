---
title: C++之标准输入流
date: 2021-12-04 14:57:11
tags:
- iostream
- cin
categories: C++
---

本文主要介绍C++标准输入流的使用，重点介绍cin，cin.get()，cin.getline()的区别和使用。

<!--more-->

### 1. 标准输入输出流的继承关系

C++中输入输出流的继承关系如下图所示（图片来自[C++ Reference](http://www.cplusplus.com/reference/ios/)）

![](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/iostream.gif)

提取其中的输入流，继承关系如下：

![](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211204151513125.png)

cin是istream的对象，get()和getline()是istream中的成员函数。

### 2. cin

cin是C++中的标准输入流，它是istream类的对象，从字符流（sequence of characters）中读取并解析数据。iostream继承了istream，并且cin对象在头文件\<iostream\>中声明，因此，在使用cin时，需要包含头文件\<iostream\>。istream类重载了>>，从而实现对各种类型数据的读取。

![](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211204161158637.png)

cin并不是直接从键盘读取输入，而是从缓冲区中读取输入。也就是说，键盘的输入首先会被存储在缓冲区中，而后，cin再从缓冲区中读取数据。cin从缓冲区中读取字符流时，有如下两个特点：

1. 若缓冲区开头为空白字符（空格，tab，换行符），cin会自动跳过并丢弃空白字符
2. 若在字符流读取的过程中，遇到数据类型不符或者空白字符（空格，tab，换行），cin会停止字符流的读取，并将已经读取的字符流解释为指定的类型，赋予变量。类型不符的字符和空白字符依然留在缓冲区中，用于下一次的cin读取。
3. 若字符与变量类型不符，cin会结束读取，并返回0。

示例程序如下：

```c++
#include <iostream>
using namespace std;

int main() {
    int sum = 0;
    int input;
    while(cin >> input) {
        sum += input;
    }
    cout << "Last Input is: " << input << std::endl;
    cout << "Sum is: " << sum << std::endl;
    return 0;
}

// 输入：  20 30 -100 200z 600
// 输出：
// Last Input is: 0
// Sum is: 150
```

![](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211204170428391.png)

由于cin在读取字符流时，会跳过空白字符，并且遇到空白字符就停止读取，在输入为字符串就会有如下问题：

```c++
#include <iostream>
#include <string>

int main() {
    std::string str;
    std::cin >> str;
    std::cout << str << std::endl;
    return 0;
}

// 输入：Hello World
// 输出：Hello
```

cin在遇到空格时，就停止了读取，World还在缓冲区中等待下一次的读取。为了解决这种情况，我们可以使用cin.get()或者cin.getline()

### 2. cin.get()

cin.get()有如下三个功能：

- 读取单个字符
- 读取C风格字符串
- stream buffer

其函数原型如下所示：

![](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211204201932050.png)

使用cin.get()读取单个字符时，有如下两种用法：

```c++
char ch1, ch2;
cin.get(ch1);      // 将ch1作为参数
ch2 = cin.get();   // ch2获取cin.get()的返回值
```

当读取单个字符时，cin.get()可以读取任意单个字符，包括空格，tab和换行符。

使用cin.get()读取C风格字符串时，需要指定字符串大小n。当读到第n-1个字符，或者遇到结束符时，停止读取。结束符默认为'\n'，也可指定为其他。用法如下：

```c++
#define SIZE 100
char ch1[SIZE], ch2[SIZE];
cin.get(ch1, SIZE);
cin.get();    // 读取留下的结束符，也可以使用cin.get(ch1, SIZE).get()
cin.get(ch2, SIZE, '#');   // 读取新的字符串，并指定#为结束符
```

**cin.get(char *s, streamsize n)读取完成后，结束符依然保留在缓冲区中**。因此，在连续读取时，需要使用cin.get()将结束符读取，否则下一次读取时，cin.get(char *s, streamsize n)直接读取到结束符，会认为是空字符串，会将空字符串赋给字符串数组。

### 3. cin.getline()

cin.getline()用于读取C风格字符串，其函数原型如下：

![](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211204204043035.png)

cin.getline()与cin.get()的用法非常相似。不同点在于，**cin.getline()在读取字符串时，会读取结束符，并将结束符丢弃。**

```c++
#define SIZE 200
char ch1[SIZE], ch2[SIZE];
cin.getline(ch1, SIZE);
cin.getline(ch2, SIZE, '#');
```

### 4. getline()

getline()可以用于从标准输入流中读取数据值string，其函数原型如下：

![](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com/img/image-20211204205653114.png)

```c++
#include <iostream>
#include <string>

int main() {
    std::string str;
    getline(std::cin, str);
    std::cout << str << std::endl;
    return 0;
}
```

