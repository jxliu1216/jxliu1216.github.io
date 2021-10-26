---
title: 线性回归与非线性回归
date: 2021-10-24 17:09:35
mathjax: true
tags:
- linear regression
- non-linear regression
categories: Math
---

最近在看矩阵分析中的投影矩阵，进而区看了最小二乘法，然后有学习了线性回归和非线性回归，发现和自己的认知不太一样，在这里总结一下线性回归和非线性回归的概念。

线性回归和非线性回归中，线性和非线性的意思并不是用来描述因变量和自变量之间的关系是线性还是非线性的，而是用来描述模型函数与待估计参数之间的关系。我们首先来看非线性回归，明白了非线性回归，自然就知道线性回归的含义了。

<!--more-->

### 1. 非线性回归

我们首先来看维基百科中对于非线性回归的定义：

In statistics, [**nonlinear regression**](https://en.wikipedia.org/wiki/Nonlinear_regression) is a form of regression analysis in which observational data are modeled by a function which is a nonlinear combination of the model parameters and depends on one or more independent variables. 

这段话的意思是：非线性回归是采用一个函数模型对观测数据进行建模，并且该函数是由**模型参数的非线性组合**构成的，并且可以通过观测数据（自变量和因变量）对模型参数进行估计。

这里需要注意的是：非线性估计中的非线性指的是模型函数是由**模型参数的非线性组合**构成，而并不是描述函数和自变量之间的关系。

举个例子，我们采用如下的模型：

$$
\mathbf{y} \sim f(\mathbf{x},\mathbf{\beta})
$$
其中，$\mathbf{x}=\{x_1,x_2,\cdots,x_n\}$ 为自变量向量，$\mathbf{y}=\{y_1,y_2,\cdots,y_n\}$为观测因变量向量，$\mathbf{\beta}=\{\beta_1,\beta_2,\cdots,\beta_n\}$为模型参数向量。

假设某个模型由一个自变量，和两个参数构成，即：
$$
f(\mathbf{x},\mathbf{\beta})=\frac{\beta_1x}{\beta_2+x}
$$
显然，函数模型$f$无法表示成模型参数$\beta_1$和$\beta_2$的线性组合，因此，这个函数是非线性的。

### 2. 线性回归

如果我们考虑如下的模型：
$$
f(\mathbf{x},\mathbf{\beta})=\beta_0+\beta_1x_1+\beta_2x_2^2+\cdots+\beta_nx_n^2
$$
这看似是一个非线性函数，但是实际上，函数$f$对于模型参数$\{\beta_0,\beta_1,\cdots,\beta_n\}$而言是线性的，即是由$\{\beta_0,\beta_1,\cdots,\beta_n\}$的线性组合构成的，系数分别为$1,x_1,x_2,\cdots,x_n$。因此，这个函数是线性的。

### 3. 总结

线性还是非线性是要根据分析的目标来决定的，在线性回归和非线性回归中，我们需要求解的是模型参数，因而，线性与非线性描述的是函数模型与模型参数之间的关系，而非因变量与自变量之间的关系。

