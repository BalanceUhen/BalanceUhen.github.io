---
layout: post
title: 线性代数的本质
author: When
date: 2019-01-29 20:07:42 +0800
categories: Math Linear-algebra
tag: Math
---

# 矩阵

- 一个$2\times2$的矩阵可以看成一个$2$维平面的线性变换，矩阵$ \left[ \begin{array}{ll}{a} & {b} \\ {c} & {d}\end{array}\right] $可以看成基向量$\left[ \begin{array}{l}{0} \\ {1}\end{array}\right]$,$\left[ \begin{array}{l}{1} \\ {0}\end{array}\right]$变换成了$\left[ \begin{array}{l}{a} \\ {c}\end{array}\right]$,$\left[ \begin{array}{l}{b} \\ {d}\end{array}\right]$的一个线性变换
  - 线性变换要求原点不变且直线依然是直线，这样才有了以上的优美的性质
  - 这种理解可以推广到$n\times n​$的矩阵
  - 因为我们将矩阵看作了线性变换，那么矩阵乘法可以看成是**线性变换的复合**
    - 注意放在右边的线性变换先被应用，以及线性变换不满足交换律
      - 解释是由于$f(x)$之类把符号放在变量的左边，同时复合函数也是$f(g(x))$也是从右向左计算，所以计算顺序是这样的
    - 所以结合律是显然的

# 行列式

- 行列式表示线性变换的放大率，正负则表示坐标系的相对位置是否发生了变化（比如三维的左手坐标系变成了右手坐标系）