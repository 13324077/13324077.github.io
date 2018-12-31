操作系统开发环境搭建

模拟器: QEMU
编译工具: gcc, gas, ld 和 make

代码分析工具：

- windows下: source insight

- Linux下: vi + ctags + cscope

- 跨平台工具: understand(收费)

代码版本管理:

- git

- svn

调试工具:

- gdb + qemu


参考

- MIT xv6

- Harvard OS161

- Tsinghua ucore


Ubuntu 18.04（64bit）下操作系统开发环境搭建

# Makefile相关知识

**echo** 与 **@echo**的区别

- echo: 命令执行时会回显

- @echo: 命令执行时不会回显

**addpredix用法**

用法: $(addprefix <prefix>,<names...>)

功能：把前缀<predix>加到<names>中的每个单词前面

返回值：加过前缀的文件名序列



**>/dev/null 2>&1** 用法


# 参考

- [汇编写启动代码之设置栈和调用C语言](http://eric-gao.iteye.com/blog/2255822)

- [汇编调用c函数为什么要设置栈](https://www.cnblogs.com/xmphoenix/archive/2012/04/28/2475399.html)

- [利用汇编详解栈结构](https://www.cnblogs.com/wh4am1/p/6818892.html)

- [汇编--学习笔记（九）-堆栈](https://blog.csdn.net/qq_28877125/article/details/72732267)

- [C语言中程序运行时的内存分配（数据段，代码段，堆，栈）](http://blog.chinaunix.net/uid-31139844-id-5752093.html)
