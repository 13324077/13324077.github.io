---
layout: post
title: 讲义7 - 特征描述
date: 2017-11-26 22:29:26 +08:00
category:
    - 计算机视觉
keywords: features，CV
tags:
    - 计算机视觉
---

**主要内容**:

> - 尺度不变区域选择
>   - 自动尺度选择
>   - DoG(Difference-of-Gaussian)检测器
> - SIFT: 图像区域描述符
>
> - HOG: 另一个图像描述符
>
> - 应用: Panorama（全景）

# 尺度不变关键点检测

当检测图像中的拐角时，使用的窗的尺度大小会改变检测到的拐角。
