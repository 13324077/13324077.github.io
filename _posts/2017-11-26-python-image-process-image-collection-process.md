---
layout: post
title: python图像处理(4)之图像批量处理
date: 2017-11-26 21:09:29 +08:00
category:
    - 数字图像处理
keywords: python，图像处理
tags:
    - python
    - 数字图像处理
---

# 图片批量处理接口

当对一批图片进行处理时，有如下方法：

- 循环进行处理

- 调用程序自带的图片集合来处理

图片集合函数为:

```python
skimage.io.ImageCollection(load_pattern, load_func=None)
```

该函数在io模块中，带两个参数

- load_pattern, 图片组的路径，可是是一个str字符串

- load_func, 是一个回调函数，对图片进行批量处理就可以通过这个回调函数实现，默认回调函数是 **imread()**, 即默认这个函数是批量读取图片


# 图片批量处理例子

例如:

```python
import skimage.io as io
from skimage import data_dir

str = data_dir + '/*.png'
coll = io.ImageCollection(str)
print(len(coll))
```

输出为
> 27

表示系统自带了27张图片，通过ImageCollection接口读取出来，放在图片集合coll变量中，若要显示其中一张图片，使用如下代码

```python
io.imshow(coll[20])
io.show()
```

显示结果如下

![image_collection_show](/images/skimage/image_collection_show.png)

当要把不同格式的图片，例如既有jpg格式图片，又有png格式图片，全部读取出来的方法如下

```python
import skimage.io as io
from skimage import data_dir
str='d:/pic/*.jpg:d:/pic/*.png'
coll = io.ImageCollection(str)
print(len(coll))
```

**注意**:

> 这个地方'd:/pic/*.jpg:d:/pic/*.png' ，是两个字符串合在一起的，第一个是'd:/pic/*.jpg', 第二个是'd:/pic/*.png' ，合在一起后，中间用**冒号(:)** 来隔开， 这样就可以把d:/pic/文件夹下的jpg和png格式的图片都读取出来。如果还想读取存放在其它地方的图片，也可以一并加进去，只是中间同样用冒号来隔开。

io.ImageCollection()这个函数省略第二个参数，就是批量读取。如果我们不是想批量读取，而是其它批量操作，如批量转换为灰度图，那就需要先 **定义一个函数**，然后将这个函数作为第二个参数，如：

```python
from skimage import data_dir,io,color

def convert_gray(f):
    rgb=io.imread(f)
    return color.rgb2gray(rgb)

str=data_dir+'/*.png'
coll = io.ImageCollection(str,load_func=convert_gray)
io.imshow(coll[20])
io.show()
```

输出为:

![image_collection_show_self_defined_func](/images/skimage/image_collection_show_self_defined_func.png)


批量处理对于视频处理来说非常有用，因为视频就是一系列的图片组成

```python
from skimage import data_dir,io,color

class AVILoader:
    video_file = 'myvideo.avi'

    def __call__(self, frame):
        return video_read(self.video_file, frame)

avi_load = AVILoader()

frames = range(0, 1000, 10) # 0, 10, 20, ...
ic =io.ImageCollection(frames, load_func=avi_load)
```

myvideo.avi这个视频中每隔10帧的图片读取出来，放在图片集合中。

得到图片集合以后，我们还可以将这些图片连接起来，构成一个维度更高的数组，连接图片的函数为：

```python
skimage.io.concatenate_images(ic)
```

带一个参数，就是以上的图片集合，如：

```python
from skimage import data_dir,io,color

coll = io.ImageCollection('d:/pic/*.jpg')
mat=io.concatenate_images(coll)
```

使用concatenate_images(ic)函数的前提是读取的这些图片尺寸必须一致，否则会出错。我们看看图片连接前后的维度变化：

```python
from skimage import data_dir,io,color

coll = io.ImageCollection('d:/pic/*.jpg')
print(len(coll))      #连接的图片数量
print(coll[0].shape)   #连接前的图片尺寸，所有的都一样
mat=io.concatenate_images(coll)
print(mat.shape)  #连接后的数组尺寸
```

显示结果：

```
2
(870, 580, 3)
(2, 870, 580, 3)
```

可以看到，将2个3维数组，连接成了一个4维数组

如果我们对图片进行批量操作后，想把操作后的结果保存起来，也是可以办到的。

例：把系统自带的所有png示例图片，全部转换成256*256的jpg格式灰度图，保存在d:/data/文件夹下

改变图片的大小，我们可以使用tranform模块的resize()函数，后续会讲到这个模块。

```python
from skimage import data_dir,io,transform,color
import numpy as np

def convert_gray(f):
     rgb=io.imread(f)                       #依次读取rgb图片
     gray=color.rgb2gray(rgb)               #将rgb图片转换成灰度图
     dst=transform.resize(gray,(256,256))   #将灰度图片大小转换为256*256
     return dst

str=data_dir+'/*.png'
coll = io.ImageCollection(str,load_func=convert_gray)
for i in range(len(coll)):
    io.imsave('/home/wxer/pic/'+np.str(i)+'.jpg',coll[i])  #循环保存图片
```

保存的图片结果如下

![image-collection-save-results](/images/skimage/image-collection-save-results.png)

# 参考

[python数字图像处理（6）：图像的批量处理](http://www.cnblogs.com/denny402/p/5123772.html)
