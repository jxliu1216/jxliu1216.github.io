---
title: C/C++函数调用分析
date: 2022-03-04 22:22:03
tags: 
- C/C++
- Operating System
categories:
- C/C++
- Operating System
---

本文记录C/C++中函数调用的栈帧变化过程。

### 1.相关寄存器

x86_64中的常见寄存器和位数如下两幅图所示。

![](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com//img/Intel_x86寄存器用途-2022-03-04.jpeg)

![](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com//img/x86-64寄存器位数-2022-03-04.png)

从图中可以看出64位是向前兼容32位，16位和8位寄存器的。



## 2.相关汇编指令

分析函数调用过程中的栈帧变化主要涉及到mov，push，pop，call，ret，leave这几条汇编指令，这几条指令的作用如下表所示

| 汇编指令 | 作用 |
| ----    | ---- |
| mov | 移动指令 |
| push | 入栈指令 |
| pop | 出站指令 |
| call | 函数调用指令 |
| ret | 返回指令 |
| leave | 返回指令 |

有时候会在指令后面加上后缀，表明操作数的大小。常见的后缀有b，w，l，q，分别表示操作数大小位1字节，2字节，4字节，8字节。

**call指令**

call指令虽然是一条函数调用指令，但实际上它包含两步。第一步：将函数的返回地址入栈，即将函数调用后的第一条指令地址入栈；第二步：将被调用函数的地址装载入rip，从而跳转到被调函数。

**ret指令**

当执行ret指令时，rsp指向的栈顶被弹出值rip，完成被调函数返回

**leave指令**

leave指令是将栈顶指针指向帧指针，然后POP备份的原帧指针到%rbp，相当于如下两条指令：

```asm
mov %rbp %rsp
pop %rbp
```


## 3.代码分析

```c++
int sum(int num1, int num2, int num3, int num4, int num5, int num6, int num7, int num8) {
    int ans;
    ans = num1 + num2 + num3 + num4 + num5 + num6 + num7 + num8;
    return ans;
}

int main() {
    int ans = sum(1, 2, 3, 4, 5, 6, 7, 8);
    return 0;
}
```

对其进行编译后，然后进行反汇编，得到对应的反汇编文件。

```bash
$ gcc func_call.c -o func_call
$ objdump -d func_call > func_call.txt
```

涉及到的相关核心部分如下所示：

![](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com//img/20220305203907-2022-03-05.png)

![](https://jxliu-picbed.oss-cn-shanghai.aliyuncs.com//img/20220304235301-2022-03-04.png)