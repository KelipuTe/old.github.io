---
title: "进程控制"
create_date: 2021-12-08 08:00:00 +0800
date: 2022-01-09 08:00:00 +0800
tags: linuc c process
comment: false
show_author_profile: true
show_subscribe: false
---

### 代码和文件

- demo_c/demo_linux_c/fork/fork.c
- demo_c/demo_linux_c/execv/execv.c
- demo_c/demo_linux_c/execv/call.c
- demo_c/demo_linux_c/nice/nice.c

### fork-and-exec

### fork

#### fork()函数

fork()函数可以创建一个进程（子进程）。fork()函数通过复制调用进程创建一个新进程，调用进程作为父进程，新进程作为子进程。

两个进程的内存地址有同样的内容（程序数据和程序指令），父进程和子进程代码是一样的，子进程会从fork()函数的下一行代码继续执行。一般父进程先执行，除非被阻塞。

但是两个进程运行在不同的内存空间里面，进程间是隔离的。两个进程执行内存写操作（定义新的变量并赋值，修改变量的值，定义新的函数）或者文件映射（进程间通信）时互不影响。

#### 父进程和子进程的区别

- 子进程有自己独立的唯一的进程标识（pid）
- 子进程的父进程id和父进程的pid是一样的
- 子进程不会继承父进程的内存锁

#### fork()函数的返回值

新进程创建成功时，父进程拿到子进程的pid，子进程拿到0。可以根据返回值判断那个是父进程。

新进程创建失败时，父进程拿到-1，子进程不会被创建。errno变量会存储错误信息。

#### copy-on-write（写时复制）

在fork()函数执行之后exec()函数执行之前，两个进程用的是相同的物理空间，子进程的代码段、数据段、堆栈都是指向父进程的物理空间。

也就是说，两者的虚拟空间不同，但其对应的物理空间是同一个。当父子进程中有更改相应段的行为发生时，再为子进程相应的段分配物理空间。

如果不是因为exec()函数，内核会给子进程的数据段、堆栈段分配相应的物理空间（至此两者有各自的进程空间，互不影响）。

而代码段继续共享父进程的物理空间（两者的代码完全相同）。而如果是因为exec()函数，由于两者执行的代码不同，子进程的代码段也会分配单独的物理空间。

#### 进程标识pid和ppid

getpid()函数返回调用进程的pid，getppid()函数返回调用进程的父进程pid。

在编码测试时需要注意，必须先让子进程先执行，父进程后执行，打印出来的ppid才是正确的。

如果父进程在子进程执行前先跑完了，子进程的ppid就会变成1，这时子进程就变成孤儿进程。1就是系统进程，被1号进程接管有可能就变成后台进程了。

#### vfork()函数

vfork()函数创建子进程并阻塞父进程，直到子进程退出。而且vfork()函数和fork()函数不同，vfork()函数创建出来的子进程和父进程共享内存。

vfork()函数有bug，当代码使用`return 0`结束或者执行到最后1行结束时，有可能会报`Segmentation fault (core dumped)`错误。但是使用exit(0)函数或者_exit(0)函数的时候不会。

通过`strace`命令追踪可以发现报错时，子进程调用exit_group(0)函数退出，但是父进程没有调用exit_group(0)函数。不报错时，两个进程都调用exit_group(0)函数退出。

### exec

#### execve()函数

在程序里可以使用的和execve()函数作用相同的函数有好几个，这里用execv()函数举例。代码详见`demo_c/demo_linux_c/execv/execv.c`和`demo_c/demo_linux_c/execv/call.c`。

#### 进程的执行顺序

进程的执行（调度）顺序受到PRI（priority）值和NI（nice）值控制，这两个值越小，进程优先级越高。这两个值可以通过`ps -ely`命令查看（PRI和NI参数），也可以通过`top`命令查看（PR和NI参数）。

可以使用`nice`命令和`renice`命令调整进程的优先级。nice的值的范围是-20~19。`nice`命令用于进程启动之前（`nice - run a program with modified scheduling priority`）。`renice`命令用于进程启动之后（`renice - alter priority of running processes`）。

在代码中，getpriority()函数可以查看进程优先级，setpriority()函数和nice()函数可以调整进程优先级。详见linux文档`getpriority(2)`、`setpriority(2)`、`nice(2)`。

这里需要注意的是，getpriority()函数和setpriority()函数的which参数选什么，who就要对应的填什么。代码详见`demo_c/demo_linux_c/nice/nice.c`。