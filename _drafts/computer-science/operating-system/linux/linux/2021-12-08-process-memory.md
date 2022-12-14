---
title: "计算机科学--操作系统--Linux--进程内存"
create_date: 2021-12-08 08:00:00 +0800
date: 2022-01-09 08:00:00 +0800
tags: computer-science operating-system linux process
comment: false
show_author_profile: true
show_subscribe: false
---

进程通过execve()函数将可执行文件的`.text`（程序指令）和`.data`（程序数据）加载到内存中去执行，此时操作系统会它分配相应的内存资源。

### 代码和文件

- demo_c/demo_linux_c/hello/hello
- demo_c/demo_linux_c/rlimit/rlimit.c

### linux进程内存布局

- 内核空间（kernel space）
- 栈（stack）
- 动态库的映射
- 堆（heap）
- 读写数据区，主要是程序数据，`.data`、`.bss` 等
- 只读数据区，主要是程序指令，`.text`、`.init`、`.rodata` 等
- 保留区

### 动态库

动态库又称为共享库，按elf格式存储，存储的内容主要是程序指令和程序数据。

动态库的使用方式：引入头文件（声明和定义）；加载这个库文件（静态库，动态库，共享库）。

可以通过ldd命令列出一个程序所需要得动态链接库。这里用之前的hello文件：`ldd hello`。

```
linux-vdso.so.1 =>  (0x00007ffeb1fcb000)
libc.so.6 => /lib64/libc.so.6 (0x00007f3a77af6000)
/lib64/ld-linux-x86-64.so.2 (0x00007f3a77ec4000)
```

### 进程的内存数据

当程序被execve()函数加载到内存中时，操作系统会给它分配内存资源，并找到程序入口开始执行。主进程默认会启动一个主线程去执行main()入口函数。

分配的内存资源主要用于存储程序指令和程序数据。还有额外的进程内存数据、进程标识、进程状态、哪个用户启动的、打开的文件等。

这些数据主要存储在系统的`/proc`目录中，`/proc`目录存储了操作系统上所有的进程的内存数据。进程对应的目录就用进程号命名，比如进程42对应的目录就是`/proc/42`。

详细的说明可以查看linux文档`proc(5)`，下面是几个常用的文件：

- cmdline文件，记录进程是用什么命令启动的，僵尸进程这个文件是空的。
- environ文件，记录进程的环境参数。
- exe软连接，进程是哪个可执行文件启动的。
- fd目录，记录进程打开的文件目录。
- limits文件，记录进程的资源限制。
- net目录，记录进程和网络有关的数据，比如socket。
- stat文件，记录进程状态数据、线程数据、信号数据等。
- status文件，和stat文件一样，不过可读性更强。

### 进程的资源限制

资源限制有硬限制和软限制，软限制必须小于等于硬限制。比如进程可以打开的文件个数，这个参数很重要，因为linux上一切皆文件。

在代码中，有getrlimit()函数可以查看资源限制，有setrlimit()函数可以调整资源限制。代码详见`demo_c/demo_linux_c/rlimit/rlimit.c`。

在生产环境优先在程序中动态修改资源限制，不要轻易修改操作系统的资源限制。