# 主库配置

## 配置文件

```properties
[mysqld]
# 集群环境下需要配置server-id
server-id=1
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

# 指定需要进行同步的库
# 如果存在，只进行指定库的同步
# 不存在，进行除了ignore的库都要进行同步
# binlog-do-db=XXX
wait_timeout=86400
bind-address=0.0.0.0
```

配置文件修改以后。需要进行服务重启。

```shell
net stop mysql57
net start myql57
```

## 授权操作

```mysql
# 复制授权
grant replication slave on *.* to 'root'@'%' identified by 'godme';
# 远程登录授权
grant all privileges on *.* to 'root'@'%' identifued by 'godme';
# 刷库
flush privileges;
```

## 设置检测

```mysql
show master status;
```

![image-20210117170138332](E:\code\java\mysql-1\img\master_status.png)

> 如果发现不生效，请查看服务具体指定的配置文件地址
>
> ![image-20210117170226878](E:\code\java\mysql-1\img\master_config.png)
> 配置文件地址不一定就在安装目录下。

# 从库配置

## 配置文件

```properties
# 服务编号
server-id=2
# relay log
relay_log=mysql-relay-bin
# 只读
read_only=1
```

> 修改加载以后，查看配置确认生效
>
> ```mysql
> # 查看服务server-id
> show variables lile 'server_id';
> ```

##　关联主库

```mysql
change master to 
# IP
master_host='127.0.0.1',
# port
master_port=3306,
# user
master_user='root',
# pass
master_password='gdome',
# log_file
master_log_file='mysql-bin.000001',
# log_pos
master_log_pos=154;
```

## 开启从库

```mysql
start slave;
```

## 从库状态

```mysql
show slave status;
```

![image-20210117172433115](E:\code\java\mysql-1\img\slave_status.png)

# 同步测试

## 库表创建

```mysql
create database godme;
use godme;
```

- ``position``

```mysql
create table position(
	id int primary key auto_increment,
    name varchar(50),
    salary int,
    city varchar(10)) engine=innodb charset=utf8;
```

- ``position_detail``

```mysql
create table position_detail(
    id int primary key auto_increment,
    pid int, 
    description text) engine=innodb charset=utf8;
```