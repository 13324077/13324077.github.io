---
layout: post
title: python模块之glob用法
date: 2017-11-12 00:03:29 +08:00
category: python模块
keywords: glob
tags: glob
---

* content
{:toc}

# glob模块介绍

glob模块根据Unix shell使用的规则找到与特定模式匹配的所有文件路径名。查找文件只用到三个匹配符：

- "*", 表示匹配0个或多个字符

- "?", 表示匹配单个字符

- "[]", 匹配指定范围内的字符，例如[0-9]表示匹配数字

# glob模块函数

## glob.glob(pathname, *, recursive=False)

返回所有匹配的文件路径列表， pathname定义了文件路径的匹配规则，可以是绝对路径，也可以是相对路径。

例如：

```python
In [4]: from glob import glob

In [5]: ls
Bag.py        helpers.pyc  LICENSE     vocab.png
figure_1.png  images/      readme_im/  vocab_unnormalized.png
helpers.py    KMeans.py    README.md

In [6]: print (glob(r"*.py"))
['Bag.py', 'helpers.py', 'KMeans.py']

In [7]: print (glob(r"*.png"))
['vocab_unnormalized.png', 'figure_1.png', 'vocab.png']
```

## glob.iglob(pathname, recursive=False)

返回一个迭代器，该迭代器不会同时保存所有匹配到的路径，遍历该迭代器的结果与使用相同参数调用glob()的返回结果一致。

例如：

```python
In [21]: from glob import iglob

In [22]: f = iglob(r"*.png")

In [23]: for py in f:
    ...:     print (py)
    ...:     
vocab_unnormalized.png
figure_1.png
vocab.png
```

## glob.escape(pathname)

这个函数是在3.4版本之后才有的，功能是忽略所有通配符。（可以用于测试某文件是否存在）

如果文件名中含有通配符，但又不想一个一个的使用’\’进行转义，那么就使用这个函数，忽略掉所有的通配符

# 参考

- [11.7. glob — Unix style pathname pattern expansion](https://docs.python.org/3.6/library/glob.html#module-glob)

- [Python模块学习：glob 文件路径查找](http://python.jobbole.com/81552/)

- [glob模块](http://blog.csdn.net/jy692405180/article/details/52245829)
