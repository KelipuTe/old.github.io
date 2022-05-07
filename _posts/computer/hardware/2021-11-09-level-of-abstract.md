---
title: "抽象层级"
create_date: 2021-11-09 08:00:00 +0800
date: 2022-05-07 08:00:00 +0800
tags: hardware
comment: false
show_author_profile: true
show_subscribe: false
---

### 逻辑门

由晶体管和电线就可以组成三个基本逻辑门：与门、或门、非门。这里进行了一层抽象。

<div style="text-align: center">
<img src="/image/hardware/and_gate.drawio.png">
</div>

<div style="text-align: center">
<img src="/image/hardware/or_gate.drawio.png">
</div>

<div style="text-align: center">
<img src="/image/hardware/not_gate.drawio.png">
</div>

由三个基本逻辑门可以组成异或门，异或门可以实现异或运算（XOR，符号：`^`）。这里进行了一层抽象。

<div style="text-align: center">
<img src="/image/hardware/xor_gate.drawio.png">
</div>

### 半加器

由异或门、与门可以组成半加器。这里进行了一层抽象。

<div style="text-align: center">
<img src="/image/hardware/half_adder.drawio.png">
</div>

### 全加器

由半加器、或门可以组成全加器。这里进行了一层抽象。

<div style="text-align: center">
<img src="/image/hardware/full_adder.drawio.png">
</div>

### 加法器

由半加器、全加器可以组成 8 位行波进位加法器。这里进行了一层抽象。

<div style="text-align: center">
<img src="/image/hardware/8_bit_ripple_carry_adder.drawio.png">
</div>

### 算术逻辑单元

由半加器、全加器、加法器、逻辑门等可以组成算术单元；由大量逻辑门可以组成逻辑单元；由算术单元、逻辑单元可以组成算术逻辑单元。这里进行了一层抽象。

<div style="text-align: center">
<img src="/image/hardware/alu.drawio.png">
</div>

### 锁存器

由逻辑门等可以组成锁存器。

<div style="text-align: center">
<img src="/image/hardware/and_or_latch.drawio.png">
</div>

### 门锁

在锁存器的基础上做一点改进，就可以得到门锁。这里进行了一层抽象。

<div style="text-align: center">
<img src="/image/hardware/gated_latch.drawio.png">
</div>

### 寄存器

由门锁可以组成 8 位寄存器。这里进行了一层抽象。

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

这里进行了一层抽象。

<div style="text-align: center">
<img src="/image/hardware/256_bit_memory_abstract.drawio.png">
</div>

### 内存

由内存的雏形可以组成 256 字节内存。

<div style="text-align: center">
<img src="/image/hardware/256_byte_memory.drawio.png">
</div>

这里进行了一层抽象。

<div style="text-align: center">
<img src="/image/hardware/256_byte_memory_abstract.drawio.png">
</div>

### CPU

由逻辑门、寄存器可以组成控制单元。这里进行了一层抽象。

由算术逻辑单元、控制单元、寄存器可以组成 CPU。这里进行了一层抽象。

<div style="text-align: center">
<img src="/image/hardware/cpu.drawio.png">
</div>

### 参考

- Crash Course Computer Science（计算机科学速成课）
- [bilibili](https://www.bilibili.com/video/BV1EW411u7th)
- [CrashCourse 字幕组](https://github.com/1c7/crash-course-computer-science-chinese)
- [Youtube 原视频](https://www.youtube.com/playlist?list=PL8dPuuaLjXtNlUrzyH5r6jN9ulI)