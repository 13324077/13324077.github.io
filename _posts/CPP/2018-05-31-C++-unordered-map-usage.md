---
layout: post
title: C++知识点 —— unordered_map用法
date: 2018-05-31 22:50：01 +08:00
category:
    - C++
keywords: C++
tags:
    - unordered_map
---



# map与unordered_map对比

## 内部实现机制

- **map**: map内部实现了一个红黑树，该结构具有自动排序的功能，因此map内部的所有元素都是有序的，红黑树的每一个节点都代表着map的一个元素，因此，对于map进行的查找，删除，添加等一系列的操作都相当于是对红黑树进行这样的操作，故红黑树的效率决定了map的效率。

- **unordered_map**: unordered_map内部实现了一个哈希表，因此其元素的排列顺序是杂乱的，无序的

## 优缺点对比

对于 **map**:

- 优点：

    - 有序性，这是map结构最大的优点，其元素的有序性在很多应用中都会简化很多的操作

    - 红黑树，内部实现一个红黑书使得map的很多操作在的时间复杂度下就可以实现，因此效率非常的高

- 缺点：

    - 空间占用率高，因为map内部实现了红黑树，虽然提高了运行效率，但是因为每一个节点都需要额外保存父节点，孩子节点以及红/黑性质，使得每一个节点都占用大量的空间


- 使用场景：

    - 对于那些有顺序要求的问题，用map会更高效一些


对于 **unordered_map**:

- 优点：

    - 因为内部实现了哈希表，因此其查找速度非常的快

- 缺点：

    - 哈希表的建立比较耗费时间

- 使用场景：

    - 对于查找问题，unordered_map会更加高效一些，因此遇到查找问题，常会考虑一下用unordered_map

# unordered_map用法


# 参考

- [unordered_map的用法](https://blog.csdn.net/u012530451/article/details/53228098)

- [关联容器：unordered_map详细介绍（附可运行代码）](https://blog.csdn.net/hk2291976/article/details/51037095)
