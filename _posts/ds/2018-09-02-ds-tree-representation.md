---
layout: post
title: 数据结构 —— 树的表示
date: 2018-09-02 22:35:30 +08:00
category:
    - 数据结构
keywords: DS
tags:
    - 树
---

# 树接口

作为树接口，常需要提供如下接口

|接口|功能|
| :------| ------: |
|root()|根节点|
|parent()|父节点|
|firstChild()|长子|
|nextSibling()|兄弟|
|insert(i, e)|将e作为第i个孩子插入|
|remove(i)|删除第i个孩子(及其后代)|
|traverse()|遍历|

# 父节点表示法

![tree-represenation-parent-node](/images/ds/tree-represenation-parent-node.png)


# 孩子节点表示法

![tree-represenation-child-node](/images/ds/tree-represenation-child-node.png)

# 父节点+孩子节点表示法

![tree-represenation-parent-and-child-node.png](/images/ds/tree-represenation-parent-and-child-node.png)
