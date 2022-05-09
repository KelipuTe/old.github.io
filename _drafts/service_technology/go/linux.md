## linux

### centos7配置go开发环境

去官网（[https://golang.google.cn/dl/](https://golang.google.cn/dl/)）下载压缩包，这里下载的是：go1.17.6.linux-amd64.tar.gz。

解压到`/go`目录：`$ tar zxvf go1.17.6.linux-amd64.tar.gz -C /go`。

设置软连接：`$ ln -s /go /usr/local/go`

设置系统环境变量

```
$ mkdir /go/gopath
$ export GOROOT=/usr/local/go
$ export PATH=$PATH:$GOROOT/bin
$ export GOPATH=/go/gopath
```

到这里应该就可以用了

```
$ go version
go version go1.17.6 linux/amd64
```

开启go mod

```
$ export GO111MODULE="on"
$ go env -w GOPROXY=https://goproxy.cn,direct
```

