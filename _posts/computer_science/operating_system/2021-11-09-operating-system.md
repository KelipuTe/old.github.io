---
title: "operating system（操作系统）"
create_date: 2021-11-09 08:00:00 +0800
date: 2021-11-09 08:00:00 +0800
tags: operating-system
comment: false
show_author_profile: true
show_subscribe: false
---

### 操作系统

operating system（OS、操作系统）也是程序，但它有操作硬件的特殊权限，可以运行和管理其他程序。操作系统一般是开机第一个启动的程序。

操作系统开始于 1950 年代，第一个操作系统加强了程序的加载方式。以前的计算机一次只能输入一个程序，程序运行结束，需要人工输入下一个程序。在使用操作系统之后，一次可以输入多个程序，当运行完一个程序，会自动运行下一个程序。这也叫 batch processing（批处理）。

#### 设备驱动程序

为了让程序员更容易的编写程序，操作系统充当软件和硬件之间的媒介。更具体地说操作系统提供 API 来抽象硬件，也叫（device drivers）设备驱动程序。程序员可以使用标准化机制和 input & output（I/O、输入输出）硬件交互。

#### 多任务处理

CPU 和其它设备存在速度上的差异。当单线运行一个打印程序时，CPU 调用打印机工作，在打印机工作的时间里，CPU 就闲着了。

为了最大程度利用 CPU，产生了任务 scheduling（调度）。当一个打印程序进入打印流程时，CPU 可以将这个程序 sleep(休眠)，转而去处理其他任务。当这个任务的打印流程完成后，打印机会通知 CPU 打印完成，然后 CPU 再将这个任务拉起来继续执行。

操作系统的这种能力叫 multitasking（多任务处理）。第一个多任务处理的解决方案是一个程序，叫 Atlas Supervisor 于 1962 年完成。

#### 内存分配

同时运行多个程序有个问题，每个程序都会占一些内存。当切换到另一个程序时，不能丢失数据。解决方法是：给每个程序分配专属内存块。

但是给每个程序分配的内存块的物理地址很复杂，为了隐藏这种复杂性，操作系统会把内存地址进行虚拟化，这叫 virtual memory（虚拟内存）。程序可以假定内存地址总是从 0 开始。

基于虚拟内存，操作系统还可以 dynamic memory allocation（动态分配内存）给程序。程序不需要关心到底是怎么实现的，对于程序来说，内存看起来是连续的。

每个程序使用的内存隔离开来的另一个好处是，每个程序只能在自己的内存块中操作，不会影响别的程序的内存块，这叫 memory protection（内存保护）。

1950 年代后期，曼彻斯特大学开始研发世界上第一台超级计算机 Atlas。它是第一台支持虚拟内存和内存保护的计算机和操作系统。

#### 分时操作系统

time-sharing（分时操作系统）不但要处理多个程序，还要处理多个用户。每个用户只能用小部分处理器、内存等。早期分时操作系统中，最有影响力的是 1969 年发布的 multics（多任务信息与计算系统）。multics 是第一个从设计时就考虑到安全的操作系统。

#### UNIX

Dennis Ritchie 和 Ken Thompson 联手打造了 UNIX 操作系统于 1971 年发布。

UNIX 将操作系统分成两部分：第一部分是操作系统核心功能也叫 kernel（内核），如：memory management（内存管理）、multitasking（多任务）、I/O（输入输出处理）。第二部分是一堆有用的工具（运行库），但它们不是内核的一部分。

在 UNIX 中，当有错误发生时，内核就会 panic（恐慌），机器就会崩溃。

### 参考

- Crash Course Computer Science（计算机科学速成课）
- [bilibili](https://www.bilibili.com/video/BV1EW411u7th)
- [CrashCourse 字幕组](https://github.com/1c7/crash-course-computer-science-chinese)
- [Youtube 原视频](https://www.youtube.com/playlist?list=PL8dPuuaLjXtNlUrzyH5r6jN9ulI)