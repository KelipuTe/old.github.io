---
title: "human-computer interaction（人机交互）"
create_date: 2021-11-10 08:00:00 +0800
date: 2022-05-09 08:00:00 +0800
tags: computer-science graphics
comment: false
show_author_profile: true
show_subscribe: false
---

### 命令行界面

早期计算机是通过齿轮、旋钮、开关等机械结构来输入输出的，这就是最基础的交互界面。那个时代的特点是，尽可能迁就机器，对人类好不好是其次。

早期的 command line interface（命令行界面）是电传交互，人的输入和计算机的输出都通过 teletype machine（电传打字机）打印出来实现。随着处理器和屏幕的发展，使得用屏幕替代电传打字机变得可行，称之为虚拟电传打字机或者终端。后来屏幕就成为了标配。

这个时间出现了最早的游戏，即 text-based computer games（交互式文字游戏）。

### 屏幕和 2D 图形显示

#### 阴极射线管

cathode ray tubes（CTR、阴极射线管）将电子发射到有磷光体涂层的屏幕上，当电子撞击涂层时，会发光几分之一秒。由于电子是带电粒子，所以路径可以由磁场控制。

有两种方法绘制图形。第一种是引导电子束描绘出形状，这叫 vector scanning（矢量扫描），如果重复的足够快，就可以得到清晰的图像。第二种是，按固定路径一行一行来，不断重复，这叫 raster scanning（光栅扫描）。

#### 字符生成器

character generator（字符生成器）从内存读取数据，转换成光栅图形，可以算是第一代 graphics cards（显卡）。内存中存着每个字符的图形，也叫 dot matrix pattern（点阵图案）。

为了显示，字符生成器会访问内存中一块特殊的区域，这块区域专为图形保留，也叫 screen buffer（屏幕缓冲区）。程序想显示文字时，修改这块区域里的值就行。
 
#### 矢量图形

vector graphics（矢量图形）用线描述图形，图形中所有的东西都是用线表示的，包括文字。最早的有画面的游戏，用的就是矢量图形。

1962 年，Ivan Sutherland（伊万·萨瑟兰）发明了第一个完整的图形程序：interactive graphical interface（交互式图形界面），用于 computer-aided design（CAD、计算机辅助设计)。

#### 显示器

最早用真正 pixel（像素）的计算机和显示器出现于 1960 年代末。用内存直接对应屏幕上的像素，图形就是一个巨大的像素矩阵，这叫 bitmapped displays（位图显示）。至此，计算机可以绘制任意图形了。

计算机把像素数据存在内存中一个特殊区域，叫 frame buffer（帧缓冲区）。早期时这些数据存在内存里，后来在显卡上的 video RAM（VRAM、高速视频内存）里。

- liquid crystal display（LCD、液晶显示器）

#### 计算机图形接口

现代操作系统都会提供 computer graphics interface（CGI、计算机图形接口）工程序员直接使用。

### 图形用户界面

graphical user interface（GUI、图形用户界面、图形用户接口）

图形用户界面是 event-driven programming（事件驱动编程），代码可以在任意时间执行以响应事件。

- 1964 年，mouse（鼠标）设备，通过设备在屏幕上移动光标。
- 1968 年，现代图形界面原型：位图图像、video conferencing（视频会议）、word pricessing（文字处理）、collaborative real-time editing of ducnments（实时协作编辑文件）、鼠标、multiple windows（多窗口）。
- 1973 年，xerox alto（施乐奥托）：desktop metaphor（桌面隐喻）
- WIMP interface：windows（窗口）、icon（图标）、menu（菜单）、pointer（指针）
- 1995 年，Windows 95：program multitasking（多任务）、protected memory（受保护内存）、start menu（开始菜单）、taskbar（任务栏）、windows explorer file manager（windows 文件管理器）

如今的用户界面是 natural selection（自然选择）的结果。

### 3D 图形

- 3-dimensional projection（3D 投影）：通过图形算法，把 3D 坐标平"拍平"，显示到 2D 屏幕上。
- wireframe rendering（线框渲染）：当所有的点都从 3D 转成 2D 之后，就可以用画 2D 线段的函数，连接这些点。
- orthographic projection（正交投影）：立方体的各边，在投影中互相平行。
- perspective projection（透视投射）：立方体的各边，在投影中会在远处收敛于一点。对应现实世界中，平行线会在远处收敛于一点。
- mesh（网格）：三角形在 3D 图形学中更常用，因为三角形能定义平面。在 3D 图形学中。三角形也叫 polygons（多边形），一堆多边形的集合叫网格。网格越密，表面越光滑，细节越多，但也意味着更多的计算量。
- scanline rendering（扫描线算法）：这里以三角形为例。先铺一层像素网络，然后找出三角形最大纵坐标和最小纵坐标，只需要在这个范围内工作。然后算法从上往下，一次处理一行，计算每一行和三角形相交的两个点，然后填充这两个点之间的像素。当然，这样的三角形比较丑，边缘满是锯齿。
- anti-aliasing（抗锯齿）：使用扫描线算法时，每个像素不都涂成一样的颜色。可以通过判断多边形切过像素的程度，来调整颜色。如果像素在多边形内部，就直接涂色；如果多边形划过像素，颜色就浅一点。这种边缘羽化的效果，看着更舒服一些。
- occlusion（遮挡）：在 3D 场景中，多边形到处都是，但只有一部分能看见，其他的会被挡住。最直接的处理办法是 painter's algorithm(画家算法)：把多边形从远到近排序，从远到近渲染，想画家画画一样。还有一种方法叫 z buffering（深度缓冲）：通过给多边形设置深度值，来判断哪个多边形的像素在上面。当两个图形重叠的部分深度值一样的时候，最终显示哪个，是看算法怎么算浮点数的。因为不可控，所以就会出现重叠的部分一直闪的情况。
- back face culling（背面剔除）：3D 游戏中的一个优化方法。比如，游戏角色的头部或者地面，都只能看到朝外的一面。所以为了节省处理时间，会忽略多边形的背面，可以减少一半的多边形面数。
- shading（明暗处理）：在 3D 场景中，物体表面应该有明暗变化。物体上不同的多边形，不都平行于屏幕，而是有不同的方向。这些多边形面对的方向叫 surface normal（表面法线）。根据光源和多边形表面法线的角度，进行不同的明暗渲染，这叫 flat shading（平面着色），是最基本的照明算法。平面着色算法渲染出来的效果，使多边形的边界非常明显，看起来不光滑。有更多算法解决这些问题，比如：gourand shading（高洛德着色）、phong shading（冯氏着色）。
- texture（纹理）在图形学中指外观，而不是手感。最简单的是 texture mapping（纹理映射），纹理算法会查询纹理，从相应区域取平均颜色，然后填充多边形。
- 渲染速度：提升渲染速度的方式有：为特定的运算做专门的硬件、把大场景拆分，然后并行处理。CPU 不是为此设计的，因此图形运算不快，工程师为图形设计了专门的处理器，叫 graphics processing unit（GPU、图形处理单元）。GPU 在显卡上，有专用的 RAM。
