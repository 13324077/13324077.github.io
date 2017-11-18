---
layout: post
title: python图像处理(1)之scikit-image库使用入门
date: 2017-11-11 09:36:59 +08:00
category: 图像处理
keywords: python, scikit-image
tags: python scikit-image
---

* content
{:toc}


**scikit-image** 是一个图像处理的python库，供numpy数组使用，在python代码中通过如下语句导入该库

```python
import skimage
```

# scikit-image入门

skimage中大多数函数在如下子模块中

```python
from skimage import data
camera = data.camera()
```

子模块和函数列表可以在 **[API](http://scikit-image.org/docs/stable/api/api.html)** 网页上找到

使用scikit-image, 图像使用Numpy的数组表示，例如对于2-D灰度图像的2-D数组

```python
In [9]: type(camera)
Out[9]: numpy.ndarray

In [10]: camera.shape
Out[10]: (512, 512)
```

skimage.data子模块提供了函数集返回图像的例子， 可以使用这些scikit-image自带的函数可以快速的学习函数用法

```
In [11]: coins = data.coins()

In [12]: from skimage import filters

In [13]: threshold_value = filters.threshold_otsu(coins)

In [14]: threshold_value
Out[14]: 107

In [15]: coins
Out[15]:
array([[ 47, 123, 133, ...,  14,   3,  12],
       [ 93, 144, 145, ...,  12,   7,   7],
       [126, 147, 143, ...,   2,  13,   3],
       ...,
       [ 81,  79,  74, ...,   6,   4,   7],
       [ 88,  82,  74, ...,   5,   7,   8],
       [ 91,  79,  68, ...,   4,  10,   7]], dtype=uint8)
```

可以使用skimage中的io子模块加载自己的图像(skimage.io.imread())，然后以Numpy数组进行存储

```python
import os
filename = os.path.join(skimage.data_dir, 'moon.png')
from skimage import io
moon = io.imread(filename)
```

# 参考

- [scikit-image documentation](http://scikit-image.org/docs/dev/user_guide/getting_started.html)
