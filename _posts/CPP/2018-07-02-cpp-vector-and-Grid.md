---
layout: post
title: C++中vector与grid
date: 2018-07-02 22:10:10 +08:00
category:
    - C++
keywords: C++
tags:
    - C++
---

**主要内容**：

- Vector: 表示链表的数据结构

- Grid: 表示二维信息的数据结构

# Vectors(Lists)

vector(又称list)， 索引从0开始的元素集合

- 一个类似于可以动态改变数据个数的数组
- vector中保存的数据元素类型通过<>来指示

例如：

```cpp
// 初始化一个包含5个整数的vecotr
//         索引  0   1   2   3   4
vector<int> nums{42, 17, -6, 0, 28};
vector<string> names;       // {}
names.add("Tom");           // {"Tom"}
names.add("Jack");          // {"Tom", "Jack"}
names.insert(0, "Edison")   // {"Edison", "Tom", "Jack"}
```
vector与数组(array)相比的优势:

- 数组具有固定的大小，且不容易调整大小

- C++的数组越界访问没有检查机制，容易造成程序崩溃

- 数组不支持vector所具有的方便的操作，像在数组的前面/中间/后面的插入或删除元素操作，搜索给定元素操作等。

## vector对象的成员函数

|成员函数名|功能描述|
|------|------|
|v.add(value);或<br> v+= value;或<br>v +=v1, v2, ...,vN;|在vector尾部追加value|
|v.clear();|删除所有元素|
|v[i]或v.get(i)|返回给定索引i处的值|
|v.insert(i, value);|在给定索引i前插入给定值value,同时后面的值向右移动|
|v.isEmpty();|若vector中没有元素，则返回true，否则返回false|
|v.remove(i);|删除/返回给定索引i处的值，同时后面的值向左移动|
|v[i] = value; 或<br>v.set(i, value);|替换给定索引i处的值|
|v.subList(start, length)|返回索引start开始的长度为length的新vector的值|
|v.size()|返回vector中元素的个数|
|v.toString()|返回vector的字符串表示，像"{3, 42, -7, 15}"|
|ostr << v|打印v到给定输出流(例如，cout << v)|

## 遍历vector

```cpp
vector<string> names{"Ki", "An", "Su"}

for (int i = 0; i < names.size(); i++)
{
    cout << names[i] << endl;       // 输出为: Ki An Su
}

for (int i = names.size() - 1; i >= 0; i--)
{
    cout << names[i] << endl;       // 输出为: Su An Ki
}

for (string name : names)
{
    cout << name << endl;           // 输出为: Ki An Su
}
```

## vector插入/删除

对应如下 **插入操作**

```cpp
v.insert(2, 42);
```

插入操作过程示意图如下所示

![vector-insert-operation](/images/cs106b/vector-insert-operation.png)

对应如下 **删除操作**

```cpp
v.remove(1);
```

表示删除索引为1位置对应的元素

删除操作过程示意图如下所示

![vector-remove-operation](/images/cs106b/vector-remove-operation.png)

# Grid

Grid数据结构，类似于二维数组，但是功能更强大

适合用于棋类游戏，矩阵，图像，城市地图等等。

在定义Grid类型变量时，必须通过<>指定元素类型，如下

```cpp
// 构造一个Grid对象
Grid<int> matrix(3, 4);
matrix[0][0] = 75;
...

// 使用{}指定Grid对象元素
Grid<int> matrix = {
    {75, 61, 83, 71},
    {95, 86, 98, 66},
    {56, 89, 58, 78}
};
```

## Grid成员函数

|成员函数名|功能描述|
|------|------|
|Grid<type\> name(r,c); <br> Grid<type\> name; |创建一个给定行数和列数的grid<br> 若不带参数，则创建一个空的0x0的grid|
|g[r][c]或g.get(r,c)|返回给定行列的值|
|g.fill(value);|设置每个单元保存给定的值|
|g.inBounds(r, c);|若给定的行列在grid中，则返回true|
|g.numCols() 或 g.width()| 返回列数|
|g.numRows() 或 g.height()| 返回行数|
|g.resize(nRows,nCols); |调整grid到新的大小，丢弃原来的内容|
|g[r][c] =value; 或<br> g.set(r,c,value);|存储值到给定的行列位置处|
|g.toString()|返回grid的字符串表示<br>例如:"\{\{3, 42}, {-7, 1}, {5, 19}}"|
|ostr<<g|打印grid, 例如："\{\{3, 42}, {-7, 1}, {5, 19}}"|

## Grid的遍历

有如下几种方式

**行优先遍历**

```cpp
for (int r = 0; r < grid.numRows(); r++)
{
    for (int c = 0; c < grid.numCols(); c++)
    {
        do something with grid[r][c];
    }
}
```

**for-each"方式遍历**

```cpp
for (int value : grid)
{
    do something with value;
}
```

**列优先遍历**

```cpp
for (int c = 0; c < grid.numCols(); c++)
{
    for (int r = 0; r < grid.numRows(); r++)
    {
        do something with grid[r][c];
    }
}
```

## Grid作为函数参数

当Grid类型变量按传值方式作为函数入参时，C++将对Grid类型变量进行一次内容拷贝。然后拷贝操作是比较慢的，影响性能，

因此，对于Grid类型变量，建议使用引用调用方式进行函数的参数传递。

**注**：当希望函数中不对引用传入的参数进行修改时，应该在定义的函数参数前加上 **const** 关键字

如下

```cpp
int computeSum(const Grid<int>& g)
{
    do something;
}
```
