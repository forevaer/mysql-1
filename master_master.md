# 配置文件

```mysql
[mysqld]
server-id=3
# 开启binlog
log_bin=mysql-bin
# 异步log_bin
sync-binlog=1
# 忽略的数据库，不进行同步 
binlog-ignore-db=information_schema
binlog-ignore-db=mysql
binlog-ignore-db=performance_schema
binlog-ignore-db=sakila
binlog-ignore-db=sys
binlog-ignore-db=nacos
# slave
relay_log=mysql-relay-bin
log_slave_updates=1
# 主键设置
# auto_increment_offset=1 起始
# auto_increment_increment=2 步长
```

是``master``，同时也是``slave``

# 双主设置

详情见[主从同步](master_slave.md)

