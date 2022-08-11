---
title: "red-black tree（红黑树）"
date: 2022-08-08 08:00:00 +0800
tags: computer-science 计算机科学 data-structure 数据结构 binary-search-tree 二叉查找树
comment: false
show_author_profile: true
show_subscribe: false
---

### readme（说明）

文中涉及的图片在 draw.io 文件 `git.io/drawio/computer-science/data-structure/red-black_tree.drawio` 中。或者看 draw.io 文件生成的 HTML 文件，[red-black_tree.drawio.html](/image/computer-science/data-structure/red-black_tree.drawio.html)，渲染需要等一会。

### 红黑树

- 红黑树树更高级的二叉查找树
- 红黑树和2-3-4 树是等价的：
  - 红黑树的黑色结点个数 = 2-3-4 树的结点个数。
  - 把红黑树的红结点移动到和父结点同层，就会变成 2-3-4 树。
  - 把 2-3-4 树的三结点和四结点拆开，就可以变成红黑树。

### 红黑树的性质

- 1、满足二叉查找树的基本性质。
- 2、树中的每个结点颜色不是红的，就是黑的
- 3、根结点的颜色是黑的。
- 4、所有为 nil 的叶子结点的颜色是黑的。
- 5、如果此结点是红的，那么它的两个孩子结点全部都是黑的。
- 6、对于每个结点，从该结点到到该结点的所有子孙结点的所有路径上包含有相同数目的黑结点。

### reference（参考）

- [红黑树（更高级的二叉查找树）算法详解](http://c.biancheng.net/view/vip_3433.html)
- [bilibili--木子喵neko](https://space.bilibili.com/27735697)
  - [【neko】红黑树/插入【算法编程#11】](https://www.bilibili.com/video/BV1BB4y1X7u3)
  - [【neko】红黑树/删除【算法编程#12】](https://www.bilibili.com/video/BV1Ce4y1Q76H)
