## strace

- 2021-12-04

`strace`命令常用来跟踪进程执行时的系统调用和进程接收的信号。

### 安装

```
yum -y install strace
```

### 参数

- `-f`：跟踪子进程
- `-i`：打印系统调用的地址
- `-t`：每一行打印时间
- `-T`：显示系统调用花费的时间
- `-p {pid}`：指定跟踪的进程号
- `-s {length}`：每一行的长度，默认32，一般设置65535
- `-o {file name}`：输出到文件

### 在doeker中使用strace命令

使用`strace -p`命令跟踪进程时报错：

```
attach: ptrace(PTRACE_SEIZE): Operation not permitted
```

参考官方文档：The solution for enabling of ptrace and PTRACE_ATTACH in Docker Containers（https://bitworks.software/en/2017-07-24-docker-ptrace-attach.html）。

启动容器的时候使用`–-privileged`参数，让容器内的root用户拥有真正的root权限。

```
docker run -it -p 127.0.0.1:9501:9501 -v {local path}:{docker path} --name={container name} --privileged centos:centos7
```

进入容器，然后使用命令：`echo 0 > /proc/sys/kernel/yama/ptrace_scope`。将`/proc/sys/kernel/yama/ptrace_scope`文件中的值修改成0。然后就可以使用`strace -p`命令跟踪进程了。
