---
title: "Linux 压缩每日日志 shell 脚本"
create_date: 2020-10-19 08:00:00 +0800
date: 2020-10-20 08:00:00 +0800
tags: linux shell
comment: false
show_author_profile: true
show_subscribe: false
---

这段脚本用于每天压缩前一天的日志文件。

```shell
#!/bin/bash

# 获取前一天的 yyyy-mm-dd 格式的日期
dir_date=$(date -d yesterday +%Y-%m-%d)
# 进入日志目录对应日期的目录
cd /tmp/logs/$dir_date/
# 查找所有以 .log 为后缀名的文件，并压缩
find ./ -name '*.log' | xargs -i gzip {}

exit;
```
