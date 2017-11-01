---
layout: post
title: 讲义6 - 边缘检测
date: 2017-10-30 21:07:45 +08:00
category: 计算机视觉
keywords: features，CV
tags: 计算机视觉
---

* content
{:toc}

# 概述

介绍 **边缘检测** 相关内容

主要有：

- 边缘检测（Edge Detection）

- Hough变换

- RANSAC

边缘检测提供有意义的语音信息， 有助于对图像的理解。这有助于分析元素形状，提取图像特征，和对描述的场景的属性变化的理解，如深度的不连续，材料类型和照明等等。

我们将探索Sobel和Canny 边缘检测技术的应用。

然后介绍Hough变换，用于图像中参数模型的检测。例如，由两个参数定义的线性直线，通过Hough变换变得可能，此外，该技术将被推广用到其他形状（例如，圆），然后，我们将看到，Hough变换对于大量参数的匹配模型不起作用，为了解决模型匹配问题，最后我们介绍RANSAC（random sampling consensus).

该不确定方法反复的对数据子集进行采样，使他们适合该模型，然后根据通过适合的模型的解释(例如与模型的距离)把剩下的数据点分为"inliers"和"outliers"，结果用于最后的数据点的选择用于获得最后的适合模型。

最后，对RANSAC和Hough变换进行一个综合的对比。

# 边缘检测

## 边缘检测目的

边缘检测与哺乳动物的眼睛强相关。大脑中的某些神经元擅长识别直线，这些神经元的信息在大脑中被放在一起用于识别对象。

实际上，边缘对于人类的识别来说非常的有用，线条图几乎和原始图像一样容易被识别，如下图所示。

![line-drawings-and-original-images](/images/cs131/lec6/line-drawings-and-original-images.png)

我们想要能从一幅图像中提取信息， 识别对象和恢复几何形状和视角。

## 边缘基础


在一幅图像中，有四种可能的边缘的来源：

- 表面正常的不连续（surface changes direction sharply)

- 深度不连续 (one surface behind another)

- 表面颜色不连续 (single surface change color)

- 照明不连续 (shadows/lighting)

这些不同类型边缘的不连续的例子如下图所示，

![edge-discontinuities-in-surface-example](/images/cs131/lec6/edge-discontinuities-in-surface-example.png)

# 寻找梯度

为了找到梯度，首先我们必须在x和y方向求出导数。

## 离散导数

$$
\frac{\mathrm df(x)}{\mathrm dx} = \lim_{\delta x\to 0}\frac{f(x)-f(x-\delta x)}{\delta x} = f^{'}(x)
$$

$$
\frac{\mathrm df(x)}{\mathrm dx} = \frac{f(x)-f(x-1)}{1} = f^{'}(x)
$$

$$
\frac{\mathrm df(x)}{\mathrm dx} = {f(x)-f(x-1)}= f^{'}(x)
$$

同时还有三种不同的求导方法

- Backward： $$f^{'}(x)=f(x)-f(x-1)$$

- Forward： $$f^{'}(x)=f(x+1)-f(x)$$

- Central： $$f^{'}(x) = \frac{f(x+1) - f(x-1)}{2}$$

这些导数叶可以表示为滤波器(滤波器和图像进行卷积)

- Backward：$$f^{'}(x)=f(x) - f(x-1) \to [0, 1, -1]$$

- Forward： $$f^{'}(x)=f(x+1)-f(x) \to [-1, 1, 0]$$

- Central： $$f^{'}(x) = \frac{f(x+1) - f(x-1)}{2} \to [1, 0, -1]$$

梯度$$(\nabla f)$$可以按如下公式进行计算：

$$
\nabla f(x,y) = \begin{bmatrix}
\frac{\partial f(x,y)}{\partial x}\\
\frac{\partial f(x,y)}{\partial y}
\end{bmatrix} \\
=\begin{bmatrix}
f_{x}\\
f_{y}
\end{bmatrix}

$$

根据上式可以计算梯度的幅度和角度

**幅度**：

$$
|\nabla f(x,y)| = \sqrt{f^2_x + f^2_y}
$$

**角度**

$$
\theta = tan^{-1}( \frac{f_y}{f_x} )
$$


## 减小噪声

噪声会干扰梯度，这样使用简单的方法很难找到边缘， 尽管肉眼仍然可以检测到边缘。

该方法首先对图像进行平滑。

假设$$f$$表示图像，$$g$$表示平滑kernel，因此，为了找到平滑的梯度，必须计算（1维的例子）

$$
\frac{d}{dx}(f*g)
$$

通过卷积的导数定理，得到：

$$
\frac{d}{dx}(f*g) = f * \frac{d}{dx}g
$$

这种简化节省了一项操作，平滑消除了噪声，但是却模糊了边缘，同时使用不同的kernel大小可以检测到不同尺度的边缘。


## Sobel噪声检测器

该算法利用2个3x3kernel
。。

## Canny边缘检测器

Canny边缘检测器有5个算法步骤

。。。

# Hough转换

## Hough转换介绍

## Hough转换用于检测线条的目标

## 在a,b空间使用Hough转换检测线条

## 累加器单元

# RANSAC

## RANSAC基础介绍
