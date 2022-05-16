---
title: "BT 种子"
create_date: 2021-12-14 08:00:00 +0800
date: 2021-12-14 08:00:00 +0800
tags: computer-science protocol
comment: false
show_author_profile: true
show_subscribe: false
---

### BT 种子

BT 种子，实际上指的是由 BitTorrent 协议所生成的一个包含资源信息的文件。

与传统的网络传输协议不同，BitTorrent 协议是一种以 P2P（peer-to-peer、用户对用户）模式为主的资源分享协议。采用的是一种去中心化的思想，不需要一个专门的文件发布者或者发布平台。从理论上来说，一个 BT 种子只要发布了，种子所包含的资源就永远存在于互联网上。

平常所使用的 HTTP、FTP 等协议需要一个中心发布者在网络上发布文件，即一种点对多的模式。当然，如果中心发布者由于某种原因被封了或者发布者删除了资源，那么就无法下载资源了。

### BitTorrent

BitTorrent 协议的思想是将一个文件划分为大小相等的 n 块，每块大小必须为 2 的整数次方。例如一个 100M 的文件，按照每块 1024kb 的大小被分为 100 个小块，每块中包含索引信息和 Hash 值，而我们的下载过程实际上就是块的交换过程。

BitTorrent 协议的资源发布者会根据要求，制作一个包含资源下载信息，例如 Tracker 服务器地址、文件大小、文件名、块文件大小等信息的 `.torrent` 文件，这个过程也就是平时说的做种。

如果要下载 BT 资源，首先要得到对应的 `.torrent` 文件，然后用专门的下载软件，例如 BitComet（比特彗星），下载过程大概为：

- 1、读取 `.torrent` 文件信息，载入内存
- 2、得到文件内的 Tracker 地址，连接 Tracker 服务器
- 3、Tracker 服务器回应下载请求，记录你的 IP 并告知其它下载者的 IP 地址
- 4、你与其他在线的下载者连接，交换各自没有的块
- 5、验证得到的块信息，若不同，则需要重新下载

### 磁力链接

由上文可以看出，Tracker 是很重要的一个东西。一但 Tracker 服务器被封，就无法进行下载了。由此，Magnet URI scheme（磁力链接）诞生了。

磁力链接，是对等网络中进行信息检索和下载文档的电脑程序。和基于位置连接的统一资源定位符不同，磁力链接是基于 metadata（元数据）文件内容，属于统一资源名称。也就是说，磁力链接不基于文档的IP地址或定位符，而是在分布式数据库中，通过散列函数值来识别、搜索来下载文档。因为不依赖一个处于启动状态的主机来下载文档，所以特别适用没有中心服务器的对等网络。

磁力链接利用 DHT（distributed hash table、分布式哈希表）和 PEX（peer exchange、节点信息交换）实现了资源的随意传播，根本无法禁止。

磁力链接下载的本质是将每一个人都变为 Tracker 服务器，将资源与下载者对应起来，每位下载者保存部分信息。这样，在下载资源时，只需寻找拥有所需资源的下载者。简单理解就是，A 认识 B，B 认识 C，C 认识 D 和 E。如果 A 想认识 E，就可以通过 B 和 C 的介绍来认识 D，不需要 A 一个个去寻找 E。

```
magnet:?xt=urn:btih:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

上面是一个常见的磁力链接。

- `magnet`：为协议名
- `xt`：exact topic，资源定位点
- `urn`：Uniform Resource Name，资源名
- `btih`：BitTorrent info hash，表示种子散列函数
- 最主要的就是 btih 后面唯一的一串 16 进制的数字

### 图种

图种就是把包含 BT 种子的压缩文件隐藏在图片中。

需要准备一张图片 `1.jpg` 和一个压缩文件 `2.rar`。然后新建一个 `.bat` 后缀的批处理文件，把下面的代码放进去。

```bat
copy /b 1.jpg+2.rar 3.jpg
```

`copy /b` 是一个基础的 DOS 命令，作用是合并文件。

把图片、压缩文件、批处理文件放在同一个目录。然后执行批处理文件，之心完成后就会得到一张图片 `3.jpg`。

这个图片实际上同时文件含了图片和压缩文件。如果把图片 `3.jpg` 的后缀名改成 `.rar`，这个时候图片就变成了压缩文件。这个压缩文件是可以解压的，压缩文件的内容就是 `2.rar` 的内容。