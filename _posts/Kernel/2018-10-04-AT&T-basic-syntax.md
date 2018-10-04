---
layout: post
title: AT&T基本汇编语法
date: 2018-10-04 21:32:30 +08:00
category:
    - Linux内核
keywords: AT&T
tags:
    - Assembly
---

# 基本AT&T汇编语法

```asm
* 寄存器命名原则
AT&T: %eax Intel: eax
* 源/目的操作数顺序
AT&T: movl %eax, %ebx Intel: mov ebx, eax
* 常数/立即数的格式
AT&T: movl $_value, %ebx Intel: mov eax, _value
把value的地址放入eax寄存器
AT&T: movl $0xd00d, %ebx Intel: mov ebx, 0xd00d
* 操作数长度标识
AT&T: movw %ax, %bx Intel: mov bx, ax
* 寻址方式
AT&T: immed32(basepointer, indexpointer, indexscale)
Intel: [basepointer + indexpointer × indexscale + imm32)
```
# 基本内联汇编语法

```
asm("statements");
```

例如

```
asm("nop"); asm("cli");
```



# 扩展内联汇编语法

```
#define read_cr0() ({ \
unsigned int __dummy; \
__asm__( \
"movl %%cr0,%0\n\t" \
:"=r" (__dummy)); \
__dummy; \
})
```
