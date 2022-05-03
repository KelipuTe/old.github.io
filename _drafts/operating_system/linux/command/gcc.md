## gcc

- 2021-12-01

### 安装

```shell
yum -y install gcc
```

### 验证

安装好之后，可以使用`gcc -v`命令查看版本信息。

成功输出如下gcc版本信息就说明安装成功了。

```
Using built-in specs.
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/usr/libexec/gcc/x86_64-redhat-linux/4.8.5/lto-wrapper
Target: x86_64-redhat-linux
Configured with: ../configure --prefix=/usr --mandir=/usr/share/man --infodir=/usr/share/info --with-bugurl=http://bugzilla.redhat.com/bugzilla --enable-bootstrap --enable-shared --enable-threads=posix --enable-checking=release --with-system-zlib --enable-__cxa_atexit --disable-libunwind-exceptions --enable-gnu-unique-object --enable-linker-build-id --with-linker-hash-style=gnu --enable-languages=c,c++,objc,obj-c++,java,fortran,ada,go,lto --enable-plugin --enable-initfini-array --disable-libgcj --with-isl=/builddir/build/BUILD/gcc-4.8.5-20150702/obj-x86_64-redhat-linux/isl-install --with-cloog=/builddir/build/BUILD/gcc-4.8.5-20150702/obj-x86_64-redhat-linux/cloog-install --enable-gnu-indirect-function --with-tune=generic --with-arch_32=x86-64 --build=x86_64-redhat-linux
Thread model: posix
gcc version 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC)
```

### 参数

- `-E`：预处理
- `-S`：预处理、编译，得到汇编代码
- `-c`：预处理、编译、汇编，得到可重定位文件
- 什么参数都不写，预处理、编译、汇编、链接，得到可执行文件
- `-o`：指定目标名称

### 示例

代码：demo_c/demo_linux_c/hello/hello.c

```shell
#预处理
> gcc -E hello.c -o hello.i
#编译，得到汇编代码
> gcc -S hello.i -o hello.s
#汇编，得到可重定位文件
> gcc -c hello.s -o hello.o
#预处理、编译、汇编、链接，得到可执行文件
> gcc hello.c -o hello
```