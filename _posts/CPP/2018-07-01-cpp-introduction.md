---
layout: post
title: C++介绍
date: 2018-07-01 10:50:01 +08:00
category:
    - C++
keywords: C++
tags:
    - C++
---

**主要内容**：

- C++语法

- C++重要语句

- 控制台输入/输出

- 第一个C++程序

# 参考书

[Proramming Abstractions in C++](https://www.pearson.com/us/higher-education/program/Roberts-Programming-Abstractions-in-C/PGM80147.html)

# 编译工具

[Qt Creator](http://web.stanford.edu/dept/cs_edu/qt-creator/qt-creator.shtml)

# 目标

- 掌握ADTs(集合)

- 理解递归和递归回溯

- 掌握通过指针进行内存管理

- 使用数据结构（链表和树等）实现集合

- 学习关于图和图算法

- 分析算法效率

# C++语言介绍

**什么是C++？**

一种编程语言，由Bjarne Stroustrup在1983年开发

- 目前世界上最广泛使用的语言，

- 具有快速和高效的建构系统编程

- C++语言是在老的C语言基础上添加面向对象编程的特性

- C++随着时间的推移，还在不断地改进中，最新的C++版本已经是 **C++17**

**C++与Java和C语言的比较**

有很多的相似点：

- 相似的数据类型（int, double, char, void)

- 相似的操作符（+， -， \*, \/, %), 关键字

- 使用"{ }"来表示当前语句块的范围

- 都配备大量的标准库供使用

**C++源代码文件格式**

C++源代码放在 **.cpp** 文件中，额外的声明放在头文件(".h文件")中

C++源代码会编译为二进制对象文件(".o文件")

与Java语言的.class文件不同（不依赖于平台），C++二进制可执行文件是平台依赖的

C++源代码到C++可执行文件的编译过程如下图所示

![CPP-Compile-Process](/images/cs106b/CPP-Compile-Process.png)


## 第一个C++程序

```cpp
/*
* hello.cpp
* This program prints a welcome message to the user.
*/

#include <iostream>
using namespace std;
int main()
{
  cout << "Hello, world!" << endl;

  return 0;
}
```

程序解析：

- /\* \*/ CPP语言中的注释， C++语言中有两种注释： 1） "/* \*/", 2) "//"



## C++基本语法

以下面程序为例，介绍C++的常用基本语法

```cpp
int x = 42 + 7 * -5;	// variables, types
double pi = 3.14159;
char c = 'Q';         /* two comment styles */
bool b = true;
for (int i = 0; i < 10; i++)  // for loops
{  
  if (i % 2 == 0) // if statements
  {
    x += i;
  }
}
while (x > 0 && c == 'Q' || b) // while loops, logic
{
  x = x / 2;
  if (x == 42)
  {
    return 0;
  }
}
fooBar(x, 17, c);   // function call
barBaz("this is a string");   //string usage
```

## 用户输入输出

**控制端输出**

```cpp
cout << expression << expression << ....;
```

**endl**: 换行符，与"\n"相同

**控制端输入**

使用stanford的c++库中的 **simpio**， 提供了如下输入方法

| 函数名|功能描述|
|------|------|
|getInteger("prompt")| 反复提示直到整数输入，并返回|
|getReal("prompt")|反复提示直到double型数据输入并返回|
|getLine("prompt")|提供并读取/返回整行文本|
|getYesOrNo("prompt") <br> getYesOrNo("prompt", "y", "n")|反复提示Yes/No回答，返回bool类型值|

**注**: 不建议使用 **cin** 进行输入，因为不能很好的处理错误的输入，同时很难进行整行文本的输入

示例如下

```cpp
/* This program prints a score of a football game. */
#include <iostream>
#include "simpio.h"
using namespace std;
int main()
{
  int stanford = getInteger("Stanford points scored? ");
  int cal = getInteger("Cal points scored? ");
  if (stanford > cal)
  {
    cout << "Stanford won!" << endl;
  }
  else if (cal > stanford)
  {
    cout << "Cal won!" << endl;
  }
  else
  {
    cout << "A tie." << endl;
  }
  return 0;
}
```
