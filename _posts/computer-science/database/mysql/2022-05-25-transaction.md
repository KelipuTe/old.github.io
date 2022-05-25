---
title: "事务（Transaction）"
create_date: 2022-05-25 08:00:00 +0800
date: 2022-05-25 08:00:00 +0800
tags: computer-science database mysql
comment: false
show_author_profile: true
show_subscribe: false
---

### ACID

- 原子性（Atomicity）
- 一致性（Consistency）
- 隔离性（Isolation）
- 持久性（Durability）

### 隔离性与隔离级别

当 MySQL 上有多个事务同时执行的时候，就可能出现脏读（dirty read）、不可重复读（non-repeatable read）、幻读（phantom read）的问题，为了解决这些问题，就有了隔离级别的概念。

隔离得越严实，效率就会越低，不同的隔离级别要在隔离与效率之间寻找一个平衡点。

SQL 标准的事务隔离级别包括：读未提交（read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（serializable）。

- 读未提交：一个事务还没提交时，它做的变更就能被别的事务看到。
- 读提交：一个事务提交之后，它做的变更才会被其他事务看到。
- 可重复读：一个事务执行过程中看到的数据，总是跟这个事务在启动时看到的数据是一致的。
- 串行化：对于同一行记录，后访问的事务必须等前一个事务执行完成，才能继续执行。

<div style="text-align: center; margin: 5px auto">
<img src="/image/computer-science/database/mysql/transaction_demo02.drawio.png">
</div>

对于上图的事物 A，在不同的隔离级别下 v1、v2、v3 的值会不同。

- 读未提交：v1 = 2；v2 = 2；v3 = 2。事务 B 的更新在提交前就能被 A 看到。
- 读提交：v1 = 1；v2 = 2；v3 = 2。事务 B 的更新在提交后才能被 A 看到。
- 可重复读：v1 = 1；v2 = 1；v3 = 2。事务在执行期间看到的数据前后必须是一致的。
- 串行化：v1 = 1；v2 = 1；v3 = 2。在事务 B 执行 `把 1 改成 2` 的时候，会被锁住。直到事务 A 提交后，事务 B 才可以继续执行。

在实现上，MySQL 会创建一个视图，访问的时候以视图的逻辑结果为准。

- 在读未提交下，直接返回记录上的最新值，没有视图概念
- 在读提交下，这个视图是在每个 SQL 语句开始执行的时候创建的
- 在可重复读下，这个视图是在事务启动时创建的，整个事务存在期间都用这个视图
- 在串行化下，直接用加锁的方式来避免并行访问。