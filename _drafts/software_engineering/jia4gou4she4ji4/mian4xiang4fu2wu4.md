## 面向服务的架构

- 2021-11-03

### 面向服务的架构

面向服务的架构（SOA）是一种设计方法，其中包含多个服务，而服务之间通过配合最终会提供一系列功能。

一个服务通常以独立的形式存在于操作系统进程中。服务之间通过网络调用，而非采用进程内调用的方式进行通信。

soa是一种架构设计模式，而soap、rest、rpc就是根据这种设计模式构建出来的规范。

simple object access protocol（soap）简单对象访问协议。

representational state transfer（rest）是一个接口设计的风格，如果一个架构符合rest原则，就称它为restful架构。

remote procedure call，远程过程调用。rpc通常特指在一个应用中调用另一个应用的接口而实现的远程调用。

其中soap通俗理解就是http+xml的形式，rest就是http+json的形式，rpc是基于socket的形式。

cxf是典型的soap/rest框架，dubbo是典型的rpc框架，springcloud是遵守rest规范的生态系统。

从soa的定义中可以看到几个关键特性：一种粗粒度、松耦合服务架构，服务之间通过简单、精确定义接口进行通讯，不涉及底层编程接口和通讯模型。

### soa的实现方式

web service：服务提供者，服务注册中心，服务请求者。

服务注册表：服务注册（提供者），服务绑定，服务位置（使用者），本质与web service类似。

企业服务总线（ESB）：核心集成服务（提供者）、基础架构服务、客户端（使用者）。
