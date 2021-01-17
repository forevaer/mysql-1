# 动态支持

```mysql
select @@having_dynamic_loading;
```

![image-20210117174123214](E:\code\java\mysql-1\img\have_dynamic_loading.png)

# 插件安装

## 安装查看

```mysql
show plugins;
```

## 插件安装

```mysql
install plugin rpl_semi_sync_master soname 'semisync_master.so' ;
```

> - 勘误
>
> ``so``是``c``的共享文件，相当于是``java``中的``jar``，通过接口调用集合，也就是``.h``头文件调用。
>
> ``windows``中显示的是``.dll``。
>
> 老师视频中说的``soname``是起名字，也就是相当于``alias``或者``renaem``。
>
> 但实际上是``so'name``，也就是库文件的名字，其实是不能随便更改的。
>
> 通过如下语句可以查看``plugin``的具体目录
>
> ```mysql
> # 插卡安装目录
> show @@basedir;
> # 查看插件目录
> show varibales like 'plugin_dir'
> ```
>
> ``soname``后面应该填写的是指定库的名称。
>
> ![image-20210117181150866](E:\code\java\mysql-1\img\plugin_soname.png)
>
> - 安装
> ```mysql
> insatll plugin rpl_semi_sync_master soname 'semisync_master.dll';
> ```
>
> ![image-20210117181311826](E:\code\java\mysql-1\img\plugin_master.png)
>
> - 卸载
>
> ```mysql
> uninstall plugin rpl_semi_sync_master;
> ```

- 主库插件

```mysql
install plugin rpl_semi_sync_master soname 'semisync_master.dll';
```

- 从库插件

```mysql
insatll plugin rpl_semi_sync_slave soname 'semisync_slave.dll';
```

# 功能开启

## 参数查看

```mysql
show variables like '%semi%';
```

![image-20210117182310698](E:\code\java\mysql-1\img\semi_master.png)

> 从库的状态会相对简单
>
> ![image-20210117182351797](E:\code\java\mysql-1\img\semi_slave.png)

| field                                         | description                                                  |
| --------------------------------------------- | ------------------------------------------------------------ |
| ``rpl_semi_sync_master_enable``               | 功能开关                                                     |
| ``rpl_semi_sync_master_timeout``              | 超时设置                                                     |
| ``rpl_semi_sync_master_trace_level``          | 日志级别                                                     |
| ``rpl_semi_sync_master_wait_for_slave_count`` | 低于此值，同步等待<br />高于此值，异步更新                   |
| ``rpl_semi_sync_master_wait_no_slave``        | 高于此值，异步更新                                           |
| ``rpl_semi_sync_master_wait_point``           | ``AFTER_COMMIT``:同步复制，从库应答，主库提交<br />``AFTER_SYNC``：从库应答，异步复制，主库提交 |

## 参数修改

- 主库

```mysql
# 功能开启
set global rpl_semi_sync_master_enable=1;
# 超时修改
set global rpl_semi_sync_master_timeout=1000;
```

- 从库

```mysql
# 功能开启
set global rpl_semi_sync_slave_enable=1;
```

## 状态重启

虽然不用重启服务，但是同步服务还是需要进行重新启动

- 从库

```mysql
# 关停从库复制
stop slave;
# 启动从库复制
start slave;
```

# 日志查看

具体详情居然在``err``日志里面。

