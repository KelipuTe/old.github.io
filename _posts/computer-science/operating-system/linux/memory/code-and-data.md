---
title: "code and data memory layout（代码和数据的内存布局）"
date: 2022-07-04 08:00:00 +0800
tags: computer-science operating-system linux memory
comment: false
show_author_profile: true
show_subscribe: false
---

> 操作系统架构：x86_64 AMD Ryzen
> 操作系统版本：Ubuntu 2204

> 样例代码 <br/>
> demo-c/demo-in-linux/memory-layout/code_and_data.c

### 内存布局

内存的存储单元会根据 CPU 位数宽度以 16 进制从 0 开始顺序编号。每个存储单元只对应一个编号，且只可以存储一个 byte 的数据。

Linux 进程的虚拟内存会对内存空间进行划分，不同的区域存储不同的数据，有不同的权限。

一般来说：可执行的函数代码存储在 code 区、全局变量存储在 data 区，局部变量存储在 stack 区（栈区），动态申请的内存存储在 heap 区（堆区）。

可执行文件运行的时候，代码和数据会被读取到内存中。

### 起始地址（首地址、基地址、基址）

起始地址指的是函数代码或者变量所占内存空间的第一个存储单元的地址。

### 函数代码

函数代码的起始地址：

```
(gdb) p &func1
$5 = (int (*)(int)) 0x555555555129 <func1>
(gdb) p &func2
$6 = (int (*)(int)) 0x55555555513c <func2>
(gdb) p &main
$7 = (int (*)()) 0x555555555151 <main>
```

函数代码的起始地址在编译时就已经确定了。

和符号表对照：

```
> readelf -s code_and_data

    18: 0000000000001129    19 FUNC    GLOBAL DEFAULT   14 func1
    29: 000000000000113c    21 FUNC    GLOBAL DEFAULT   14 func2
    34: 0000000000001151    53 FUNC    GLOBAL DEFAULT   14 main
```

起始地址也占了一个位置，所以计算结束地址的时候不是直接加函数大小。

- func1 函数大小 19 个 byte，起始地址为 0x0000000000001129，结束地址为 0x000000000000113b = 起始地址（16） + 18（10）
- func2 函数大小 21 个 byte，起始地址为 0x000000000000113c，结束地址为 0x0000000000001150 = 起始地址（16） + 20（10）
- main 函数大小 53 个 byte，起始地址为 0x0000000000001151，结束地址为 0x0000000000001185 = 起始地址（16） + 52（10）

内存空间上存储的数据：

```
(gdb) x/19xb &func1
0x555555555129 <func1>:	0xf3	0x0f	0x1e	0xfa	0x55	0x48	0x89	0xe5
0x555555555131 <func1+8>:	0x89	0x7d	0xfc	0x8b	0x45	0xfc	0x83	0xc0
0x555555555139 <func1+16>:	0x10	0x5d	0xc3

(gdb) x/21xb &func2
0x55555555513c <func2>:	0xf3	0x0f	0x1e	0xfa	0x55	0x48	0x89	0xe5
0x555555555144 <func2+8>:	0x89	0x7d	0xfc	0x8b	0x45	0xfc	0x05	0x00
0x55555555514c <func2+16>:	0x01	0x00	0x00	0x5d	0xc3

(gdb) x/53xb &main
0x555555555151 <main>:	0xf3	0x0f	0x1e	0xfa	0x55	0x48	0x89	0xe5
0x555555555159 <main+8>:	0x48	0x83	0xec	0x10	0xc7	0x45	0xf8	0x10
0x555555555161 <main+16>:	0x00	0x00	0x00	0x8b	0x45	0xf8	0x89	0xc7
0x555555555169 <main+24>:	0xe8	0xbb	0xff	0xff	0xff	0xc7	0x45	0xfc
0x555555555171 <main+32>:	0x00	0x01	0x00	0x00	0x8b	0x45	0xfc	0x89
0x555555555179 <main+40>:	0xc7	0xe8	0xbd	0xff	0xff	0xff	0xb8	0x00
0x555555555181 <main+48>:	0x00	0x00	0x00	0xc9	0xc3

(gdb) x/93xb &func1
0x555555555129 <func1>:	0xf3	0x0f	0x1e	0xfa	0x55	0x48	0x89	0xe5
0x555555555131 <func1+8>:	0x89	0x7d	0xfc	0x8b	0x45	0xfc	0x83	0xc0
0x555555555139 <func1+16>:	0x10	0x5d	0xc3	0xf3	0x0f	0x1e	0xfa	0x55
0x555555555141 <func2+5>:	0x48	0x89	0xe5	0x89	0x7d	0xfc	0x8b	0x45
0x555555555149 <func2+13>:	0xfc	0x05	0x00	0x01	0x00	0x00	0x5d	0xc3
0x555555555151 <main>:	0xf3	0x0f	0x1e	0xfa	0x55	0x48	0x89	0xe5
0x555555555159 <main+8>:	0x48	0x83	0xec	0x10	0xc7	0x45	0xf8	0x10
0x555555555161 <main+16>:	0x00	0x00	0x00	0x8b	0x45	0xf8	0x89	0xc7
0x555555555169 <main+24>:	0xe8	0xbb	0xff	0xff	0xff	0xc7	0x45	0xfc
0x555555555171 <main+32>:	0x00	0x01	0x00	0x00	0x8b	0x45	0xfc	0x89
0x555555555179 <main+40>:	0xc7	0xe8	0xbd	0xff	0xff	0xff	0xb8	0x00
0x555555555181 <main+48>:	0x00	0x00	0x00	0xc9	0xc3
```

### 变量

变量就是内存单元的助记符，指向变量所占内存单元的起始地址。 比如 int 类型的变量就由 4 个内存单元组成，起始地址就是 4 个内存单元中第一个内存单元的地址。

#### 全局变量

全局变量的起始地址：

```
(gdb) p &x
$1 = (int *) 0x555555558010 <x>
(gdb) p &y
$2 = (int *) 0x555555558014 <y>
```

全局变量的起始地址在编译时就已经确定了。

和符号表对照：

```
> readelf -s code_and_data

     3: 0000000000004010     4 OBJECT  GLOBAL DEFAULT   25 x
    34: 0000000000004014     4 OBJECT  GLOBAL DEFAULT   25 y
```

内存空间上存储的数据：

```
(gdb) x/4bx &x
0x555555558010 <x>:	0x10	0x00	0x00	0x00
(gdb) x/4bx &y
0x555555558014 <y>:	0x00	0x01	0x00	0x00
```

#### 局部变量

局部变量是分配在栈上的，每次执行的时候都不一样。

```
> ./code_and_data

&x start address=0x557ba472f010
&x end address  =0x557ba472f014
&y start address=0x557ba472f014
&y end address  =0x557ba472f018
&a start address=0x7ffd3a8bd450
&b start address=0x7ffd3a8bd454

> ./code_and_data

&x start address=0x557deac82010
&x end address  =0x557deac82014
&y start address=0x557deac82014
&y end address  =0x557deac82018
&a start address=0x7fffaa4168e0
&b start address=0x7fffaa4168e4

> ./code_and_data

&x start address=0x556862c6b010
&x end address  =0x556862c6b014
&y start address=0x556862c6b014
&y end address  =0x556862c6b018
&a start address=0x7ffe89646a90
&b start address=0x7ffe89646a94
```

可以和进程内存信息比对一下，局部变量的地址都在 stack 区（栈区）。

```
(gdb) info proc mappings
process 2810
Mapped address spaces:

          Start Addr           End Addr       Size     Offset  Perms  objfile
      0x555555554000     0x555555555000     0x1000        0x0  r--p   /mnt/hgfs/demo-c/demo-in-linux/memory-layout/code_and_data
      0x555555555000     0x555555556000     0x1000     0x1000  r-xp   /mnt/hgfs/demo-c/demo-in-linux/memory-layout/code_and_data
      0x555555556000     0x555555557000     0x1000     0x2000  r--p   /mnt/hgfs/demo-c/demo-in-linux/memory-layout/code_and_data
      0x555555557000     0x555555558000     0x1000     0x2000  r--p   /mnt/hgfs/demo-c/demo-in-linux/memory-layout/code_and_data
      0x555555558000     0x555555559000     0x1000     0x3000  rw-p   /mnt/hgfs/demo-c/demo-in-linux/memory-layout/code_and_data
      0x555555559000     0x55555557a000    0x21000        0x0  rw-p   [heap]
      0x7ffff7d81000     0x7ffff7d84000     0x3000        0x0  rw-p
      0x7ffff7d84000     0x7ffff7dac000    0x28000        0x0  r--p   /usr/lib/x86_64-linux-gnu/libc.so.6
      0x7ffff7dac000     0x7ffff7f41000   0x195000    0x28000  r-xp   /usr/lib/x86_64-linux-gnu/libc.so.6
      0x7ffff7f41000     0x7ffff7f99000    0x58000   0x1bd000  r--p   /usr/lib/x86_64-linux-gnu/libc.so.6
      0x7ffff7f99000     0x7ffff7f9d000     0x4000   0x214000  r--p   /usr/lib/x86_64-linux-gnu/libc.so.6
      0x7ffff7f9d000     0x7ffff7f9f000     0x2000   0x218000  rw-p   /usr/lib/x86_64-linux-gnu/libc.so.6
      0x7ffff7f9f000     0x7ffff7fac000     0xd000        0x0  rw-p
      0x7ffff7fbb000     0x7ffff7fbd000     0x2000        0x0  rw-p
      0x7ffff7fbd000     0x7ffff7fc1000     0x4000        0x0  r--p   [vvar]
      0x7ffff7fc1000     0x7ffff7fc3000     0x2000        0x0  r-xp   [vdso]
      0x7ffff7fc3000     0x7ffff7fc5000     0x2000        0x0  r--p   /usr/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
      0x7ffff7fc5000     0x7ffff7fef000    0x2a000     0x2000  r-xp   /usr/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
      0x7ffff7fef000     0x7ffff7ffa000     0xb000    0x2c000  r--p   /usr/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
      0x7ffff7ffb000     0x7ffff7ffd000     0x2000    0x37000  r--p   /usr/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
      0x7ffff7ffd000     0x7ffff7fff000     0x2000    0x39000  rw-p   /usr/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2
      0x7ffffffde000     0x7ffffffff000    0x21000        0x0  rw-p   [stack]
  0xffffffffff600000 0xffffffffff601000     0x1000        0x0  --xp   [vsyscall]
```

### reference（参考）

- 北风之神（微信：Le-studyg）
  - 高级课程
