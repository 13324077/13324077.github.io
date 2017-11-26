---
layout: post
title: 讲义5 - 边缘检测
date: 2017-11-26 13:27:26 +08:00
category:
    - cv
keywords: features，CV
tags:
    - 计算机视觉
---

**主要内容**:

> - 边缘检测
>
> - 图像梯度
>
> - 简单边缘检测器
>
> - Sobel边缘检测器
>
> - Cannay边缘检测器
>
> - Hough变换


# 边缘检测

从人类（哺乳类动物）视觉研究，边缘是非常特别的。

**边缘检测的目标**：

识别图像中突然的变化改变（不连续）

直观上，图像中大多数的语义和形状信息在边缘信息中，边缘中包含的信息比像素更加紧凑。


**为什么关注边缘**:

提取信息，识别对象，恢复几何形状和视角

**边缘来源**:

- 表面正常不连续

- 深度不连续

- 表面颜色不连续

- 亮度不连续

**边缘例子特写**:

# 图像梯度

## 1维(1D)连续导数

$$
\frac{df}{dx} = \lim_{\Delta\to 0} \frac{f(x)-f(x-\Delta x)}{\Delta x} = f^{'}(x) = f_x
$$

例如，

1) $$y = x^2 + x^4$$的导数为: $$\frac{dy}{dx}=2x+4x^3$$

2) $$y=sin x + e^{-x}$$的导数为: $$\frac{dy}{dx}=cos x + (-1)e^{-x}$$

## 1维(1D)离散导数

$$
\frac{df}{dx} = \lim_{\Delta\to 0} \frac{f(x)-f(x-\Delta x)}{\Delta x} = f^{'}(x) = f_x
$$

$$
\frac{df}{dx} = \frac{f(x)-f(x-1)}{1} = f^{'}(x)
$$
$$
\frac{df}{dx} = f(x)-f(x-1) = f^{'}(x)
$$

1维离散导数类型

后向: $$\frac{df}{dx} = f(x)-f(x-1)=f^{'}(x)$$

前向: $$\frac{df}{dx} = f(x)-f(x+1)=f^{'}(x)$$

中心: $$\frac{df}{dx} = f(x+1)-f(x-1)=f^{'}(x)$$


## 1维(1D)离散导数滤波器

后向滤波器

$$f(x) - f(x-1)=f^{'}(x)$$  ===> $$[0, 1, -1]$$


前向滤波器

$$f(x) - f(x+1)=f^{'}(x)$$  ===> $$[-1, 1, 0]$$

中心滤波器

$$f(x+1) - f(x-1)=f^{'}(x)$$  ===> $$[1, 0, -1]$$


## 2维(2D)离散导数

给定函数$$f(x,y)$$，
对应的梯度向量为:
$$\bigtriangledown f(x,y)=
\begin{bmatrix}
\frac{\partial f(x,y)}{\partial x} \\
\frac{\partial f(x,y)}{\partial y}
\end{bmatrix} =
 \begin{bmatrix}
 f_x \\
 f_y
 \end{bmatrix}
 $$

梯度幅度: $$\mid \bigtriangledown f(x,y)\mid = \sqrt{f_x^2 + f_y^2}$$

梯度方向: $$\theta = tan^{-1}({\frac{\partial f}{\partial y}}/{\frac{\partial f}{\partial x}})$$


## 2维(2D)离散导数滤波器

# 简单边缘检测器

# Sobel边缘检测器

使用3x3的的kernel来和原始图像进行卷积计算导数的估计值

其中一个用于水平变化，一个用于垂直变化
