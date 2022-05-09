---
title: "compression（压缩）"
create_date: 2021-11-11 08:00:00 +0800
date: 2021-11-11 08:00:00 +0800
tags: computer-science algorithm
comment: false
show_author_profile: true
show_subscribe: false
---

### 压缩

compression（压缩）可以把数据占用的空间压得更小，用更少的空间来表示数据。让一定的空间可以存储更多的文件，通行时的传输速度也更快。

压缩的常见思路有：removing redundancies（消除冗余）和使用 more compact representations（更紧凑的表示）。

### 无损压缩

lossless compression（无损压缩）的文件解压缩后，数据和压缩前完全一样。

#### 游程编码

run-length encoding（游程编码）适合处理经常有连续数据的文件，比如将完全展开存储的"11111111111111111111" 压缩成 "20个1" 的形式。

#### 字典编码

字典编码需要一个 map（字典）存储 compact codes（紧凑代码）和原始数据之间的对应关系。1950 年代，David Huffman（大卫·霍夫曼）发明了一种高效编码的方式，叫 huffman tree（霍弗曼树）。它是字典编码的典型案例。

霍弗曼树可以根据数据出现的频率重新编码数据，让高频出现的数据使用短编码表示。然后用字典记录原来的数据和重新编码后的编码的对应关系。需要注意的是，字典也需要占空间的。

### 有损压缩

lossy compression（有损压缩）会丢掉那些人类感知不出区别的数据。比如：声音。人耳能识别的声音频率是有范围的，那么超过这个范围的数据就都可以丢掉。

另外，打电话时的声音和真实的人声可能有一些不同，那是因为压缩算法删除了部分数据导致的，这是传输效率的需要。当网速变慢时，压缩算法会删除更多的数据，让声音变得像机器人一样。

比较常见的有损压缩有：JPEG

#### 感知编码

删掉人类无法感知的数据的方法叫 perceptual coding（感知编码），它依赖于人类的感知模型，模型来自 psychophtsics（心理物理学）领域。

#### 时间冗余

视频里大部分场景相同时，不需要每一帧都存这些像素，可以只存变了的部分。这种方式叫 temporal redundancy（时间冗余）。这比逐帧存储更有效率，利用了 inter-frame similarity（帧与帧之间的相似性）。

高级的视频压缩方式会找出帧与帧之间相似的部分，然后用简单的操作实现。比如一个人在挥手，压缩算法会把这个手看成一个整体，然后平移和旋转这个整体，可以节省大量的空间。

有的时候视频画面出现撕裂或者画面错误，就是压缩算法出错导致的，但是播放器会忠实的渲染画面。

### 参考

- Crash Course Computer Science（计算机科学速成课）
- [bilibili](https://www.bilibili.com/video/BV1EW411u7th)
- [CrashCourse 字幕组](https://github.com/1c7/crash-course-computer-science-chinese)
- [Youtube 原视频](https://www.youtube.com/playlist?list=PL8dPuuaLjXtNlUrzyH5r6jN9ulI)