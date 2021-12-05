---
title: 快速幂算法
date: 2021-12-05 16:26:44
mathjax: true
tags:
- math
- algorithm
categories: algorithm
---

快速幂算法是用来计算$x^n$的算法，它是分治算法的典型应用之一。

<!--more-->

### 1. 递归实现

$x^n$即为n个x相乘，可以使用for循环来实现，但是其复杂度为$O(n)$。考虑计算$x^{100}$，我们并不需要在计算出$x^{99}$的基础上乘以$x$，而是在计算出$x^{50}$以后，就能得到$x^{100}=x^{50}*x^{50}$。计算规模从100降至50。对于奇数次方，可以通过补乘以一个x来实现，例如：$x^{25}=x^{12}*x^{12}*x$。根据上述计算过程，很容易利用递归来实现：

```c++
// 利用递归实现快速幂计算
double quickPow(double x, int n) {
    if(n == 1) return x;
    double midVal = quickPow(x, n / 2);
    return n % 2 == 0 ? midVal * midVal : midVal * midVal * x;
}
```

### 2. 迭代实现

上述的递归过程实际上将计算规模逐次减半，直到规模为0，按照0到n的视角，实际上完成了下述的计算过程：
$$
x^{1} \rightarrow x^{1} \rightarrow x^{2} \rightarrow x^{4} \rightarrow x^{8} \rightarrow x^{16} \rightarrow \cdots
$$
在计算的过程中，我们会得到$x^{1}, x^{2}, x^{4}, x^{8}, x^{16}, \cdots$这些值。那么$x^n$是由这其中的哪些组合相得到的呢？以$x^{56}$为例，将56化为二进制，$(56)_{10}=(111000)_{2}$，其中3个1所在位置的权重分别为：$2^3=8$，$2^4=16$，$2^5=32$。同时，我们可以发现：$x^{56}=x^{32}*x^{16}*x^{8}$。因此，我们可以x进行不断的平方，得到$x^{2}, x^{4}, x^{8}, x^{16}, \cdots$，若n的第i位为1，则将$x^{2^i}$的计入结果。代码如下：

```c++
// 利用迭代实现快速幂计算
double quickPow(double x, int x) {
    double ans = 1;
    while(n) {
        if(n % 2 == 1) {
            ans = ans * x;
        }
        x = x * x;
        n = n / 2;
    }
    return x;
}
```

### 3. 快速幂的应用

这是LeetCode上的第372题（[超级次方](https://leetcode-cn.com/problems/super-pow/)），这道题的不同之处在于幂次n是一个非常大的数值，并且是通过数组的方式给出的。例如：幂次是1024，则输入为[1,0,2,4]。因此，我们首先需要将数组转换为实际的数字，设数组为$b=[b_0,b_1,b_2,\cdots,b_{m-1}]$，则幂次n为：
$$
n=\sum_{i=0}^{m-1}10^{m-1-i}*b_i
$$
根据$a^{x+y}=a^x*a^y$和$a^{xy}=(a^x)^y$，可得：
$$
x^n=x^{\sum_{i=0}^{m-1}10^{m-1-i}*b_i}=\prod_{i=0}^{m-1}x^{10^{m-1-i}*b_i}=\prod_{i=0}^{m-1}(x^{10^{m-1-i}})^{b_i}
$$
原题中考虑了结果过大，将结果对1337取了模。如果不考虑这个的话，计算$x^{n}$的代码如下：

```c++
// 计算x^n
double quickPow(double x, int x) {
    double ans = 1;
    while(n) {
        if(n % 2 == 1) {
            ans = ans * x;
        }
        x = x * x;
        n = n / 2;
    }
    return x;
}

double superPow(double x, vector<int> b) {
    double ans = 1.0;
    for(int i = b.size() - 1; i >= 0; i--) {
        ans = ans * quickPow(x, b[i]);
        x = quickPow(x, 10);
    }
    return ans;
}
```

