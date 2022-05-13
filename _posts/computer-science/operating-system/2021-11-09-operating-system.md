---
title: "operating system（操作系统）"
create_date: 2021-11-09 08:00:00 +0800
date: 2021-11-17 08:00:00 +0800
tags: computer-science operating-system
comment: false
show_author_profile: true
show_subscribe: false
---

- OS（operating system、操作系统）
  - batch processing（批处理）
  - device drivers（设备驱动程序）
  - multitasking（多任务处理）
    - scheduling（调度）
  - virtual memory（虚拟内存）
    - memory protection（内存保护）
    - dynamic memory allocation（动态分配内存）
    - physical memory address（物理内存地址）
    - virtual memory address（虚拟内存地址）
    - MMU（memory management unit、内存管理单元）
    - segmented memory management（段式管理）
      - segment（段）
      - 虚拟内存地址
        - 段选择子
          - 段号
        - 段内偏移量
      - segment table（段表）
        - 段描述符
          - 段基地址
          - 段界限
          - 特权等级
      - memory fragmentation（内存碎片）
    - page memory management（页式管理）
      - page（页）
      - 虚拟内存地址
        - 页号
        - 页内偏移
      - page table（页表）
        - 虚拟页号
        - 物理页号
      - missing page interruption（缺页中断）
      - swap out（换出）
      - swap in（换入）
      - multi-level page table（多级页表）
      - TLB（Translation Lookaside Buffer、页表缓存）
    - segmented paged memory management（段页式管理）
  - time-sharing（分时操作系统）
  - UNIX
    - kernel（内核）
      - memory management（内存管理）
      - multitasking（多任务）
      - I/O（输入输出处理）
    - runtime library（运行库）
    - panic（恐慌）
- interrupt（中断）
  - hardware interrupt（硬件中断）
  - soft interrupt（软中断）

### 操作系统

操作系统也是程序，但它有操作硬件的特殊权限，可以运行和管理其他程序。

批处理：一次输入多个程序，当运行完一个程序，会自动运行下一个程序。

### 虚拟内存

单片机的 CPU 可以直接操作内存物理地址。

操作系统把进程所使用的内存地址隔离，让进程间互不干涉。

操作系统为进程提供虚拟内存地址和物理内存地址的映射。

#### 段式管理

程序由若干个逻辑分段组成的，如可由代码分段、数据分段、栈段、堆段组成。

虚拟地址由段选择子和段内偏移量两部分组成。段选择子保存在段寄存器。段选择子里的段号用作段表的索引。段表保存段基地址、段界限和特权等级等。

<div style="text-align: center; margin: 5px auto">
<img src="/image/computer-science/operating-system/segmented_management.drawio.png">
</div>

<div style="text-align: center; margin: 5px auto">
<img src="/image/computer-science/operating-system/segment_table.drawio.png">
</div>

虚拟地址中的段内偏移量应该位于 0 和段界限之间，如果段内偏移量是合法的，就将段基地址加上段内偏移量得到物理内存地址。

不足之处：内存碎片、内存交换的效率低。

内存碎片有两种：

- 外部碎片，多个不连续的小物理内存，导致新的程序由于内存不够无法被装载；
- 内部碎片，程序所有的数据都被装载到了物理内存，但是这个程序有部分的数据可能并不是很常使用，这会导致内存的浪费；

内存交换可以解决外部碎片的问题。可以把程序占用的内存写到硬盘上，然后再从硬盘上读回到内存。

在 Linux 中，内存交换空间也就是 Swap 空间，这块空间是从硬盘划分出来的，用于内存与硬盘的空间交换。

对于多进程的系统来说，用分段的方式，很容易产生内存碎片。内存交换又受硬盘速度限制，效率低。

#### 页式管理

分页：整个虚拟内存空间和物理内存空间都切成一个个固定尺寸大小的内存空间（页）。

在 Linux 中，每一页的大小为 4KB。分页可以让内存交换时，读写的数据少一点。

虚拟地址与物理地址之间通过页表来映射，页表存储在内存里。从页表的性质来看，保存在内存中的页表承担的职责是将虚拟地址翻译成物理地址，所以页表一定要覆盖全部虚拟地址空间。

虚拟地址分为两部分：页号和页内偏移。页号作为页表的索引，页表包含物理页每页所在物理内存的基地址，这个基地址与页内偏移的组合就形成了物理内存地址。

<div style="text-align: center; margin: 5px auto">
<img src="/image/computer-science/operating-system/page_management.drawio.png">
</div>

<div style="text-align: center; margin: 5px auto">
<img src="/image/computer-science/operating-system/page_table.drawio.png">
</div>

对于一个内存地址转换，分三个步骤：

- 把虚拟内存地址，切分成页号和偏移量；
- 根据页号，从页表里面，查询对应的物理页号；
- 直接拿物理页号，加上前面的偏移量，就得到了物理内存地址。

当进程访问的虚拟地址在页表中查不到时，系统会产生一个缺页异常。然后进入系统内核空间，分配物理内存，更新进程页表，最后再返回用户空间，恢复进程的运行。

- 换出：把最近没被使用的内存页面释放掉（暂时写在硬盘上）
- 换入：需要内存页面的时候，从硬盘加载到内存

由于内存空间都是预先划分好的，不会产生内存碎片。一次性写入磁盘的也只有少数的一个页或者几个页，不会花太多时间，内存交换的效率比较高。

分页的方式使得在加载程序的时候，不需要一次性都把程序加载到物理内存中。

#### 多级页表

简单的分页有空间上的缺陷。在 32 位的环境下，虚拟地址空间共有 4GB（2^32）。假设一个页的大小是 4KB（2^12），每个页表项 4 个字节。那么 4GB 空间就需要 100 万（2^20）个页，大概 4MB 来存储页表。每个进程都有自己的虚拟地址空间（页表）。那么，100 个进程就需要 400MB 来存储页表。

多级页表把 100 多万个页表项再分页，用一个一级页表 4KB 表示全部的虚拟地址空间，一级页表一共有 1024 个页表项。每个一级页表项对应一个二级页表，每个二级页表也有 1024 个页表项。二级页表可以在需要时进行创建。

<div style="text-align: center; margin: 5px auto">
<img src="/image/computer-science/operating-system/multi_level_page_table.drawio.png">
</div>

对于 64 位的系统，多级页表变成了四级目录，分别是：PGD（page global directory、全局页目录项）、PUD（page upper directory、上层页目录项）、PMD（page middle directory、中间页目录项）、PTE（page table entry、页表项）。

多级页表解决了空间上的问题，但是转换工序带来了时间上的开销。

页表缓存（又叫转址旁路缓存、快表等）：在 CPU 里放一块内存用来存放最常访问的页表项。

利用局部性原理，程序在一段时间内的执行，仅限于程序的一部分。

#### 段页式管理

先将程序划分为多个逻辑段，接着再把每个段划分为多个页。地址结构由段号、段内页号和页内位移三部分组成。每个程序一张段表，每个段一张页表，段表中的地址是页表的起始地址，而页表中的地址则是物理页号。

<div style="text-align: center; margin: 5px auto">
<img src="/image/computer-science/operating-system/segmented_paged_management.drawio.png">
</div>

段页式地址变换中要得到物理地址须经过三次内存访问：

- 访问段表，得到页表起始地址
- 访问页表，得到物理页号
- 将物理页号与页内位移组合，得到物理地址

可用软、硬件相结合的方法实现，虽然增加了硬件成本和系统开销，但提高了内存的利用率。

### 中断

中断是一种异步的事件处理机制，可以提高系统的并发处理能力。

#### 硬中断

硬中断：操作系统用来响应硬件设备请求的一种机制。系统收到硬件的中断请求，会打断正在执行的进程，然后调用内核中的中断处理程序来响应请求。

操作系统收到了中断请求，会打断其他进程的运行。而且，中断处理程序在响应中断时，可能还会临时关闭中断。这意味着，如果当前中断处理程序没有执行完之前，系统中其他的中断请求都无法被响应，也就说中断有可能会丢失。

#### 软中断

Linux 系统为了解决中断处理程序执行过长和中断丢失的问题，将中断过程分成了两个阶段。

- 上半阶段：直接处理硬件请求（硬中断），一般会暂时关闭中断请求。主要负责处理跟硬件紧密相关或者时间敏感的事件。特点是快速执行。
- 下半阶段：由内核触发（软中断），一般以内核线程的方式延迟处理上半部未完成的工作。特点是延迟执行。
  
上半阶段相当于，内核先应答一声，但是有什么事等到了下半阶段再说。这个时候因为应答一声这个动作很快，所以即使暂时关闭中断请求，也会很快恢复，这中间一般不会发生中断丢失的问题。这个交给内核线程处理的下半部分，就是通常意义上的软中断。

软中断最常见的例子就是网卡收包。网卡收到网络包后，会通过硬中断通知内核有新的数据到了，于是内核就会调用对应的中断处理程序来响应该事件，这个事件的处理也是会分成上半部和下半部。

上半部分只要把网卡的数据读到内存中，然后更新一下硬件寄存器的状态，比如把状态更新为表示数据已经读到内存中的状态值。接着，内核会触发一个软中断，把一些处理比较耗时且复杂的事情，交给软中断处理程序去做，也就是中断的下半部分，其主要是需要从内存中找到网络数据，再按照网络协议栈，对网络数据进行逐层解析和处理，最后把数据送给应用程序。

在 Linux 中，

- 可以通过查看 /proc/softirqs 的内容来知晓软中断的运行情况。
- 可以通过查看 /proc/interrupts 的内容来知晓硬中断的运行情况。
- 但是这些数值是系统运行以来的累计中断次数，如果有需要应该关注中断次数的变化速率。
- 可以使用 `watch -d cat /proc/softirqs` 命令观察中断次数的变化速率。

一般对于网络 I/O 比较高的 Web 服务器，NET_RX 网络接收中断的变化速率相比其他中断类型快很多。如果发现 NET_RX 网络接收中断次数的变化速率过快，就可以使用 `sar -n DEV` 查看网卡的网络包接收速率情况，然后分析是哪个网卡有大量的网络包进来。然后就可以通过 `tcpdump` 抓包，分析这些包的来源。

### 参考

- Crash Course Computer Science（计算机科学速成课）
  - [bilibili](https://www.bilibili.com/video/BV1EW411u7th)
  - [CrashCourse 字幕组](https://github.com/1c7/crash-course-computer-science-chinese)
  - [Youtube 原视频](https://www.youtube.com/playlist?list=PL8dPuuaLjXtNlUrzyH5r6jN9ulI)
- [小林coding](https://xiaolincoding.com/)
  - [图解系统](https://xiaolincoding.com/os/)