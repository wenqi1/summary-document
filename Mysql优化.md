# 1. explain返回列介绍

## 1.1 type

 system > const > eq_ref > ref > range > index > ALL

|     值      |                             含义                             |
| :---------: | :----------------------------------------------------------: |
|   system    |       表仅有一行(系统表)，这是const联接类型的一个特例        |
|    const    | 表最多有一个匹配行，它将在查询开始时被读取。const用于用常数值比较PRIMARY KEY或UNIQUE索引的所有部分时 |
|   eq_ref    | 对于每个来自于前面的表的行组合，从该表中读取一行。它用在一个索引的所有部分被联接使用并且索引是UNIQUE或PRIMARY KEY。eq_ref可以用于使用 = 操作符比较的带索引的列 |
|     ref     | 对于每个来自于前面的表的行组合，所有有匹配索引值的行将从这张表中读取。如果联接只使用键的最左边的前缀，或如果键不是UNIQUE或PRIMARY KEY，则使用ref。ref可以用于使用=或<=>操作符的带索引的列 |
| ref_or_null | 该联接类型如同ref，但是添加了MySQL可以专门搜索包含NULL值的行 |
|    range    | 只检索给定范围的行，使用一个索引来选择行。当使用=、<>、>、>=、<、<=、IS NULL、<=>、BETWEEN或者IN操作符，用常量比较关键字列时，可以使用range |
|    index    |          该联接类型与ALL相同，除了只有索引树被扫描           |
|     ALL     |       对于每个来自于先前的表的行组合，进行完整的表扫描       |

 实际sql优化中，最后达到ref或range级别。 

## 1.2 Extra

该列包含MySQL解决查询的详细信息。 

|       值        |                             含义                             |
| :-------------: | :----------------------------------------------------------: |
|   Using index   |            只从索引树中获取信息，而不需要回表查询            |
|   Using where   | WHERE子句用于限制哪一个行匹配下一个表或发送到客户，需要回表查询。 |
| Using temporary | 创建一个临时表来容纳结果，常见使用GROUP BY和ORDER BY子句时； |

# 2. 单表优化

## 2.1 sql的编写过程

```sql
select distinct ... from ... join ... on ... where ... group by ... having ... order by ... limit ...
```

## 2.2 sql的解析过程

```sql
from ... on ... join ... where ... group by ... having ... select distinct ... order by ... limit ...
```

## 2.3 优化示例

```sql
CREATE TABLE `student` (
  `id` int(10) NOT NULL,
  `name` varchar(20) NOT NULL,
  `age` int(10) NOT NULL,
  `sex` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `student_union_index` (`name`,`age`,`sex`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

```sql
explain select name from student where age = 18 and sex in (0,1);
-- type类型是index
```

```sql
-- 删除旧索引
drop index student_union_index on student;
-- 创建新索引，修改顺序
alter table student add index student_union_index(age,sex,name);
explain select name from student where age = 18 and sex in (0,1);
-- type级别发生了变化，由index变为了range
```

```sql
-- 去掉in
-- in会导致索引失效，所以触发using where，进而导致回表查询
explain select name from student where age = 18 and sex = 1;
-- type级别发生了变化，由range变为了ref
```

## 2.4 优化总结

1. 保持索引的定义和使用顺序一致性；
2. 将含in的范围查询，放到where条件的最后，防止索引失效；

# 3. 两表优化

## 3.1 优化示例

```sql

CREATE TABLE `student` (
  `id` int(10) NOT NULL,
  `name` varchar(20) NOT NULL,
  `age` int(10) NOT NULL,
  `sex` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `teacher` (
  `id` int(11) DEFAULT NULL,
  `name` varchar(100) DEFAULT NULL,
  `course` varchar(100) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

```sql
-- 左连接，联合查询时，小表驱动大表
explain select teacher.name,student.name from teacher left join student on teacher.id = student.id  where teacher.course = '数学'
-- teacher表type级别为ALL
-- student表type级别为ALL
```

```sql
-- left join，一般给左表加索引，因为是驱动表
alter table teacher add index teacher_index(id);
alter table teacher add index teacher_course(course);
explain select teacher.name,student.name from teacher left join student on teacher.id = student.id  where teacher.course = '数学'
-- teacher表type级别为ref
-- student表type级别为ALL
```

## 3.2 优化总结

1. 小表驱动大表
2. 索引建立在经常查询的字段上
3. sql优化，是一种概率层面的优化，是否实际使用了我们的优化，需要通过explain推测

# 4. 避免索引失效

1、复合索引，不要跨列或无序使用（最佳左前缀）；

2、复合索引，尽量使用全索引匹配；

3、不要在索引上进行任何操作，例如对索引进行（计算、函数、类型转换）；

4、复合索引不能使用不等于（!= 或 <>）或 is null（is not null）；

5、尽量使用覆盖索引（using index）；

6、like尽量以常量开头，不要以%开头，否则索引失效；如果必须使用%name%进行查询，可以使用覆盖索引挽救，不用回表查询时可以触发索引；

7、尽量不要使用类型转换，否则索引失效；

8、尽量不要使用or，否则索引失效；

# 5. 其他优化方式

## 5.1 exist和in的选择

```sql
select name,age from student exist/in (子查询);

-- 如果主查询的数据集大，则使用in；
-- 如果子查询的数据集大，则使用exist；
```

## 5.2 order by的优化

using filesort有两种算法：

1. 双路排序（根据IO的次数）：扫描两次磁盘（①从磁盘读取排序字段，对排序字段进行排序；②获取其它字段）；

2. 单路排序：只读取一次（全部字段），在buffer中进行排序。但单路排序会有一定的隐患（不一定真的是只有一次IO，有可能多次IO）。

```sql
-- 单路排序会比双路排序占用更多的buffer，如果数据量较大，可以调大buffer的容量大小
set max_length_for_sort_data = 1024;单位是字节byte。
```

MySQL4.1之前，默认使用双路排序；MySQL4.1之后，默认使用单路排序；如果max_length_for_sort_data值太低，MySQL底层会自动将单路切换到双路。 

提高order by查询的策略：

1. 选择使用单路或双路，调整buffer的容量大小；
2. 避免select * from student;（① MySQL底层需要对*进行翻译，消耗性能；② *永远不会触发索引覆盖 using index）；
3. 复合索引不要跨列使用，避免using filesort；
4. 保证全部的排序字段，排序的一致性（都是升序或降序）；

# 6. 慢查询日志

慢查询日志就是MySQL提供的一种日志记录，用于记录MySQL响应时间超过阈值的SQL语句（long_query_time，默认10秒） ；慢查询日志默认是关闭的，开发调优时打开，最终部署时关闭。

## 6.1 开启慢查询日志

```sql
-- 查询慢查询日志是否开始
mysql> show variables like '%slow_query_log%';
+---------------------+-----------------------------+
| Variable_name       | Value                       |
+---------------------+-----------------------------+
| slow_query_log      | OFF                         |
| slow_query_log_file | /var/lib/mysql/192-slow.log |
+---------------------+-----------------------------+
```

```sql
-- 临时开启
set global slow_query_log = 1;

-- 永久开启，在etc/my.cnf中添加配置
slow_query_log=1
slow_query_log_file=/var/lib/mysql/localhost-slow.log
```

```sql
-- 重启数据库
service mysql restart;
```

## 6.2 设置慢查询阈值

```sql
-- 查询慢查询阈值
mysql> show variables like '%long_query_time%';
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| long_query_time | 10.000000 |
+-----------------+-----------+
```

```sql
-- 临时修改阈值
set global long_query_time = 5;

-- 永久修改阈值，在etc/my.cnf中添加配置
long_query_time = 5;
```

## 6.3 查看慢查询的数量

```sql
show global status like '%slow_queries%';
```

## 6.4 mysqldumpslow工具

通过mysqldumpslow工具查看慢sql，可以通过一些过滤条件，快速查出需要定位的慢sql。 

```shell
mysqldumpslow [ OPTS... ] [ LOGS... ]
-s ORDER     what to sort by (al, at, ar, c, l, r, t), 'at' is default
              al: average lock time
              ar: average rows sent
              at: average query time
               c: count
               l: lock time
               r: rows sent
               t: query time 
-r           reverse the sort order
-t NUM       just show the top n queries
-g PATTERN   grep: only consider stmts that include this string
-h HOSTNAME  hostname of db server for *-slow.log filename (can be wildcard),
               default is '*', i.e. match all
-i NAME      name of server instance (if using mysql.server startup script)
-l           don't subtract lock time from total time
```

# 7. 锁机制

## 7.1 操作分类

读写：对同一个数据，多个读操作可以同时进行，互不干扰。

写锁：如果当前写操作没有完毕，则无法进行其它的读写操作。

## 7.2 操作范围

表锁：一次性对一张表整体加锁。

如MyISAM存储引擎使用表锁，开销小、加锁快、无死锁；但锁的范围大，容易发生冲突、并发度低。

行锁：一次性对一条数据加锁。

如InnoDB存储引擎使用的就是行锁，开销大、加锁慢、容易出现死锁；锁的范围较小，不易发生锁冲突，并发度高.

## 7.3 加锁和查看加锁

```sql
-- 加锁
lock table 表1 read/write，表2 read/write，...
-- 查看加锁的表
show open tables;
```

## 7.4 MyISAM表级锁的锁模式

MyISAM在执行查询语句前，会自动给涉及的所有表加读锁，在执行增删改前，会自动给涉及的表加写锁。所以对MyISAM表进行操作，会有如下情况发生：

1. 对MyISAM表的读操作（加读锁），不会阻塞其它会话（进程）对同一表的读请求。但会阻塞对同一表的写操作。只有当读锁释放后，才会执行其它进程的写操作。
2. 对MyISAM表的写操作（加写锁），会阻塞其它会话（进程）对同一表的读和写操作，只有当写锁释放后，才会执行其它进程的读写操作。

## 7.5 InnoDB分析表锁定

```sql
mysql> show status like '%innodb_row_lock%';
+-------------------------------+-------+
| Variable_name                 | Value |
+-------------------------------+-------+
| Innodb_row_lock_current_waits | 0     |
| Innodb_row_lock_time          | 0     |
| Innodb_row_lock_time_avg      | 0     |
| Innodb_row_lock_time_max      | 0     |
| Innodb_row_lock_waits         | 0     |
+-------------------------------+-------+

-- Innodb_row_lock_current_waits：当前正在等待锁的数量
-- Innodb_row_lock_time：等待总时长。从系统启动到现在一共等待的时间
-- Innodb_row_lock_time_avg：平均等待时长。从系统启动到现在一共等待的时间
-- Innodb_row_lock_time_max：最大等待时长。从系统启动到现在一共等待的时间
-- Innodb_row_lock_waits：等待次数。从系统启动到现在一共等待的时间
```

