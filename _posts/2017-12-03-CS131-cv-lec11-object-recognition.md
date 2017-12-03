---
layout: post
title: 讲义11 - 对象识别
date: 2017-12-03 21:09:26 +08:00
category:
    - cv
keywords:
tags:
    - 计算机视觉
---

**主要内容**

> - 对象识别介绍
>
> - k-近邻算法
>
> - 简单的对象识别流程

# 对象识别介绍

不同的视觉识别任务之间有什么差别？

- 分类： 图像中是否包含特定的物体[Yes/No?]

- 图像搜索

- 组织相片集

- 检测：

    - 图像中是否包含特定的物体，在图像中什么位置？

    - 估计对象语义 & 几何属性

计算机视觉应用：

- 计算摄影学

- 辅助技术

- 监控

- 安检

- 辅助驾驶

- 识别地标


可视化识别就是设计算法具有如下能力：

- 分类图片或视频

- 检测或定位对象

- 估计语义和几何属性

- 分类人类活动和事件

然后非常具有挑战，**原因**:

- 现实世界中物体对象太多，～10，000到30,000

- 角度变化

- 亮度（illumination）

- 尺度（scale）

- 变形（deformation）

- 遮挡（occlusion）

- 背景混淆（background clutter）

- 相同类的变化（intra-class variation）

# k-近邻算法

机器学习框架

![machine-learning-framework](/images/cs131/lec11/machine-learning-framework.png)

- **训练**:

给定带标签的训练数据集 $$\{(x_1, y_1), ..., (x_N, y_N)\}$$, 通过对训练数据集上的预测错误进行最小化估计得到预测函数$$f$$。

- **测试**:

对之前未见过的测试数据$$x$$，应用预测函数$$f$$，得到预测的输出值$$y = f(x)$$

## 分类问题

即把输入向量赋给两个或多个类别之一。

任何决策规则通过决策边界把输入空间划分为决策边界，如下图

![decision-boundary](/images/cs131/lec11/decision-boundary.png)


## k-NN分类算法

那么所谓的 **最近邻分类(k-NN)**: 即把与各个测试数据点最近的训练数据点的标签分配给各个测试点。如下图所示

![k-nn-algo-demo](/images/cs131/lec11/k-nn-algo-demo.png)

距离计算——欧几里德

$$Dist(X^n, X^m) = \sqrt{\sum_{i=1}^{D}{(X_i^n - X_i^m)^2}}$$

其中$$X^n$$和$$X^m$$是第n个和第m个数据点， 例如如下例：

![k-nn-example](/images/cs131/lec11/k-nn-example.png)

**k-NN**: 是一个非常有用的算法

- 简单，可以首先尝试。

- 非常灵活的决策边界。

- 对于无限的例子，1-NN的错误至多是贝叶斯优化错误的两倍。

对于**k值**的选择：

- 若太小，则对噪声点敏感

- 若太大，则邻居可能包含其他类别的数据点

—— 方案： 交叉验证！

![k-nn-cross-validation](/images/cs131/lec11/k-nn-cross-validation.png)

- 使用欧几里德度量会产生违反直觉的结果，如下：

![k-nn-counter-intuitive-result](/images/cs131/lec11/k-nn-counter-intuitive-result.png)

—— 方案：对向量进行规范化为单位长度

- 维度灾难
—— 方案：没有办法解决

## 分类算法选择

对于分类问题，有很多的分类算法可以选择，如下：

- K-近邻算法

- 支持向量机(SVM)

- 神经网络(Neural Networks)

- 朴素贝叶斯(Naive Bayes)

- 贝叶斯网络(Bayesian network)

- 逻辑回归

- 随机森林

- 提升的决策树(Boosted Decision Trees)

- RBMs

- 等等。

那么哪一个算法是最好的呢？

衡量一个学习的模型的好坏，在于该模型从训练数据到新的测试数据集的泛化能力。


## 偏差(Bias)和方差(Variance)

偏差与方差之间均衡如下图所示

![bias-variance-backoff](/image/cs131/lec11/bias-variance-backoff.png)

那么 **如何减小方差**:

- 选择一个简答的分类器

- 规则化参数

- 获取更多的训练数据


最后，关于机器学习方法应用到对象识别：

- 首先，有很多机器学习算法可以选择

- 然后，知道数据情况

    - 有多少财政预算

    - 可以提供多少的训练数据

    - 是否有噪声？

- 最后，明确目标：

    - 影响特征表示的选择

    - 影响学习算法的选择

    - 影响评估度量的选择


- 理解各个机器学习算法背后的数学知识


# 简单的对象识别过程

对象识别： 一个分类框架

应用预测函数到一幅图像的特征表示得到想要的输出结果

![object-recogntion-a-classification-framework](/images/cs131/lec11/object-recogntion-a-classification-framework.png)

整体过程如下

![object-recognition-pipeline](/images/cs131/lec11/object-recognition-pipeline.png)


## 图片特征

- 颜色：量化RGB值

    - 不变特征？

        - 转换

        - 尺度

        - 旋转

        - 遮挡
- 全局形状(PCA 空间)

- 局部形状(Local Shape)

- 纹理(Texture)

## 分类器——最近邻算法

![k-nn-algo-for-object-recognition](/images/cs131/lec11/k-nn-algo-for-object-recognition.png)

# 参考

[stanford-cs131-Object-Recognition](http://cs131.stanford.edu/files/11_intro_objrecog.pdf)
