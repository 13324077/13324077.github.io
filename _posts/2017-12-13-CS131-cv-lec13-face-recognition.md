---
layout: post
title: 讲义13 - 脸部识别和特征降维
date: 2017-12-13 21:13:16 +08:00
category:
    - 计算机视觉
keywords:
tags:
    - 计算机视觉
---

**主要内容**

> - 脸部识别介绍
>
> - 特征脸算法
>
> - 线性判别分析(Linear Discriminant Analysis, LDA)

# 脸部识别介绍

**检测与识别的区别**，如下图

![dectection-and-recognition](/images/cs131/lec13/dectection-and-recognition.png)

人脸识别常见应用

- 数码相机

- 监控

- 相册集

- 人物跟踪

- 情感和表情

- 视频电话会议

- ...

## 人脸空间

一幅图像是一个高维空间的点组成

- 若用灰度值表示，则$$N\times M$$的图片的点数为$$R^{NM}$$

- 例如，100x100的图像=10,000维

100x100的图像可以包含很多东西而不仅仅是脸部

# 特征脸算法

# 线性判别分析(LDA)

## Fischer线性判别分析

**目标**:找到两个类别之间最优的间隔

**PCA和LDA之间的区别**:

PCA保留最大的方差

LDA保留判别(Discrimination)
