---
layout: post
title: C++中stack
date: 2018-07-04 21:47:10 +08:00
category:
    - C++
keywords: C++
tags:
    - C++
---

**主要内容**：

- 算法分析方法 —— 大O分析

- stack的用法


# 算法分析

使用大O分析方法来对算法的效率进行分析。

对于一个问题来说，有很多种的解决方法，那么哪一种是最好的呢？

因此需要一个手段来度量算法的效率， 例如:

- 实现程序需要的资源（时间？内存？等等）

此处我们关注的是算法的运行时间，因此一个算法运行所需的时间越少，则该算法越好。

然后，一个算法的运行时间依赖于很多的因素，像：

- 使用的计算机的配置

- 是否有其他程序和当前程序一起在运行，

- 等等...

此处我们使用大0分析算法来度量算法的效率。

我们假设每个代码语句花费的时间是相同的，因此我们统计程序语句的执行次数，即可知道整个的运行时间。

因此，整个算法的运行时间依赖于算法的输入规模。如下举例说明

**例1**：

```cpp
statement1;
```

运行时间: O(1)

**例2**：

```cpp
for (int i = i; i <= N; i++)
{
    for (int j = 1; j <= N; j++)
    {
        statements;
    }
}
```

由于有两层循环，因此运行时间: $$O(N^2)$$

**例3**:

```cpp
for (int i = i; i <= N; i++)
{
    statement1;
    statement2;
    statement3;
}
```

运行时间为: O(3N)

以上程序放在的话，则整个运行时间为：$$O(N^2 + 3N + 1)$$

由于3N和1相对$$N^2$$来说，随着N的增大，$$N^2$$的增长速度相对3N和1快很多，因此3N和1可以忽略，因此整个运行时间用大O表示为: $$O(N^2)$$

# 栈(stack)

一种新的ADT，栈只允许用户对最后一个元素进行添加、访问和删除。栈具有如下特点：

- 后进先出(Last in, First Out)

- 栈操作的运行时间O(1)

## 栈的主要操作

|成员函数名|运行时间|功能描述|
|------|------|------|
|push(value);|O(1)|添加一个元素到栈顶|
|pop();|O(1)|从栈顶删除一个元素，并返回该元素|
|peek();|O(1)|返回栈顶元素(注: 不从栈顶删除该元素)|
|isEmpty();|O(1)|若栈中没有元素，则返回true|
|size();|O(1)|返回栈顶中元素个数|

注: 不能通过索引对栈中元素进行访问，一次只能从栈顶访问一个元素

应用栈的例子：**句子反转**

例如，

- "Hello world" => "world Hello"

- "Good" => "Good"

假设字符仅仅有字母和空格，

实现代码如下：

```cpp
void printSentenceReverse(const string& sentense)
{
    Stack<string> wordStack;
    for (char c : sentence)
    {
        if (c == SPACE)
        {
            wordStack.push(word);
            word = "";      // reset
        }
        else
        {
            word += c;
        }

    }

    if (word != "")
    {
        wordStack.push(word);
    }
    cout << "New sentense: ";
    while(!wordsStack.isEmpty())
    {
        word = wordStack.pop();
        cout << word << SPACE;
    }
    cout << endl;
}
```


