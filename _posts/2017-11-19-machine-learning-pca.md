---
layout: post
title: PCA原理分析
date: 2017-11-19 21:26:29 +08:00
category:
    - 机器学习
    - PCA
    - linear-algebra
keywords: PCA
tags:
    - 机器学习
    - PCA
---

* content
{:toc}

# PCA概述

PCA， Principal Components Analysis， 主成分分析。是一个非常有用的统计技术用于人脸识别和图像压缩，是一种非常通用的在高维数据中寻找模式的方法。

以下按如下思路对PCA进行分析

-  PCA中使用的数学概念

    - 标准方差(standard deviation)

    - 协方差（covariance）

    - 特征向量（eigenvectors) 和 特征值（eigenvalues）

- PCA原理介绍


# 数学背景知识

## 统计

统计的整个主题，基于整个大的数据集 ，分析数据集中各个点之间的关系。以及基于数据集的一些度量，从而获得数据本身的一些信息。

### 标准方差

为了理解标准方差，我们需要一个数据集。

例如，如下特征集

$$ X = [1, 2, 4, 6, 12, 15, 25, 45, 68, 67, 65, 98]$$

其中$$X_3$$表示数据集$$X$$中第三个元素，即4。注意$$X_1$$是第一个元素。 $$n$$用于表示数据集$$X$$的元素个数。

那么， 有如下一些值我们比较关注

**均值（Mean）**：

计算样本的均值的公式如下：

$$\overline{X} = \frac{\displaystyle\sum_{i=1}^{n} X_{i}}{n}$$

$$\overline{X}$$表示是数据集X的平均值。该公式的含义： **把数据集中所有值相加，然后除以元素个数**。

然而， 均值只能告诉我们数据集的中间点而已，并没有其他的一些有用信息。

例如，如下两个数据集

$$[0, 8, 12, 20]$$和$$[8, 9, 11, 12]$$

它们的均值都为10，然后它们很明显非常不同，它们的扩展是不同，那么度量数据的扩展程度呢，即是如下将要介绍的概念——标准方差（Standard Deviation，SD）。

**标准方差（Standard Deviation）**：

计算公式如下：

$$s = \sqrt{\frac{\displaystyle\sum_{i=1}^{n} {(X_i - \overline{X})}^2}{(n-1)}}$$


### 方差（Variance）

计算公式如下：

$$s^2 = \frac{\displaystyle\sum_{i=1}^{n} {(X_i - \overline{X})}^2}{(n-1)}$$

由公式可知，只是标准方差的平方。和标准方差一样，都是用来度量数据集的扩展。

### 协方差（Covariance）


## 矩阵代数（Matrix Algebra）

主要介绍PCA中使用到的矩阵代数知识。尤其是特征向量和特征值。

### 特征向量
