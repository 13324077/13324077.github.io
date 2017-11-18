---
layout: post
title: 神经网络编程基础
date: 2017-11-04 09:16:30 +08:00
category: 机器学习
keywords: python, ML, kNN
tags: 逻辑回归 梯度下降
---

* content
{:toc}

神经网络编程基础

首先，我们从逻辑回归开始进行介绍，

逻辑回归是一个二元分类算法

# 分类问题描述

给定一幅图像，判断是否是猫，1表示是猫， 0表示不是。

计算机中存储一幅（彩色）图像，需要三个独立的矩阵，分别对应着红（R)，绿（G)，蓝（B）三个颜色通道, 如下示意图

![neural-network-single-sample-notation](/images/deep-learning-ai/neural-network-single-sample-notation.png)

若图像大小是 64pixel X 64 pixel， 那么在计算机中会有3个64X64大小的实数矩阵

为了用向量来表示，我们把矩阵中的像素展开为一个向量，作为机器学习算法的输入

向量大小：64x64x3 = 12288

用$$n = nx = 12288 $$  表示输入向量的维度


# 分类问题的目标

二元分类中，目标是学习一个分类器：

>**输入一幅特征向量x表示图像，预测对应的输出是0还是1.**

以下是介绍一些机器学习中常用的记号

单个样本： 用$$(x,y)$$表示
x是一个nx维的特征向量, $$x\in \Re^{n_x}$$

y是对应的标签（类别），取值为0或者1, $$y\in\{0,1\}$$

对于如下图所示的训练集

![neural-network-image-training-data](/images/deep-learning-ai/neural-network-image-training-data.png)

对于m的训练样本记为：

$$(x^{(1)}, y^{(1)}), (x^{(2)}, y^{(2)}), ..., (x^{(m)}, y^{(m)})$$

$$(x^{(1)}, y^{(1)})$$ ： 第一个输入样本

$$(x^{(2)}, y^{(2)})$$ ： 第二个输入样本

$$(x^{(m)}, y^{(m)})$$ ： 第m个输入样本

我们用小写的**m**表示训练集中样本总数，有时为了强调是训练集的样本总数，又表示为：**$$m=m_{train}$$**

当测试集时，使用 **$$m_{test}$$** 来表示测试集的样本总数

可以使用更加紧凑的方式进行表示，

定义一个矩阵，用大写的 **X** 表示， 如下：

$$
\begin{gather}
X =
\left.
\underbrace
{
  \begin{bmatrix}
  |&& |&& \cdots&& |\\
  x^{(1)}&& x^{(2)}&& \cdots&& x^{(m)}\\
  |&& |&& \cdots &&|
  \end{bmatrix}
}_{m}
  \right \}~{n_x}
\end{gather}
$$

$$X\in\Re^{n_x \times m}$$， 是一个$$n_x \times m$$的矩阵

在python中使用X.shape，得到矩阵形状的命令的输出$$n_{(x)}, m$$

对于训练集中，标签（类别）向量表示如下：

$$Y=[y^{(1)}, y^{(2)},..., y^{(m)}], Y\in \Re^{1\times m}$$

**Y** 是一个$$1 \times m$$的向量，因此，在Python在Y.shape，输出$$(1, m)$$

# 逻辑回归

给定 $$x\in \Re^{n_x}$$， 计算 $$\widehat{y} = p(y=1 \mid x) $$

即给定图像的输入向量，计算$$y=1$$的概率。

约定逻辑回归参数也是一个$$n_x$$维的向量：$$w\in\Re^{n_x}$$，

另一个参数$$b$$是一个实数： **$$b\in \Re$$**

那么，给定参数x以及参数w和b，如何得到输出$$\widehat{y}$$

一种不太靠谱的方法, 使用线性回归，如下

$$\widehat{y} = w^{T} x + b$$

但是对于分类问题，这个不是一个很好的算法。

因为对于分类算法，希望能输出y为1个概率，即$$ 0 \leq \widehat{y} \leq 1$$, 这个线性回归算法很难实现这个，因为$$w^{T} x + b$$可能是一个比1大很多的数，或者是一个负数。

对于逻辑回归，可以对$$w^{T} x + b$$应用sigmoid函数，得到$$\widehat{y}$$的范围在[0,1]之间。

因此，如下先介绍下sigmoid函数

## sigmoid函数介绍

函数形式如下：

$$
\sigma (z) = \frac{1}{1 + e^{-z}}
$$

```python
import numpy as np
import matplotlib.pyplot as plt

z = np.linspace(-9, 10, 20)

sigma = 1.0 / (1 + np.exp(-z))

plt.xkcd()
plt.plot(z, sigma)
plt.show()
```

用Python画出对应的函数图形如下

![sigmoid_function_show_pic](/images/deep-learning-ai/sigmoid_function_show_pic.png)

从上图可知，

若$$z$$是一个非常大整数时，则$$\sigma(z) \approx \frac{1}{1+0} \approx 1$$

若$$z$$是一个非常大的负数时，则$$\sigma(z) \approx \frac{1}{1+\infty} \approx 0$$

这样$$\widehat{y} = \sigma(w^T x + b)$$的值就是[0,1]范围了，

因此，实现逻辑回归时， 目标是学习到参数w和b，这样$$\widehat{y}$$就能很好的估计y=1的概率了。

那么现在的问题是如何学习到参数w和b呢？ 下面通过成本函数(cost function)和梯度下降(gradient descent)算法来计算得到w和b

> 注意：
> 在神经网络中我们把参数w和b进行分别处理，b对应一个偏置量，
>
> 在其他的一些课程中，定义了一个$$x_0=1$$, 这样输入的x的维度变成了$$n_x + 1$$, 即$$x = [x_0, x_1, x_2, ..., x_{n_x}]$$
>
> 定义$$\theta_0 = b, w=[\theta_0, \theta_1,...,\theta_{n_x}]$$, 用$$\theta$$表示参数w和b，即$$ \theta = [\theta_0, \theta_1, \theta_2,..., \theta_{n_x}]$$
>
> 这样$$\widehat{y} = \sigma(\theta^{T} x)$$
>
> **但是，在神经网络中，不用这种记号！**

## 成本函数介绍

为了训练逻辑回归模型中的参数w和b， 需要定义一个成本函数

从前面的学习知道:$$\widehat{y}=\sigma(w^T x +b)$$, 其中$$\sigma(z) = \frac{1}{1 + e^{-z}}$$

给定m个样本的训练集：$${(x^{(1)}, y^{(1)}),(x^{(2)}, y^{(2)}), ..., (x^{(m)}, y^{(m)})}$$, 希望对于每一个样本，都有$$\widehat{y}^{(i)} \approx y^{(i)}$$。即在训练集中，希望学习到的w和b，使得$$\widehat{y}$$尽量接近$$y$$，


而$$\widehat{y}^{(i)}$$为: $$\widehat{y}^{i} = \sigma(w^T x^{(i)} + b)$$, 其中$$\sigma(z^{(i)}) = \frac{1}{1 + e^{-z^{(i)}}}, z^{(i)} = w^T x^{(i)} + b$$


首先 **定义损失（错误）函数（Loss（error) function)**,

$$L(\widehat{y}, y) = \frac{1}{2}(\widehat{y}-y)^2$$

用来度量$$\widehat{y}$$与真实的标签$$y$$之间的接近程度

进一步，损失函数写为如下如下形式：

$$L(\widehat{y}, y) = -(ylog(\widehat{y}) + (1-y)log(1-\widehat{y}))$$

若 **y = 1**，则$$L(\widehat{y}, y) = -log(\widehat{y})$$, 即希望$$log(\widehat{y})$$尽可能的大， 由于sigmoid函数的特性，则是对应的希望$$\widehat{y}$$尽量的接近于1.

若 **y = 0**，则$$L(\widehat{y}, y) = -(log(1 - \widehat{y})$$，即希望$$log(1- \widehat{y})$$尽可能的大， 由于sigmoid函数的特性，则是对应的希望$$\widehat{y}$$尽量的接近于0.

以上介绍的损失函数定义是对于单个训练例子来说的，

以下定义 **成本函数(cost function)**:

$$
J(w,b) = \frac{1}{m}\sum^{m}_{i = 1}L(\widehat{y}^{(i)}, y^{(i)}) = -\frac{1}{m}\sum^{m}_{i=1}[(y^{(i)}log(\widehat{y}^{(i)}) + (1-y^{(i)})log(1-\widehat{y}^{(i)}))]
$$

成本函数是对整个训练集的每个样本的损失函数进行加权取平均得到。

## 梯度下降

如下介绍如何基于训练集通过梯度下降来学习，或者调整逻辑回归模型中的参数w和b。

前面介绍的成本函数定义如下：

$$
J(w,b) = \frac{1}{m}\sum^{m}_{i = 1}L(\widehat{y}^{(i)}, y^{(i)}) = -\frac{1}{m}\sum^{m}_{i=1}[(y^{(i)}log(\widehat{y}^{(i)}) + (1-y^{(i)})log(1-\widehat{y}^{(i)}))]
$$

成本函数可以用来衡量训练你的参数w和b在训练集上的效果，那么找到最合理的w和b，就是找到使成本函数$$J(w,b)$$尽可能小对应的w和b。

即找到$$w,b$$使$$J(w,b)$$最小化，如下图

![gradient-descent-model](/images/deep-learning-ai/gradient-descent-model.png)

从上图我们可以看到成本函数$$J(w,b)$$是一个凸函数（convex function)。

为了找到最优的参数，我们首先需要给w和b初始值，通常初始化为0, 当然随机初始化也是可以的

随机梯度下降算法，从初始化点开始，从最陡的下坡方向走一步，这是梯度下降的一次迭代。经过多次迭代，最终收敛到全局最优值或接近全局最优值

上图说明了梯度下降模型。

**梯度下降的更多细节**

梯度下降将重复执行以下更新的操作直到收敛


![single-variable-gradient-descent-repeated-calc](/images/deep-learning-ai/single-variable-gradient-descent-repeated-calc.png)


其中

$$\alpha$$：表示学习率(learning rate) —— 可以用来控制我们每一次迭代或者梯度下降法中步长大小， **后面再讨论如何选择学习率$$\alpha$$**

导数$$\frac{dJ(w)}{dw}$$: 这个是成本函数对参数w的基本更新或者变化。


记住，导数的意义是函数在对应点上的斜率。

以下以$$y = (x-5)^2 + 10$$为例，通过梯度下降方法计算得到y最小值对应的x。

如下图

![single-variable-gradient-descent-demo.png](/images/deep-learning-ai/single-variable-gradient-descent-demo.png)

因为y的导致为：$$\frac{dy}{dx} = 2 * (x - 5) $$
以初始值$$x0 = 10, \alpha = 0.4$$开始迭代计算

第一次迭代：

$$\frac{dy}{dx_0} = 2 * (10 - 5) = 10$$，则$$x_1 = x_0 - \alpha \frac{dy}{dx_0} = 10 - 0.4 * 10 = 6$$


第二次迭代：

$$\frac{dy}{dx_1} = 2 * (6 - 5) = 2$$， 则$$x_2 = x_1 - \alpha \frac{dy}{dx_1} = 6 - 0.4 * 2 = 5.2$$

第三次迭代：

$$\frac{dy}{dx_2} = 2 * (5.2 - 5) = 0.4$$，则$$x_3 = x_2 - \alpha \frac{dy}{dx_2} = 5.2 - 0.4 * 0.4 = 5.04$$

继续迭代计算，最终得到$$x_n$$为5，则得到最优的结果为5.


对于一个变量的导数，使用$$d$$来表示求导，$$\frac{dJ(w)}{dw}$$

对于多个变量的导数，使用$$\partial$$表示求偏导，$$\frac{\partial J(w,b)}{dw}$$和$$\frac{\partial J(w,b)}{db}$$

对于$$J(w,b)$$的两个变量w和b的迭代计算如下：


![multi-variable-gradient-descent-repeated-calc](/images/deep-learning-ai/multi-variable-gradient-descent-repeated-calc.png)


# 参考

1. [Logistic Regression as a Neural Network](https://www.coursera.org/learn/neural-networks-deep-learning)

2. [Matplotlib之xkcd画图](http://matplotlib.org/xkcd/index.html)
