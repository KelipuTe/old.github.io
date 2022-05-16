---
title: "goroutine"
create_date: 2022-05-16 08:00:00 +0800
date: 2022-05-16 08:00:00 +0800
tags: computer-science software programming-language golang
comment: false
show_author_profile: true
show_subscribe: false
---

### 并行和并发

- 并行：多个程序在多个核心同时跑
- 并发：多个程序看上去在同时跑

### 进程

操作系统会为应用程序创建一个进程。进程会包含应用程序运行的资源，如：线程、内存、文件描述符等。

一个进程从主线程开始，当该线程终止时，进程终止。线程可以启动更多的线程。

线程是操作系统调度的一种执行路径，用于在处理器执行我们在函数中编写的代码。

操作系统调度线程在可用处理器上运行，Go 运行时调度 goroutine 在绑定到单个操作系统线程的逻辑处理器（P）中运行。

### goroutine

使用 goroutine 的两个注意点：

- 什么时候结束
- 有没有强制结束的手段

如果函数启动 goroutine，则必须向调用方提供显式停止该 goroutine 的方法。

并发环境下，要保证只有写的一方可以关闭 chan。

### 串行化

- sync
  - Mutex
  - RWMutex
- sync/atomic

### happen-before

编译器和 CPU 在不改变 goroutine 的行为前提下，为了更高的性能可能修改读和写的执行顺序。由于存在重排，`a=1`、`b=2` 两条语句，在两个 goroutine 中可能会看到不同的执行顺序。

并发不加锁的场景下，`i++` 都不一定能得到争取的结果，`i++` 最终在执行的时候会变成 3 条汇编指令。并发不加锁时，多个 CPU 核心的 CPU 高速缓存和内存会出现类似缓存不一致的问题。

### machine word

对于 single machine word 的变量的读写操作应该是原子的。比如 64 位的机器，把 int 指针从 int a 指向 int b 这个操作应该是原子的。如果是把 20 字节的字符串从 a 赋值给 b，因为一次最多处理 64 位的数据，所以这个赋值操作可能存在中间状态。
