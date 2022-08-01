---
title: "Linux memory（Linux 内存）"
date: 2022-05-12 08:00:00 +0800
tags: computer-science 计算机科学 operating-system 操作系统 memory 内存
comment: false
show_author_profile: true
show_subscribe: false
---

### readme（说明）

笔记中涉及的图片在 draw.io 文件 `git.io/drawio/computer-science/memory/linux-memory.drawio` 中。或者看 draw.io 文件生成的 HTML 文件，[linux-memory.html](/image/computer-science/memory/linux-memory.drawio.html)，渲染需要等一会。

### 页式管理

Linux 内存主要采用的是页式内存管理，但是由于 Intel 处理器的发展史，Linux 无法避免分段管理。 因为操作系统必须按照硬件结构设计，所以 Linux 的内核必须服从 CPU 的硬件结构。

### 虚拟地址空间

在 Linux 中，虚拟地址空间被分为内核空间和用户空间。

- 32 位系统，内核空间占用 1G，位于最高处，剩下的 3G 是用户空间。
- 64 位系统，内核空间和用户空间都是 128T，分别占据整个内存空间的最高和最低处，剩下的中间部分是未定义的。

- 图片：linux-memory.drawio/1-1、32 位空间地址
- 图片：linux-memory.drawio/1-2、64 位空间地址

进程在用户态时，只能访问用户空间内存。只有进入内核态后，才可以访问内核空间的内存。虽然每个进程都各自有独立的虚拟内存，但是虚拟内存中的内核地址，关联的是相同的物理内存。这样，进程切换到内核态后，就可以很方便地访问内核空间内存。

### 用户空间分布

32 位系统的用户空间分布的情况：

- 图片：linux-memory.drawio/2-1、32 位系统的用户空间分布情况

- 程序文件段（.text），包括二进制可执行代码。
- 已初始化数据段（.data），包括静态常量。
- 未初始化数据段（.bss），包括未初始化的静态变量。
- 堆段，包括动态分配的内存，从低地址开始向上增长。
- 文件映射段，包括动态库、共享内存等，从低地址开始向上增长。
- 栈段，包括局部变量和函数调用的上下文等。栈的大小是固定的，一般是 8 MB。当然系统也提供了参数，以便自定义大小.

在这 6 个内存段中，堆和文件映射段的内存是动态分配的。比如，使用 C 标准库的 malloc() 或者 mmap()，就可以分别在堆和文件映射段动态分配内存。

### malloc

`malloc()` 不是系统调用，而是 C 库里的函数，用于动态分配内存。

malloc 申请内存的时候，会有两种方式向操作系统申请堆内存:

- 通过 `brk()` 系统调用从堆分配内存，通过 brk 将堆顶指针向高地址移动，获得新的内存空间。
- 通过 `mmap()` 系统调用在文件映射区域分配内存，也就是从文件映射区拿一块内存。

- 图片：linux-memory.drawio/3-1、brk() 申请内存
- 图片：linux-memory.drawio/3-2、mmap() 申请内存

malloc 源码默认定义了一个阈值：如果用户分配的内存小于 128 KB，则使用 brk；如果用户分配的内存大于 128 KB，则使用 mmap。

分类内存采用两种方式的原因有两个：1、避免堆内存碎片；2、避免频繁的进行系统调用。

malloc 分配的是虚拟内存。如果分配后的虚拟内存没有被访问的话，是不会将虚拟内存不会映射到物理内存，这样就不会占用物理内存了。只有在访问已分配的虚拟地址空间的时候，操作系统通过查找页表，发现虚拟内存对应的页没有在物理内存中，就会触发缺页中断，然后操作系统会建立虚拟内存和物理内存之间的映射关系。

### 堆内存碎片

假设，先 malloc 2+2+2 K 的内存，然后 free 2+2 K。因为通过 brk 从堆空间分配的内存，并不会归还给操作系统。所以，这时 malloc 内存池就有 4K 的空闲。

如果这个时候 malloc 小于 4K ，就可以直接从内存池分配，如果 malloc 大于 4K，就必须再从堆上申请。如果程序后来的 malloc 都大于 4K ，那么这个空闲的 4K 就变成了无法使用到的内存碎片。

- 图片：linux-memory.drawio/3-2、mmap() 申请内存

生产环境的程序通常会长时间运行，所以这样的碎片有可能会越积越多，尤其是如果频繁的 malloc 和 free 小块内存。malloc 在分配内存的时候，并不是按用户预期申请的字节数来分配内存空间大小，而是会预分配更大的空间作为内存池。具体会预分配多大的空间，跟 malloc 使用的内存管理器有关系。

### 内存池

brk 和 mmap 都是系统调用。执行系统调用是要进入内核态的，然后在回到用户态，运行态的切换会耗费不少时间。如果都用 mmap 来分配内存，等于每次都要执行系统调用。另外，因为 mmap 分配的内存每次释放的时候，都会归还给操作系统，于是每次 mmap 分配的虚拟地址都是缺页状态的，然后在第一次访问该虚拟地址的时候，就会触发缺页中断。频繁通过 mmap 分配的内存话，不仅每次都会发生运行态的切换，还会发生缺页中断（在第一次访问虚拟地址后），这样会导致 CPU 消耗较大。

为了改进这两个问题，malloc 通过 brk 系统调用在堆空间申请内存的时候，由于堆空间是连续的，所以直接预分配更大的内存来作为内存池，当内存释放的时候，就缓存在内存池中。等下次在申请内存的时候，就直接从内存池取出对应的内存块就行了，而且可能这个内存块的虚拟地址与物理地址的映射关系还存在，这样不仅减少了系统调用的次数，也减少了缺页中断的次数，这将大大降低 CPU 的消耗。

### `/proc/{pid}/maps`

程序运行后可以通过 `/proc/{pid}/maps` 文件查看进程的内存分布情况。

```
02dc7000-02de8000 rw-p 00000000 00:00 0                                  [heap]
```

`02de8000 - 02dc7000 = 21000`，也就是在 heap（堆上）分配了 21000 字节。如果 malloc 是通过 mmap 分配的，右边显示 `[heap]` 的那个位置就啥都没有。

需要注意的是，程序中返回的地址应该是 `02dc7010` 而不是 `02dc7000`。前面的 16 字节（0x10）是内存块的头信息。内存块头信息在 《Linux\Unix 系统编程手册》 第 7 章里有提到。

- 图片：linux-memory.drawio/5-1、内存块头信息

### free

如果 malloc 通过 brk 方式申请的内存，free 内存后，堆内存还存在。这是因为内存会被放入 malloc 的内存池里，当进程再次申请内存时就可以直接复用。当进程退出后，操作系统就会回收进程的所有资源。

如果 malloc 通过 mmap 方式申请的内存，free 释放内存后就会立刻归还给操作系统。也就是，如果是通过 brk 方式申请的内存，free 后，依然可以在 `/proc/{pid}/maps` 文件里看到，因为内存还没有被归还给操作系统。如果是通过 mmap 方式申请的内存，free 后，就看不到了。

free 函数使用的时候只传入一个内存地址，没有传入内存大小。内存大小被存在了这个指针指向的内存块的内存块的头信息里。

### reference（参考）

- Crash Course Computer Science（计算机科学速成课）
  - [bilibili](https://www.bilibili.com/video/BV1EW411u7th)
  - [CrashCourse 字幕组](https://github.com/1c7/crash-course-computer-science-chinese)
  - [Youtube 原视频](https://www.youtube.com/playlist?list=PL8dPuuaLjXtNlUrzyH5r6jN9ulI)
- [小林coding](https://xiaolincoding.com/)
  - [图解系统](https://xiaolincoding.com/os/)
- [Linux 文档](https://man7.org/linux/man-pages/index.html)
  - [malloc(3) - allocate and free dynamic memory](https://man7.org/linux/man-pages/man3/malloc.3.html)
  - [proc(5) - process information pseudo-filesystem](https://man7.org/linux/man-pages/man5/proc.5.html)
- 《Linux\Unix 系统编程手册》 第 7 章
