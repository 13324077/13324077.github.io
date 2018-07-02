---
layout: post
title: C++ 函数与字符串
date: 2018-07-01 14:50:22 +08:00
category:
    - C++
keywords: C++
tags:
    - C++
---

**主要内容**：

- 函数

  - 语法

  - 原型

  - 函数的传值调用 & 引用调用

- 字符串

  - 常用的字符串处理函数

  - C字符串与C++字符串对比

# 函数

**函数定义**, 如下图所示

![function-defition](/images/cs106b/function-defition.png)

**函数调用** 的方式如下所示

![function-call](/images/cs106b/function-call.png)

如下是函数定义和调用的示例代码

```cpp
#include <iostream>
using namespace std;
const string DRINK_TYPE = "Coke";

// 函数定义
void bottles(int count)
{
    cout << count << " bottles of " << DRINK_TYPE << " on the wall." << endl;
    cout << count << " bottles of " << DRINK_TYPE << "." << endl;
    cout << "Take one down, pass it around, " << (count - 1) << " bottles of " << DRINK_TYPE << " on the wall." << endl << endl;
}

int main()
{
    for (int i = 99; i > 0; i--)
    {
        bottles(i);
    }

    return 0;
}
```

**注**: 函数在调用前必须先定义或者声明，不然编译会出错

所谓"声明", 就是在文件中声明一个函数，但是没有定义函数体， 函数的具体定义在其他地方定义，如下代码所示

```cpp
#include <iostream>
using namespace std;
const string DRINK_TYPE = "Coke";

// 函数声明
void bottles(int count);

int main()
{
    for (int i = 99; i > 0; i--)
    {
        bottles(i);
    }

    return 0;
}

// 函数定义
void bottles(int count)
{
    cout << count << " bottles of " << DRINK_TYPE << " on the wall." << endl;
    cout << count << " bottles of " << DRINK_TYPE << "." << endl;
    cout << "Take one down, pass it around, " << (count - 1) << " bottles of " << DRINK_TYPE << " on the wall." << endl << endl;
}
```

一个定义的好函数具有如下属性

- 执行单一的任务

- 尽量不要在函数中调用其他函数，即不要函数的"链式"调用。

## 函数传值调用

所谓"传值调用"，指的是把变量的值作为参数传入函数内，函数内部对传入参数的修改，并不会影响变量的值。

示例代码如下

```cpp
void swap(int a, int b)
{
    int temp = a;
    a = b;
    b = temp;
}

int main()
{
    int x = 17;
    int y = 35;
    swap(x, y);
    cout << x << "," << y << endl;  // x = 17, y = 35

    return 0;
}
```

用下图来展示传值调用swap(x, y)之前 各个变量的值得变化

![pass_by_value](/images/cs106b/pass_by_value.png)


## 函数引用传参调用

所谓"引用调用"：就是调用函数的入参指向的是同一个变量的值，因此函数内部对函数参数的修改，实际上就是对变量的值得修改。

示例代码如下

```cpp
void swap(int &a, int &b)
{
    int temp = a;
    a = b;
    b = temp;
}

int main()
{
    int x = 17;
    int y = 35;
    swap(x, y);
    cout << x << "," << y << endl;  // x = 35, y = 17

    return 0;
}
```

用下图来展示引用调用swap(x, y)前后各个变量的值变化，函数中对a,b变量的操作其实就是对x, y变量的操作，因为引用调用时，a，b指向了x,y所在的内存块处。

![pass_by_reference](/images/cs106b/pass_by_reference.png)

## 函数常参数

定义函数时，若参数变量加上 **const** 关键字则表示当前该参数在函数体内部是不能修改的。

```cpp
void printString(const string& str)
{
    cout << "I will print this string" << endl;
    cout << str << endl;
}

int main()
{
    printString("This could be a really really long string");
}
```

## 输出参数

通过引用传参调用，可以返回多个函数执行的值， 如下示例所示

```cpp
void datingRange(int age, int& max, int& min)
{
    min = age / 2 + 7;
    max = (age - 7) * 2;
}

int main()
{
    int young;
    int old;
    datingRange(40, young, old);
    cout << "A 40-year-old could date someone from " << young << " to " << old << " years old." << endl;

    return 0;
}
```

# 字符串

**字符串**：就是字符序列(也可以是空序列)

字符即类型为char的值， 索引从0开始，例如

```cpp
string s = "Hi Cpp!";
```

结构如下图所示

![string-char-index](/images/cs106b/string-char-index.png)

- 可以通过索引 **[index]** 来访问字符串中各个字符, 或者at访问，如下

```cpp
char c1 = s[3];         // 'C'
char c2 = s.at(1);      // 'i'
```

- 字符是ASCII编码(整数映射)

```cpp
cout << (int)s[0] << endl;      // 'H'的ASCII值为72
```
## string成员函数

常用的字符串成员函数如下表

|成员函数名|功能描述|
|-------|------|
|s.append(str)|添加文本到字符串尾|
|s.compare(str)|返回<0, 0, >0根据字符串的比较结果|
|s.erase(index, length)|从字符串中删除从索引index，长度为length的文本|
|s.find(str) <br> s.rfind(str)|从字符串中出现str的开始的第一个或最后一个索引(若没有找到则返回 string::npos)|
|s.insert(index, str)|添加文本到字符串给定index开始处|
|s.length()或s.size()|字符串中的字符个数|
|s.replace(index, len, str)|把新的文本str替换给定索引index开始的len长度字符|
|s.substr(start, length)或s.substr(start)|在字符串start(包括start位置)开始的length长度字符；若length忽略，则到字符串结尾|

例如：

```cpp
string name = "Donald Knuth"
if (name.find("Knu") != string::npos)
{
    name.erase(7, 5);       //删除后，字符串name = "Donald"
}
```
## string操作符

**字符串连接符**： "+" 或 "+="

```cpp
string s1 = "Don"
s1 += "ald"     // s1 = "Donald"
```

**关系操作符**: ">" 或"=="或"<"或"!="或">="或"<="

根据ASCII顺序进行字符串比较

```cpp
string s2 = "Shreya"
if (s1 < s2 && s2 != "Joe") // 返回true
{

}
```

## C与C++字符串比较

C++有两种类型字符创

- C字符串(字符数组)

- C++字符串(string对象)

C字符串没有前面提到的成员方法，也没有字符串操作符

**两种类型字符串之间的转换**：

- C字符串转换为C++字符串: string("text")

- C++字符串转换为C字符串: string.c_str()
