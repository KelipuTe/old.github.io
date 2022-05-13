---
title: "计算机科学的抽象层级"
create_date: 2021-11-09 08:00:00 +0800
date: 2022-05-07 08:00:00 +0800
tags: computer-science
comment: false
show_author_profile: true
show_subscribe: false
---

### 逻辑门

由晶体管和电线就可以组成三个基本逻辑门：与门、或门、非门。一层抽象。

<div style="text-align: center">
<img src="/image/hardware/and_gate.drawio.png">
</div>

<div style="text-align: center">
<img src="/image/hardware/or_gate.drawio.png">
</div>

<div style="text-align: center">
<img src="/image/hardware/not_gate.drawio.png">
</div>

由三个基本逻辑门可以组成异或门，异或门可以实现异或运算（XOR，符号：`^`）。一层抽象。

<div style="text-align: center">
<img src="/image/hardware/xor_gate.drawio.png">
</div>

### 半加器

由异或门、与门可以组成半加器。一层抽象。

<div style="text-align: center">
<img src="/image/hardware/half_adder.drawio.png">
</div>

### 全加器

由半加器、或门可以组成全加器。一层抽象。

<div style="text-align: center">
<img src="/image/hardware/full_adder.drawio.png">
</div>

### 加法器

由半加器、全加器可以组成 8 位行波进位加法器。一层抽象。

<div style="text-align: center">
<img src="/image/hardware/8_bit_ripple_carry_adder.drawio.png">
</div>

### 算术逻辑单元

由半加器、全加器、加法器、逻辑门等可以组成算术单元；由大量逻辑门可以组成逻辑单元；由算术单元、逻辑单元可以组成算术逻辑单元。一层抽象。

<div style="text-align: center">
<img src="/image/hardware/alu.drawio.png">
</div>

### 锁存器

由逻辑门等可以组成锁存器。

<div style="text-align: center">
<img src="/image/hardware/and_or_latch.drawio.png">
</div>

### 门锁

在锁存器的基础上做一点改进，就可以得到门锁。一层抽象。

<div style="text-align: center">
<img src="/image/hardware/gated_latch.drawio.png">
</div>

### 寄存器

由门锁可以组成 8 位寄存器。一层抽象。

<div style="text-align: center">
<img src="/image/hardware/8_bit_register.drawio.png">
</div>

### 内存的雏形

数量庞大的门锁合适的排列方式是矩阵排列。

<div style="text-align: center">
<img src="/image/hardware/latch_matrix_unit.drawio.png">
</div>

由门锁、多路复用器可以组成 256 位内存的雏形。

<div style="text-align: center">
<img src="/image/hardware/256_bit_memory.drawio.png">
</div>

一层抽象。

<div style="text-align: center">
<img src="/image/hardware/256_bit_memory_abstract.drawio.png">
</div>

### 内存

由内存的雏形可以组成 256 字节内存。

<div style="text-align: center">
<img src="/image/hardware/256_byte_memory.drawio.png">
</div>

一层抽象。

<div style="text-align: center">
<img src="/image/hardware/256_byte_memory_abstract.drawio.png">
</div>

### CPU

由逻辑门、寄存器可以组成控制单元。一层抽象。

由算术逻辑单元、控制单元、寄存器可以组成 CPU。一层抽象。

<div style="text-align: center">
<img src="/image/hardware/cpu.drawio.png">
</div>

### 程序

由指令可以组成程序（一大堆连续的纸卡变成一条连续的纸带）。一层抽象。

#### 汇编语言

由汇编器将汇编语言翻译成机器语言（用助记符替代机器码）。一层抽象。

高级语言到汇编语言（隐藏了不必要的细节，降低编程复杂度），也可以看做一层抽象。

#### 面向对象编程

由属性和函数组成对象（隐藏了不必要的细节，降低编程复杂度）。一层抽象。

### 操作系统

由操作系统提供底层硬件的操作接口（隐藏了硬件驱动，降低编程复杂度）。一层抽象。

#### 虚拟内存

由操作系统管理分配给程序的内存（隐藏了不必要的细节，降低编程复杂度）。一层抽象。

#### 文件系统

由文件系统管理存储器中文件的具体位置（隐藏了不必要的细节，降低编程复杂度）。一层抽象。

#### 计算机图形接口

由操作系统提供预先写好的图形函数（隐藏了不必要的细节，降低编程复杂度）。一层抽象。

#### 图形用户界面

由操作系统提供整套的图形界面（隐藏了不必要的细节，降低用户理解成本）。一层抽象。

### 参考

- Crash Course Computer Science（计算机科学速成课）
- [bilibili](https://www.bilibili.com/video/BV1EW411u7th)
- [CrashCourse 字幕组](https://github.com/1c7/crash-course-computer-science-chinese)
- [Youtube 原视频](https://www.youtube.com/playlist?list=PL8dPuuaLjXtNlUrzyH5r6jN9ulI)