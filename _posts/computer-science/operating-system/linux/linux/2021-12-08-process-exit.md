---
title: "进程结束"
create_date: 2021-12-08 08:00:00 +0800
date: 2022-01-09 08:00:00 +0800
tags: linuc c process
comment: false
show_author_profile: true
show_subscribe: false
---

### 代码和文件

- demo_c/demo_linux_c/hello/hello.c
- demo_c/demo_linux_c/hello/strace.log
- demo_c/demo_linux_c/wait/wait.c

### 进程退出的方式

- 程序运行到最后一行代码
- 进程调用`exit()`函数退出进程
- 进程调用`exit_group()`函数
- 进程调用`_exit()`函数退出进程
- 进程调用`_Exit()`函数退出进程
- 进程接收到了中断信号

调用`abort()`函数，进程会被异常终止。

### 不同退出方式的区别

程序运行到最后1行代码、代码`return 0`、调用`exit()`函数退出进程时，会输出输出缓冲区内的内容。而调用`_exit()`函数、调用`_Exit()`函数退出进程时不会。也就是对于`printf("hello, world\r\n")`来说，前面3个方式会输出`hello, world`，后面两个不会输出。

### 退出状态码

当进程结束时，会返回1个退出状态码，一般都是0，0表示成功退出。

使用`echo $?`命令可以打印上一个程序的退出状态码。

退出状态码最大1个字节（也就是最大只能是255），大于1个字节会被处理（值和255逻辑与）。比如返回300，最后会被处理成44。

```
[pid   204] [00007f6e21067da9] exit_group(300) = ?
[pid   204] [????????????????] +++ exited with 44 +++
```

子进程退出时会向父进程发送SIGCHLD中断信号，同时父进程调用`wait4()`函数也会返回并且得到退出的相关信息。

```
[00007f67fa7f7630] --- SIGCHLD {si_signo=SIGCHLD, si_code=CLD_EXITED, si_pid=204, si_uid=0, si_status=44, si_utime=0, si_stime=0} ---
[00007f67fa88646c] wait4(-1, 0x7ffdc7754a50, WNOHANG|WSTOPPED|WCONTINUED, NULL) = -1 ECHILD (No child processes)
```

### 进程回收

当子进程调用`exit(0)`退出，但是父进程没有运行结束而且没有回收子进程的时候，子进程会变成僵尸进程。当父进程也结束的时候，操作系统会回收僵尸进程和父进程。

当进程变成僵尸进程时，它的内存数据还驻留在内存中，同时在`/proc`目录还有相关文件没有移除掉，还在占用系统资源。如果僵尸进程过多，会导致系统资源紧张，会影响操作系统的运行。所以我们必须要回收退出的子进程。

可以使用`wait()`函数或`waitpid()`函数回收子进程。`wait()`函数和`waitpid()`函数会阻塞父进程，直到有子进程退出。

#### 示例

- `demo_c/demo_linux_c/wait/wait.c`，通过`wait()`函数回收子进程。
- `demo_c/demo_linux_c/waitpid/waitpid_status.c`，通过`waitpid()`函数回收正常退出的子进程。
- `demo_c/demo_linux_c/waitpid/waitpid_signal.c`，通过`waitpid()`函数回收被信号终止的子进程。
- `demo_c/demo_linux_c/waitpid/waitpid_signal_handler.c`，信号处理函数。

程序运行后，在另外一个终端上，通过`kill -s SIGUSR1 {pid}`命令向子进程发送SIGUSR1信号。可以通过`kill -l`命令查看所有的信号。

测试信号处理函数时，当测试的是信号是SIGSTOP时，并不会像预期的那样输出SIGSTOP信号的值。因为进程收到SIGSTOP后已经停止作业（并没有退出）。如果这时再向进程发送SIGCONT信号。这时进程会恢复作业，并输出`signal no=18`，也就是收到的SIGCONT信号的值。

通过strace命令检测，可以得到下面的内容。进程确实收到了SIGSTOP信号和SIGCONT信号。

```
--- SIGSTOP {si_signo=SIGSTOP, si_code=SI_USER, si_pid=84, si_uid=0} ---
--- stopped by SIGSTOP ---
--- SIGCONT {si_signo=SIGCONT, si_code=SI_USER, si_pid=84, si_uid=0} ---
write(1, "signal no=18\r\n", 14)        = 14
```