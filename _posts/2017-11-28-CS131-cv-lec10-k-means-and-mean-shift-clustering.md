---
layout: post
title: 讲义10 - K-means & Mean-Shift聚类
date: 2017-11-28 21:56:26 +08:00
category:
    - cv
keywords:
tags:
    - 计算机视觉
---

**主要内容**

> - K-means聚类
>
> - Mean-shift聚类

# K-means聚类

**目标**: 选择中心点，最小化所有点到各个最近的中心点的距离的均方和(Sum of Square Distance, SSD)

$$SSD = \sum_{cluser\; i} \sum_{x \in cluster\; i} {(x - c_i)}^2$$

## K-means计算

K-means聚类步骤：

1) 初始化(t = 0)： 聚类中心$$c_1, ..., c_K$$

2) 计算$$\delta^t$$: 分配各个点到最近的聚类中心点

$$\delta^t$$表示在迭代t时各个$$x_j$$分配到聚类中心点$$c_i$$的集合

$$\delta^t = arg\,\min_{\delta} \frac{1}{N}\sum_{j}^{N}\sum_{i}^{K}\delta_{ij}^{t-1}{(c_{i}^{t-1}-x_j)}^2$$

3)计算$$c^t$$：更新聚类中心，按如下方法

$$c^t = arg\,\min_{c}\frac{1}{N}\sum_{j}^{N}\sum_{i}^{K}\delta_{ij}^{t}{(c_{i}^{t-1} - x_j)}^2$$

4) 更新t=t+1, 重复步骤2)～3)直到结束。

如下示意图所示
![k-means-demo](/images/cs131/lec10/k-means-demo.png)

## K-means++

## K-means优缺点

**优点**:

- 找到聚类中心点最小化条件方差（很好的表示数据）

- 简单快速，容易实现

**缺点**:

- 需要选择K

- 对异常值敏感

- 容易陷入局部最小值

# Mean-shift聚类

Mean-shift聚类是一种高级的和通用的用于基于聚类的分割技术。

迭代模式搜索

1. Initialize random seed, and window W

2. Calculate center of gravity(the	“mean”) of W: $$\sum_{x\in W}xH(x)$$

3. Shift the search window to the mean
4. Repeat Step 2 until convergence
