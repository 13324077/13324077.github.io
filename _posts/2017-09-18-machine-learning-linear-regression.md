---
layout: post
title: 线性回归
date: 2017-09-18 21:23:30 +08:00
category: 机器学习
keywords: python, ML, LR
tags: 机器学习
---

* content
{:toc}

# 线性回归介绍

一般来说， 线性模型即通过对输入特征的加权和计算进行预测，同时加上一个常数,称为偏置(也称为截距）。

公式如下：

$$
\hat{y}= \theta_0 + \theta_1 x_1 + \theta_2 x_2 + ... + \theta_n x_n
$$

其中
> $$\hat{y}$$: 预测值
>
> n: 特征数
>
> $$x_i$$: 第$$i^{th}$$个特征值
>
> $$\theta_j$$: 第$$j^{th}$$个模型参数(包括偏置$$\theta_0$$，和特征权值$$\theta_1, \theta_2, ..., \theta_n$$)


上面的公式，用向量化方式重写为：

$$
\hat{y}=h_{\theta}(x) = \theta_{T}\cdot x
$$

其中

> $$\theta$$: 模型的参数向量，包含偏置$$\theta_0$$和特征权值$$\theta_1, \theta_2,...,\theta_n$$
>
> $$\theta^{T}$$: $$\theta$$的转置（行向量替换为列向量）
>
> $$x$$: 实例的特征向量，包含$$x_0, x_1, ..., x_n$$, 其中$$x_0$$总是等于1
>
> $$\theta^{T}\cdot x$$: $$\theta_{T}$$和$$x$$的点积
>
> $$h_{\theta}$$: 假设函数(hypothesis function)，使用模型参数$$\theta$$

以上就是线性回归模型的公式描述。

那么如何训练该模型呢？

所谓训练一个模型，就是设置参数，使模型和训练集最佳匹配。那么怎么样算是最佳匹配呢？所以需要一个度量方式，用来表示训练模型与实际训练集之间的匹配度。

在前面部分有学习到使用均方根误差(Root Mean Square Error, RMSE)来进行度量，即找到$$\theta$$值最小化RMSE。在实践中，使用比RMSE更加简单的均方误差(Mean Square Error, MSE)，但产生相同的结果（因为最小化一个函数同时也最小化均方根）

因此，线性回归模型的MSE损失函数为:

$$
MSE(X, h_{\theta}) = \frac{1}{m}\sum_{i=1}^{m}{(\theta^{T}\cdot x^{(i)} - y^{(i)})}^2
$$

以下介绍两种方法求解$$\theta$$值


# $$\theta$$值求解法

有两种求解法

- 正规方程法

- 梯度下降法

## 正规方程法

为了找到是损失函数最小的$$\theta$$值， 有一种直接求封闭解的方法 —— 即通过数学灯饰直接求解结果，

—— 这种方法称之为 **正规方程法**。

求解$$\theta$$的公式如下:

$$
\hat{\theta} = (X^T \cdot X)^{-1} \cdot X^T \cdot y
$$

其中
> $$\hat{\theta}$$: 最小化损失函数的$$\theta$$值
>
> $$y$$: 包含目标值$$y^{(1)},..., y^{(m)}$$的向量

如下通过python代码实例说明

```python
import numpy as np
from matplotlib import pyplot as plt

# 产生随机数据
X = 2 * np.random.rand(100, 1)
y = 4 + 3 * X + np.random.randn(100, 1)

plt.plot(X,y, "b.")
plt.show()

# 求解 theta
X_b = np.c_[np.ones((100, 1)), X]
theta_best = np.linalg.inv(X_b.T.dot(X_b)).dot(X_b.T).dot(y)  # inv计算矩阵的逆， dot()方法用于矩阵乘法

print("theta_best=\n", theta_best)

# 结果
X_new = np.array([[0], [2]])
X_new_b = np.c_[np.ones((2,1)), X_new]
y_predict = X_new_b.dot(theta_best)
y_predict

plt.plot(X_new, y_predict, "r-")
plt.plot(X, y, "b.")
plt.axis([0, 2, 0, 15])
plt.show()
```

运行的结果如下：
> 求解的theta值为：
>
> theta_best=
>    [[ 3.79895105]
>    [ 3.00291763]]

对应的数据集分布，以及线性回归结果如下图：

![linear-regression-normal-equation](/images/ml/linear-regression-normal-equation.png)

## 梯度下降法
