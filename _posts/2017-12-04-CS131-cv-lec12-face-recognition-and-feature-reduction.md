---
layout: post
title: 讲义12 - 脸部识别和特征降维
date: 2017-12-04 20:35:16 +08:00
category:
    - 计算机视觉
keywords:
tags:
    - 计算机视觉
---

**主要内容**

> - 奇异值分解(SVD)
>
> - 主成分分析(PCA)
>
> - 图像压缩

# 奇异值分解(SVD)

## 奇异值分解算法

有一些计算机算法可以"因式分解"一个矩阵，由其他矩阵的乘积表示，其中最有用的是 **奇异值分解算法(Singular Value Decomposition)**。

任何矩阵$$A$$可以表示成三个矩阵的乘积: $$U\Sigma V^T$$

用python实现如下:

```python
[U, S, V] = numpy.linalg.svd(A)
```

**奇异值分解**：

$$U\Sigma V^T = A$$

其中$$U$$和$$V$$是旋转矩阵(Rotation Matrics)， $$\Sigma$$是一个缩放矩阵(Scaling Matrix)。

例如:

![svd-example](/images/cs131/lec12/svd-example.png)

对于矩阵$$A$$，若是$$m\times n$$，那么$$U$$则是$$m \times m$$, $$\Sigma$$是$$m \times n$$，$$V^T$$是$$n\times n$$, 例如：

![svd-example-2](/images/cs131/lec12/svd-example-2.png)

$$U$$和$$V$$总是旋转矩阵

$$\Sigma$$是对角矩阵，其中

- 非零项的个数称之为$$A$$的秩

- 算法总是对非零项从高到低进行排序

## SVD应用

以上讨论了SVD的几何转换矩阵，但是图像矩阵的SVD非常的有用，


# 主成分分析(PCA)

## 协方差(Covariance)

## 协方差矩阵(Covariance Matrix)

## PCA的几何解释

## PCA公式化

## PCA算法

# 图像压缩
