### 基于二进制文件位置的复制

#### 一. 异步复制

`vagrant up`启动2个节点（node1,node2）

其中node1计划作为master节点，node2计划作为slave节点

#### 1. master节点

`vagrant ssh node1`进入node1节点控制台
修改mysql配置文件`/etc/my.cnf`，添加如下配置：

```
# 指定二进制日志文件名
log-bin=master-logbin
# 指定mysql服务器id
server-id=1
```

重启mysql

```
systemctl restart mysqld
```

创建用户并导出数据


```
# 进入mysql控制台
mysql -uroot -p
# 创建用户
CREATE USER 'repl'@'192.168.0.%' IDENTIFIED BY 'asd123456';
# 授予该用户复制权限
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'192.168.0.%';
# 刷新权限
flush privileges;
# 加读锁防止数据变化，并保留当前会话不退出
FLUSH TABLES WITH READ LOCK;
# 在状态信息中找到二进制文件和读取位置序号
SHOW MASTER STATUS;
# 当所有的slave实例都设置并开启复制后，才解锁
UNLOCK TABLES;


# 重新建立一个会话vagrant ssh node1
# 导出mysql数据
mysqldump --all-databases --master-data -p > dbdump.db
```


#### 2. slave节点

`vagrant ssh node2`进入node2节点控制台

修改mysql配置文件`/etc/my.cnf`，添加如下配置：

```
# 指定mysql服务器id
server-id=2
```

删除auto.cnf文件。因为box里的mysql是提前安装好的，里面的文件也全都一样，auto.cnf储存了server的uuid，这个在启动复制里是不能一样的。

```
rm -rf /var/lib/mysql/auto.cnf
```

重启数据库

```
systemctl restart mysqld
```

开启复制

```
# 进入mysql控制台
mysql -uroot -p
# 导入master节点的数据
mysql -p < dbdump.db
# 指定master节点的信息
CHANGE MASTER TO MASTER_HOST='192.168.0.11', MASTER_USER='repl', MASTER_PASSWORD='asd123456', MASTER_LOG_FILE='master-logbin.000001', MASTER_LOG_POS=155 , GET_MASTER_PUBLIC_KEY=1;

# 开启复制
START SLAVE;
```



#### 二. 半同步复制

在异步复制的基础上进行操作

首先判断是否支持动态加载插件

```
select @@have_dynamic_loading;
```

检查mysql的安装目录下是否存在插件，插件的目录可以通过以下语句查询：

```
show variables like 'plugin_dir';
```

本示例是`/usr/lib64/mysql/plugin/`

```
cd /usr/lib64/mysql/plugin/
ll semisync*
```

在插件目录应该有以下两个插件：

```
semisync_master.so   # 在主库中安装
semisync_slave.so    # 在从库中安装
```

在主库（node1）中安装插件：

```
# 进入mysql控制台
mysql -uroot -p 
# 安装master半同步插件
install plugin rpl_semi_sync_master soname 'semisync_master.so';
```

在从库（node2）中安装插件：

```
# 进入mysql控制台
mysql -uroot -p 
# 安装slave半同步插件
install plugin rpl_semi_sync_slave soname 'semisync_slave.so';
```

查看插件是否安装成功，有显示则成功：

```
select * from mysql.plugin;
```

分别在主库和从库上配置参数打开半同步semi-sync，默认半同步设置是不打开的：

主库上配置：

```
set global rpl_semi_sync_master_enabled=1
set global rpl_semi_sync_master_timeout=30000
```

从库中配置：

```
set global rpl_semi_sync_slave_enabled=1
```

重启从库的IO线程：

```
STOP SLAVE IO_THREAD;
START SLAVE IO_THREAD;
```




