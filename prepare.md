# 前言

没钱导致

- 没有多个服务器
- 没有足够空间安装虚拟机

因此，需要再单机器上面创建多个``MySQL``实例就只能使用特殊的技巧。

# 服务关闭

## 服务管理

我的电脑$\Rightarrow$管理

![image-20210117011101714](E:\code\java\mysql-1\img\manage.png)

## 服务列表

![image-20210117011256169](E:\code\java\mysql-1\img\list.png)

## 关闭服务

![image-20210117011340397](E:\code\java\mysql-1\img\server1.png)

# 服务复制

## 服务地址

如果不知道自己的服务安装地址，可以通过服务属性进行查看

![image-20210117011648008](E:\code\java\mysql-1\img\attr.png)

## 安装目录

![image-20210117011725855](E:\code\java\mysql-1\img\location.png)

## 目录复制

![image-20210117011823757](E:\code\java\mysql-1\img\server_dir.png)

没让我选择过，居然给我塞到了``C``盘，我把它复制到``D``盘。

## 配置修改

- 配置文件

![image-20210117011940119](E:\code\java\mysql-1\img\config_file.png)

- 端口配置

```properties
[mysqld]
wait_timeout=86400
# 指定新端口，避免冲突
port = 3307
# 重新指定服务地址
basedir=D:\\MySQL57
bind-address=0.0.0.0
datadir=D:\\MySQL57\\data
```

我顺便改了一下目录名称。

- 安装服务

```shell
mysqld.exe install mysql2 --defaults-file="D:\MySQL57\my.ini"
```

![image-20210117013309290](E:\code\java\mysql-1\img\install.png)

- 启动服务

```shell
net start mysql2
```

![image-20210117013847377](E:\code\java\mysql-1\img\service2.png)

# 密码修改

重新登录启动可能登录失败

![image-20210117014103813](E:\code\java\mysql-1\img\login_failure.png)

需要进行密码修改

## 修改配置

```properties
[mysqld]
# 跳过鉴权
skip-grant-tables
wait_timeout=86400
port = 3307
basedir=D:\\MySQL57
bind-address=0.0.0.0
```

## 重启服务

![image-20210117014406253](E:\code\java\mysql-1\img\restart.png)

也可以通过服务列表中图形化界面进行重启。

## 密码修改

- 登录

```shell
mysql -uroot -P3307 -p 
# 要指定端口，否则无服务可以连接
# 直接回车，不需要密码
```

![image-20210117014549447](E:\code\java\mysql-1\img\login.png)

- 密码修改

![image-20210117014957960](E:\code\java\mysql-1\img\password.png)

> 为了安全起见，可以刷新数据库之后再推出进行其他操作
>
> ```mysql
> flush privileges
> ```
>
> 

- 删除配置

```properties
[mysqld]
# 去掉鉴权配置
# skip-grant-tables
wait_timeout=86400
port = 3307
basedir=D:\\MySQL57
bind-address=0.0.0.0
```

- 重启服务、
- 登录验证

```shell
mysql -uroot -P3309 -p
# godme 密码验证登录成功
```

