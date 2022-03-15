# 单机版MySQL安装
```shell
# 1.搜索镜像
docker search mysql
```
```shell
# 2.拉取镜像
docker pull mysql[:tag]
```
```shell
# 3.运行镜像
docker run -d -p 3306:3306 -privileged=true -v /users/wenqi/mysql/log:/var/log/mysql -v /users/wenqi/mysql/data:/var/lib/mysql -v /users/wenqi/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 --name mysql1 mysql:tag
```
```shell
# 4.修改字符集编码
cd /users/wenqi/mysql/conf
vi my.cnf
[client]
default_character_set=utf8
[mysqld]
collation_server=utf8_general_ci
character_set_server=utf8
```
```shell
# 5.重启mysql容器
docker restart mysql1
```
```shell
# 6.查看字符集编码
docker exec -it mysql1 /bin/bash
mysql -uroot -p
show variables like 'character%';
```
```shell
# 7.开启远程访问权限
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456';
flush privileges; 
```
# 主从版MySQL安装
```shell
# 1.运行主MySQL在3307端口
docker run -d -p 3307:3306 -privileged=true -v /users/wenqi/mysql-master/log:/var/log/mysql -v /users/wenqi/mysql-master/data:/var/lib/mysql -v /users/wenqi/mysql-master/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 --name mysql-master mysql:tag
```
```shell
# 2.添加主MySQL配置
cd /users/wenqi/mysql-master/conf
vi my.cnf
[mysqld]
## 设置server_id，同一局域网中需要唯一
server_id=101
## 指定不需要同步的数据库
binlog-ignore-db=mysql
## 开启二进制日志功能
log-bin=mall-mysql-bin
## 设置二进制日志使用内存大小
binlog_cache_size=1M
## 设置使用二进制日志格式(mixed|statement|row)
binlog_format=mixed
## 二进制日志过期清理时间，默认为0，表示不清理
expore_logs_days=7
## 跳过主从复制中遇到的错误,避免复制中断
slave_skip_errors=1062
```
```shell
# 3.重启主MySQL
docker restart mysql-master
```
```shell
# 4.创建用户并授权
docker exec -it mysql-master /bin/bash
mysql -uroot -p
CREATE USER 'slave'@'%' IDENTIFIED BY '123456';
GRANT REPLICATION SLAVE,REPLICATION CLIENT ON *.* TO 'slave'@'%';
flush privileges; 
```
```shell
# 5.运行从MySQL在3308端口
docker run -d -p 3308:3306 -privileged=true -v /users/wenqi/mysql-slave/log:/var/log/mysql -v /users/wenqi/mysql-slave/data:/var/lib/mysql -v /users/wenqi/mysql-slave/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 --name mysql-slave mysql:tag
```
```shell
# 6.添加从MySQL配置
cd /users/wenqi/mysql-slave/conf
vi my.cnf
[mysqld]
## 设置server_id，同一局域网中需要唯一
server_id=102
## 指定不需要同步的数据库
binlog-ignore-db=mysql
## 开启二进制日志功能
log-bin=mall-mysql-slave1-bin
## 设置二进制日志使用内存大小
binlog_cache_size=1M
## 设置使用二进制日志格式(mixed|statement|row)
binlog_format=mixed
## 二进制日志过期清理时间，默认为0，表示不清理
expore_logs_days=7
## 跳过主从复制中遇到的错误,避免复制中断
slave_skip_errors=1062
## 配置中继日志
relay_log=mall-mysql-relay-bin
## 将复制事件写进自己的二进制日志
log_slave_updates=1
## 设置为只读
read_only=1
```
```shell
# 7.重启从MySQL
docker restart mysql-slave
```
```shell
# 8.获取主MySQL的主从同步状态
# File字段对应master_log_file
# Position字段对应master_log_pos
show master status;
```
```shell
# 9.在从MySQL中配置主从
docker exec -it mysql-slave /bin/bash
mysql -uroot -p
change to master_host='主MySQL的ip',master_user='主MySQL用于同步的用户',master_password='主MySQL用于同步的用户密码',master_port='主MySQL的port',master_log_file='指定要复制数据的日志文件',master_log_pos=617,master_connect_retry=30;
```
```shell
# 10.在从MySQL的开启主从
start slave;
```
```shell
# 11.获取从MySQL的主从同步状态
show slave status;
```
