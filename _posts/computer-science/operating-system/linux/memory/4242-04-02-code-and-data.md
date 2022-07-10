---
title: "code and data memory layout（代码和数据的内存布局）"
date: 2022-07-04 08:00:00 +0800
tags: computer-science operating-system linux memory
comment: false
show_author_profile: true
show_subscribe: false
---

> 操作系统架构：x86_64 AMD Ryzen <br/>
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

函数代码的起始地址在编译时就已经确定了。

函数代码的起始地址：

```
(gdb) p &func1
$5 = (int (*)(int)) 0x555555555129 <func1>
(gdb) p &func2
$6 = (int (*)(int)) 0x55555555513c <func2>
(gdb) p &main
$7 = (int (*)()) 0x555555555151 <main>
```

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

全局变量的起始地址在编译时就已经确定了。

全局变量的起始地址：

```
(gdb) p &x
$1 = (int *) 0x555555558010 <x>
(gdb) p &y
$2 = (int *) 0x555555558014 <y>
```

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

| 变量 x 的内存地址 | 数据 | 变量 y 的内存地址 | 数据 |
| --- | --- | --- | --- |
| 0x555555558010 | 0x10 | 0x555555558014 | 0x00 |
| 0x555555558011 | 0x00 | 0x555555558015 | 0x01 |
| 0x555555558012 | 0x00 | 0x555555558016 | 0x00 |
| 0x555555558013 | 0x00 | 0x555555558017 | 0x00 |

- 注意，在内存中是小端字节序。
- `int x = 1 << 4;` = $2^{4}$ = 0x 00 00 00 10
- `int y = 1 << 8;` = $2^{8}$ = 0x 00 00 01 00

#### 局部变量

[局部变量]({% link _posts/computer-science/operating-system/linux/memory/4242-04-02-stack-and-heap.md %})

### reference（参考）

- 北风之神（微信：Le-studyg）
  - 高级课程