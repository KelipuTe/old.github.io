网路传递具有不确定性。

良好的超时策略，可以尽可能让服务不堆积请求，尽快清空高延迟的请求。

项目要求实现简易MYSQL：
1 支持CS架构
2 支持数据存储，支持快速排序，支持查看，支持基础命令CRUD
3 支持多线程，多进程
4 支持启动方式为前台，和守护进程模式运行
5 支持事务功能
6 支持友好停止服务
7 支持主从复制

Safari 标签，TODO Reading
Bookmark，收藏
OneNote 分类存储链接

github.com/Terry-Mao/bfs 
Facebook haystack 


DDD，DTO=>DO=>PO，分别是在哪一层来处理模型转换的。 
DTO gRPC ，service -> biz
DO biz, DTO -> DO，Do business policy/rules，DO 持久化  -> repo
PO data, DO -> PO，orm /ent  