---
layout: post
title: Deep Learning —— 逻辑回归
date: 2018-04-04 23:16:00 +08:00
category:
    - 深度学习
keywords: DL
tags:
    - 深度学习
---
# 逻辑回归

逻辑回归算法是用于二元分类的学习算法，用在监督学习问题中。

首先来看下二元分类的例子

输入一张图片，判断它是不是一只猫，是一只猫，输出为1，非猫，则输出0，用y表示输出， 输出是离散值


![Logistic-regression-cat-or-not-cat](/images/deep-learning-ai/Logistic-regression-cat-or-not-cat.png)

那么一张图片在计算机中是如何表示的呢？

在计算机中，图片是通过矩阵来表示，矩阵中的每一个点表示一个像素点

对于一张64x64的彩色图片，则是由3个64x64的矩阵来表示，分别表示红、绿、蓝三个通道

通常我们把3x64x64的矩阵拉伸为一个1维的向量，用x来表示，用n=nx = 64x64x3 = 12288表示向量x中元素个数, 如下图

![image-representation-in-computer](/images/deep-learning-ai/image-representation-in-computer.png)


而二元分类问题就是学习到一个分类器来预测输入的图片是否是1(猫)还是0(非猫)

# 逻辑回归中标记表示法

![logistic-regression-notation](/images/deep-learning-ai/logistic-regression-notation.png)


# 损失函数和成本函数

对于逻辑回归表达式

$$\hat{y}^{(i)} = \sigma(w^T x^{(i)} + b)$$

其中 $$\sigma(z) = \frac{1}{1 + e^{-z}}$$

对于$$\{(x^{(1)}, y^{(1)}), (x^{(2)}, y^{(2)}),...,(x^{(m)}, y^{(m)})\}$$, 要使$$\hat{y}^{(i)} \approx y^{(i)}$$, 那么需要定义一个损失(错误)函数:

$$L(\hat{y}, y) = \frac{1}{2}(\hat{y} - y)^2$$

$$L(\hat{y}, y) = -(ylog \hat(y) + (1 - y)log(1 - \hat{y}))$$


**损失函数:** 是在单个训练样本上定义的，它衡量的是在单个训练样本上的表现

**成本函数**: 衡量的是整个训练样本上的表现，定义如下

$$
\begin{align}
J(w,b) &= \frac{1}{m}\sum_{i=1}^{m}L(\hat{y}^{(i)}, y^{(i)}) \\
&=-\frac{1}{m}\sum_{i=1}^{m}(y^{(i)}log \hat{y}^{(i)} + (1 - y^{(i)})log(1 - \hat{y}^{(i)})) 
\end{align}
$$

即所有训练样本的损失函数之和

以上的逻辑回归过程可以看做是非常非常小的一个神经网络
