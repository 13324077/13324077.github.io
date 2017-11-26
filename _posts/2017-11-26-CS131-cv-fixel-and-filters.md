---
layout: post
title: 讲义4 - 像素和滤波
date: 2017-11-23 16:31:26 +08:00
category:
    - cv
keywords: features，CV
tags:
    - 计算机视觉
---

**主要内容**:

> - 图像采样和量化
>
> - 图像柱状图（直方图)
>
> - 图像作为函数
>
> - 线性系统(滤波器)
>
> - 卷积和相关性

# 图像采样和量化

## 图像类型

- 黑白图像

- 灰度图像

- 彩色图像

![image_type](/images/cs131/lec4/image_type.png)

## 图像表示

其中，

**黑白图像表示**:

![binary-image-representation](/images/cs131/lec4/binary-image-representation.png)

**灰度图像表示**:

![grey-image-representation](/images/cs131/lec4/grey-image-representation.png)

**彩色图像表示**:

一个颜色通道（R通道）如下：

![color-red-channel-image-representation](/images/cs131/lec4/color-red-channel-image-representation.png)

三个颜色通道（RGB通道）如下：

![color-RGB-channel-image-representation](/images/cs131/lec4/color-RGB-channel-image-representation.png)


## 图像采样和量化

当我们把图像放大时，会发现图像是有一个个像素点组成，如下图

![sampled-image-pixels](/images/cs131/lec4/sampled-image-pixels.png)

而实际上，一幅图像中所有的像素点是连续的，但计算机只能对离散的点进行处理，因此需要对连续的图像进行采样，如下图

![error-due-sampling](/images/cs131/lec4/error-due-sampling.png)

由上图可以， 通过采样获得的离散点和实际的连续的点之间会存在差异，即采样导致的误差。

以下介绍采样有关的参数

**分辨率**: 定义为每英尺寸上的像素点数(dots per inch, 缩写DPI)，等价为空间像素密度的度量，当前屏幕的标准分辨率为72dpi。

![dpi-example](/images/cs131/lec4/dpi-example.png)


由上可知，图像是包含离散的像素点

例如，如下灰度图像，各个像素点表示的是对应的灰度值，灰度值(强度)的范围:[0~255]

![grey-image-sampled-quantized](/images/cs131/lec4/grey-image-sampled-quantized.png)

如下采样图像，各个像素点表示的是对应的颜色空间的值，以RGB为例，分别表示红绿蓝的值

![colors-image-sampled-quantized](/images/cs131/lec4/colors-image-sampled-quantized.png)

常见的颜色空间有:

- RGB: [R, G, B]

- Lab: [L, a, b]

- HSV: [H, S, V]

# 图像直方图(Histogram)

图像的直方图提供了图像中各个亮度(强度)值的频数

如下python代码可以求得一幅图像中各个亮度值的频数

```python
import numpy as np

def histogram(img):
    h = np.zeros(255)
    for row in img.shape[0]:
        for col in img.shape[1]:
            val = img[row, col]
            h[val] += 1
```

![histogram-example](/images/cs131/lec4/histogram-example.png)

对于灰度图像来说，直方图反映了图像中灰度值的分布情况，如上图

# 图像作为函数

## 图像作为离散函数

图像通常是数字的（离散的）， 在常规网格的2D空间的样本点，通过整数矩阵表示，如下图

![image-integer-matrix](/images/cs131/lec4/image-integer-matrix.png)

## 图像作为坐标

**迪卡尔坐标**

![image-Cartesian-coordinates](/images/cs131/lec4/image-Cartesian-coordinates.png)


## 图像作为函数

一幅图像作为$$R^2 \to R^M$$映射的函数$$f$$

- 对于灰度图像， $$f(x,y)$$表示位置$$(x,y)$$的强度

- 对于彩色图像， $$f(x,y) = \begin{bmatrix}
r(x,y) \\
g(x,y) \\
b(x,y)
\end{bmatrix}$$


# 线性系统(滤波器)

**什么是滤波**：

对图像的原始像素值进行转换得到的新的图像。

**目的**:

从图像中提取有用的信息，或者是转变图像到另一个域中，这样我们可以对图像的属性进行修改或者增强。

- 特征(边缘，拐角，斑点)

- 超分辨率， 图像修复，去噪

![image-filter-application](/images/cs131/lec4/image-filter-application.png)




# 卷积和相关性

## 1D离散卷积

例如，函数$$f$$和滤波器$$h$$进行进行卷积

$$g[n] = \sum_{k}f[k]h[n-k]$$

其中$$f$$和$$h$$函数如下

![1d-convolution-f-h](/images/cs131/lec4/1d-convolution-f-h.png)

**1D卷积计算过程**:

首先要计算$$h[n-k]$$, 如下:

![h-function-calc](/images/cs131/lec4/h-function-calc.png)

那么，计算$$g[-2]$$， 即$$g[-2] = \sum_{k}f[k]h[-2-k]$$, 则计算过程如下图示意所示

![g=-2-calc.png](/images/cs131/lec4/g=-2-calc.png)

$$g[-1] = \sum_{k}f[k]h[-1-k]$$, 则计算过程如下图示意所示

![g=-1-calc.png](/images/cs131/lec4/g=-1-calc.png)


即$$g[0] = \sum_{k}f[k]h[0-k]$$, 则计算过程如下图示意所示

![g=0-calc.png](/images/cs131/lec4/g=0-calc.png)

$$g[1] = \sum_{k}f[k]h[1-k]$$, 则计算过程如下图示意所示

![g=1-calc.png](/images/cs131/lec4/g=1-calc.png)


即$$g[2] = \sum_{k}f[k]h[2-k]$$, 则计算过程如下图示意所示

![g=2-calc.png](/images/cs131/lec4/g=2-calc.png)

......

即$$g[n] = \sum_{k}f[k]h[n-k]$$, 则计算过程如下图示意所示

![g=n-calc.png](/images/cs131/lec4/g=n-calc.png)


到此，离散卷积的计算步骤如下:

1. 对$$h[k]$$进行反转得到$$h[-k]$$

2. 移动并折叠结果得到$$h[n-k]$$

3. $$h[n-k]$$乘以$$f[k]$$

4. 对所有的$$k$$的乘积项进行相加

5. 对于每一个$$n$$，重复步骤1~4

$$g[n]=\sum_{k}f[k]h[n-k]$$

## 2D卷积

2D卷积和1D卷积类似

主要的差别是需要迭代2维的数据

$$f[n,m] * h[n,m] = \sum_{k=-\infty}^{\infty} \sum_{l=-\infty}^{\infty} f[k,l] h[n-k,m-l]$$

其中$$f[.]$$是一个7x7的矩形， $$h[.]$$是一个3x3的滤波器

2D卷积计算过程如下图所示

![2d-convolution-process](/images/cs131/lec4/2d-convolution-process.png)
