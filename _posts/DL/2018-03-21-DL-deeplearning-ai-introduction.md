---
layout: post
title: Deep Learning —— 深度学习介绍
date: 2018-03-21 20:06:00 +08:00
category:
    - 深度学习
keywords: DL
tags:
    - 深度学习
---

Deep Learning.AI课程主要的内容包括:

> - 神经网络和深度学习基础
>
> - 改进深度神经网络: Hyperparameter tuning，Regularization和Optimization
>
> - 构建机器学习项目
>
> - 卷积神经网络（CNN）
>
> - 自然语言处理：构建序列模型（RNN，LSTM等）

**Part I - Week 1**

深度学习，可以认为是训练一个神经网络，一个非常大的神经网络。

以下先对深度学习进行简要介绍，首先从神经网络说起

# 什么是神经网络？

![what-is-nn.png](/images/deep-learning-ai/what-is-nn.png)

对于房价预测的例子，房子的大小与价格的关系如上图所示，通过线性回归得到蓝色的斜线，这个就可以认为是一个非常简单的 **神经网络**

该简单神经网络的输入是房子的大小，用x表示，输出是房价，用y表示，如下图所示，就是一个单神经元神经网络，而较大规模的神经网络则是由多个单神经元神经网络组合在一起得到

单个神经元的结构如下

![single-neuron-structure.png](/images/deep-learning-ai/single-neuron-structure.png)

中间的圆圈表示”神经元”，实现的功能就是前面房价预测的线性回归中的蓝色直线。而实现的这个函数功能就是ReLU函数，形状如下

![ReLU-function.png](/images/deep-learning-ai/ReLU-function.png)

**ReLU** —— Rectified Linear Unit

较大规模的神经网络结构如下图

![larger-NN-example.png](/images/deep-learning-ai/larger-NN-example.png)

输入x是多个特征值，输出y表示房价, 神经网络结构如下

![larger-NN-example-1.png](/images/deep-learning-ai/larger-NN-example-1.png)

输入四个特征值：

- 房子大小

- 卧室数目

- 邮政编码

- 邻居财富

神经网络结构中的输入层，隐藏层，输出层如下图所示

![larger-NN-example-2.png](/images/deep-learning-ai/larger-NN-example-2.png)

以上就是基本的神经网络


# 监督学习

如下是监督学习的一些例子

![supervised-learning-examples.png](/images/deep-learning-ai/supervised-learning-examples.png)

对于不同的监督学习，选择使用不同类型的神经网络算法

- 对于房价预测、广告点击预测，使用标准NN

- 对于图像，使用CNN

- 对于语音识别、机器翻译，使用RNN

- 对于自动驾驶，使用定制的、混合的复杂NN

常见的神经网络结构如下图

![NN-types.png](/images/deep-learning-ai/NN-types.png)

# 结构化数据和非结构化数据

![structured-data-and-unstructured-data.png](/images/deep-learning-ai/structured-data-and-unstructured-data.png)

神经网络可以应用到这两种类型的数据


# 为什么近来深度学习得到巨大的发展?

下图说明了为什么近来深度学习得到巨大发展

![Why-deep-learning-progress-so-largely.png](/images/deep-learning-ai/Why-deep-learning-progress-so-largely.png)

此处的scale的指的是两个方面：

- 一个是神经网络规模
- 一个是数据规模

主要是如下方面驱动了深度学习的巨大发展

- Data (数据规模)

- Computation （CPU 和 GPU的发展）

- Algorithms （算法革新）

例如，如下神经网络的激活函数从sigmoid改为ReLU, 这样梯度下降算法的收敛速度更加快速

![sigmoid-to-ReLU.png](/images/deep-learning-ai/sigmoid-to-ReLU.png)

# 神经网络算法调试过程

![deep-learning-development-process.png](/images/deep-learning-ai/deep-learning-development-process.png)
