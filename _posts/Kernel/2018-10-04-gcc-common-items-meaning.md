---
layout: post
title: gcc编译器常用选项的含义
date: 2018-10-04 19:32:30 +08:00
category:
    - Linux内核
keywords: gcc
tags:
    - gcc
---

# -w,-wall和-W的用法

**-w**： 关闭编译时的警告， 也就是编译后不显示任何warning，因此有时编译中会出现一些诸如数据转换之类的可忽略警告，

**-Wall**: 显示编译后所有警告

**-W**: 显示警告，但是只是显示编译器认为的会出现错误的警告。

举例说明

对于如下程序

```c
#include <stdio.h>

void main()
{
    int a = 10;
    return 0;
}
```

**情况1**: 直接编译，使用如下命令

> gcc -o test test.c

结果只有一个警告

```
test.c: In function ‘main’:
test.c:7:2: warning: ‘return’ with a value, in function returning void [enabled by default]
  return 0;
```

**情况2**: 使用-w选项编译，使用如下命令

> gcc -w -o test test.c

结果没有任何的警告

**情况3**: 使用-Wall选项编译，使用如下命令

> gcc -Wall -o test test.c

结果显示所有的警告

```
test.c:3:6: warning: return type of ‘main’ is not ‘int’ [-Wmain]
 void main()
      ^
test.c: In function ‘main’:
test.c:7:2: warning: ‘return’ with a value, in function returning void [enabled by default]
  return 0;
  ^
test.c:5:6: warning: unused variable ‘a’ [-Wunused-variable]
  int a = 10;
      ^
```

**情况4**: 使用-W选项编译，使用如下命令

> gcc -W -o test test.c

只显示了一个编译器认为会出错的警告

```
test.c: In function ‘main’:
test.c:7:2: warning: ‘return’ with a value, in function returning void [enabled by default]
  return 0;
```

# -I的用法

-I选项用于gcc编译器到指定的目录下查找头文件。

以下通过例子进行说明

**情况1**

例如，当前目录下有main.c add.c add.h三个文件

main.c的内容如下

```c
#include <stdio.h>
#include "add.h"

int main()
{
	int a = 1;
	int b = 2;
	int c = add(a, b);

	printf("%d + %d is: %d\n", a, b, c);

	return 0;
}
```

add.c的内容如下

```c
int add(int x, int y)
{
	return x + y;
}
```

add.h的内容如下

```c
extern int add(int x, int y);
```

此时，在当前目录使用gcc命令进行编译

> gcc -o main main.c add.

生成可执行文件main, 然后执行

> ./main

运行结果如下，

```
1 + 2 is: 3
```

没有任何问题, 此处gcc会在

- 当前目录

- /usr/include目录

- /usr/local/include目录

下查找 **add.h** 文件，由于add.h文件在当前目录下， 所以一切正常。

**情况2**

在当前目录下新建一个inc文件夹，然后把add.h移到inc文件夹中，重新编译，会出现如下找不到add.h头文件的问题

```
wxer-> ls
add.c  inc  main.c

wxer-> gcc -o main main.c add.c
main.c:2:17: fatal error: add.h: No such file or directory
 #include "add.h"
                 ^
compilation terminated.
```

此时要通过-I选项来指定头文件所在目录，通过如下命令进行编译，则没有问题

> gcc -Iinc/ -o main main.c add.c

或者

> gcc -I inc/ -o main main.c add.c


# -fno-builtin与-fno-builtin-function用法

**-fno-builtin** 用于解决当用户自定义的函数与C语言的内建函数冲突的问题。

当用户自定义的函数与内建函数冲突时，若在gcc的编译选项中加上-fno-builtin时，则表示 **不使用C语言的内建函数**

对于如下场景，有些函数不想用内建函数，而其他的某些函数还是希望使用内建函数时，那么久需要使用 **-fno-builtin-function** 选项，其中的function就是冲突的函数名。

# -m32与-m64选项用法

**-m32**：表示生成32位机器汇编代码

**-m64**：表示生成64位机器汇编代码

# -g，-ggdb，-g3与-ggdb3选项用法

-g和-ggdb几乎相同，仅仅有细微的区别。

**-g** 产生的debug信息是OS native format的(stabs, COFF, XCOFF, or DWARF 2)， GDB可以使用之。

**-ggdb** 产生的debug信息更倾向于给GDB使用的。

所以，如果你用的GDB调试器，那么使用-ggdb选项。如果是其他调试器，则使用-g。

3只是级别。这个级别会产生更多的额外debug信息。3这个级别可以调试宏。

# -nostdinc用法

该选项表示编译器不再系统缺省的头文档目录里面找头文档,一般和-I联合使用,明确限定头文档的位置。

# -fno-stack-protector用法

该选项表示禁用堆栈保护。



# 参考

1. [gcc中的-w -W和-Wall选项](https://blog.csdn.net/m7548352/article/details/49520069)

2. [GCC编译器中的-I -L -l 选项](https://blog.csdn.net/qq_32790673/article/details/52873496)

3. [What is the difference between gcc -ggdb and gcc -g](https://stackoverflow.com/questions/668962/what-is-the-difference-between-gcc-ggdb-and-gcc-g)

4. [GCC 中的编译器堆栈保护技术](https://www.cnblogs.com/napoleon_liu/archive/2011/02/14/1953983.html)
