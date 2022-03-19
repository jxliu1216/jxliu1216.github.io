---
title: C/C++函数指针
date: 2022-03-19 16:23:32
tags:
- C/C++
- 函数指针
categories: C/C++
---

<!--more-->

### 1.什么是函数指针

每个函数在内存的程序段都占用一段存储单元，这段存储单元的首地址称为函数入口地址，指向这个函数入口地址的指针称为函数指针。


### 2.函数指针的声明

声明一个函数指针有两种方式：

1. 直接声明一个函数指针
2. 先定义一个函数指针类型，再通过该类型声明一个函数指针

```c++
// 直接声明一个函数指针
int (*pf1)(int, int);     // pf1是一个指向返回类型为int，含有两个int类型参数的函数的函数指针
double (*pf2)();          // pf2是一个指向返回类型为double，无参数的函数的函数指针


// 定义函数指针类型
typedef int (*PF)(int, int);  // PF为一个函数指针类型
PF pf3;    // pf3为一个函数指针

```


### 3.函数指针的使用

```c++
typedef int (*PF)(int, int);

int add(int num1, int num2)
{
    return num1 + num2;
}

int main()
{
    // 直接声明函数指针
    int (*pf1)(int, int) = add;
    // 通过函数指针调用
    int res1 = pf1(100, 100);
    int res2 = (*pf1)(100, 100);

    // 通过PF类型声明函数指针
    PF pf2 = add;
    // 通过函数指针调用
    int res3 = pf2(200, 200);
    int res4 = (*pf2)(200, 200);
}
```