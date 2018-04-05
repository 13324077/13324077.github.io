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

**成本函数**: 衡量在整个训练样本上对参数w和b的表现，定义如下

$$
\begin{align}
J(w,b) &= \frac{1}{m}\sum_{i=1}^{m}L(\hat{y}^{(i)}, y^{(i)}) \\
&=-\frac{1}{m}\sum_{i=1}^{m}(y^{(i)}log \hat{y}^{(i)} + (1 - y^{(i)})log(1 - \hat{y}^{(i)})) 
\end{align}
$$

即所有训练样本的损失函数之和

以上的逻辑回归过程可以看做是非常非常小的一个神经网络

# 梯度下降

梯度下降用来基于训练集来学习参数$$w$$和$$b$$

回顾:

$$\hat{y} = \sigma(w^T x + b)$$, 其中$$\sigma(z)=\frac{1}{1+e^{-z}}$$


成本函数:

$$
\begin{align}
J(w,b) &= \frac{1}{m}\sum_{i=1}^{m}L(\hat{y}^{(i)}, y^{(i)}) \\
&=-\frac{1}{m}\sum_{i=1}^{m}(y^{(i)}log \hat{y}^{(i)} + (1 - y^{(i)})log(1 - \hat{y}^{(i)})) 
\end{align}
$$

用来衡量参数$$w$$和$$b$$在训练集上的效果，此处所谓的效果，很自然的想到，就是使成本函数最小。

所以，我们要找到$$w$$和$$b$$，是成本函数$$J(w,b)$$最小。

若$$J(w,b)$$具有最小值，那么该函数应该是一个凸函数，如下图所示

![J_w_b_global_minimal](/images/deep-learning-ai/J_w_b_global_minimal.png)

从初始值$$w$$和$$b$$开始，朝最陡的方向往下走，经过多次迭代，最终到达$$J(w,b)$$最小的点，此时对应的$$w$$和$$b$$就是我们要计算的值。

其中w和b的每次迭代更新如下

$$w := w - \alpha \frac{dJ(w, b)}{dw}$$

$$b := b - \alpha \frac{dJ(w, b)}{db}$$

其中$$\frac{dJ(w, b)}{dw}$$和$$\frac{dJ(w, b)}{db}$$是$$J(w,b)$$对$$w$$和$$b$$的导数，

注: 对于J(w,b)中有多个变量的情况，对于每个变量的导数，称为偏导数，用如下偏导数符号

$$w := w - \alpha \frac{\partial J(w, b)}{\partial w}$$

$$b := b - \alpha \frac{\partial J(w, b)}{\partial b}$$

> $$d$$表示求导数， $$\partial$$表示求偏导数

其中$$\alpha$$, 称为 **学习率**， 用于控制每次迭代的步长， 后续有关于如何选择学习率的讨论。

此处的导数(偏导数)就是对w和b的更新或者变化量。下图示意了$$J(w,b)$$对参数$$w$$的迭代变化过程

![J_w_b_to_w.png](/images/deep-learning-ai/J_w_b_to_w.png)


对于左边，$$\frac{d J(w,b)}{dw}$$小于0, 所以$$w := w - \alpha \frac{d J(w,b)}{dw}$$逐渐变大

对于右边，$$\frac{d J(w,b)}{dw}$$大于0, 所以$$w := w - \alpha \frac{d J(w,b)}{dw}$$逐渐变小

这样，最终参数$$w$$收敛到中间使$$J(w,b)$$最小的位置。

参数$$b$$的迭代过程与此相同。

> 注: 对于多变量的情况，同时对多个变量进行迭代更新(例如，此处迭代对w，b同时进行更新)

# 逻辑回归中梯度下降法

逻辑回归的公式

$$z=w^T x + b$$

$$\hat{y} = a = \sigma(z)$$

$$L(a, y) = -(ylog(a) + (1 - y)log(1 - a))$$

对于$$w (w_1和w_2)$$和$$b$$的计算图形式如下

![Logistic_regression_derivatives.png](/images/deep-learning-ai/Logistic_regression_derivatives.png)

若想要计算损失函数$$L$$对$$w$$和$$b$$的导数，根据"链式规则"，计算如下

$$
\frac{\partial L(a,y)}{\partial w_1} = \frac{dL(a,y)}{da} \frac{da}{dz} \frac{dz}{dw_1}
$$

$$
\frac{\partial L(a,y)}{\partial w_2} = \frac{dL(a,y)}{da} \frac{da}{dz} \frac{dz}{dw_2}
$$

$$
\frac{\partial L(a,y)}{\partial b} = \frac{dL(a,y)}{da} \frac{da}{dz} \frac{dz}{db}
$$

其中

$$
\frac{dL(a,y)}{da} = -\frac{y}{a} + \frac{1-y}{1-a}
$$

$$
\frac{da}{dz}=\frac{e^{-z}}{(1+e^{-z})^2} = \frac{1 + e^{-z} - 1}{(1+e^{-z})^2} = \frac{1}{1 + e^{-z}} - \bigg(\frac{1}{1 + e^{-z}}\bigg)^2 = a - a^2
$$

$$
\frac{dz}{dw1} = x1
$$

这样
$$
\begin{align}
\frac{\partial L(a,y)}{\partial w_1} &= \bigg(-\frac{y}{a} + \frac{1-y}{1-a}\bigg) * (a - a^2) * x_1 \\
&=(-y(1-a) + (1-y)a) * x_1 \\
& = (a - y) x_1
\end{align}
$$

以此类推

$$
\frac{\partial L(a,y)}{\partial w_2} = (a-y)x_2
$$

$$
\frac{\partial L(a,y)}{\partial b} = (a-y)
$$

从而对w和b进行更新，如下

$$w_1 := w1 - \alpha dw_1$$

$$w_2 := w2 - \alpha dw_2$$

$$b := b - \alpha db$$

以上的计算都是针对单个训练样本的损失函数的计算，以下介绍针对整个训练样本的成本函数的计算

成本函数的公式如下：

$$
J(w,b) = \frac{1}{m}\sum_{i = 1}^{m}L(a^{(i)}, y)
$$

其中$$a^{(i)} = \hat{y}^{(i)} = \sigma{(z^{(i)})}=\sigma(w^T x^{(i)} + b)$$

基于单个训练样本的导数可知

$$\frac{\partial L}{\partial w_1^{(i)}}$$, $$\frac{\partial L}{\partial w_2^{(i)}}$$表示第$$i$$个训练样本的导数

而成本函数就是对所有训练样本的损失函数的和的平均，因此成本函数对$$w_1$$的导数就是每个训练样本对$$w_1$$的导数的和平均，如下

$$
\frac{\partial}{\partial w_1}J(w,b) = \frac{1}{m} \sum_{i=1}^{m} \frac{\partial}{\partial w_1}L(a^{(i)}, y^{(i)})
$$

因此，对$$m$$样本的逻辑回归的更新计算伪代码如下

![logist_regression_cost_function_parameter_calc.png](/images/deep-learning-ai/logist_regression_cost_function_parameter_calc.png)

上面左边使用for循环的方式实现参数的迭代更新计算，这种方式的计算效率是比较低的。

由于对于深度学习算法，由于数据特征规模非常大，for训练的方式效率非常的低下，因此可以使用**向量化**的技术，可以极大的提高计算效率。

# 参考

[神经网络基础](http://mooc.study.163.com/learn/2001281002?tid=2001392029#/learn/content?type=detail&id=2001702010)
