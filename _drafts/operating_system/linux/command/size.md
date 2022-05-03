## size

- 2021-12-02

size命令显示一个目标文件或者链接库文件中的目标文件的各个段的大小。

### 代码文件

- ../code/demo01.c

### size

```shell
> size demo01.o
   text    data     bss     dec     hex filename
    159       8       4     171      ab demo01.o
```

- text，代码段通常是指用来存放程序执行代码的一块内存区域。
- data，数据段通常是指用来存放程序中已初始化的全局变量的一块内存区域。数据段属于静态内存分配。 
- bss，BSS段通常是指用来存放程序中未初始化的全局变量的一块内存区域。BSS段属于静态内存分配。 
- 默认情况下，段的大小是以十进制的方式来展示。

data占8个字节，`int a`和`static int i`。
bss占4个字节，`int b`和`static int j`。