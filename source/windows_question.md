---
title: windows版疑难问题集
permalink: /windows_question/
description: Verison 1.0.0
---
# 疑难问题集

## 错误1 redis无法正常启停

####问题1
权限不够，导致redis无法正常启停

#####错误日志
```
2481:M 08 Feb 15:07:57.716 # User requested shutdown...2481:M 08 Feb 15:07:57.716 * Saving the final RDB snapshot before exiting.2481:M 08 Feb 15:07:57.716 # Failed opening the RDB file dump.rdb (in server root dir /u01/HDM/redis-3.2.5/redis/bin) for saving: Permission denied2481:M 08 Feb 15:07:57.716 # Error trying to save the DB, can't exit.
```
#####处理方式
原因是由于hdm无权限写dump文件，修改dump路径可以成功解决该问题修改配置文件/u01/HDM/redis-3.2.5/redis.conf的dump路径**dir /u01/HDM/redis-3.2.5/logs**